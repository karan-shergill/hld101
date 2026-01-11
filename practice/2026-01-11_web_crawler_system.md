# Web Crawler System

# My Approach
![web_crawler_system](images/web_crawler_system.png)

Systems:

1. Queue for Seed URLs: Seed URLs are the base URL + URLs that needs to be get from the webpage and parse them for the content and other hyperlinked webpages.
2. Web Crawler Service: Get Seed URL, against that URL, call DNS gets IP address and save these in cache, with IP makes request to the server and get robots.txt file, save that as well in the cache.
3. Content Getter Service: Get the webpage for all the text and images, push the text into a blob storage and in case unable to get the webpage, put it in queue again
4. Content Parser Service: Parse the text and save the data, that data can be used for training LLM, indexing or research.
5. DB: url_table - this is to keep the URL and metadata related to it like its status, its hash, hash useful to prohibit from getting and parsing a duplicate page

Workflow:

1. Seed URL are provided and are added in the Queue
    1. These seed URL will be pulled by Web Crawler Service
2. Web Crawler Service
    1. calls DNS service and get IP and save in redis cache
    2. calls the server of page url using IP and get robots.txt and save in redis cache
    3. pushed the Url in another queue
3. Content Getter Service
    1. pull the URL from the queue
    2. makes request to host server and get page content
    3. saves that content in a check s3 kind storage
    4. save the details regarding the URL and content in a DB
    5. push the URL/id in the queue
    6. in case we are unable to get the content we will put back the URL back in Seed URL queue with some interval time gap
    7. We can keep track of retry count in cache, in case we retry more than 5 times and got no success in getting the data in can put these URL in DLQ
4. Content Parser Service
    1. get Url/id from the queue
    2. get metadata regarding the url from DB like s3 link
    3. form s3 get the actual content, do processing on this add the details in the wear-house
    4. gets the linked new URL from the content and pushed them in Seed URL again with from interval time gap

# ChatGPT HLD Review

Great job overall 👍
This is a **solid, interview-ready HLD** for a Web Crawler. You’ve covered most of the **core components, async flow, retries, deduplication, and scalability concerns**. I’ll structure the feedback like an interviewer would:

---

## Overall Verdict (Interview Perspective)

**Rating: 7.5 / 10**

**What you did well**

* Clear separation of concerns (Crawler vs Getter vs Parser)
* Async, queue-based architecture (scales well)
* Robots.txt + DNS caching (very important and often missed)
* Retry + DLQ handling
* Deduplication via hash
* Thought about downstream usage (LLM training, indexing)

**What’s missing / needs strengthening**

* Politeness & rate limiting (VERY important for crawlers)
* URL prioritization strategy
* Duplicate URL normalization (beyond content hash)
* Crawl frontier management
* Backpressure & fairness across domains
* Clear ownership of “what service decides what”
* Storage & DB scaling details
* Failure modes and consistency guarantees

Let’s go step by step.

---

## 1. Architecture Feedback (High Level)

Your architecture is **correct**, but you can improve clarity by grouping responsibilities:

### Good

* Queues decouple crawling stages ✅
* Redis for cache is a good choice ✅
* S3-like blob storage for raw content is correct ✅
* Parser is downstream and async (perfect) ✅

### Improvement

Right now, **Web Crawler Service is doing too much**:

* DNS
* robots.txt
* URL validation
* Queue pushing

💡 **Suggestion**
Introduce a **Crawler Coordinator / Frontier Manager** role (logical, not necessarily a new service).

---

## 2. Crawl Frontier (BIG Missing Concept)

Interviewers expect this term.

### What is missing?

Right now:

* URLs are blindly pushed back into Seed Queue
* No prioritization
* No domain-level control

### Improvement

Introduce a **Crawl Frontier** abstraction:

**Crawl Frontier responsibilities**

* URL prioritization (BFS, DFS, freshness-based)
* Domain-level politeness
* URL de-duplication (before crawling)
* Retry scheduling

**Implementation**

