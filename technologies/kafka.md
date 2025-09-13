# Kafka

- [Reference](#reference)
- [What is Kafka](#what-is-kafka)
- [Kafka Terms](#kafka-terms)
    - [1. Event](#1-event)
    - [2. Producer](#2-producer)
    - [3. Topic](#3-topic)
    - [4. Partition](#4-partition)
    - [5. Broker](#5-broker)
    - [6. Replication](#6-replication)
    - [7. Consumer](#7-consumer)
    - [8. Consumer Group](#8-consumer-group)
    - [9. Offset](#9-offset)
    - [10. Retention Policy](#10-retention-policy)
    - [11. Cluster](#11-cluster)
    - [12. ZooKeeper (Legacy, being replaced by KRaft)](#12-zookeeper-legacy-being-replaced-by-kraft)
- [Kafka as message queue or a stream](#kafka-as-message-queue-or-a-stream)
- [How Kafka Works](#how-kafka-works)
- [When to use Kafka in your interview](#when-to-use-kafka-in-your-interview)
- [What you should know about Kafka for System Design Interviews](#what-you-should-know-about-kafka-for-system-design-interviews)
    - [Scalability](#scalability)
    - [Fault Tolerance and Durability](#fault-tolerance-and-durability)
    - [Handling Retries and Errors](#handling-retries-and-errors)
    - [Performance Optimizations](#performance-optimizations)
    - [Retention Policies](#retention-policies)
- [Kafka Use Cases](#kafka-use-cases)

## Reference

1. Article - [HelloInterview - Key Technologies - Kafka](https://www.hellointerview.com/learn/system-design/deep-dives/kafka)
2. Youtube - [Kafka Deep Dive w/ a Ex-Meta Staff Engineer](https://www.youtube.com/watch?v=DU8o-OTeoCc&pp=ygUOa2Fma2EgdHV0b3JpYWzYBtkF)
3. Youtube - [Kafka Tutorial for Beginners | Everything you need to get started](https://www.youtube.com/watch?v=QkdkLdMBuL0&pp=ygUOa2Fma2EgdHV0b3JpYWw%3D)
4. Youtube - [Apache Kafka Crash Course | What is Kafka?](https://www.youtube.com/watch?v=ZJJHm_bd9Zo&pp=ygUOa2Fma2EgdHV0b3JpYWw%3D)
5. Youtube - [Apache Kafka: a Distributed Messaging System for Log Processing](https://www.youtube.com/watch?v=hNDjd9I_VGA&pp=ygUOa2Fma2EgdHV0b3JpYWzSBwkJyQkBhyohjO8%3D)
6. Youtube - [Introduction to Kafka | Stream Processing System | Simplified](https://youtu.be/ynRI8R_tYkY)
7. Youtube - [Apache Kafka Tutorials](https://www.youtube.com/playlist?list=PLiMWaCMwGJXlL8-E-xu8RBwyC5YfS3V5e)
8. Kafka Terms
    1. [Apache Kafka 101: Topics (2023)](https://youtu.be/kj9JH3ZdsBQ?list=PLa7VYi0yPIH0KbnJQcMv5N9iW8HkZHztH)
    2. [Apache Kafka 101: Partitioning (2023)](https://youtu.be/y9BStKvVzSs?list=PLa7VYi0yPIH0KbnJQcMv5N9iW8HkZHztH)
    3. [Apache Kafka 101: Brokers (2023)](https://youtu.be/jHnyBSUVcOU?list=PLa7VYi0yPIH0KbnJQcMv5N9iW8HkZHztH)
    4. [Apache Kafka 101: Replication (2023)](https://youtu.be/Vo6Mv5YPOJU?list=PLa7VYi0yPIH0KbnJQcMv5N9iW8HkZHztH)
    5. [Apache Kafka 101: Producers (2023)](https://youtu.be/I7zm3on_cQQ?list=PLa7VYi0yPIH0KbnJQcMv5N9iW8HkZHztH)
    6. [Apache Kafka 101: Consumers (2023)](https://youtu.be/Z9g4jMQwog0?list=PLa7VYi0yPIH0KbnJQcMv5N9iW8HkZHztH)

## What is Kafka

Apache Kafka is an open-source distributed event streaming platform that can be used either as a message queue or as a stream processing system. Kafka excels in delivering high performance, scalability, and durability. It’s engineered to handle vast volumes of data in real-time, ensuring that no message is ever lost and that each piece of data is processed as swiftly as possible.

## Kafka Terms

### 1. Event

The basic unit of data in Kafka.

- An **event** is like a message or record that describes “something that happened.”
- In Kafka, each event has:
    - **Key** (optional, used to decide partition placement),
    - **Value** (the actual data),
    - **Timestamp** (when it happened),
    - **Metadata** (like offset, headers).

**Example**:

- Event key: `user123`
- Event value: `{ "action": "purchase", "item": "Laptop", "price": 70000 }`

  This means *User123 purchased a Laptop for ₹70,000*.


### 2. Producer

Producers are **applications that create and send events** to Kafka.

- They decide **which topic** to send the event to.
- Optionally, they can also decide the **partition** (based on event key).

**Example**:

- Flipkart’s checkout service produces an event like *“User123 purchased Laptop”* and sends it to a Kafka topic called `orders`.

### 3. Topic

A topic is just a logical grouping of partitions. Topics are the way you publish and subscribe to data in Kafka. When you publish a message, you publish it to a topic, and when you consume a message, you consume it from a topic. Topics are always multi-producer; that is, a topic can have zero, one, or many producers that write data to it.

A **logical channel** in Kafka where events are stored.

- Think of a topic as a folder that holds related events.
- Topics are **append-only logs** (new events are written at the end).

**Example**:

- `orders` → holds all order events.
- `payments` → holds all payment events.
- `shipments` → holds all shipping updates.

So, multiple applications can read/write from the same topic.

### 4. Partition

Each broker has a number of partitions. Each partition is an ordered, immutable sequence of messages that is continually appended to -- think of like a log file. Partitions are the way Kafka scales as they allow for messages to be consumed in parallel.

Each topic is split into **partitions** for scalability and parallelism.

- Each partition is an **ordered, immutable sequence of events**.
- Events inside a partition are always stored in the order they arrive.
- Kafka spreads partitions across brokers for **load balancing**.

**Example**:

- Topic `orders` has 3 partitions:
    - Partition 0 → Orders of some users
    - Partition 1 → Orders of other users
    - Partition 2 → More orders

This means **Kafka can handle higher throughput** because multiple consumers can read in parallel.

*So what is the difference between a topic and a partition? - A topic is a logical grouping of messages. A partition is a physical grouping of messages. A topic can have multiple partitions, and each partition can be on a different broker. Topics are just a way to organize your data, while partitions are a way to scale your data.*

### 5. Broker

A Kafka cluster is made up of multiple brokers. These are just individual servers (they can be physical or virtual). Each broker is responsible for storing data and serving clients. The more brokers you have, the more data you can store and the more clients you can serve.

A **Kafka server** that stores partitions of topics.

- Kafka is usually deployed as a **cluster of brokers**.
- Each broker is identified by an ID and manages part of the data.

**Example**:

- A Kafka cluster with 3 brokers:
    - Broker 1 → Partitions 0 of `orders`, 1 of `payments`
    - Broker 2 → Partitions 1 of `orders`, 2 of `payments`
    - Broker 3 → Partitions 2 of `orders`, 0 of `payments`

### 6. Replication

Kafka **duplicates partitions** across multiple brokers for fault tolerance.

- One broker is the **Leader** for a partition.
- Other brokers hold **Replicas** (followers).
- If the leader fails, a follower is promoted to leader.

**Example**:

- `orders` partition 0 → Leader on Broker 1, Replica on Broker 2.
- If Broker 1 crashes, Broker 2 takes over, ensuring **no data loss**.

### 7. Consumer

Consumers are **applications that read events** from Kafka topics.

- They can read in **real-time** (stream processing) or **batch** (later).

**Example**:

- The Flipkart analytics service consumes from the `orders` topic to calculate *“Top 10 most purchased items today.”*
- The shipping service consumes from the `orders` topic to trigger *“Create shipment”* after an order is placed.

### 8. Consumer Group

A **group of consumers** that work together to read from a topic.

- Kafka assigns **partitions to consumers** within a group.
- Ensures **each partition is consumed by only one consumer in the group**.
- If a consumer crashes, another consumer in the group takes over.

**Example**:

- Topic `orders` has 3 partitions.
- Consumer Group `order-processing-group` has 3 consumers:
    - Consumer 1 → Partition 0
    - Consumer 2 → Partition 1
    - Consumer 3 → Partition 2

This way, processing is balanced and no event is missed.

### 9. Offset

The **position (ID) of an event inside a partition**.

- Kafka keeps events in partitions like a numbered log file.
- Consumers keep track of which offset they last read.

**Example**:

- Partition 0 of `orders`:
    - Offset 0 → `user123 purchased laptop`
    - Offset 1 → `user456 purchased phone`
- If a consumer has read till offset 1, the next read will be offset 2.

### 10. Retention Policy

Defines **how long Kafka keeps events** in a topic.

- Two main types:
    1. **Time-based** → Keep data for X days (e.g., 7 days).
    2. **Size-based** → Keep only last X GB of data.

**Example**:

- Retention = 7 days → Events older than 7 days are deleted.
- This ensures Kafka doesn’t run out of storage, but consumers must process events before expiry.

### 11. Cluster

- A **Kafka cluster** is a collection of Kafka brokers working together.
- Topics are distributed across brokers in a cluster.

**Example:** A production Kafka setup might have **5 brokers** forming a single cluster that handles millions of events per second.

### 12. ZooKeeper (Legacy, being replaced by KRaft)

- Kafka originally used **Apache ZooKeeper** to manage metadata like broker info, topics, partitions, leaders, etc.
- Newer Kafka versions (post 2.8) support **KRaft mode** (Kafka Raft) to remove the ZooKeeper dependency.

**Example:**

- ZooKeeper keeps track of which broker is the leader for `orders` partition 0.
- In modern Kafka (KRaft), this coordination is done internally without ZooKeeper.

![kafka_terms_relationship](/technologies/images/kafka_terms_relationship.png)

## Kafka as message queue or a stream

Importantly, you can use Kafka as either a message queue or a stream. Frankly, the distinction here is minor. The only meaningful difference is with how consumers interact with the data. In a message queue, consumers read messages from the queue and then acknowledge that they have processed the message. In a stream, consumers read messages from the stream and then process them, but they don't acknowledge that they have processed the message. This allows for more complex processing of the data.

| Feature | Queue (Work Queue) | Stream (Pub-Sub) |
| --- | --- | --- |
| **Consumer delivery** | Each message → delivered to **only one consumer** in a group | Each consumer group gets **all messages** |
| **Data ownership** | Message considered “consumed” after one read | Messages remain for retention window |
| **Scaling** | Horizontal scaling (distributes load across consumers in a group) | Multiple groups can process the same data independently |
| **Replay** | No replay (in classic queues) | Kafka supports replay by resetting offsets |
| **Best for** | Task distribution (work queues, job processing) | Event-driven apps, streaming analytics, multiple subscribers |

**Analogy**:

- **Queue**: A line at a ticket counter. Each ticket goes to one person, then it’s gone.
- **Stream**: A YouTube livestream. Many people can watch the same event independently, and they can join late or replay from earlier.

## How Kafka Works

When an event occurs, the producer formats a message, also referred to as a record, and sends it to a Kafka topic. A message consists of one required field, the value, and three optional fields: a key, a timestamp, and headers. The key is used to determine which partition the message is sent to, and the timestamp is used to order messages within a partition. Headers, like HTTP headers, are key-value pairs that can be used to store metadata about the message.

*While not strictly required, the key is used to determine which partition the message is sent to. If you don't provide a key, Kafka will randomly assign the message to a partition. So when designing a large, distributed system like you're likely to be asked about in your interview, you'll want to use keys to ensure that messages are processed in order, and the choice of that key is important.*

When a message is published to a Kafka topic, Kafka first determines the appropriate partition for the message. This partition selection is critical because it influences the distribution of data across the cluster.

1. **Partition Determination:** Kafka uses a partitioning algorithm that hashes the message key to assign the message to a specific partition. If the message does not have a key, Kafka can either round-robin the message to partitions or follow another partitioning logic defined in the producer configuration. This ensures that messages with the same key always go to the same partition, preserving order at the partition level.
2. **Broker Assignment:** Once the partition is determined, Kafka then identifies which broker holds that particular partition. The mapping of partitions to specific brokers is managed by the Kafka cluster metadata, which is maintained by the Kafka controller (a role within the broker cluster). The producer uses this metadata to send the message directly to the broker that hosts the target partition.

Each partition in Kafka functions essentially as an append-only log file. Messages are sequentially added to the end of this log, which is why Kafka is commonly described as a distributed commit log. This append-only design is central to Kafka’s architecture, providing several important benefits:

1. **Immutability**: Once written, messages in a partition cannot be altered or deleted. This immutability is crucial for Kafka’s performance and reliability. It simplifies replication, speeds up recovery processes, and avoids consistency issues common in systems where data can be changed.
2. **Efficiency**: By restricting operations to appending data at the end of the log, Kafka minimizes disk seek times, which are a major bottleneck in many storage systems.
3. **Scalability**: The simplicity of the append-only log mechanism facilitates horizontal scaling. More partitions can be added and distributed across a cluster of brokers to handle increasing loads, and each partition can be replicated across multiple brokers to enhance fault tolerance.

Each message in a Kafka partition is assigned a unique offset, which is a sequential identifier indicating the message’s position in the partition. This offset is used by consumers to track their progress in reading messages from the topic. As consumers read messages, they maintain their current offset and periodically commit this offset back to Kafka. This way, they can resume reading from where they left off in case of failure or restart.

Once a message is published to the designated partition, Kafka ensures its durability and availability through a robust replication mechanism. Kafka employs a leader-follower model for replication, which works as follows:

1. **Leader Replica Assignment**: Each partition has a designated leader replica, which resides on a broker. This leader replica is responsible for handling all read and write requests for the partition. The assignment of the leader replica is managed centrally by the cluster controller, which ensures that each partition’s leader replica is effectively distributed across the cluster to balance the load.
2. **Follower Replication**: Alongside the leader replica, several follower replicas exist for each partition, residing on different brokers. These followers do not handle direct client requests; instead, they passively replicate the data from the leader replica. By replicating the messages received by the leader replica, these followers act as backups, ready to take over should the leader replica fail.
3. **Synchronization and Consistency**: Followers continuously sync with the leader replica to ensure they have the latest set of messages appended to the partition log. This synchronization is crucial for maintaining consistency across the cluster. If the leader replica fails, one of the follower replicas that has been fully synced can be quickly promoted to be the new leader, minimizing downtime and data loss.
4. **Controller's Role in Replication**: The controller within the Kafka cluster manages this replication process. It monitors the health of all brokers and manages the leadership and replication dynamics. When a broker fails, the controller reassigns the leader role to one of the in-sync follower replicas to ensure continued availability of the partition.

Last up, consumers read messages from Kafka topics using a pull-based model. Unlike some messaging systems that push data to consumers, Kafka consumers actively poll the broker for new messages at intervals they control. As explained by [Apache Kafka's official documentation](https://kafka.apache.org/documentation.html#design_pull), this pull approach was a deliberate design choice that provides several advantages: it lets consumers control their consumption rate, simplifies failure handling, prevents overwhelming slow consumers, and enables efficient batching.

![kafka_architecture](/technologies/images/kafka_architecture.png)

## When to use Kafka in your interview

Kafka can be used as either a message queue or a stream.

The key difference between the two lies in how consumers interact with the data. In a message queue, consumers typically pull messages from the queue when they are ready to process them. In a stream, consumers continuously consume and process messages as they arrive in real-time, similar to drinking from a flowing river.

Consider adding a message queue to your system when:

- You have processing that can be done asynchronously. YouTube is a good example of this. When users upload a video we can make the standard definition video available immediately and then put the video (via link) a Kafka topic to be transcoded when the system has time.
- You need to ensure that messages are processed in order. We could use Kafka for our virtual waiting queue in [**Design Ticketmaster**](https://www.hellointerview.com/learn/system-design/problem-breakdowns/ticketmaster) which is meant to ensure that users are let into the booking page in the order they arrived.
- You want to decouple the producer and consumer so that they can scale independently. Usually this means that the producer is producing messages faster than the consumer can consume them. This is a common pattern in microservices where you want to ensure that one service can't take down another.

Streams are useful when:

- You require continuous and immediate processing of incoming data, treating it as a real-time flow. See [**Design an Ad Click Aggregator**](https://www.hellointerview.com/learn/system-design/problem-breakdowns/ad-click-aggregator) for an example where we aggregate click data in real-time.
- Messages need to be processed by multiple consumers simultaneously. In [**Design FB Live Comments**](https://www.hellointerview.com/learn/system-design/problem-breakdowns/fb-live-comments) we can use Kafka as a pub/sub system to send comments to multiple consumers.

## What you should know about Kafka for System Design Interviews

### Scalability

Let's start by understanding the constraints of a single Kafka broker. It's important in your interview to estimate the throughput and number of messages you'll be storing in order to determine whether we need to worry about scaling in the first place.

First, there is no hard limit on the size of a Kafka message as this can be configured via message.max.bytes. However, it is recommended to keep messages under 1MB to ensure optimal performance via reduced memory pressure and better network utilization.

*It's a common anti-pattern in system design interviews to store large blobs of data in Kafka. Kafka is not a database, and it's not meant to store large files. It's meant to store small messages that can be processed quickly.*

On good hardware, a single broker can store around 1TB of data and handle as many as 1M messages per second (this is very hand wavy as it depends on message size and hardware specs, but is a useful estimate). If your design does not require more than this, than scaling is likely not a relevant conversation.

In the case that you do need to scale, you have a couple strategies at your disposal:

1. **Horizontal Scaling With More Brokers**: The simplest way to scale Kafka is by adding more brokers to the cluster. This helps distribute the load and offers greater fault tolerance. Each broker can handle a portion of the traffic, increasing the overall capacity of the system. It's really important that when adding brokers you ensure that your topics have sufficient partitions to take advantage of the additional brokers. More partitions allow more parallelism and better load distribution. If you are under partitioned, you won't be able to take advantage of these newly added brokers.
2. **Partitioning Strategy**: This should be the main focus of your scaling strategy in an interview and is the main decision you make when dealing with Kafka clusters (since much of the scaling happens dynamically in managed services nowadays). You need to decide how to partition your data across the brokers. This is done by choosing a key for your messages. The partition is determined by hashing the key using a hash function (murmur2 by default) and taking the modulo with the number of partitions: partition = hash(key) % num_partitions. If you choose a bad key, you can end up with hot partitions that are overwhelmed with traffic. Good keys are ones that are evenly distributed across the partition space.

When working with Kafka, you're usually thinking about scaling topics rather than the entire cluster. This is because different topics can have different requirements. For example, you may have a topic that is very high throughput and needs to be partitioned across many brokers, while another topic is low throughput and can be handled by a single broker. To scale a topic, you can increase the number of partitions, which will allow you to take advantage of more brokers.

**How can we handle hot partitions?**

Interviewers love to ask this question. Consider an [**Ad Click Aggregator**](https://www.hellointerview.com/learn/system-design/problem-breakdowns/ad-click-aggregator) where Kafka stores a stream of click events from when users click on ads. Naturally, you would start by partitioning by ad id. But when Nike launches their new Lebron James ad, you better believe that partition is going to be overwhelmed with traffic and you'll have a hot partition on your hands.

There are a few strategies to handle hot partitions:

1. **Random partitioning with no key**: If you don't provide a key, Kafka will randomly assign a partition to the message, guaranteeing even distribution. The downside is that you lose the ability to guarantee order of messages. If this is not important to your design, then this is a good option.
2. **Random salting**: We can add a random number or timestamp to the ad ID when generating the partition key. This can help in distributing the load more evenly across multiple partitions, though it may complicate aggregation logic later on the consumer side. This is often referred to as "salting" the key.
3. **Use a compound key**: Instead of using just the ad ID, use a combination of ad ID and another attribute, such as geographical region or user ID segments, to form a compound key. This approach helps in distributing traffic more evenly and is particularly useful if you can identify attributes that vary independently of the ad ID.
4. **Back pressure**: Depending on your requirements, one easy solution is to just slow down the producer. If you're using a managed Kafka service, they may have built-in mechanisms to handle this. If you're running your own Kafka cluster, you can implement back pressure by having the producer check the lag on the partition and slow down if it's too high.

### Fault Tolerance and Durability

If you chose Kafka, one reason may have been because of its strong durability guarantees. But how does Kafka ensure that your data is safe and that no messages are lost?

Kafka ensures data durability through its replication mechanism. Each partition is replicated across multiple brokers, with one broker acting as the leader and others as followers. When a producer sends a message, it is written to the leader and then replicated to the followers. This ensures that even if a broker fails, the data remains available. Producer acknowledgments (`acks` setting) play a crucial role here. Setting `acks=all` ensures that the message is acknowledged only when all replicas have received it, guaranteeing maximum durability.

Depending on how much durability you need, you can configure the replication factor of your topics. The replication factor is the number of replicas that are maintained for each partition. A replication factor of 3 is common, meaning that each partition has 2 replicas. So if one broker fails, the data is still available on the other two and we can promote a follower to be the new leader.

**But what happens when a consumer goes down?**

Kafka is usually thought of as always available. You'll often hear people say, "Kafka is always available, sometimes consistent." This means that a question like, "what happens if Kafka goes down?" is not very realistic, and you may even want to gently push back on the interviewer if they ask this.

What is far more relevant and likely is that a consumer goes down. When a consumer fails, Kafka's fault tolerance mechanisms help ensure continuity:

1. **Offset Management**: Remember that partitions are just append-only logs where each message is assigned a unique offset. Consumers commit their offsets to Kafka after they process a message. This is the consumers way of saying, "I've processed this message." When a consumer restarts, it reads its last committed offset from Kafka and resumes processing from there, ensuring no messages are missed or duplicated.
2. **Rebalancing**: When part of a consumer group, if one consumer goes down, Kafka will redistribute the partitions among the remaining consumers so that all partitions are still being processed.

The trade-off you may need to consider in an interview is when to commit offsets. In [**Design a Web Crawler**](https://www.hellointerview.com/learn/system-design/problem-breakdowns/web-crawler), for example, you want to be careful not to commit the offset until you're sure the raw HTML has been stored in your blob storage. The more work a consumer has to do, the more likely you are to have to redo work if the consumer fails. For this reason, keeping the work of the consumer as small as possible is a good strategy -- as was the case in Web Crawler where we broke the crawler into 2 phases: downloading the HTML and then parsing it.

### Handling Retries and Errors

While Kafka itself handles most of the reliability (as we saw above), our system may fail getting messages into and out of Kafka. We need to handle these scenarios gracefully.

**Producer Retries**

First up, we may fail to get a message to Kafka in the first place. Errors can occur due to network issues, broker unavailability, or transient failures. To handle these scenarios gracefully, Kafka producers support automatic retries. Here's a sneak peek of how you can configure them:

```jsx
const producer = kafka.producer({
  retry: {
    retries: 5, // Retry up to 5 times
    initialRetryTime: 100, // Wait 100ms between retries
  },
  idempotent: true,
});
```

**Consumer Retries**

On the consumer side, we may fail to process a message for any number of reasons. Kafka does not actually support retries for consumers out of the box (but [**AWS SQS**](https://aws.amazon.com/sqs/) does!) so we need to implement our own retry logic. One common pattern is to set up a custom topic that we can move failed messages to and then have a separate consumer that processes these messages. This way, we can retry messages as many times as we want without affecting the main consumer. If a given message is retried too many times, we can move it to a dead letter queue (DLQ). DLQs are just a place to store failed messages so that we can investigate them later.

### Performance Optimizations

Especially when using Kafka as an event stream, we need to be mindful of performance so that we can process messages as quickly as possible.

This first thing we can do is batch messages in the producer before sending them to Kafka. Again, this is just a simple configuration.

```jsx
const producer = kafka.producer({
  batch: {
    maxSize: 16384,// Maximum batch size in bytes
    maxTime: 100,// Maximum time to wait before sending a batch
  }
});
```

Another common way to improve throughput is by compressing the messages on the producer. This can be done by setting the compression option in the producer configuration. Kafka supports several compression algorithms, including GZIP, Snappy, and LZ4. Essentially, we're just making the messages smaller so that they can be sent faster.

```jsx
const producer = kafka.producer({
  compression: CompressionTypes.GZIP,
});
```

Arguably the biggest impact you can have to performance comes back to your choice of partition key. The goal is to maximize parallelism by ensuring that messages are evenly distributed across partitions. In your interview, discussing the partition strategy, as we go into above, should just about always be where you start.

### Retention Policies

Kafka topics have a retention policy that determines how long messages are retained in the log. This is configured via the retention.ms and retention.bytes settings. The default retention policy is to keep messages for 7 days.

In your interview, you may be asked to design a system that needs to store messages for a longer period of time. In this case, you can configure the retention policy to keep messages for a longer duration. Just be mindful of the impact on storage costs and performance.

## Kafka Use Cases

**Core System Design Interview Scenarios**

| **Use Case** | **System Example** | **Kafka Pattern** | **Why Kafka?** | **Message Volume** | **Alternatives** |
| --- | --- | --- | --- | --- | --- |
| **Social Media Feed** | Twitter, Instagram | Event Stream | Real-time updates, Multiple consumers, Event replay | Very High (100K+ msg/sec) | Redis Streams, Pulsar |
| **Chat/Messaging** | WhatsApp, Slack | Message Queue | Reliable delivery, Multiple devices, Ordering | High (50K+ msg/sec) | RabbitMQ, SQS |
| **E-commerce Orders** | Amazon, Flipkart | Event Stream | Order pipeline, Multiple services, Audit trail | High (10K+ msg/sec) | SQS, RabbitMQ |
| **Notification System** | Push, Email, SMS | Message Queue | Fan-out, Reliability, Retry mechanism | Medium (5K+ msg/sec) | SQS, Redis Queue |
| **Live Streaming** | YouTube Live, Twitch | Event Stream | Real-time chat, Metrics, High throughput | Very High (200K+ msg/sec) | WebSocket + Redis |
| **Payment Processing** | PayPal, Stripe | Event Stream | Transaction pipeline, Fraud detection, Audit | Medium (1K+ msg/sec) | SQS, RabbitMQ |

**Analytics & Data Processing**

| **Use Case** | **System Example** | **Kafka Pattern** | **Data Flow** |
| --- | --- | --- | --- |
| **Real-time Analytics** | Google Analytics, Mixpanel | Event Stream | Events → Kafka → Stream Processing → Dashboard |
| **Log Aggregation** | ELK Stack, Splunk | Event Stream | Logs → Kafka → LogStash → ElasticSearch |
| **User Activity Tracking** | Netflix Recommendations | Event Stream | Clicks → Kafka → ML Pipeline → Recommendations |
| **IoT Data Processing** | Smart City, Manufacturing | Event Stream | Sensors → Kafka → Time Series DB → Alerts |
| **Financial Trading** | Stock Exchanges | Event Stream | Market Data → Kafka → Trading Algorithms → Orders |
| **Fraud Detection** | Banking, Credit Cards | Event Stream | Transactions → Kafka → ML Models → Alerts |

**Interview Question Templates**

| **Question Type** | **When to Suggest Kafka** | **Key Discussion Points** |
| --- | --- | --- |
| **"Design Twitter"** | For timeline generation, tweet fanout | Partitioning strategy, Consumer groups |
| **"Design Chat System"** | For message delivery, presence updates | Message ordering, Multiple device sync |
| **"Design Uber"** | For trip events, driver location updates | Real-time tracking, Event sourcing |
| **"Design Payment System"** | For transaction events, fraud detection | Event sourcing, Audit compliance |
| **"Design Netflix"** | For user activity, recommendation pipeline | Stream processing, ML pipeline |
| **"Design Food Delivery"** | For order pipeline, real-time tracking | Order state management, Multi-service coordination |