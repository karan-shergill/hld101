# Price Tracking Service

# My Approach
![price_tracking_service](images/price_tracking_service.png)

Flow:
1. Via Client browser Price Update Service get data of the amazon page that client is on(the DOM)
2. If this product is not new, we save the details of this product in our products table.
3. If we feel the price drop/up is genuine we can directly add the new price entry in our Price table
4. If we feel the price drop/up is genuine we pass the info to a queue.
5. Web crawler scheduler pulls the info.
6. Product info added in priority queue.
7. Product info reached web crawler worker.
8. Get actual price from amazon website.
9. Web crawler worker node add price info in the price table.
    1. In that webpage if we have other hyperlinks for other products, we add info of these products in the Web crawler scheduler.
10. Web crawler worker node add new product info on products table.
11. Client via web app or extension ask for price history of a product.
12. Price History Service get price history info from price table.
13. Client see a price history chart.
14. Client subscription for a product, to get notification on price from below threshold.
15. Subscription service saves the subscription details in subscription table.
16. When a new price entry for a product is added to price table via change data capture (CDC) info of price drop in sent to a data store like kafka.
17. Price Change Notification Service pulls even info from the kafka.
18. Price Change Notification Service get subscription details again product id and check for which user’s subscription the threshold condition holds true.
19. Get email info of user.
20. Check Notification Table to rate limit no of email send last to current user and to check wether last email notification was for same / diff products.
21. Send email notification.

<details>
  <summary>ChatCPT Design Review</summary>
Your design is **quite good for a senior engineer interview**. It shows strong thinking about **data ingestion, validation, async pipelines, crawling, and notifications**. Many candidates miss these aspects. 👍

I'll review it like an **interviewer reviewing a system design** and split it into:

1. 👍 What is good in your design
2. ⚠️ Gaps / risks
3. 🚀 Improvements that would make it “senior-level strong”

---

# 👍 What is good in your design

## 1. Multiple data sources for price updates

You intelligently combine:

* **Client browser extension data**
* **Web crawler verification**

This is exactly how systems like Keepa work.

Why this is good:

* Crawlers alone are expensive.
* User traffic gives **free signals** of price changes.

Your flow:

```
Browser -> Price Update Service -> Validate -> Queue -> Crawler verification
```

This is **very strong thinking**.

---

# 👍 2. Validation before trusting price updates

You added:

```
If price change seems genuine -> write
Else -> verify via crawler
```

This prevents:

* malicious extensions
* parsing errors
* temporary DOM glitches

Excellent design decision.

---

# 👍 3. Asynchronous architecture

You correctly use:

* Queue
* Scheduler
* Worker nodes

This is critical because:

* Crawling is slow
* Price updates can spike

So this pipeline is good:

```
Queue
 → Scheduler
 → Priority Queue
 → Crawler Workers
```

Very scalable.

---

# 👍 4. Priority queue for products

You introduced:

```
Priority queue for products
```

Good thinking.

Typical priorities:

| Priority | Product type     |
| -------- | ---------------- |
| High     | Many subscribers |
| Medium   | Popular products |
| Low      | Unknown products |

This improves **resource allocation**.

---

# 👍 5. CDC → Kafka for notifications

This is a **very good event-driven design**.

```
Price Table
   ↓ CDC
Kafka
   ↓
Notification Service
```

Benefits:

* decouples services
* scales notifications
* avoids polling

Very solid system design concept.

---

# 👍 6. Notification rate limiting

You added:

```
Notification Table
→ rate limit emails
```

This shows product thinking.

Example problems you avoided:

* sending 20 emails for fluctuating price
* spamming users

Good detail.

---

# 👍 7. Separation of services

You have clear services:

* Price Update Service
* Price History Service
* Subscription Service
* Notification Service

This is **clean microservice separation**.

---

# 👍 8. Product discovery via crawler

You added:

```
Crawler finds other product links
→ schedule crawling
```

That is **graph-based crawling** used by search engines.

Great addition.

---

# Overall score (interview perspective)

Your design would likely get **7.5 / 10** from an interviewer.

