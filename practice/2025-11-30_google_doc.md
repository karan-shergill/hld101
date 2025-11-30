# Google Docs

- [ChatGPT HLD Review](#chatgpt-hld-review)
- [Summary — what looks good](#summary--what-looks-good)
- [High-impact areas for improvement (ranked)](#high-impact-areas-for-improvement-ranked)
- [Concrete architectural suggestions / additions](#concrete-architectural-suggestions--additions)
- [Sample schemas & sample rows](#sample-schemas--sample-rows)
    - [Metadata DB (RDBMS) — `document_table`](#metadata-db-rdbms--document_table)
    - [ACLs table — `document_acl_table`](#acls-table--document_acl_table)
    - [Document Operations DB (Cassandra / wide-row) — `document_operation_table`](#document-operations-db-cassandra--wide-row--document_operation_table)
- [Compaction & snapshot metadata (store in metadata DB)](#compaction--snapshot-metadata-store-in-metadata-db)
- [Interview questions to ask the candidate (grouped)](#interview-questions-to-ask-the-candidate-grouped)
- [Small checklist / quick wins to add to the diagram](#small-checklist--quick-wins-to-add-to-the-diagram)
- [Example questions you (as interviewer) can probe during interview](#example-questions-you-as-interviewer-can-probe-during-interview)
- [Final notes / what to show next in the diagram](#final-notes--what-to-show-next-in-the-diagram)

![google_docs.png](images/30-11-2025-google_docs.png)

![google_docs.png](images/30-11-2025-google_docs_2.png)

# ChatGPT HLD Review

Nice diagram — good high-level separation (metadata vs ops DB, compaction/snapshots to S3, a dedicated metadata service, IMS for auth, and a pool of document/OT servers). Below I give focused feedback, concrete improvements, small sample data for the two DBs, and a set of interview questions you can ask a candidate (and expect to be asked).

# Summary — what looks good

* Clear separation of responsibilities: Metadata (RDBMS) vs Document operations (NoSQL/append-only).
* Compaction service that writes snapshots to S3 and truncates/compacts ops — correct core idea.
* Use of a cache between metadata service and DB (good for read latency).
* Dedicated IMS for identity and ACL enforcement — nice separation of auth concerns.
* Document service instances + Redis cache for low-latency cursor/presence — appropriate.

---

# High-impact areas for improvement (ranked)

1. **WebSocket scaling & routing**

    * Diagram shows load balancer → doc service instances, but you must show sticky routing or a shared pub/sub for presence/fanout. If you use non-sticky LBs you need cross-instance pub/sub (Redis pub/sub, NATS, Kafka) for fanout of edits & cursor events.
    * Consider connection proxies (e.g., NGINX/Envoy) and autoscaling concerns (connection churn, file descriptors).

2. **Ordering & global sequencing of operations**

    * OT requires a consistent ordering model: per-document sequence numbers or a server-generated global sequence per doc. Diagram should show where sequence generation happens (document service leader? separate sequencer?). Without it convergence will be problematic.
    * Add deduplication/idempotency info (client retry handling and unique op IDs).

3. **Sharding & partitioning of document ops DB**

    * Ops DB (Cassandra) supports wide rows, but you must show sharding by `doc_id` and how hot docs are handled. Hot-spot mitigation (move popular docs to different partitions or throttle clients) should be explicit.

4. **Compaction / snapshot protocol details**

    * Who triggers snapshot creation? Frequency? RPO/RTO implications?
    * Must specify: compaction creates new snapshot with snapshot_version, writes to S3, then truncates ops older than snapshot_version, and stores compaction metadata in metadata DB. Also record compaction jobs status and failure/retry logic.

5. **Presence / cursor model**

    * Diagram mentions "track live cursor position" — describe how presence is fanned out (pub/sub channel per doc?), how stale presence is expired, and how to prevent presence storms when thousands of cursors update frequently.

6. **Consistency guarantees and tradeoffs**

    * Liveness vs consistency, OT vs CRDT choice and why OT was chosen. Include explanation of intention preservation guarantees and conflict resolution approach.

7. **Security and ACL enforcement**

    * ACLs stored in metadata DB are good — but ensure enforcement happens at the document service on every op (not just at metadata service). Provide signed URLs/short-lived tokens for S3 snapshots and audit trails for ops.

8. **Operational concerns**

    * Monitoring, metrics (ops/sec per doc, connections/instance, op latency, compaction lag), alarms, and chaos testing. Add metrics endpoints, dashboards, and runbooks for degraded fanout or compaction failures.

9. **Failure modes and recovery**

    * Show how to handle partial compaction failures, ops ingestion rate spikes, network partitions (multi-region), and how clients resync (e.g., fetch latest snapshot + replay ops).

10. **Cost & retention**

    * Add policy: retention window for ops DB (e.g., keep last N versions or M days), storage size estimates, S3 lifecycle (GLACIER), and compaction tradeoffs.

---

# Concrete architectural suggestions / additions

* **Sequencer / Leader per doc**: either elect a leader per doc (via consul/zookeeper/raft) or use a central sequencer service to assign monotonically increasing sequence numbers per `doc_id`. Show this in the diagram.
* **Per-doc pub/sub channel**: document service publishes ops to a pub/sub (Redis streams, Kafka, NATS JetStream). Other instances subscribe to that doc channel to forward ops to connected clients.
* **Hot-spot mitigation**: add a caching/fanout tier (dedicated fanout nodes) for very popular docs, and rate-limit cursor updates (throttle and sample updates).
* **Snapshot/compaction workflow**:

    1. Trigger: compaction triggered when ops length > threshold or time-based.
    2. Document service or a dedicated compaction worker builds a snapshot, writes to S3 with metadata, records `snapshot_version` in metadata DB.
    3. Compaction service deletes/truncates ops up-to `snapshot_version` and marks compaction complete.
    4. Make compaction idempotent and record audit trail for rollback.
* **Client sync protocol**: define clear client sync sequence:

    * On reconnect: fetch metadata (current_snapshot_id), fetch snapshot from S3, fetch ops > snapshot_version, apply ops locally.
    * Provide a fast-path: if client has recent base_version, only fetch incremental ops.
* **OT correctness tests**: add a test harness that injects random concurrent ops and validates convergence (unit/integration).
* **Offline edits**: handle sequence reconciliation when a client reconnects with offline ops; consider vector clocks or causal metadata.
* **Metrics & SLOs**: plan SLOs for op application latency (e.g., 99th percentile < 200ms), snapshot creation time, fanout latency, and WS connect success.

---

# Sample schemas & sample rows

## Metadata DB (RDBMS) — `document_table`

Schema (columns):

```sql
document_id UUID PRIMARY KEY,
created_on TIMESTAMP,
owner_id UUID, -- FK to user
title STRING,
last_modified TIMESTAMP,
current_snapshot_id UUID, -- points to S3 snapshot meta
size_bytes BIGINT,
is_archived BOOLEAN,
is_public BOOLEAN,
tags JSONB
```

Sample row:

```json
{
  "document_id": "7f3f3e20-1111-4a2b-8b3a-0a1b2c3d4e5f",
  "created_on": "2025-11-01T10:00:00Z",
  "owner_id": "2a9d1c5f-aaaa-4b5c-9d6e-111222333444",
  "title": "Project Plan",
  "last_modified": "2025-11-30T11:02:33Z",
  "current_snapshot_id": "snap-0001",
  "size_bytes": 234567,
  "is_archived": false,
  "is_public": false,
  "tags": ["planning","q4"]
}
```

## ACLs table — `document_acl_table`

```sql
document_id UUID,
user_id UUID,
role ENUM('OWNER','EDITOR','COMMENTER','VIEWER'),
PRIMARY KEY(document_id, user_id)
```

Sample rows:

```json
{"document_id":"7f3f...5f","user_id":"2a9d...334","role":"OWNER"}
{"document_id":"7f3f...5f","user_id":"3b7c...999","role":"EDITOR"}
```

## Document Operations DB (Cassandra / wide-row) — `document_operation_table`

Schema:

```
doc_id (partition key),
seq BIGINT (clustering key, ascending),
op_id UUID,         -- idempotency
ts TIMESTAMP,
user_id UUID,
base_version BIGINT, -- client base version
op_type STRING,      -- INSERT/DELETE/PATCH/RETAIN etc.
op_payload BLOB/json,
is_compacted BOOLEAN
```

Sample rows (in chronological order):

```json
{
  "doc_id": "7f3f3e20-1111-4a2b-8b3a-0a1b2c3d4e5f",
  "seq": 1001,
  "op_id": "c1b2...",
  "ts": "2025-11-30T10:59:00Z",
  "user_id": "3b7c...999",
  "base_version": 1000,
  "op_type": "INSERT",
  "op_payload": {"pos": 125, "text":"Hello"},
  "is_compacted": false
}
```

---

# Compaction & snapshot metadata (store in metadata DB)

Schema `snapshot_table`:

```
snapshot_id UUID,
doc_id UUID,
version BIGINT,
s3_path STRING,
created_on TIMESTAMP,
created_by STRING,
status ENUM('IN_PROGRESS','DONE','FAILED')
```

---

# Interview questions to ask the candidate (grouped)

### Architecture & high-level

1. Walk me through how an edit from a client is delivered to all other clients. Where is ordering assigned? How do you guarantee everyone converges?
2. Why did you choose OT instead of CRDT? What are tradeoffs?
3. How would you scale to **millions** of concurrent WebSocket connections? (be specific about LB, proxies, sticky sessions, and pub/sub)
4. Explain the compaction lifecycle. What happens if compaction fails halfway?

### Data model & storage

5. Show sample schema for metadata and ops (ask for indexes). How will you query "list all docs I can see" efficiently?
6. How would you shard the document ops DB? How do you handle hot documents?

### Consistency & correctness

7. How do you ensure idempotency if the client retries the same op? What unique identifiers are required?
8. How do you handle offline edits and reconciliation? (vector clocks, base versions, merge rules)
9. How do you test correctness of OT implementation? What test harness would you build?

### Performance & scaling

10. How do you handle fanout for a doc with 100k connected clients? (batching, multicast, CDN, rate-limiting)
11. What are the key metrics and SLOs you would define? (fanout latency, op apply latency, snapshot lag)
12. How would you reduce bandwidth for cursors/presence updates?

### Reliability & operations

13. What are failure modes and recovery steps for: compaction job failure, ops DB node down, network partition?
14. How would you perform backup & restore of the system?
15. How would you mitigation DDoS or abuse: malicious clients spamming ops?

### Security & compliance

16. Where are ACL checks enforced? How do you ensure no unauthorized ops are applied?
17. How do you protect snapshots and S3 objects (signed URLs, encryption, access logs)?
18. How do you audit edits and present history for compliance?

### Cost & product tradeoffs

19. Given cost constraints, what would you optimize first (storage vs latency vs consistency)?
20. How long would you keep raw ops vs snapshots? How would you configure S3 lifecycle?

---

# Small checklist / quick wins to add to the diagram

* Show **sequencer** or **leader election** for per-doc ordering.
* Add explicit **pub/sub** (Redis/Kafka) between document services for fanout.
* Add **snapshot metadata table** in the diagram.
* Show **lock / leader** mechanism (consul/zk/raft) for compaction to avoid two compactions on same doc.
* Indicate where ACL enforcement happens (metadata service + runtime checks in document service).
* Add monitoring boxes: Prometheus, Grafana, tracing (Jaeger), and logging.
* Add offline sync flow (client reconnect diagram).
* Add rate-limiting & throttling component.

---

# Example questions you (as interviewer) can probe during interview

* "Describe the client reconnect flow and how we prevent duplicate application of operations when the client resends ops after reconnect."
* "How will you handle schema migration for the metadata DB while cluster is live?"
* "If a compaction deletes ops but you later find a bug in snapshot creation, how do you roll-back or re-create events needed to build correct snapshots?"
* "Give capacity planning: if average op is 500 bytes and you expect 50M ops/day, estimate storage and throughput needs." (expect candidate to show calculations, assumptions, and tradeoffs)

---

# Final notes / what to show next in the diagram

If you update the diagram, add:

1. Sequencer / leader/ordering component.
2. Pub/sub (labeled with chosen tech) between document instances for fanout.
3. Snapshot metadata & compaction status boxes.
4. Explicit arrows showing where ACL checks happen (both at auth and at op apply).
5. Monitoring, tracing, and runbook references.

---