* Priority Queue (Kafka + partitions, or Redis ZSET)
* Key = normalized URL
* Value = priority + next_fetch_time

💡 This immediately upgrades your design maturity.

---

## 3. Politeness, Rate Limiting & Fairness (Critical)

### What you have

* robots.txt caching ✅

### What’s missing

* Crawl delay per domain
* Avoid hammering one host
* Concurrent request limits per domain/IP

### How to fix

Add **Domain-based rate limiting**:

* Redis key: `domain → last_access_time`
* Enforce `crawl-delay` from robots.txt
* Limit concurrent fetches per domain

**Example**

```
example.com:
  max_concurrent = 2
  crawl_delay = 5s
```

Interviewer will **definitely ask**:

> “How do you make sure you don’t DDoS a website?”

---

## 4. URL De-duplication (More Than Hash)

### What you have

* Content hash to avoid duplicate pages ✅

### Missing layers

You need **three levels of deduplication**:

1. **URL normalization**

    * Remove fragments (#)
    * Sort query params
    * Canonicalize http vs https
2. **URL-level dedupe**

    * Don’t crawl same URL twice
3. **Content-level dedupe**

    * Hash to detect same content on different URLs

💡 Add:

* `normalized_url` column
* Unique index on it

---

## 5. Queue Design Improvements

### Current queues

* Seed URL queue
* Content fetch queue
* Parse queue
* DLQ

### Improvements

1. **Separate queues per stage (good)**
2. **Add delay queues**

    * Retry after backoff
3. **Partition queues by domain hash**

    * Enables fairness
    * Prevents hot partitions

💡 Mention Kafka / SQS FIFO + delay queues in interview.

---

## 6. Retry & Failure Handling (Good but Can Be Sharper)

### What you did well

* Retry count in cache
* DLQ after 5 retries ✅

### Improvements

* Classify failures:

    * 4xx → no retry
    * 5xx → retry
    * Timeout → retry
* Exponential backoff instead of fixed delay
* Store failure reason in DB

This shows production maturity.

---

## 7. Storage & DB Design Feedback

### URL Table (Good start)

You have:

* id
* url
* status
* last_crawled_at
* s3_content_link
* content_hash

### Improvements

Add:

* `normalized_url`
* `domain`
* `http_status`
* `crawl_priority`
* `retry_count`
* `content_type`

### Scaling DB

Mention:

* Shard by domain hash or URL hash
* Read replicas for parser
* Write-heavy workload optimization

---

## 8. Content Parsing Pipeline (Strong)

This part is well designed 👍

### Good

* Parser is async
* Reads from S3
* Pushes extracted URLs back to crawl queue
* Supports multiple downstream use cases (LLM, search)

### Improvement

* Version raw content
* Keep parser idempotent
* Separate parsing from feature extraction (future ML use)

---

## 9. Observability & Metrics (Often Missed)

Add these explicitly:

### Metrics

* Pages crawled/sec
* Fetch success vs failure
* Avg fetch latency per domain
* Queue lag
* Robots.txt cache hit rate

### Alerts

* Sudden spike in failures
* Domain blocked errors
* Queue backlog growth

Interviewers love this.

---

## 10. Security & Compliance (Optional but Bonus)

Mention briefly:

* Respect robots.txt and noindex
* User-agent identification
* Blacklist sensitive domains
* GDPR / PII considerations

---

## How I’d Summarize This in an Interview

> “At a high level, the system is a distributed, polite, scalable web crawler built around a crawl frontier. URLs move asynchronously through crawling, fetching, and parsing stages using queues. We ensure politeness via robots.txt, domain-based rate limiting, and retries with backoff. Deduplication happens at both URL and content levels, and the pipeline supports downstream indexing and ML training.”

---

## Final Improvement Checklist (Quick)

If you refine these, your design jumps to **9/10**:

* [ ] Introduce **Crawl Frontier**
* [ ] Add **domain-based rate limiting**
* [ ] URL normalization & multi-level dedupe
* [ ] Prioritization strategy
* [ ] Better retry classification
* [ ] Explicit observability section

---