To reach **9 / 10**, you need improvements below.

---

# ⚠️ Major areas of improvement

## 1. Price table will explode (BIG issue)

Your design stores:

```
product_id
timestamp
price
```

But think scale.

Example:

```
100M products
price update every 30 minutes
```

Daily writes:

```
100M × 48 = 4.8B rows/day
```

Year:

```
~1.7 trillion rows
```

Your DB will collapse.

---

### How Keepa solves this

They **compress price history**.

Instead of:

```
timestamp | price
```

They store:

```
price changes only
```

Example:

```
10:00 -> $100
12:00 -> $90
16:00 -> $95
```

Not every sample.

---

### Better design

Use **time-series DB**:

Options:

* Cassandra
* ClickHouse
* TimescaleDB
* Bigtable

---

# ⚠️ 2. Product ID strategy missing

You use:

```
product_id
```

But Amazon products are identified by:

```
ASIN
```

Your system must map:

```
ASIN -> product_id
```

Better:

```
PK = ASIN
```

---

# ⚠️ 3. No caching layer

Price history queries will be heavy.

Example:

```
Show 1 year price graph
```

Without cache:

```
millions of reads/day
```

You should add:

```
Redis Cache
```

Architecture:

```
Client
 → API
 → Redis
 → DB
```

Cache:

```
product_price_history
```

---

# ⚠️ 4. Web crawler politeness / blocking

Amazon **blocks aggressive crawlers**.

Your design lacks:

* crawl delay
* proxy pool
* rotating IPs

Needed:

```
Crawler Worker
  ↓
Proxy Pool
  ↓
Amazon
```

Otherwise you get **IP banned quickly**.

---

# ⚠️ 5. Deduplication problem

Multiple sources may push same price:

* crawler
* extension
* crawler again

You must deduplicate.

Example rule:

```
if (price == last_price)
    ignore
```

---

# ⚠️ 6. Missing sharding strategy

Your databases must be sharded.

Example:

```
Shard by product_id hash
```

Otherwise hot partitions occur.

---

# ⚠️ 7. Missing failure handling

Queue processing must support:

* retry
* dead letter queue

Example:

```
Crawler fails
→ retry 3 times
→ DLQ
```

---

# 🚀 Improvements that make this **senior-level**

## 1. Add Redis caching layer

```
Price History Service
      ↓
   Redis Cache
      ↓
   Price DB
```

---

## 2. Product popularity scoring

Priority queue score:

```
score =
    subscribers * 5
  + page_views * 3
  + recent_price_change * 2
```

---

## 3. Smart crawling frequency

Popular products:

```
every 5 minutes
```

Rare products:

```
every 24 hours
```

---

## 4. Price anomaly detection

Avoid bad data.

Example:

```
if price drops 90%
→ verify via crawler
```

---

## 5. Price compression

Store:

```
timestamp
price
duration
```

Example:

```
100$ from 10:00–14:00
90$ from 14:00–18:00
```

Reduces storage by **100x**.

---

# ⭐ Small improvements

Add:

* search service
* product metadata service
* notification queue

---

# ⭐ Final architecture (ideal)

```
Browser Extension
        │
        ▼
Price Update Service
        │
        ▼
Queue (Kafka)
        │
        ▼
Crawler Scheduler
        │
        ▼
Crawler Workers
        │
        ▼
Price DB (Time Series)
        │
        ▼
CDC → Kafka
        │
        ▼
Notification Service
        │
        ▼
Email / Push
```

Plus:

```
Client
  │
  ▼
API Gateway
  │
  ▼
Price History Service
  │
  ▼
Redis Cache
  │
  ▼
Price DB
```

---

# ⭐ Final verdict (interview)

If you explain like this:

| Category           | Score |
| ------------------ | ----- |
| Architecture       | ⭐⭐⭐⭐  |
| Scalability        | ⭐⭐⭐   |
| Data modeling      | ⭐⭐⭐   |
| Practical thinking | ⭐⭐⭐⭐  |

Overall:

**Strong senior engineer design.**
</details>
