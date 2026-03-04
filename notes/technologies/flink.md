# Flink

- [What is Flink?](#what-is-flink)
- [Why Do We Even Need Flink?](#why-do-we-even-need-flink)
- [Core Concept: Stream Processing](#core-concept-stream-processing)
- [Where Flink Is Used](#where-flink-is-used)
- [When to Use Flink](#when-to-use-flink)
- [How Flink Works (Architecture)](#how-flink-works-architecture)
    - [Step 1: Data Source](#step-1-data-source)
    - [Step 2: Stream Processing](#step-2-stream-processing)
    - [Step 3: Sink](#step-3-sink)
- [Important Concepts in Flink](#important-concepts-in-flink)
    - [1. Event Time vs Processing Time](#1-event-time-vs-processing-time)
    - [2. Windowing](#2-windowing)
    - [3. State Management](#3-state-management)
    - [4. Checkpointing (Fault Tolerance)](#4-checkpointing-fault-tolerance)
- [How Flink Scales](#how-flink-scales)
- [Example System](#example-system)
    - [Top K Hashtags](#top-k-hashtags)
    - [Flow:](#flow)
    - [Ad Click Aggregator](#ad-click-aggregator)
    - [Fraud Detection](#fraud-detection)
- [How Flink Differs From Spark Streaming](#how-flink-differs-from-spark-streaming)
- [How Flink Handles Top K Internally](#how-flink-handles-top-k-internally)
- [Mental Model](#mental-model)
- [When Interviewer Expects You to Use Flink](#when-interviewer-expects-you-to-use-flink)
- [Where Flink Fits in Your System Design Stack](#where-flink-fits-in-your-system-design-stack)
- [Simple Diagram](#simple-diagram)
- [Final Summary](#final-summary)
- [Java Code Examples](#java-code-examples)
    - [Top K Hashtags (Last 1 Minute)](#top-k-hashtags-last-1-minute)
    - [Ad Click Aggregator](#ad-click-aggregator)
    - [Fraud Detection](#fraud-detection)

# What is Flink?

Flink is a distributed stream processing framework.

It is used to process **continuous streams of data in real time**, with:

- Low latency
- High throughput
- Fault tolerance
- Exactly-once guarantees

Think of Flink as:

> “A system that continuously processes events as they arrive, at massive scale.”
>

# Why Do We Even Need Flink?

Imagine you're building: **Ad Click Aggregator**

Millions of ad clicks per second.

You need:

1. Click count per ad
2. Click count per region
3. Revenue per campaign
4. Top K ads every minute

If you store clicks in DB and run batch query every 10 minutes:

1. ❌ Too slow
2. ❌ Not real-time
3. ❌ Expensive

Instead:

1. ✔ Process clicks as they arrive
2. ✔ Maintain running counters
3. ✔ Emit results every few seconds

That’s what Flink does.

# Core Concept: Stream Processing

Traditional systems:

1. Collect data
2. Store in DB
3. Run batch jobs

Flink: Processes data **as it flows**

Example event stream:

```json
User1 clicked AdA at 10:01
User2 clicked AdB at 10:01
User3 clicked AdA at 10:02
```

Flink continuously updates:

```json
AdA → 2 clicks
AdB → 1 click
```

# Where Flink Is Used

1. Real-Time Analytics
    1. Ad click aggregation
    2. Fraud detection
    3. Stock market processing
    4. Gaming leaderboards
2. Top K Systems
    1. Top 10 trending hashtags in last 5 minutes
3. Monitoring Systems
    1. CPU logs, app logs, error tracking
4. Financial Systems
    1. Transaction risk scoring
    2. Real-time payment validation

# When to Use Flink

**✅ Use Flink When:**

1. You need **real-time results**
2. You process **continuous event streams**
3. You need **windowing**
4. You need **exactly-once consistency**
5. You process millions of events per second

Example:

- Top 100 products in last 10 minutes
- Fraud detection within 2 seconds

**❌ Don’t Use Flink When:**

1. Data is small
2. Batch job once per day is enough
3. Simple SQL query works
4. No need for low latency

Example:  Daily sales report → use batch system (like Spark batch or SQL)

# How Flink Works (Architecture)

### Step 1: Data Source

Flink consumes from:

- Kafka
- Kinesis
- Files
- Databases

Example: Ad clicks come from Kafka.

### Step 2: Stream Processing

Inside Flink:

```json
Source → Transform → Window → Aggregate → Sink
```

Example:

```json
clickStream
   .keyBy(adId)
   .window(Tumbling 1 minute)
   .sum(clickCount)
```

### Step 3: Sink

Outputs go to:

- Database
- Kafka
- Redis
- Dashboard

# Important Concepts in Flink

### 1. Event Time vs Processing Time

This is VERY important in interviews - Imagine click happened at 10:00. But reached system at 10:03

Which time should we use?

Flink supports:

- Processing time → when system receives event
- Event time → when event actually happened

For accurate analytics → use **Event Time**

### 2. Windowing

Streams are infinite. So how do we calculate Top K?

We break the stream into windows:

Types:

- Tumbling window (fixed size)
- Sliding window
- Session window

Example: “Top 10 ads in last 5 minutes” → That’s a sliding window.

### 3. State Management

This is why Flink is powerful. It stores state like:

```json
AdA → 10,000 clicks
AdB → 8,000 clicks
```

State is:

- Distributed
- Fault-tolerant
- Persistent

### 4. Checkpointing (Fault Tolerance)

If a machine crashes, Flink:

- Periodically saves state (checkpoint)
- Restores from last checkpoint
- Guarantees exactly-once processing

# How Flink Scales

Very important for system design. Flink runs on a cluster.

Architecture:

- Job Manager
- Task Managers

**Parallelism** → If you set:

```json
parallelism = 10
```

Then stream is split into 10 partitions. Each machine processes part of stream.

If traffic increases:

- Add more TaskManagers
- Increase parallelism

Flink redistributes partitions.

# Example System

### Top K Hashtags

Requirement: “Top 5 hashtags in last 1 minute”

### Flow:

Users post tweets → Kafka → Flink

Flink:

- keyBy(hashtag)
- 1 minute sliding window
- Count
- Sort
- Emit top 5

Without Flink: Very expensive queries.

### Ad Click Aggregator

Requirement:

- Total clicks per campaign
- Revenue per region
- Fraud detection

Flink:

- Groups by campaignId
- Window 10 seconds
- Aggregate revenue
- Emit to dashboard

This is a classic interview problem.

### Fraud Detection

Requirement: If user makes 5 transactions in 10 seconds → flag fraud

Flink:

- keyBy(userId)
- Sliding window 10 sec
- Count transactions
- If > 5 → alert

Low-latency detection.

# How Flink Differs From Spark Streaming

| Feature | Flink | Spark Streaming |
| --- | --- | --- |
| True streaming | ✅ | Micro-batch |
| Latency | Very low | Higher |
| Exactly-once | Strong | Good |
| Event-time handling | Excellent | Good |

Flink is more “real-time native”.

# How Flink Handles Top K Internally

Let’s break Top K:

1. keyBy item
2. Window
3. Maintain count state
4. Use heap to maintain Top K
5. Emit periodically

So internally it maintains:

- Map state
- Priority queue

Very interview-relevant.

# Mental Model

Think:

```json
Kafka = Data pipe
Flink = Real-time brain
Database = Storage
```

If the question says: “Process real-time events” → Think Flink

If the question says: “Daily report” → Think batch system

# When Interviewer Expects You to Use Flink

Use Flink when:

- Event-driven architecture
- Continuous analytics
- Top K real-time
- Sliding window logic
- Fraud detection
- Real-time counters

# Where Flink Fits in Your System Design Stack

For example, in your Instagram Feed HLD:If you want:

- Real-time trending posts
- Real-time notification system
- Real-time engagement metrics

Flink fits perfectly between: Kafka → Flink → Redis → API

# Simple Diagram

```json
Users → Kafka → Flink → Redis → Backend API → Client
```

# Final Summary

Flink is:

1. Distributed
2. Stateful
3. Stream-first
4. Fault-tolerant
5. Real-time

Use it for:

1. Top K
2. Ad click aggregation
3. Fraud detection
4. Monitoring
5. Realtime analytics

Don’t use it for:

1. Small data
2. Daily batch jobs
3. Simple CRUD apps

# Java Code Examples

## Top K Hashtags (Last 1 Minute)

**Requirement** - Every minute, emit Top 5 hashtags based on count.

How We’ll Implement:

- Read hashtag events
- keyBy(hashtag)
- Tumbling window (1 min)
- Count per hashtag
- Re-group by window
- Maintain Top K using PriorityQueue
- Emit results

Java Code

```java
// POJO representing hashtag event
public class HashtagEvent {
    public String hashtag;
    public long timestamp;

    public HashtagEvent() {}

    public HashtagEvent(String hashtag, long timestamp) {
        this.hashtag = hashtag;
        this.timestamp = timestamp;
    }
}

/************************************************************************/

// Aggregate function to count hashtags
public class CountAgg implements AggregateFunction<HashtagEvent, Long, Long> {

    // Initialize accumulator
    @Override
    public Long createAccumulator() {
        return 0L;
    }

    // Increment count
    @Override
    public Long add(HashtagEvent value, Long accumulator) {
        return accumulator + 1;
    }

    // Return final result
    @Override
    public Long getResult(Long accumulator) {
        return accumulator;
    }

    @Override
    public Long merge(Long a, Long b) {
        return a + b;
    }
}

/************************************************************************/

public class TopKProcessFunction
        extends ProcessWindowFunction<Long, String, String, TimeWindow> {

    private final int topSize;

    public TopKProcessFunction(int topSize) {
        this.topSize = topSize;
    }

    @Override
    public void process(
            String hashtag,
            Context context,
            Iterable<Long> elements,
            Collector<String> out) {

        long count = elements.iterator().next();

        // Maintain a min-heap for Top K
        PriorityQueue<Tuple> minHeap =
                new PriorityQueue<>(Comparator.comparingLong(t -> t.count));

        minHeap.add(new Tuple(hashtag, count));

        if (minHeap.size() > topSize) {
            minHeap.poll();
        }

        out.collect("Window: " + context.window().getEnd()
                + " Hashtag: " + hashtag
                + " Count: " + count);
    }

    // Simple helper class
    static class Tuple {
        String hashtag;
        long count;

        Tuple(String hashtag, long count) {
            this.hashtag = hashtag;
            this.count = count;
        }
    }
}
```

## Ad Click Aggregator

Requirement - Count clicks per Ad per 10 seconds and compute revenue.

What This Does - Every 10 seconds:

- Groups by adId
- Aggregates revenue
- Emits total revenue per ad

Java Code

```java
// POJO
public class AdClickEvent {
    public String adId;
    public double cost;
    public long timestamp;

    public AdClickEvent() {}

    public AdClickEvent(String adId, double cost, long timestamp) {
        this.adId = adId;
        this.cost = cost;
        this.timestamp = timestamp;
    }
}

/************************************************************************/

// Aggregation Logic
// Reduce function to sum clicks and revenue
public class AdClickReduceFunction implements ReduceFunction<AdClickEvent> {

    @Override
    public AdClickEvent reduce(AdClickEvent value1, AdClickEvent value2) {

        // Combine revenue
        return new AdClickEvent(
                value1.adId,
                value1.cost + value2.cost, // total revenue
                value2.timestamp
        );
    }
}

/************************************************************************/

// Stream Pipeline
DataStream<AdClickEvent> stream = env.addSource(...);
// Key by Ad ID
stream
    .keyBy(event -> event.adId)
    // 10-second tumbling window
    .window(TumblingEventTimeWindows.of(Time.seconds(10)))
    // Aggregate revenue
    .reduce(new AdClickReduceFunction())
    .print();
```

## Fraud Detection

Requirement - If user makes 5 transactions in 10 seconds → alert

Java Code

```java
// POJO
public class Transaction {
    public String userId;
    public double amount;
    public long timestamp;

    public Transaction() {}

    public Transaction(String userId, double amount, long timestamp) {
        this.userId = userId;
        this.amount = amount;
        this.timestamp = timestamp;
    }
}

/************************************************************************/

// Fraud Detection Logic Using ProcessFunction
public class FraudDetectionFunction
        extends KeyedProcessFunction<String, Transaction, String> {

    // State to maintain transaction count per user
    private transient ValueState<Integer> transactionCount;

    @Override
    public void open(Configuration parameters) {

        // Initialize state descriptor
        ValueStateDescriptor<Integer> descriptor =
                new ValueStateDescriptor<>("txnCount", Integer.class);

        transactionCount = getRuntimeContext().getState(descriptor);
    }

    @Override
    public void processElement(
            Transaction transaction,
            Context context,
            Collector<String> out) throws Exception {

        Integer count = transactionCount.value();

        if (count == null) {
            count = 0;
        }

        count++;

        transactionCount.update(count);

        // If more than 5 transactions → fraud alert
        if (count >= 5) {
            out.collect("FRAUD ALERT for user: " + transaction.userId);
        }

        // Reset state after 10 seconds
        context.timerService()
                .registerProcessingTimeTimer(
                        context.timerService().currentProcessingTime() + 10000);
    }

    @Override
    public void onTimer(
            long timestamp,
            OnTimerContext ctx,
            Collector<String> out) throws Exception {

        // Clear count after window
        transactionCount.clear();
    }
}

/************************************************************************/

// Pipeline
stream
    .keyBy(txn -> txn.userId)
    .process(new FraudDetectionFunction())
    .print();
```