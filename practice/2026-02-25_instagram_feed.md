# Instagram Feed

# My Approach
![instagram-feed](images/instagram-feed.png)

<details>
  <summary>ChatCPT Design Review</summary>

Great job 👏 — this is actually a **solid HLD**. You’ve clearly thought through:

* Media upload via pre-signed URL
* Content processing pipeline
* DynamoDB schema design
* Fan-out at write vs read
* Celebrity optimization
* CDN separation
* Queue-based async processing

This is already **interview-level good**. Now I’ll review it like a system design interviewer would.

---

# 🔎 High-Level Feedback Summary

### ✅ What You Did Very Well

* ✔ Correct separation of **write vs read services**
* ✔ Correct usage of **pre-signed S3 upload**
* ✔ Good use of **queue for async fan-out**
* ✔ Hybrid strategy (fan-out write + fan-out read for celebrities)
* ✔ Redis cache for feed
* ✔ CDN for media delivery
* ✔ Proper DynamoDB partition key design

You’re thinking in the right direction.

---

# 🧠 Now Let’s Go Deeper — Detailed Review

---

# 1️⃣ Post Creation Flow

### ✅ What’s Good

* Pre-signed URL → user uploads directly to S3 (correct)
* Metadata written to DynamoDB
* S3 event → processing pipeline
* Async processing via queue (correct)

### ⚠ Improvements

### ❗ Issue 1: Race Condition Possibility

You currently:

* Upload to S3
* Write metadata
* Then processing updates manifest

But what if:

* User reads feed before processing finishes?

You need:

* A `status` field in Post Table

Example:

```
status: UPLOADING | PROCESSING | READY | FAILED
```

Feed service should only serve:

```
status = READY
```

---

### ❗ Issue 2: Idempotency

What if:

* S3 sends duplicate event?
* Queue retries?
* Lambda retries?

You need:

* Idempotent processing
* Either versioning or status check before updating DB

---

# 2️⃣ DynamoDB Schema Review

You proposed:

```
Post Table:
PK = user_id
SK = timestamp
```

### ✅ Good for:

* Fetching user profile posts (sorted by time)

### ⚠ But for Feed System — You Also Need:

```
PostId as primary entity
```

Why?

Because:

* Feed stores post_id references
* Feed service will need quick lookup by post_id

Better approach:

## Option A (Recommended)

### Post Table:

```
PK = post_id
Attributes:
  user_id
  timestamp
  caption
  s3_manifest
  cdn_manifest
  status
```

### GSI:

```
GSI1:
  PK = user_id
  SK = timestamp
```

This gives:

* Fast profile fetch
* Fast direct post lookup

More scalable.

---

# 3️⃣ Feed Generation – Fan-Out Strategy

You correctly chose:

* Normal users → Fan-out at write
* Celebrities → Fan-out at read

Excellent.

---

## 🔥 But Here’s What You’re Missing

### ❗ Issue: Redis Storage Explosion

You said:

> For each follower, we push post_id into Redis cache list.

Let’s calculate.

If:

* User has 10K followers
* 1 post

That’s 10K writes.

If:

* 1M users post daily
* Avg 500 followers

That’s 500M Redis writes/day.

This becomes expensive.

---

## 🧠 Improvement: Don’t Store Full Feed in Redis

Instead:

### Store Feed in Persistent DB (Fan-out Table)

```
Feed Table:
PK = user_id
SK = post_timestamp
post_id
```

Then:

* Cache only top N posts in Redis
* Use lazy cache population

This prevents Redis from becoming primary DB.

---

# 4️⃣ Celebrity Strategy

You defined:

> > 50K followers = celebrity

That’s good.

But:

### ❗ Problem: Merging at Read

You said:

* Read from cache
* Then fetch celebrity posts
* Merge
* Sort

That means:

* Every read requires sorting and merging
* Latency increases

---

## Better Approach

Maintain:

### 2 Feed Sources:

* HomeFeed (fan-out write)
* CelebrityFeed (global)

When reading:

* Fetch from both
* Merge using **k-way merge with heap**
* Return top 20

This is acceptable.

But to optimize further:

* Cache merged result for short TTL (30s)

---

# 5️⃣ Missing Important Components

Here’s what interviewers expect but you didn’t mention:

---

## 🔹 A. Ranking Service

Instagram feed is NOT chronological.

You need:

* ML ranking layer

Flow:

```
Raw Feed → Ranking Service → Sorted Feed
```

Without ranking, system is incomplete.

Even if you mention:

> “Initially chronological, later plug ML ranking”

That’s good.

---

## 🔹 B. Pagination

You didn’t mention:

* How pagination works?
* Cursor-based or offset?

Correct approach:

* Cursor-based pagination
* Pass last_seen_timestamp or feed_cursor

---

## 🔹 C. Hot Key Problem

If:

* 10M users follow 1 celebrity
* Feed read will repeatedly fetch same celebrity posts

Solution:

* Cache celebrity post list separately
* TTL caching
* Possibly CDN edge caching metadata

---

## 🔹 D. Backfill Strategy

When:

* User follows someone new

How do we populate historical posts?

Need:

* Async backfill job

---

## 🔹 E. Deletion Flow

What happens when:

* Post is deleted?
* User blocks someone?

You need:

* Feed cleanup strategy

---

# 6️⃣ CDN + Media Pipeline Review

Very good thinking.

But you should clarify:

* Use CloudFront or similar CDN
* Store only processed media
* Original S3 object may be private
* CDN URL signed if private content

Also mention:

* Video chunking via HLS/DASH
* Multiple resolution versions

That shows depth.

---

# 7️⃣ Scalability Discussion (Interview Gold)

Add numbers:

Assume:

* 100M DAU
* 5M posts/day
* Avg 500 followers

Then estimate:

* Writes/sec
* Read QPS
* Storage size

This makes your design interview-ready.

---

# 🎯 Overall Architecture Score

If I were interviewer:

| Area                  | Score  |
| --------------------- | ------ |
| Core Design           | 8.5/10 |
| Scalability Thinking  | 7/10   |
| Edge Cases            | 6.5/10 |
| Production Readiness  | 7/10   |
| Communication Clarity | 8/10   |

Overall: **7.8–8.2 / 10**

Very strong.

---

# 🚀 How To Make This 9.5/10

Add these:

1. Ranking service
2. Feed table separate from Redis
3. GSI for post lookup
4. Cursor pagination
5. Post status lifecycle
6. Backfill & delete handling
7. Idempotency handling

---

# 🧠 Important Question For You (Thinking Exercise)

If:

* A celebrity with 10M followers posts
* 1M users open app at same second

How will you prevent:

* Thundering herd on DynamoDB?

Think about:

* Caching layers
* Pre-computation
* TTL
* Read replicas

If you answer this well → you’re senior-level.

</details>