# BookMyShow System Design

# My Approach
![bookmyshow](images/bookmyshow.png)

<details>
  <summary>ChatCPT Design Review</summary>

# ✅ What you did really well

### 1. Clear separation of responsibilities

* **Add Event Service, Booking Service, Payment Service, Search**
* Async ticket creation via queue → good decoupling

👉 This shows strong service decomposition thinking.

---

### 2. Async event ingestion (Queue-based)

* Organizer → Queue → Ticket creation

This is **exactly how high-scale systems work**

* Prevents blocking UI
* Allows retries
* Handles spikes

---

### 3. Search via Elastic + CDC

* DB → CDC → Elasticsearch

This is **production-grade thinking**

* Avoids dual writes
* Keeps search eventually consistent

---

### 4. Seat locking using Redis + TTL

* Temporary lock for 10 min

This is **core to ticketing systems**, and you got it right:

* Prevents overselling
* Handles abandoned carts

---

### 5. CDN for media

* S3 + CDN → correct pattern

---

### 6. Virtual queue for high traffic

* This is 🔥 — many candidates miss this

---

# ⚠️ Key improvements (this is where interviews get interesting)

Now let’s sharpen it.

---

# 🚨 1. Biggest gap: Seat-level contention strategy

Right now you rely on:

* Redis lock
* DB optimistic locking

👉 This **will still fail under extreme concurrency** (like IPL / concert launches)

### Problem

If 50K users try booking the same seat:

* Redis lock race
* DB version check → heavy contention

---

### ✅ Better approach (industry pattern)

Use **single source of truth for seat state**:

#### Option A (Better)

* Maintain **seat state in Redis only (hot path)**
* DB is eventually updated

Flow:

1. Seat availability → Redis
2. Lock → atomic operation (SETNX)
3. Payment success → persist to DB

👉 Redis becomes **primary booking engine**

---

#### Option B (Even better for scale)

* Use **partitioning/sharding by event_id**
* Route all seat ops for an event to same shard

---

# ⚠️ 2. Ticket pre-creation is risky

You said:

> create ticket for every seat

### Problem

* Huge storage (millions of seats × shows)
* Wasteful for low-demand events

---

### ✅ Better approach

Instead of pre-creating tickets:

👉 Store:

* seat map
* booking status separately

Create ticket **only after booking**

---

# ⚠️ 3. Missing idempotency in payment flow

Right now:

* Payment webhook updates DB

### Problem

* Payment gateways send **duplicate callbacks**

---

### ✅ Fix

Add:

* `idempotency_key` or `transaction_id`
* Ensure:

```text
if already_processed(transaction_id) → ignore
```

---

# ⚠️ 4. Redis TTL edge case

You said:

> TTL = 10 mins → seat released

### Problem

What if:

* Payment succeeds at 9:59
* TTL expires before DB update

👉 Seat becomes available again 😬

---

### ✅ Fix

* Extend lock on payment start
* Or:

    * Move seat to **“pending confirmation” state**
    * Finalize before releasing

---

# ⚠️ 5. No rate limiting / bot protection

For high-demand events:

* Bots will kill your system

---

### ✅ Add:

* Rate limiting (per user/IP)
* CAPTCHA before booking
* Queue + token system

---

# ⚠️ 6. Search vs DB inconsistency

You’re using:

* ES for search
* DB for details

### Problem

* Event updated → ES lag

---

### ✅ Fix

* Show **cached response**
* Add **versioning/timestamps**
* Or fallback to DB for critical fields

---

# ⚠️ 7. Booking service doing too much

Your **Event Details & Booking Service**:

* Fetch event
* Seat map
* Lock logic

👉 Too heavy

---

### ✅ Better split

* **Event Service** → metadata
* **Inventory Service** → seat availability
* **Booking Service** → transactions

---

# ⚠️ 8. No mention of caching strategy

You should explicitly mention:

### Add:

* Redis cache for:

    * Event details
    * Venue list
    * Seat map (read-heavy)

---

# ⚠️ 9. Missing failure handling

Examples:

* Queue failure?
* Ticket creation fails?
* Payment success but DB down?

---

### ✅ Add:

* Retry mechanism
* Dead Letter Queue (DLQ)
* Saga pattern (for distributed consistency)

---

# ⚠️ 10. No geo-scaling / partitioning

For large systems:

### Add:

* Partition by:

    * city
    * event_id

---

# 🧠 Bonus (interview-level upgrades)

If you mention these, you stand out:

### 🔹 1. Dynamic pricing

* Prices increase based on demand

### 🔹 2. Seat recommendation

* Best available seats algorithm

### 🔹 3. Real-time updates

* WebSocket for seat map updates

### 🔹 4. Analytics pipeline

* Track demand trends

---

# 🏁 Final verdict

### Your design level:

👉 **Strong mid-to-senior (SDE2)**

### To reach SDE3 / Senior:

Focus on:

* Concurrency correctness (very important)
* Failure scenarios
* Data consistency trade-offs

---

# 👍 Quick summary

### Keep:

✔ Queue-based ingestion
✔ Redis locking
✔ CDN + ES
✔ Virtual queue

### Improve:

❗ Seat booking concurrency model
❗ Remove ticket pre-creation
❗ Add idempotency
❗ Handle TTL edge cases
❗ Add rate limiting + bot protection

</details>