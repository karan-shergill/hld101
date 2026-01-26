# Distributed Cache

# My Approach
![distributed_cache](images/distributed_cache.png)

## Functional Requirements

1. Client can GET / PUT and DELETE from cache
2. LRU will be the eviction policy for the cache

## Non-Functional Requirements

1. Cache needs to be scalable
2. Highly available (From CAP, we need AP, eventual consistency is accepted)
3. Low latency
4. Durable

## HLD FLows

### Users should be able to set, get, and delete key-value pairs

1. For MVP we can use a hashMap as our cache
2. HashMap can be put on a separate server that will be requested by the client to GET, PUT and DELETE key and value.
3. GET PUT and DELETE will be done in O(1) using hashMap

### Users should be able to configure the expiration time for key-value pairs

1. To have a TTL, we can save the TTL time in the value section of hashMap itself
2. When a new key value is put, in addition we will put TTL time stamp as well like map.put(key, value + (curr_time+TTL))
3. So when client does a GET, and hashMap has the key, before returning the value, we can check the TTL value as well, if TTL is beyond the current time, we can return null and actually delete this key-value pair.
4. We can have a smaller worker named Janitor in the cache server that will run every 1/5 min and check all the TTL for all key, and if found any expired TTL, it can delete that key value pair, this is not let the hashMap increase in size with deletion via TTL happening only during GET.

### Data should be evicted according to the LRU policy

1. To implement LRU eviction policy we can use a Doubly linked list (DLL)
2. Whenever a KEY is PUT or GET that KEY can be placed at the HEAD of the DLL
3. We will keep this NODE address in the value of the cache hashmap so that we can access any node of DLL at O(1) times
4. SO all the recently accessed data will be at head node
5. when we need to evict any data from hashMap we can evict the key value that’s at the TAIL of DLL

### How do we ensure our cache is highly available and fault-tolerant?

1. In MVP we has only single server with hashMap & DLL as the cache
2. To make this highly available we can need to implement replication
3. We can have here master slave architecture, where master cache node is been used for all WRITEs and the slave nodes would be used for READS
4. Whenever there is an update in master node, slave node need to replicate that change in its own node.
    1. If we want a high consistent cache we can have replication factor high, i.e. when update is done in all cache nodes then only respond back - this can make cache slow
    2. So to keep cache fast we can keep a quorum as replication factor for our cache
5. Using quorum as replication factor we have eventual consistency but latency is lower
6. When a MASTER node is crashed any SLAVE node can be promoted as MASTER node
7. How to keep track of MASTER & SLAVE node? How will client know which node to call while WRITE and which one to call while READ? When Master node crashed how slave node is selected for promotion to Master node - for all this we can has a Zookeeper
8. Zookeeper will have info regarding all master and slave node, that info it will pass to client server that needs to GET or PUT in cache
9. Zookeeper will also be involved in identifying the correct slave node who can be promoted to Master node

### How do we ensure our cache is scalable?

1. As size of cache increases it not possible to keep all data in single node, and here we need to shard the cache data
2. The shard logic can be dependent on KEY, but we need to take care that distribution is eventual so that we don’t make a hot shard issue
3. We have have multiple Master cache servers that will keep the shared part of data
4. How will client know which shard to request? Here as well Zookeeper will help, which shard to call will be decided using consistent hashing ring, that will be kept in the Zookeeper and is hashed on key - as we using  consistent hashing this ensure we have minimal data transferred when a new shard is added or existing one is deleted

### How can we ensure an even distribution of keys across our nodes?

1. consistent hashing on key to decide shard
2. consistent hashing ring is kept in zookeeper
3. before reaching the shard client first makes request to the zookeeper to know which shard has data for the key, or in which shard to keep this key value pair

### What happens if you have a hot key that is being read from a lot?

1. We will have analytics on keys
2. If a single key is READ a lot in set time window, we can decide wether its a HOT key or not
3. If its a HOT key we can further add this same key in diffrent shard with prefix number in the key
4. SO same key and value is stored in diffrent multiple shards to READ from

### What happens if you have a hot key that is being written to a lot?

1. We will have analytics on keys
2. If a single key is UPDATE a lot in set time window, we can decide wether its a HOT key or not
3. If its a HOT key we can further add this same key in diffrent shard with prefix number in the key
4. SO same key and value is stored in diffrent multiple shards to Update to - BUT this added complexity to keep these key on diffrent shard in sync

<details>
  <summary>ChatCPT Design Review</summary>

## 1️⃣ What you did really well

### ✅ Core cache fundamentals (very strong)

* HashMap + DLL for **O(1) GET/PUT/DELETE** → textbook correct
* Correct LRU handling (move to head on GET/PUT, evict tail)
* TTL handling + background janitor → realistic and production-like

👉 This part alone is already “hire-level”.

---

### ✅ Clear CAP trade-off

