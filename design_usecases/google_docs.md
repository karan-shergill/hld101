# Google Docs

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs
2. https://systemdesignschool.io/problems/google-doc/solution
3. https://vishal-vishal-gupta48.medium.com/google-docs-hld-high-level-system-design-d2e62f1d7ff5

## Area to focus

1. If multiple users are making edit to same text, how will we resolve the conflicts?
2. How will we see live cursor position of all the online users?
3. How do we scale to millions of web socket connections? How can Zookeeper/Console/Hash Ring can help us here? How consistence hashing help us here?
4. How do we keep storage under control? How we do Compaction and keep multiple snapshot / version?
5. Document URL creation on sharing (Mask the original document path)
6. Database design & query pattern.
7. How to keep permission data? (OWNER/EDITOR/COMMENTER/VIEWER)
8. How to handle Offline edits to a document?

## New Learning

1. Algorithms for realtime conflict resolution OT(Operational Transformation) & CRDT (Conflict-free Replicated Data Types)
   1. OT(Operational Transformation): https://youtu.be/OHd8M54-mNQ
   2. CRDT (Conflict-free Replicated Data Types): https://youtu.be/UEA9Ti2TdrQ
2. If a service has multiple instance, how to handle them?

## Design Decision

In reference to [link](https://www.hellointerview.com/learn/system-design/problem-breakdowns/google-docs)

If all clients working on the same document connect to a single document server, doesn’t that create a single point of failure?
Why can’t clients editing the same document connect to different document servers, and use a Redis pub/sub (or a similar messaging layer) to synchronize updates between those servers?

<details>
  <summary>See AI Explanation on same</summary>

You’re seeing the right tension here. Yes, “everyone on one document server” *sounds* like a single point of failure, and “multiple servers with Redis pub/sub between them” sounds more fault-tolerant. The catch is that real‑time collaborative editing cares a lot more about **strong ordering and conflict resolution** than most pub/sub setups are designed to guarantee.

You can think of it like running a meeting. It is much easier to keep one person as the moderator for a single discussion than to have three moderators in three rooms shouting updates at each other through walkie‑talkies and hoping they stay in sync.

Let’s break down the trade‑off.

1. **Why a single document server per doc is attractive**

   For a *given* document, you typically want one clear “leader” that:

   - Sees all edits in a single ordered stream
   - Applies your OT / CRDT logic in one place
   - Broadcasts a consistent version to all clients

   If everyone editing doc D1 hits the same document server S1, then:

   - Ordering is “whatever order S1 receives operations in”
   - Conflict resolution runs once, in one process
   - The document’s in‑memory state is the source of truth for live edits

   That keeps your reasoning simple and your bugs fewer, which is huge for something as tricky as collaborative editing.

   To avoid a real single point of failure here, you usually:

   - **Shard by document** (docs 0‑N across many document servers)
   - **Replicate each document server** (primary plus one or more hot standbys)
   - On failure, quickly fail over to a replica that takes over as the new leader

   So from the *system* point of view, there are many doc servers. From the *document* point of view, there is one leader at a time.

2. **What goes wrong with “multiple servers per doc plus Redis pub/sub”**

   Your idea is effectively “multi‑leader for a single document” where each leader is a doc server instance and Redis is the gossip channel.

   You now have to solve some hard problems that Redis pub/sub does not solve by itself:

   - **Total ordering** Redis pub/sub does not guarantee a single global order of messages across subscribers. Two servers might see operations in different orders. OT is very sensitive to order.
   - **Exactly‑once / duplicates** Redis pub/sub is fire‑and‑forget. If a server briefly disconnects, it can miss messages. If you add retries, you can get duplicates. Now every server must dedupe and maybe request missing ops.
   - **Conflict resolution in multiple places** Each server is transforming operations relative to its own view of history. If their histories diverge even slightly, their transformations can diverge, and you end up with different final documents unless you add a second reconciliation layer.
   - **Network partitions** If server A is cut off from Redis for a bit but still has connected clients, it keeps accepting edits. Server B is also accepting edits. When the partition heals, you effectively have two competing timelines for the same doc and must merge them. That is strictly harder than letting one leader reject edits when it is partitioned.

   Can you fix some of this? Yes, but you then end up reinventing a lot of machinery:

   - A proper log with sequence numbers and durable storage instead of vanilla pub/sub
   - A consensus protocol or at least a clear leader election so one node assigns the global order
   - Replay/recovery logic so late servers can catch up

   At that point you have mostly rebuilt “one leader per document plus replication” with more moving parts.

3. **A practical compromise**

   In practice, systems like Google Docs tend to:

   - **Partition by document** Each doc is assigned to one doc server process or shard.
   - **Use that server as the leader** for real‑time edits on that doc.
   - **Make that server highly available** with replication and failover, not by allowing multiple active leaders for the same doc.

   You absolutely can use Redis in this architecture, but usually for:

   - Broadcast fan‑out of already ordered updates from the leader to many frontends
   - Presence / cursor updates where minor inconsistencies are tolerable
   - Caching or routing metadata

   You generally do **not** want Redis pub/sub to be the mechanism that multiple independent leaders use to agree on the order of document operations.


**TLDR**

Your instinct to avoid a single point of failure is good. The usual pattern is “one leader per doc with fast failover” rather than “many leaders per doc synchronized via pub/sub,” because a single authoritative sequencer per document makes real‑time consistency and conflict resolution much simpler.
</details>
