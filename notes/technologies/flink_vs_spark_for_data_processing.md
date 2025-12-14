# What is Apache Flink?

Apache Flink is a distributed stream-processing engine designed to process unbounded data streams in real time.

**What Flink does well:**

- Processes events **as they arrive** (true streaming)
- Handles **event time** natively (watermarks, late events)
- Guarantees **exactly-once processing**
- Maintains **large state** efficiently
- Very **low latency** (milliseconds)

**Typical Flink use cases:**

- Real-time analytics (clickstream, metrics)
- Fraud detection
- Real-time monitoring & alerting
- Continuous ETL pipelines

Mental model: Flink = *“Always running streaming application”*

# What is Apache Spark?

Apache Spark is a general-purpose distributed data processing engine. Originally built for batch processing, later extended for streaming via Spark Streaming / Structured Streaming.

**What Spark does well:**

- Batch analytics on huge datasets
- SQL analytics (Spark SQL)
- Machine learning (MLlib)
- Graph processing
- Streaming using **micro-batches**

**Typical Spark use cases:**

- Daily/hourly ETL jobs
- Offline analytics
- ML training pipelines
- Near-real-time streaming (seconds latency)

**Mental model:** Spark = “Powerful batch engine that can also stream”

# Core Difference(Flink vs Spark)

| Concept | Flink | Spark |
| --- | --- | --- |
| Processing model | True streaming | Micro-batching |
| Latency | Milliseconds | Seconds |
| Event-time support | Native & strong | Good but added later |
| State handling | First-class | Works, but heavier |
| Fault tolerance | Exactly-once by design | Exactly-once with Structured Streaming |
| Batch support | Yes | Yes (best at it) |

# Streaming Model Explained

### Spark Streaming

```
Events → Batch (1 sec) → Process → Output
```

- Collects data for a short window
- Processes them together
- Slight delay is unavoidable

### Flink Streaming

```
Event → Process → Output (immediately)
```

- Each event processed independently
- No batching delay

# Which one to use when?

### Choose **Flink** when:

- Real-time processing is critical
- Low latency (< 1s) matters
- Heavy stateful streaming
- Event-time correctness is important
- Streaming-first architecture
- **Example:** Real-time ad click analytics, fraud detection, alerting systems

### Choose **Spark** when:

- Batch processing is primary
- Streaming is secondary
- You need SQL, ML, batch + stream together
- Latency of few seconds is acceptable
- Team already knows Spark
- **Example:** Daily reports, offline analytics, ML pipelines

# Reference:

1. **What is Apache Flink:** https://youtu.be/PVoc5tRr6to
2. **Apache Spark:** https://youtu.be/IELMSD2kdmk
3. **Apache Spark Vs Flink**
    1. https://www.macrometa.com/event-stream-processing/spark-vs-flink
    2. https://www.datacamp.com/blog/flink-vs-spark
    3. https://www.redpanda.com/guides/event-stream-processing-flink-vs-spark
    4. https://www.decodable.co/blog/comparing-apache-flink-and-spark-for-modern-stream-data-processing

# Kafka → Flink/Spark → OLAP architecture

## High-level Flow

```json
Producers → Kafka → Stream Processor → OLAP → Analytics / Dashboard
                     (Flink / Spark)
```

What happening?

**Events are produced → streamed → enriched & aggregated → stored in OLAP → queried by UI**

## Kafka (Ingestion Layer)

**Role**

- Acts as **durable event log**
- Decouples producers and consumers
- Handles **high throughput**

**What Kafka stores**

- Raw events (JSON / Avro / Protobuf)

```json
{
  "event_id": "e123",
  "user_id": "u42",
  "ad_id": "ad9",
  "event_type": "click",
  "event_time": 1734501223000
}
```

**Why Kafka**

- Replayability
- Backpressure handling
- Multiple consumers (Flink + Spark + others)

## Stream Processing Layer (Flink or Spark)

Responsibilities:

- Deserialize events
- Validate & enrich data
- Windowed aggregations
- Stateful processing
- Exactly-once semantics
- Write to OLAP

## **Kafka → Flink → OLAP** (True Streaming)

Architecture

```
Kafka Topics
   ↓
Flink Job (always running)
   ↓
OLAP (Druid / ClickHouse / Pinot)
```

Flink processing logic

- Event-time windows
- Stateful aggregations
- Low latency updates

Example (pseudo logic)

```
keyBy(ad_id)
→ window(1 minute)
→ count(clicks)
→ write to OLAP
```

Characteristics

| Aspect | Flink |
| --- | --- |
| Latency | Milliseconds |
| Processing | Event-by-event |
| State | Native & efficient |
| Late events | Strong support |
| Use case | Real-time dashboards |

## **Kafka → Spark → OLAP** (Micro-Batch Streaming)

Architecture

```
Kafka Topics
   ↓
Spark Structured Streaming
   ↓
OLAP
```

Spark processing logic

- Read data in micro-batches (e.g., every 5 sec)
- Process batch
- Write aggregated results

Example

```
Every 5 seconds:
→ read Kafka
→ aggregate
→ write to OLAP
```

Characteristics

| Aspect | Spark |
| --- | --- |
| Latency | Seconds |
| Processing | Micro-batch |
| State | Heavier |
| Batch + Stream | Strong |
| Use case | Near-real-time analytics |

## OLAP Layer (Analytics Storage)

Common OLAP stores

| OLAP DB | Strength |
| --- | --- |
| Apache Druid | Real-time analytics |
| Apache Pinot | Low-latency queries |
| ClickHouse | Very fast OLAP |
| BigQuery / Redshift | Managed cloud |

Data stored

- Pre-aggregated metrics
- Time-series data

```
ad_id | window_start | click_count
----------------------------------
ad9   | 10:00        | 1200
```

Why OLAP

- Fast aggregations
- Columnar storage
- Sub-second queries

## Query Layer

```
Dashboard / API
   ↓
OLAP Query
   ↓
Aggregated Results
```

Examples:

- “Clicks per ad in last 5 minutes”
- “Top campaigns today”
- “Revenue per hour”

## Flink vs Spark in this Architecture (Side-by-Side)

| Requirement | Choose |
| --- | --- |
| Real-time dashboard | **Flink** |
| Fraud detection | **Flink** |
| Complex event-time logic | **Flink** |
| Batch + stream together | Spark |
| ML + SQL analytics | Spark |
| Seconds-level delay acceptable | Spark |

## Fault Tolerance (Important HLD Point)

Kafka

- Replication
- Message replay

Flink

- Checkpoints state
- Restores exactly-once

Spark

- Checkpoints + WAL
- Reprocess micro-batches

## Scaling Strategy

Kafka

- Increase partitions

Flink

- Increase parallelism
- Key-based scaling

Spark

- Increase executors
- Tune batch size