* You explicitly choose **AP**
* You accept **eventual consistency**
* You understand quorum vs strong consistency

Most candidates forget to say this explicitly — good signal.

---

### ✅ Sharding + Consistent Hashing

* You correctly:

    * Use **consistent hashing**
    * Minimize rebalancing when nodes change
    * Think about **hot shard** issues

---

### ✅ Hot key awareness (big plus)

Most people completely miss this.

You considered:

* Hot read keys
* Hot write keys
* Analytics-driven detection

This is **advanced thinking** 👌

---

## 2️⃣ Major gaps / risks (important)

Now the **critical feedback** — this is where interviewers will push you.

---

## 🔴 Issue 1: Client → ZooKeeper on every request ❌

> *“before reaching the shard client first makes request to the zookeeper”*

This is a **big red flag**.

### Why this is a problem

* ZooKeeper is **not designed for high QPS**
* Cache traffic = **millions of requests/sec**
* ZK should **never** be in the hot path

### Fix (must say this)

✅ Clients **cache metadata locally**

* Shard → node mapping
* Master/slave info
* Consistent hashing ring

ZooKeeper is used only for:

* Membership changes
* Failover
* Rebalancing notifications

📌 **Pattern:**

> ZooKeeper = control plane
> Cache nodes = data plane

---

## 🔴 Issue 2: Master-Slave for cache writes (sub-optimal)

Master-slave works, but it has drawbacks **for a cache**.

### Problems

* Write bottleneck on master
* Failover complexity
* Slower write path

### Better alternatives

You should at least **mention** these:

#### Option A: Leaderless replication (Dynamo-style)

* Client writes to **N replicas**
* Uses quorum (W + R > N)
* No single master bottleneck

#### Option B: Master per shard (acceptable, but say this clearly)

* One master **per shard**
* Not global master
* Limits blast radius

📌 Interviewer-friendly line:

> “Master-slave is okay per shard, but for very high write throughput, a leaderless quorum-based approach scales better.”

---

## 🔴 Issue 3: LRU is local, not global ❗

Right now:

* LRU works **only within a node**
* But eviction is **not globally optimal**

This is acceptable — but you must **acknowledge the trade-off**.

### What to say

* Global LRU is too expensive
* Per-node LRU is an industry standard compromise
* Slightly suboptimal eviction is acceptable for caches

👍 This shows maturity.

---

## 🔴 Issue 4: Durability claim is weak

You say:

> “Cache should be durable”

But current design:

* In-memory HashMap
* Data lost on restart

### Options (pick one and explain)

1. **Accept non-durability** (most caches do)
2. Write-ahead log (WAL)
3. Periodic snapshot to disk / object storage
4. Backed by source of truth (DB)

📌 Strong statement:

> “Cache durability is best-effort; source of truth is DB. On restart, cache is rebuilt.”

---

## 🔴 Issue 5: Hot key sharding via key suffix = risky

This part is **conceptually correct**, but dangerous.

### Problems

* Writes must update **multiple shards**
* Read-write inconsistency
* Complex client logic

### Better approaches

Mention at least one:

#### Option A: Read replicas only

* One writer
* Many read replicas
* Good for read-heavy hot keys

#### Option B: Request coalescing

* One backend request per key
* Others wait

#### Option C: In-node caching

* Cache hot key in multiple nodes
* TTL-based

---

## 3️⃣ Concrete improvements (what to add)

If you add these, your HLD becomes **top-tier**:

---

### 🔹 Add request flow (GET / PUT)

Example:

**GET**

1. Client hashes key → shard
2. Picks nearest healthy replica
3. Checks TTL
4. Updates LRU
5. Returns value

**PUT**

1. Client hashes key → shard
2. Writes to quorum replicas
3. Async replication
4. Update LRU + TTL

---

### 🔹 Failure scenarios (interview gold)

Add explicit handling for:

* Cache node down
* Master down
* Network partition
* Split brain (ZK fencing)

---

### 🔹 Metrics & observability

Mention:

* Hit ratio
* Eviction count
* Hot key detection
* Replication lag

---

## 4️⃣ Cleaned-up “ideal” version (summary)

If I had to **summarize your design in 5 lines**:

> We design an AP, low-latency distributed cache using sharded in-memory nodes.
> Each shard uses HashMap + DLL for O(1) LRU eviction and TTL handling.
> Consistent hashing ensures scalability with minimal rebalancing.
> Replication with quorum provides high availability and eventual consistency.
> ZooKeeper is used only for coordination, not on the request path.

That’s an **excellent answer**.

---

## Final Verdict 🎯

**Overall rating: 8.5 / 10**

What’s strong:

* Core cache internals
* CAP trade-offs
* Sharding & hot key awareness

What to improve:

* Remove ZooKeeper from hot path
* Clarify replication strategy
* Be explicit about durability trade-offs

</details>