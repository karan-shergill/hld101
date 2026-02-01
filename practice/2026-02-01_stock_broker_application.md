# Stock Broker Application

# My Approach
![stock_broker_application](images/stock_broker_application.png)

<details>
  <summary>ChatCPT Design Review</summary>

Nice work 👌 — this is a **solid first-principles HLD**, and it already hits many interview-grade expectations. I’ll review it like an interviewer would: **what’s strong**, **what’s risky**, and **how to level it up** to “this person really understands trading systems”.

---

## Big Picture Verdict

✅ **Overall**:
This is a **good, scalable brokerage-side design** (not an exchange), with clear separation between:

* **Market data (read-heavy, streaming)**
* **Order management (write-heavy, consistency-critical)**
* **Limit order matching logic**
* **Historical vs live data**

You’re thinking in the **right mental model**.

If you present this well, most interviewers would be happy — but there are **4–5 important refinements** that will significantly improve correctness and realism.

---

# 👍 What You Did Well

## 1. Clear Separation of Concerns (Very Important)

You’ve cleanly separated:

| Area                    | Why it’s good                        |
| ----------------------- | ------------------------------------ |
| Read Stock Data Service | Read-heavy, cacheable                |
| Order Service           | Strong consistency focus             |
| Limit Order Service     | Stateful, symbol-based logic         |
| Exchange Service        | Single integration point             |
| Time-series DB          | Correct choice for historical prices |

This already puts you ahead of many candidates who mash everything into one service.

---

## 2. Live Price Streaming via SSE / Kafka ✅

* Using **SSE/WebSocket** for price updates → correct
* Kafka topics per symbol (logical) → good abstraction
* Redis for hot/latest prices → correct optimization

💡 In interviews, explicitly say:

> “Clients never talk to Kafka directly. Kafka is internal fan-out.”

That shows maturity.

---

## 3. Historical vs Live Data Path Split ✅

This is **excellent**:

* Live prices → Kafka / Redis
* Historical prices → Time-series DB (TimescaleDB)

Many people forget this distinction — you didn’t.

---

## 4. Limit Orders Using Heap (Strong Signal)

This is a **very good idea** and interviewers love it:

* Buy orders → Max-heap
* Sell orders → Min-heap
* One heap per symbol
* Redis for fast access

This maps perfectly to **price-time priority**.

---

## 5. Non-Functional Requirements Awareness ✅

You explicitly called out:

* Low latency
* High consistency
* Minimize exchange connections
* Scalability

That’s a strong HLD habit.

---

# ⚠️ What Needs Improvement (Important)

Now the **real feedback** — this is where designs usually break.

---

## 1. ❌ Limit Order Service Should NOT Call Exchange Directly

### Current flow (problematic)

```
Limit Order Service → Exchange
```

### Why this is risky

* You lose **centralized control** over order lifecycle
* Harder to guarantee **exactly-once execution**
* Exchange sees multiple callers → bad for throttling & compliance

### Better Design (Industry-Style)

```
Limit Order Service
      ↓
Order Service (single writer)
      ↓
Exchange Service
```

✅ **Rule of thumb**:

> Only ONE service talks to the exchange for placing orders.

This gives you:

* Auditability
* Idempotency
* Retry safety
* Easier reconciliation

---

## 2. Order Service Needs Stronger Consistency Guarantees

Right now:

* Order Service writes to DB
* Then triggers downstream actions

### Missing pieces:

* **Idempotency keys** (very important)
* **Transactional boundaries**
* **Outbox pattern**

### Recommended improvement:

Use **Transactional Outbox Pattern**:

```
DB Transaction:
  - Insert order (PENDING)
  - Insert outbox event (ORDER_CREATED)

Async worker:
  - Reads outbox
  - Calls Exchange Service
  - Marks order EXECUTED / FAILED
```

💬 Interview gold line:

> “We never directly call the exchange inside the DB transaction.”

---

## 3. Kafka Topics per Symbol ❌ (At Large Scale)

This works conceptually, but at scale it breaks.

### Problem:

* Thousands of symbols = thousands of topics
* Kafka metadata & partition overhead explodes

### Better approach:

* Few topics (e.g. `market-data`)
* Partition by `symbol`
* Consumer groups scale horizontally

```
topic: market-data
key: stock_symbol
```

Still preserves ordering **per symbol**.

---

## 4. Redis Heap for Limit Orders → Persistence Gap ⚠️

You mentioned:

> Redis cache (for LIMIT order)

### What happens if Redis crashes?

* You lose all pending limit orders

### Fix:

* Redis = **speed**
* DB = **source of truth**

Recommended:

* Persist limit orders in DB
* On restart, rebuild Redis heaps from DB
* Or snapshot heaps periodically

Say this explicitly in interview.

---

## 5. Exchange SSE → Fanout Bottleneck

Right now:

```
Exchange → SSE → Read Exchange Service
```

At scale:

* Exchanges don’t like too many streams
* You should **buffer & normalize**

Better:

```
Exchange → Market Data Ingestor
         → Kafka
         → Read Stock Data Service
```

Kafka becomes your **shock absorber**.

---

# 🔧 Architectural Improvements (Level-Up)

These aren’t mandatory, but they push you into **senior-level thinking**.

---

## 1. Add Risk & Validation Layer (Very Realistic)

Before order placement:

* Funds check
* Quantity limits
* Market hours
* Fat-finger protection

```
Client → API Gateway
      → Risk Service
      → Order Service
```

Interviewers love this.

---

## 2. Add Read-Optimized Order View

Instead of hitting Order DB for UI:

* Maintain **Order Read Model**
* Eventually consistent
* Optimized for UI queries

CQRS-lite approach.

---

## 3. Webhook ACK Handler Needs Idempotency

Exchanges retry webhooks.

Your handler must:

* Deduplicate by exchange transaction ID
* Be idempotent

Mention this explicitly.

---

# ⭐ Final Interview Assessment

### If I were interviewing you:

I’d say:

✅ Strong fundamentals
✅ Correct mental model of trading systems
✅ Good data separation
⚠️ Needs tightening around consistency & exchange interaction
🎯 Easily fixable with explanation

### Overall Rating:

**8 / 10 HLD**
With fixes above → **9+**

</details>