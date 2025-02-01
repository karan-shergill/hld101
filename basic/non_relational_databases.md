# Non Relational Databases

- [Relational VS non-relational databases](#relational-vs-non-relational-databases)
- [Why  Non-Relational Databases (NoSQL) scales well Horizontalally?](#why--non-relational-databases-nosql-scales-well-horizontalally)
    - [Why Relational Databases Struggle with Horizontal Scaling](#why-relational-databases-struggle-with-horizontal-scaling)
    - [Why NoSQL Scales Horizontally Well](#why-nosql-scales-horizontally-well)
    - [**Conclusion**](#conclusion)
    - [Conclusion](#conclusion)
- [ACID vs BASE](#acid-vs-base)
    - [ACID Model (Used in Relational Databases - SQL)](#acid-model-used-in-relational-databases---sql)
    - [BASE Model (Used in Non-Relational Databases - NoSQL)](#base-model-used-in-non-relational-databases---nosql)
- [NoSQL Databases & Joins](#nosql-databases--joins)
    - [Why NoSQL Databases Don’t Support Joins Well](#why-nosql-databases-dont-support-joins-well)
    - [Solution: Denormalization in NoSQL](#solution-denormalization-in-nosql)
    - [Trade-offs of Denormalization](#trade-offs-of-denormalization)
    - [When to Use Denormalization in NoSQL](#when-to-use-denormalization-in-nosql)
- [Types of Non-Relational Databases](#types-of-non-relational-databases)
    - [1. Key-Value Stores](#1-key-value-stores)
    - [2. Document Stores](#2-document-stores)
    - [3. Column-Family Stores](#3-column-family-stores)
    - [4. Graph Databases](#4-graph-databases)
    - [5. Time-Series Databases](#5-time-series-databases)
    - [6. NewSQL Databases (Hybrid)](#6-newsql-databases-hybrid)
    - [Summary](#summary)
- [Choosing between non relational databases](#choosing-between-non-relational-databases)
- [How to Scale a Non-Relational (NoSQL) Database](#how-to-scale-a-non-relational-nosql-database)
    - [1. Scaling Vertically vs. Horizontally](#1-scaling-vertically-vs-horizontally)
    - [2. Sharding (Horizontal Partitioning)](#2-sharding-horizontal-partitioning)
    - [3. Replication (Ensuring High Availability)](#3-replication-ensuring-high-availability)
    - [4. Caching (Reducing Query Load)](#4-caching-reducing-query-load)
    - [5. Auto-Scaling (Dynamic Resource Allocation)](#5-auto-scaling-dynamic-resource-allocation)
- [Partitioning in NoSQL Databases](#partitioning-in-nosql-databases)
- [Sharding in NoSQL Databases](#sharding-in-nosql-databases)
    - [How is Sharding Different from Partitioning?](#how-is-sharding-different-from-partitioning)
    - [How to Achieve Sharding in NoSQL Databases?](#how-to-achieve-sharding-in-nosql-databases)
- [How is Sharding Implemented in Popular NoSQL Databases?](#how-is-sharding-implemented-in-popular-nosql-databases)
    - [1. MongoDB Sharding (Built-in)](#1-mongodb-sharding-built-in)
    - [2. Cassandra Sharding (Partitioned by Design)](#2-cassandra-sharding-partitioned-by-design)
    - [3. DynamoDB Auto-Sharding](#3-dynamodb-auto-sharding)
    - [4. HBase Sharding (Region Splitting)](#4-hbase-sharding-region-splitting)
- [Challenges of Sharding in NoSQL](#challenges-of-sharding-in-nosql)
- [Sharding in Relational vs. Non-Relational Databases](#sharding-in-relational-vs-non-relational-databases)
    - [Key Differences Between Sharding in SQL vs. NoSQL](#key-differences-between-sharding-in-sql-vs-nosql)
    - [Similarities Between Sharding in SQL and NoSQL](#similarities-between-sharding-in-sql-and-nosql)
    - [Example: Sharding in SQL vs. NoSQL](#example-sharding-in-sql-vs-nosql)
    - [When to Use Sharding in SQL vs. NoSQL?](#when-to-use-sharding-in-sql-vs-nosql)
- [Indexing in Non-Relational (NoSQL) Databases](#indexing-in-non-relational-nosql-databases)
    - [Why Indexing is Important in NoSQL?](#why-indexing-is-important-in-nosql)
    - [Types of Indexing in NoSQL Databases](#types-of-indexing-in-nosql-databases)
- [Scaling a non-relational (NoSQL) database from 100 users to 1 billion users](#scaling-a-non-relational-nosql-database-from-100-users-to-1-billion-users)
    - [Phase 1: Small Scale (100 - 10,000 Users)](#phase-1-small-scale-100---10000-users)
    - [Phase 2: Medium Scale (10,000 - 1 Million Users)](#phase-2-medium-scale-10000---1-million-users)
    - [Phase 3: Large Scale (1 Million - 100 Million Users)](#phase-3-large-scale-1-million---100-million-users)
    - [Phase 4: Massive Scale (100 Million - 1 Billion+ Users)](#phase-4-massive-scale-100-million---1-billion-users)
    - [Summary: Scaling Strategy by User Growth](#summary-scaling-strategy-by-user-growth)
- [Qs for an Interview](#qs-for-an-interview)
    - [Conceptual Questions](#conceptual-questions)
    - [Practical and Real-World Scaling Scenarios](#practical-and-real-world-scaling-scenarios)
    - [Advanced and Design-Based Questions](#advanced-and-design-based-questions)
    - [Failure and Recovery Scenarios](#failure-and-recovery-scenarios)
    - [Comparative Analysis and Trade-offs](#comparative-analysis-and-trade-offs)

Non-relational databases, also known as NoSQL databases, are designed to handle large-scale, distributed, and schema-less data storage needs. They are widely used in modern system design for applications requiring high availability, scalability, and flexibility.

# Relational VS non-relational databases

| Feature | Relational Databases (SQL) | Non-Relational Databases (NoSQL) |
| --- | --- | --- |
| **Schema** | Fixed, predefined schema (tables, columns, rows) | Schema-less or flexible schema (JSON, key-value, graphs, etc.) |
| **Data Structure** | Structured, tabular format | Unstructured or semi-structured (key-value, document, graph, column-family) |
| **Scalability** | Vertical scaling (adding more resources to a single machine) | Horizontal scaling (distributing data across multiple nodes) |
| **Consistency Model** | Strong consistency (ACID transactions) | Eventual consistency (BASE model, though some support ACID) |
| **Query Language** | SQL (Structured Query Language) | No fixed query language, uses APIs or query-specific languages |
| **Joins** | Supports complex joins and relationships | Typically does not support joins (denormalization needed) |
| **Use Case** | Best for structured data, complex transactions (e.g., banking, ERP, CRM) | Best for large-scale, distributed applications (e.g., real-time analytics, social networks, IoT) |
| **Examples** | MySQL, PostgreSQL, Oracle, SQL Server | MongoDB, Redis, Cassandra, DynamoDB, Neo4j |

# Why  Non-Relational Databases (NoSQL) scales well Horizontalally?

The reason **NoSQL databases scale well horizontally, while relational databases (SQL) struggle with it**, comes down to **fundamental architectural differences**.

### Why Relational Databases Struggle with Horizontal Scaling

1. **Strong ACID Transactions (Strict Consistency)**
    - Relational databases **ensure strong consistency** across all nodes (Atomicity, Consistency, Isolation, Durability).
    - If a transaction updates multiple rows/tables across different nodes, the system must **synchronize** them, making horizontal scaling difficult.
2. **Complex Joins and Relationships**
    - In SQL databases, queries often involve **joins across multiple tables**.
    - When data is spread across multiple servers, joins become expensive and require **cross-node communication**, slowing down performance.
3. **Centralized Transaction Coordination**
    - SQL databases rely on a **centralized transaction manager** to enforce ACID properties.
    - Coordinating transactions across multiple servers introduces **significant overhead**.
4. **Manual Sharding is Hard**
    - Some SQL databases support **sharding**, but it’s often **manual and complex**.
    - The application must be **rewritten** to know which node stores which data.
    - Example: If you shard a relational database by **User ID**, a query across multiple users **must fetch data from multiple shards**, making queries slow.

### Why NoSQL Scales Horizontally Well

1. **Schema Flexibility**
    - NoSQL databases **don’t have a fixed schema** with strict relationships and constraints.
    - Data can be distributed easily across multiple servers without worrying about joins or complex integrity rules.
2. **Sharding (Partitioning) is Built-in**
    - NoSQL databases are **designed from the ground up** to distribute data across multiple nodes.
    - They often use **hash-based or range-based sharding**, where different data chunks are stored on different servers.
3. **No Joins = Independent Reads/Writes**
    - In relational databases, queries often require **joins** (combining data from multiple tables).
    - **Joins make horizontal scaling hard** because data from multiple servers must be merged, increasing network latency and complexity.
    - NoSQL **denormalizes data** (stores related data together), so each query can be handled by a **single node**.
4. **Eventual Consistency (BASE vs. ACID)**
    - Many NoSQL databases **sacrifice strict consistency** (ACID transactions) in favor of **eventual consistency (BASE)**.
    - This means data can be **read/written in parallel on different nodes** without waiting for synchronization.
5. **Peer-to-Peer Distributed Model** 
    - Some NoSQL databases like **Cassandra** use a **peer-to-peer** architecture instead of a **primary-replica model**.
    - This allows all nodes to handle queries independently, avoiding bottlenecks.

Comparison: Horizontal Scaling in SQL vs. NoSQL

| Feature | NoSQL (Horizontal Scaling) | SQL (Difficult Horizontal Scaling) |
| --- | --- | --- |
| **Schema** | Flexible (No strict schema) | Rigid (Strict schema & constraints) |
| **Sharding** | Automatic & built-in | Mostly manual & complex |
| **Joins** | No joins, data is denormalized | Joins require cross-node communication |
| **Consistency** | Eventual consistency (BASE) | Strong consistency (ACID) |
| **Query Performance** | Fast, as each node works independently | Slower due to cross-node queries |
| **Best Use Cases** | High availability, big data, real-time analytics | Banking, transactions, enterprise applications |

Examples of NoSQL Databases with Horizontal Scaling

| Database | Scaling Method |
| --- | --- |
| **MongoDB** | Sharding (distributes JSON documents across nodes) |
| **Cassandra** | Peer-to-peer distributed model (data spread across nodes) |
| **DynamoDB** | Auto-scalable with partitioning and replication |
| **Redis Cluster** | Data is partitioned across multiple instances |

### **Conclusion**

NoSQL databases scale well horizontally because they:

1. Distribute data across multiple machines (nodes).
2. Use **sharding** to balance data and traffic.
3. Support **replication** to prevent data loss.
4. Favor **high availability** in distributed systems.

### Conclusion

✅ **NoSQL scales horizontally well** because it avoids joins, allows eventual consistency, and distributes data easily.

❌ **Relational databases struggle with horizontal scaling** because they require joins, ACID transactions, and centralized consistency.

# ACID vs BASE

The **ACID** and **BASE** models define how databases handle transactions and consistency. They represent two different approaches to data integrity, with ACID favoring **strong consistency** and BASE prioritizing **availability and performance**

### ACID Model (Used in Relational Databases - SQL)

The **ACID** model ensures **strict consistency** and guarantees reliable transactions. It is commonly used in relational databases like MySQL, PostgreSQL, and Oracle.

**ACID Properties:**

1. **Atomicity** (All or Nothing)
    - A transaction is either **fully completed** or **fully rolled back**.
    - Example: If money is transferred between two bank accounts, either **both debit and credit occur**, or neither happens.
2. **Consistency** (Valid State Transitions)
    - The database **always remains in a valid state** before and after a transaction.
    - Example: If a bank transfer violates a rule (e.g., negative balance), the transaction fails.
3. **Isolation** (Concurrent Transactions Don’t Interfere)
    - Multiple transactions can happen **at the same time**, but they don’t affect each other.
    - Example: Two users booking the last plane ticket should not both succeed.
4. **Durability** (Permanent Changes)
    - Once a transaction is committed, it is **permanently saved**, even if the system crashes.
    - Example: If a user purchases an item, the order remains in the system even after a power failure.

**Use Cases for ACID Databases:**

1. Banking & Financial Transactions
2. E-commerce Order Processing
3. Enterprise Software (ERP, CRM)

### BASE Model (Used in Non-Relational Databases - NoSQL)

The **BASE** model is an alternative to ACID that focuses on **scalability, availability, and performance**. It is commonly used in NoSQL databases like Cassandra, DynamoDB, and MongoDB.

**BASE Properties:**

1. **Basically Available** (Always Responds)
    - The system **guarantees availability**, even if some data is not up-to-date.
    - Example: Amazon DynamoDB allows reading stale data instead of blocking users.
2. **Soft State** (Intermediary States Allowed)
    - Data may temporarily be **inconsistent** while updates propagate across nodes.
    - Example: A social media post might not appear instantly to all users.
3. **Eventual Consistency** (Data Becomes Consistent Over Time)
    - The system guarantees **data consistency eventually**, but not instantly.
    - Example: In a distributed database, some servers may take time to sync.

**Use Cases for BASE Databases:**

1. Real-Time Applications (Social Media, Chat Apps)
2. Big Data & Analytics (Log Processing, IoT)
3. Content Delivery Networks (CDNs)

**ACID vs. BASE:**

| Feature | ACID (SQL Databases) | BASE (NoSQL Databases) |
| --- | --- | --- |
| **Consistency** | Strong (Immediate) | Eventual (Delayed) |
| **Availability** | Low (May reject requests) | High (Always responds) |
| **Scalability** | Vertical (Single machine scaling) | Horizontal (Distributed across nodes) |
| **Performance** | Slower (Ensures integrity) | Faster (Allows some inconsistency) |
| **Use Case** | Banking, Transactions, ERP | Social Media, Real-Time Apps, Big Data |

# NoSQL Databases & Joins

In **non-relational (NoSQL) databases**, it's often said that they **"typically do not support joins, so denormalization is needed."** This is because of the **distributed architecture** and **scalability focus** of NoSQL databases.

### Why NoSQL Databases Don’t Support Joins Well

**a) Distributed Nature of NoSQL Databases**

- In relational databases (SQL), joins are performed **inside a single machine (vertical scaling)**, making them efficient.
- NoSQL databases are **horizontally scalable** (data is spread across multiple machines).
- Performing a **join across multiple machines** would require **cross-network communication**, which is **slow and inefficient**.

**b) Schema Flexibility & Data Models**

- Relational databases enforce **normalization** (breaking data into multiple related tables).
- NoSQL databases use **schema-less or flexible models** (JSON, key-value, graph, columnar).
- These models prioritize **fast reads/writes** over **complex queries**, like joins.

**c) Performance Issues with Joins**

- In SQL, joins work well because **data is stored in indexed tables** with strong relationships.
- In NoSQL, performing joins **requires scanning multiple documents or making multiple queries**, which is **slow and resource-intensive**.

### Solution: Denormalization in NoSQL

Because joins are expensive in NoSQL, data is **denormalized** to optimize for read performance.

Instead of storing data in **separate related tables**, NoSQL **embeds related data inside documents or keeps redundant copies**.

**Example: Storing User & Orders Data**

**SQL (Normalized Approach)**

| Users Table | Orders Table |
| --- | --- |
| user_id (PK) | order_id (PK) |
| name | user_id (FK) |
| email | product_name |
- To get **all orders for a user**, we need to **join** `Users` and `Orders` tables.

**NoSQL (Denormalized Approach)**

```json
{
  "user_id": 101,
  "name": "John Doe",
  "email": "john@example.com",
  "orders": [
    {"order_id": 1, "product": "Laptop"},
    {"order_id": 2, "product": "Phone"}
  ]
}
```

- Instead of storing **orders separately**, they are **embedded inside the user document**.
- This allows **fast reads** with a **single query**.

### Trade-offs of Denormalization

| Feature | Denormalization (NoSQL) | Normalization (SQL) |
| --- | --- | --- |
| **Read Speed** | ✅ Faster (single query) | ❌ Slower (joins needed) |
| **Write Speed** | ❌ Slower (duplicate data updates) | ✅ Faster (small updates) |
| **Storage** | ❌ Higher (data duplication) | ✅ Lower (no redundancy) |
| **Scalability** | ✅ Better (works in distributed systems) | ❌ Harder (joins are slow across nodes) |

### When to Use Denormalization in NoSQL

1. **Read-heavy applications** (e.g., social media feeds, caching, analytics).
2. **Highly distributed databases** where joins would be inefficient.
3. **Flexible schema requirements** where data structure frequently changes.
4. ❌ **Avoid if you need frequent updates**, since updating denormalized data can be **complex and costly**.

# Types of Non-Relational Databases

Non-relational (NoSQL) databases come in various types, each suited for specific use cases and data models.

### 1. Key-Value Stores

- **Data Model:** Stores data as **key-value pairs** (like a dictionary or hash table).
- **How it Works:** Each record is accessed via a **unique key**, and the value associated with that key can be any type of data (string, number, JSON, etc.).
- **Advantages:**
    - **High performance** for simple lookups.
    - Easy to scale horizontally.
    - Suitable for caching and session management.
- **Best For:**
    - **Caching** (e.g., Redis, Memcached).
    - **Session stores** (storing user sessions in web apps).
    - **Configuration settings** (storing key-value pairs for app configurations).

**Examples:** Redis, DynamoDB (though it can support other models as well), Riak

### 2. Document Stores

- **Data Model:** Stores data as **documents** (usually JSON, BSON, or XML format). Documents are grouped into **collections**.
- **How it Works:** Each document can have different fields (key-value pairs), and the structure of documents can vary across the same collection. This is more flexible than relational tables.
- **Advantages:**
    - Supports complex, hierarchical, or nested data.
    - Easily stores data that might change frequently in structure.
    - Great for rapidly evolving data.
- **Best For:**
    - **Content management systems (CMS)** (e.g., blog posts, product catalogs).
    - **User profiles** (for websites or social platforms).
    - **Real-time analytics**.

**Examples**: MongoDB, CouchDB, Couchbase, Firebase Firestore

### 3. Column-Family Stores

- **Data Model:** Data is stored in **columns** rather than rows. Each "column family" is a set of related data that is stored together for efficient retrieval.
- **How it Works:** Data is organized in a sparse, distributed manner. Each column family contains rows identified by a unique key, but each row can have a different set of columns.
- **Advantages:**
    - Optimized for read-heavy applications.
    - Good for storing **large amounts of data** with varying columns.
    - Efficient for **analytical queries** (such as aggregation).
- **Best For:**
    - **Real-time analytics** and reporting.
    - **Data warehousing**.
    - **Event logging**.

**Examples:** Cassandra, HBase, ScyllaDB

### 4. Graph Databases

- **Data Model:** Represents data as **nodes** (entities) and **edges** (relationships). It’s based on graph theory.
- **How it Works:** Graph databases are designed to efficiently handle data with **complex relationships**. Each node and edge can have attributes that are also represented as key-value pairs.
- **Advantages:**
    - Great for applications that need to perform **complex relationships or graph traversals**.
    - Perfect for **network-related data**.
- **Best For:**
    - **Social networks** (e.g., finding connections between users).
    - **Recommendation engines** (e.g., suggesting products based on user behavior).
    - **Fraud detection** (e.g., finding suspicious patterns in transactions).

**Examples:** Neo4j, ArangoDB, Amazon Neptune, OrientDB

### 5. Time-Series Databases

- **Data Model:** Optimized for **time-stamped data**. It stores data records that are associated with a specific timestamp.
- **How it Works:** Time-series databases are designed to efficiently handle large volumes of data that is continuously generated over time. It is optimized for **insertion speed** and retrieval of recent data.
- **Advantages:**
    - Efficient at **storing and querying time-series data**.
    - Supports **real-time analytics** on large datasets.
    - Handles **high write throughput**.
- **Best For:**
    - **IoT data** (e.g., temperature sensors, devices reporting metrics).
    - **Monitoring** (e.g., server logs, application performance).
    - **Financial data** (e.g., stock prices, crypto trends).

**Examples:** InfluxDB, TimescaleDB, Prometheus, OpenTSDB

### 6. NewSQL Databases (Hybrid)

- **Data Model:** Combines the **relational model** with the scalability and flexibility of NoSQL databases.
- **How it Works:** NewSQL databases offer **ACID compliance** (like traditional SQL) while providing the **horizontal scalability** that NoSQL databases offer.
- **Advantages:**
    - **SQL-style queries** with relational features.
    - Can scale **horizontally** like NoSQL databases.
- **Best For:**
    - Applications that require **SQL functionality** but need to handle large volumes of data at scale.
    - **Cloud-native applications** that require high availability and scalability.

**Examples:** Google Spanner, CockroachDB, NuoDB

### Summary

| Type | Data Model | Best For | Examples |
| --- | --- | --- | --- |
| **Key-Value Stores** | Key-Value | Caching, Session Stores, Configuration | Redis, DynamoDB, Riak |
| **Document Stores** | Document | Content Management, Real-time Analytics | MongoDB, CouchDB, Firebase Firestore |
| **Column-Family Stores** | Column | Data Warehousing, Real-Time Analytics | Cassandra, HBase, ScyllaDB |
| **Graph Databases** | Graph | Social Networks, Fraud Detection | Neo4j, ArangoDB, Amazon Neptune |
| **Time-Series Databases** | Time-stamped | IoT, Monitoring, Financial Data | InfluxDB, TimescaleDB, Prometheus |
| **Object-Oriented DBs** | Object | CAD, Multimedia, Real-time Simulations | db4o, ObjectDB, Versant |
| **NewSQL Databases** | Relational (with NoSQL scalability) | Cloud-Native Apps, High Availability | Google Spanner, CockroachDB, NuoDB |

# Choosing between non relational databases

Here’s a **decision table** to help you decide which type of **NoSQL database** to use based on your application’s needs:

| **Criteria** | **Key-Value Stores** | **Document Stores** | **Column-Family Stores** | **Graph Databases** | **Time-Series Databases** | **Object-Oriented Databases** |
| --- | --- | --- | --- | --- | --- | --- |
| **Data Structure** | Simple key-value pairs | Flexible documents (JSON/BSON/XML) | Rows with columns, can vary per row | Nodes and edges (graph) | Time-stamped data | Objects with attributes and methods |
| **Use Case** | Caching, Session management, Real-time data lookup | Flexible and hierarchical data, Dynamic schemas | High write throughput, Large-scale data analysis, Real-time analytics | Complex relationships, Social networks, Recommendation engines | Time-stamped data, Real-time analytics, IoT data | Storing objects, CAD, Multimedia, Real-time simulations |
| **Examples of Real-Life Use Cases** | - **Session stores** (e.g., storing user sessions)  - **Caching** (e.g., Redis for website caching) | - **Product catalogs** with dynamic attributes (e.g., MongoDB for e-commerce)  - **Content management systems** (e.g., CouchDB for blogs) | - **Log data analysis** (e.g., Cassandra for server logs)  - **Real-time analytics** (e.g., HBase for large-scale metrics analysis) | - **Social networks** (e.g., Neo4j for finding friend relationships)  - **Fraud detection** (e.g., ArangoDB for detecting suspicious patterns) | - **IoT sensor data** (e.g., InfluxDB for monitoring temperatures or humidity)  - **Financial data** (e.g., TimescaleDB for stock prices over time) | - **CAD applications** (e.g., db4o for storing 3D models)  - **Multimedia** (e.g., storing video and image data) |
| **Performance Focus** | High performance for lookups | Flexibility, fast reads | High write throughput, efficient storage | Efficient graph traversals, relationship-heavy queries | High write throughput, optimized for time-based queries | Storing complex data directly from programming objects |
| **Examples of Databases** | - **Redis**  - **DynamoDB** | - **MongoDB**  - **CouchDB**  - **Firebase Firestore** | - **Cassandra**  - **HBase**  - **ScyllaDB** | - **Neo4j**  - **ArangoDB**  - **Amazon Neptune** | - **InfluxDB**  - **TimescaleDB**  - **Prometheus** | - **db4o**  - **ObjectDB**  - **Versant** |

---

**How to Choose the Right NoSQL Database:**

- **Use Key-Value Stores** when you need **high-performance** lookups and **simple data structures** (like caching or session management).
- **Use Document Stores** when your data is **hierarchical or flexible** (e.g., e-commerce, content management).
- **Use Column-Family Stores** for **real-time analytics** and handling **large-scale data** that requires **high write throughput** (e.g., log processing).
- **Use Graph Databases** when your data has **complex relationships** (e.g., social networks, fraud detection).
- **Use Time-Series Databases** for handling **time-stamped data** with high-frequency writes (e.g., IoT sensor data or financial data).
- **Use Object-Oriented Databases** when your data aligns closely with **object-oriented programming** (e.g., real-time simulations, CAD).

# How to Scale a Non-Relational (NoSQL) Database

Scaling a NoSQL database effectively is crucial for handling large volumes of data and high traffic efficiently. NoSQL databases are designed to scale **horizontally** by distributing data across multiple servers, ensuring high availability and performance. Scaling can be achieved through **sharding, replication, caching, and tuning the architecture** based on workload requirements.

### 1. Scaling Vertically vs. Horizontally

Before discussing NoSQL-specific techniques, it is important to understand the two main scaling approaches:

| **Scaling Type** | **Description** | **Advantages** | **Disadvantages** |
| --- | --- | --- | --- |
| **Vertical Scaling (Scaling Up)** | Increasing the resources (CPU, RAM, SSDs) of a single database server. | Simple to implement, no data distribution required. | Hardware limits, single point of failure, expensive. |
| **Horizontal Scaling (Scaling Out)** | Adding more database servers (nodes) and distributing data across them. | No limit on scalability, fault tolerance, cost-effective. | More complex data distribution and query routing. |

Most NoSQL databases rely on **horizontal scaling**, as it allows the system to handle increasing load by adding more servers.

### 2. Sharding (Horizontal Partitioning)

**Sharding** is the process of **splitting data across multiple database servers**. Each shard contains a subset of the data, reducing the load on individual nodes.

**How Sharding Works in NoSQL Databases**

- Data is **divided across multiple servers** based on a **shard key**.
- Each shard functions as an **independent database**.
- A **query router** directs requests to the correct shard.

**Sharding Strategies**

| **Sharding Method** | **Description** | **Example Scenario** |
| --- | --- | --- |
| **Key-Based (Hash) Sharding** | A hash function is applied to the shard key, ensuring an even distribution across shards. | User data distributed based on `user_id % num_shards`. |
| **Range-Based Sharding** | Data is assigned to shards based on a value range. | Orders with `order_date < 2023` in **shard_1**, newer orders in **shard_2**. |
| **Geographic Sharding** | Data is partitioned based on user location. | Users from **North America** in `shard_USA`, users from **Europe** in `shard_EU`. |
| **Dynamic Sharding (Auto-Sharding)** | Shards are automatically created or split based on load. | AWS DynamoDB and Google Firestore manage sharding automatically. |

**Example: MongoDB Sharding**

MongoDB supports bult-in sharding, where collections are distributed across multiple shards.

```
sh.enableSharding("ecommerceDB")
sh.shardCollection("ecommerceDB.orders", { "order_id": "hashed" })
```

- This command enables **sharding on the `orders` collection** using a **hashed `order_id`**, distributing data evenly.

**Challenges of Sharding**

- **Cross-shard queries** can be slow and complex.
- **Shard key selection** is critical to avoid hotspots (uneven data distribution).
- **Resharding** can be complex and may require data migration.

### 3. Replication (Ensuring High Availability)

Replication involves maintaining **multiple copies of data across different servers** to improve fault tolerance and availability.

**Replication Types**

| **Replication Type** | **Description** | **Example** |
| --- | --- | --- |
| **Master-Slave Replication** | One primary node handles writes, multiple secondary nodes handle reads. | MongoDB’s **Replica Sets**, Cassandra’s **Multi-Region Replication**. |
| **Master-Master Replication** | Multiple nodes can handle both reads and writes, ensuring high availability. | DynamoDB, CouchDB, and ScyllaDB support multi-master setups. |
| **Eventually Consistent Replication** | Changes are propagated asynchronously, leading to temporary inconsistencies. | Cassandra, DynamoDB, and Riak use **eventual consistency** for high availability. |

**Example: Cassandra Replication**

Cassandra automatically replicates data across multiple nodes.

```
CREATE KEYSPACE myKeyspace
WITH replication = {'class': 'NetworkTopologyStrategy', 'datacenter1': 3, 'datacenter2': 2};
```

- This ensures **3 copies of data in `datacenter1`** and **2 copies in `datacenter2`**.

**Challenges of Replication**

- Increased storage requirements due to **duplicate copies**.
- **Consistency vs. Availability** trade-offs (eventual consistency vs. strong consistency).
- **Network latency** when replicating across geographically distributed data centers.

### 4. Caching (Reducing Query Load)

Caching stores frequently accessed data in memory to reduce database load and improve query performance.

**How Caching Works**

- Frequently requested data is stored in **high-speed memory (RAM)**.
- Reduces expensive database queries by serving data from cache.
- Helps improve **read performance** in NoSQL databases.

**Caching Strategies**

| **Strategy** | **Description** | **Example** |
| --- | --- | --- |
| **Write-Through Cache** | Data is written to both the cache and the database simultaneously. | Ensures cache is always up-to-date, used in Redis. |
| **Write-Back Cache** | Data is written to cache first, then asynchronously to the database. | Reduces database writes but risks data loss on failure. |
| **Read-Through Cache** | Data is first checked in the cache before querying the database. | Commonly used in Memcached. |

**Example: Using Redis with MongoDB**

```python
import redis
import pymongo

cache = redis.Redis(host='localhost', port=6379, db=0)
mongo_client = pymongo.MongoClient("mongodb://localhost:27017/")
db = mongo_client["ecommerce"]

def get_product(product_id):
    cached_product = cache.get(product_id)
    if cached_product:
        return cached_product
    else:
        product = db.products.find_one({"product_id": product_id})
        cache.set(product_id, product)
        return product
```

- This implementation **checks Redis cache first** before querying MongoDB.

**Challenges of Caching**

- **Cache invalidation**: Ensuring cache is updated when the database changes.
- **Memory management**: Storing large datasets in cache can be expensive.
- **Consistency issues**: Cached data may become outdated.

### 5. Auto-Scaling (Dynamic Resource Allocation)

Auto-scaling adjusts resources **based on demand**, ensuring efficiency and cost optimization.

**Auto-Scaling Approaches**

| **Auto-Scaling Type** | **Description** | **Example** |
| --- | --- | --- |
| **Read Scaling** | More read replicas are added to handle increased traffic. | Amazon DynamoDB **Read Auto-Scaling**. |
| **Write Scaling** | Data is distributed across multiple write nodes. | MongoDB **Sharded Cluster**. |
| **Compute Scaling** | Additional processing power is allocated dynamically. | Google Firestore and AWS DynamoDB auto-scale compute resources. |

**Example: DynamoDB Auto-Scaling**

AWS DynamoDB automatically adjusts read/write capacity based on traffic.

```json
{
    "TableName": "Users",
    "ProvisionedThroughput": {
        "ReadCapacityUnits": 10,
        "WriteCapacityUnits": 10
    },
    "AutoScalingSettings": {
        "MinCapacity": 5,
        "MaxCapacity": 50
    }
}
```

**Challenges of Auto-Scaling**

- **Cost management**: Scaling up increases costs.
- **Latency spikes**: Scaling actions may introduce brief delays.
- **Data consistency**: Auto-scaling may impact consistency models.

# Partitioning in NoSQL Databases

Partitioning in NoSQL databases **splits data across multiple partitions** **within a single cluster** to improve performance and manageability. NoSQL databases are **designed for partitioning** from the ground up.

**Types of Partitioning in NoSQL**

| **Partitioning Type** | **Description** | **Example Scenario** |
| --- | --- | --- |
| **Range-Based Partitioning** | Data is stored in different partitions based on a range of values of a partition key. | Users with `user_id < 10M` in **partition 1**, `user_id >= 10M` in **partition 2**. |
| **Hash-Based Partitioning** | A hash function is applied to a key to determine the partition. | Distributes user data **evenly** across partitions. |
| **List-Based Partitioning** | Data is divided into partitions based on **specific key values**. | Users from "USA" in **partition_USA**, users from "India" in **partition_INDIA**. |
| **Composite Partitioning** | Combines **multiple partitioning strategies** (e.g., range + hash). | Users partitioned by **date (range)** and **country (hash)**. |

**How is Partitioning Done in NoSQL?**

- **MongoDB**: Uses **sharding with partitioned collections**.
- **Cassandra**: Uses **partition keys** to distribute data across nodes.
- **DynamoDB**: Automatically partitions data based on throughput requirements.
- **HBase**: Uses **row keys** to determine partition placement.

**Example in Cassandra:**

Cassandra partitions data based on the **partition key**, ensuring even distribution.

```sql
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    name TEXT,
    country TEXT
) WITH CLUSTERING ORDER BY (name ASC);
```

- The **partition key (`user_id`)** determines in which node the data is stored.

# Sharding in NoSQL Databases

Sharding is **horizontal partitioning** that distributes data **across multiple physical machines (shards)**. Each shard is **a separate database instance** with its own storage and computing resources.

### How is Sharding Different from Partitioning?

| **Feature** | **Partitioning** | **Sharding** |
| --- | --- | --- |
| **Scope** | Within a **single database** cluster | Across **multiple database instances** |
| **Scaling** | Improves **performance** but stays within one system | Scales **horizontally** by adding more servers |
| **Management** | Managed **internally by the database** | Needs **shard routing** (middleware or app logic) |

---

### How to Achieve Sharding in NoSQL Databases?

NoSQL databases are **designed for sharding**, and many offer **automatic sharding out of the box**.

**Sharding Approaches in NoSQL**

| **Sharding Strategy** | **Description** | **Example** |
| --- | --- | --- |
| **Key-Based (Hash) Sharding** | A **hash function** is applied to the shard key to evenly distribute data across shards. | `user_id % num_shards` determines which shard stores the data. |
| **Range-Based Sharding** | Data is assigned to shards based on **value ranges**. | Users with `user_id < 10M` go to **shard_1**, `>= 10M` to **shard_2**. |
| **Geographical Sharding** | Data is distributed based on location. | Users from **USA** go to `shard_USA`, users from **Europe** go to `shard_EU`. |
| **Dynamic / Auto-Sharding** | Shards automatically split and merge based on traffic. | AWS **DynamoDB**, Google **Firestore** do this automatically. |

# How is Sharding Implemented in Popular NoSQL Databases?

### 1. MongoDB Sharding (Built-in)

MongoDB uses **sharded clusters** to distribute data across nodes.

- **Shard Key**: Determines how data is split.
- **Config Servers**: Manage metadata and routing.
- **Query Router (`mongos`)**: Routes client requests to the right shard.

**Example: Setting up a MongoDB Sharded Cluster**

```
sh.enableSharding("myDatabase")
sh.shardCollection("myDatabase.users", { "user_id": "hashed" })
```

- This enables **hashed sharding** on `user_id`, distributing data **evenly across shards**.

### 2. Cassandra Sharding (Partitioned by Design)

Cassandra automatically shards data **across nodes** using a **partition key**.

- **Partition Key**: Determines how data is distributed across nodes.
- **Consistent Hashing**: Ensures even distribution.

**Example: Using a Partition Key in Cassandra**

```sql
CREATE TABLE users (
    user_id UUID PRIMARY KEY,
    name TEXT,
    country TEXT
);
```

- The **partition key (`user_id`)** ensures even distribution across nodes.
- Cassandra **automatically scales** by adding new nodes.

### 3. DynamoDB Auto-Sharding

DynamoDB automatically partitions data **based on throughput needs**.

- **Partitions grow automatically** when traffic increases.
- No need to manually set up sharding.

**Example: Partitioning in DynamoDB**

- `user_id` as **partition key**.
- AWS **automatically scales and rebalances shards**.

```json
{
    "TableName": "Users",
    "KeySchema": [
        {"AttributeName": "user_id", "KeyType": "HASH"}
    ],
    "ProvisionedThroughput": {
        "ReadCapacityUnits": 10,
        "WriteCapacityUnits": 10
    }
}
```

- DynamoDB **handles sharding internally**, based on **request load**.

### **4. HBase Sharding (Region Splitting)**

HBase **splits data into regions** based on the **row key**.

- **Automatic region splits** when data grows.
- **Regions are moved across RegionServers**.

**Example: Row Key for Partitioning in HBase**

```java
Table table = connection.getTable(TableName.valueOf("Users"));
Put put = new Put(Bytes.toBytes("user_123"));
put.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name"), Bytes.toBytes("Alice"));
table.put(put);

```

- HBase **automatically** places the row in the correct partition.

# Challenges of Sharding in NoSQL

**1. Query Complexity**

- **Cross-shard queries** are difficult and slow.
- NoSQL **avoids joins**, so data **must be denormalized**.

**2. Data Rebalancing**

- If shards become **uneven**, they need **manual or automatic resharding**.
- Some databases (DynamoDB, Firestore) **automatically rebalance**.

**3. Increased Operational Complexity**

- Requires **shard management tools** (like `mongos` for MongoDB).
- **Writes must be routed to the correct shard**.

**When to Use Partitioning vs. Sharding in NoSQL**

| **Factor** | **Partitioning** | **Sharding** |
| --- | --- | --- |
| **Scope** | Inside a **single cluster** | Across **multiple database instances** |
| **Scaling** | Increases **performance** within one system | Horizontally scales across **multiple servers** |
| **Query Performance** | Fast for **filtered queries** | More complex but **handles high traffic** |
| **Implementation Complexity** | **Automatic in most NoSQL databases** | Needs **shard routing** and careful key selection |

# Sharding in Relational vs. Non-Relational Databases

Sharding, also known as **horizontal partitioning**, is a technique used in both **Relational (SQL) and Non-Relational (NoSQL) databases** to distribute data across multiple nodes for scalability and performance.

### Key Differences Between Sharding in SQL vs. NoSQL

| **Aspect** | **Sharding in Relational Databases (SQL)** | **Sharding in Non-Relational Databases (NoSQL)** |
| --- | --- | --- |
| **Schema Design** | Requires a **fixed schema**, making sharding more complex. Foreign keys and joins must be avoided or handled manually. | No fixed schema, making it easier to distribute data dynamically. |
| **Data Distribution** | Typically **manual** and requires careful shard key selection and indexing to maintain referential integrity. | Often **built-in** (e.g., MongoDB, Cassandra) and can use automatic partitioning. |
| **Joins & Queries** | Cross-shard joins are difficult and require **application-level** logic to handle them. Query performance may degrade. | NoSQL generally avoids joins (denormalization), making cross-shard queries more efficient. |
| **Shard Key Selection** | Choosing a shard key is **complex** due to foreign key constraints and normalized tables. | NoSQL databases are **designed for denormalized data**, making shard key selection more flexible. |
| **Resharding Complexity** | Very **complex and expensive**. Schema constraints and data relationships make it difficult to move data between shards. | Easier to reshard, as data is often independent and distributed with minimal constraints. |
| **Scalability** | Designed for **vertical scaling**, so sharding is an additional challenge. Often requires middleware solutions (e.g., Vitess for MySQL). | Designed for **horizontal scaling**, so sharding is a **native feature** in many NoSQL databases. |
| **Replication & Fault Tolerance** | Sharding often requires **manual replication** setup and management. A single shard failure can cause **data inconsistency**. | NoSQL databases often have **built-in replication and fault tolerance**, automatically handling shard failures. |

### Similarities Between Sharding in SQL and NoSQL

Despite the differences, there are common principles shared between relational and non-relational database sharding:

| **Similarity** | **Explanation** |
| --- | --- |
| **Shard Key Importance** | Both require careful **shard key selection** to ensure even data distribution and avoid hotspots. |
| **Performance Benefits** | Sharding helps both SQL and NoSQL databases handle **high read/write traffic** efficiently. |
| **Cross-Shard Query Complexity** | Queries involving multiple shards can be **complex and slower**, requiring query routing logic. |
| **Data Distribution Strategies** | Both use **range-based**, **hash-based**, or **geographic-based sharding** strategies to distribute data. |
| **Application-Level Query Routing** | In both cases, applications need logic to **route queries to the correct shard**, unless a middleware or built-in mechanism is used. |

### Example: Sharding in SQL vs. NoSQL

**Example 1: MySQL Sharding (Relational Database)**

- Assume a **user database** with millions of users.
- Users are sharded based on their **user_id**.
- A **proxy layer (e.g., Vitess, MySQL Fabric)** routes queries.

```sql
-- Creating separate shards (databases)
CREATE DATABASE users_shard1;
CREATE DATABASE users_shard2;

-- Storing user data based on user_id range
CREATE TABLE users_shard1.users (
    user_id INT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);

CREATE TABLE users_shard2.users (
    user_id INT PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);
```

- **Application logic** determines whether `user_id` belongs in `shard1` or `shard2`.

**Example 2: MongoDB Sharding (NoSQL)**

- MongoDB uses **built-in sharding** to distribute user data automatically.

```
sh.enableSharding("userDatabase")
sh.shardCollection("userDatabase.users", { "user_id": "hashed" })
```

- MongoDB automatically assigns user documents to different shards.
- No need for a **proxy layer**; MongoDB handles **query routing**.

### When to Use Sharding in SQL vs. NoSQL?

| **Use Case** | **Best for SQL Sharding?** | **Best for NoSQL Sharding?** |
| --- | --- | --- |
| **High transactional consistency (ACID)** | ✅ Yes | ❌ No |
| **Handling complex joins and relationships** | ✅ Yes | ❌ No |
| **Scalable, distributed architecture** | ❌ No | ✅ Yes |
| **High-speed, high-volume reads/writes** | ❌ No | ✅ Yes |
| **Dynamic schema and flexible data models** | ❌ No | ✅ Yes |

# Indexing in Non-Relational (NoSQL) Databases

Indexing in NoSQL databases improves query performance by allowing efficient lookups rather than scanning entire datasets. Since NoSQL databases do not follow strict relational schemas, indexing strategies vary depending on the database type (Document, Key-Value, Column-Family, or Graph).

### Why Indexing is Important in NoSQL?

- **Faster Queries:** Avoids full table/collection scans.
- **Efficient Read Performance:** Helps retrieve specific documents or key-value pairs quickly.
- **Reduced Latency:** Optimized lookups, especially in large datasets.
- **Optimized Sorting & Filtering:** Helps with ordering and complex queries.

### Types of Indexing in NoSQL Databases

| **Index Type** | **Description** | **Example Databases** | **Example Use Case** |
| --- | --- | --- | --- |
| **Primary Indexing** | Automatically created on primary keys. | MongoDB, Cassandra, DynamoDB | Fast lookups on `_id` or `Partition Key`. |
| **Secondary Indexing** | Allows querying on non-primary key fields. | MongoDB, DynamoDB (Global Secondary Index), Cassandra (Secondary Index) | Searching users by email instead of `user_id`. |
| **Composite Indexing** | Indexes multiple fields together. | MongoDB, Cassandra | Querying users by `name` **AND** `age`. |
| **Full-Text Indexing** | Enables text search capabilities. | Elasticsearch, MongoDB (Text Index) | Searching for blog posts containing certain keywords. |
| **Geospatial Indexing** | Optimizes location-based queries. | MongoDB, Elasticsearch, Cassandra | Finding nearby restaurants based on latitude/longitude. |
| **Bitmap Indexing** | Efficient for boolean or categorical values. | Cassandra, Elasticsearch | Indexing `is_active` or `status` fields for quick filtering. |
| **Inverted Indexing** | Used in search engines for keyword lookups. | Elasticsearch, Solr | Searching documents based on word frequency. |

# Scaling a non-relational (NoSQL) database from 100 users to 1 billion users

### Phase 1: Small Scale (100 - 10,000 Users)

**Database Setup**

- A **single instance** of a NoSQL database (e.g., **MongoDB, DynamoDB, or Cassandra**).
- Runs on a **single server** with adequate CPU, memory, and disk.
- Simple **indexing** for optimizing queries.

**Key Considerations**

1. Choose a **flexible schema** to accommodate future changes.
2. Set up **basic indexing** to optimize read queries.
3. Implement **database backups** to prevent data loss.

**Example Database Architecture**

- **Single Node** running a NoSQL database (e.g., MongoDB, Redis, DynamoDB).
- **Data stored locally** on the server.

### Phase 2: Medium Scale (10,000 - 1 Million Users)

**Database Scaling Approach:**

1. **Vertical Scaling (Scale Up)**
    - Upgrade the single database server (increase CPU, RAM, and disk storage).
    - Use **faster SSDs** for quicker reads/writes.
2. **Read Replicas (Replication for Reads)**
    - Add **read replicas** to handle increasing read traffic.
    - **Primary node (master)** handles writes, **secondary nodes (slaves)** handle reads.
3. **Introduce Caching Layer**
    - Use **Redis or Memcached** to cache frequently accessed data.
    - Reduce database load by **storing user sessions and frequent queries**.

**Key Considerations**

1. **Monitor database performance** using metrics (query times, CPU, memory usage).
2. **Read vs. Write Separation:** Use **replication** to offload read operations to replicas.
3. **Ensure consistency:** Use eventual consistency if required.

**Example Database Architecture**

- **Primary Database + Read Replicas**
- **Caching Layer (Redis)**
- **Load Balancer for Read Requests**

### Phase 3: Large Scale (1 Million - 100 Million Users)

**Database Scaling Approach**

1. **Sharding (Horizontal Scaling)**
    - Distribute data across multiple servers using **sharding keys**.
    - Each **shard** contains a portion of the data (e.g., users 1-10M on shard 1, 10M-20M on shard 2).
    - NoSQL databases like **MongoDB and Cassandra** have built-in support for sharding.
2. **Auto-Scaling (Cloud-Based Databases)**
    - Move to **cloud NoSQL solutions** like **DynamoDB (AWS), Google Firestore, or MongoDB Atlas** for automatic scaling.
    - Leverage **auto-scaling groups** to add/remove nodes dynamically.
3. **Multi-Region Deployment (Global Scalability)**
    - Deploy replicas in **multiple data centers** to improve performance for users worldwide.
    - Use a **geo-distributed database** like **Cassandra or AWS DynamoDB Global Tables**.

**Key Considerations**

1. **Sharding Strategy:** Select a **good sharding key** to prevent uneven data distribution.
2. **Replication Lag:** Monitor read consistency across regions.
3. **Database Partitioning:** Avoid **hotspots** (high-traffic partitions).

**Example Database Architecture**

- **Sharded NoSQL Database (MongoDB/Cassandra)**
- **Multiple Read Replicas**
- **Distributed Caching (Redis)**
- **Geo-Distributed Databases for Global Users**

### Phase 4: Massive Scale (100 Million - 1 Billion+ Users)

**Database Scaling Approach**

1. **Fully Distributed, Multi-Cloud NoSQL Databases**
    - Use **DynamoDB, Google Spanner, or Apache Cassandra** for global scalability.
    - **Multi-region, multi-cloud deployments** for high availability.
    - **Data is automatically partitioned across servers**.
2. **Eventual Consistency with High Availability**
    - Use **eventual consistency models** to **allow better write scalability**.
    - Store **user-generated content (posts, comments) in a distributed manner**.
3. **Microservices & Database Separation**
    - Split the **monolithic database** into **multiple purpose-driven databases**.
    - Example:
        - **User Authentication → Key-Value Store (Redis, DynamoDB)**
        - **User Profiles → Document Store (MongoDB, Firestore)**
        - **User Relationships → Graph Database (Neo4j, ArangoDB)**

**Key Considerations**

1. **Ensure Fault Tolerance**: Use database clusters with automatic failover.
2. **Manage Costs**: Optimize storage by **archiving cold data**.
3. **High Throughput:** Use **streaming pipelines (Kafka, Kinesis) for real-time data processing**.

**Example Database Architecture**

- **Multi-Cloud, Multi-Region NoSQL (DynamoDB, Cassandra, Google Spanner)**
- **Microservices-Based Data Storage (Each service has its own optimized DB)**
- **Streaming Pipelines for Data Processing (Kafka, Kinesis)**
- **Advanced Caching & CDNs for Fast Data Access**

### Summary: Scaling Strategy by User Growth

| **User Base** | **Scaling Approach** | **Database Example** | **Key Techniques** |
| --- | --- | --- | --- |
| **100 - 10K** | Single node | MongoDB, DynamoDB | Basic indexing, backups |
| **10K - 1M** | Vertical Scaling + Read Replicas | MongoDB, Cassandra | Read replication, caching (Redis) |
| **1M - 100M** | Sharding + Auto-Scaling | MongoDB, DynamoDB, Cassandra | Sharding, multi-region replication, cloud scaling |
| **100M - 1B+** | Fully Distributed, Multi-Cloud | Cassandra, Google Spanner, DynamoDB | Multi-cloud, microservices-based DBs, streaming |

**Start Simple** → Begin with a single instance, optimize indexing, and add caching.

**Scale in Phases** → Move from vertical scaling → replication → sharding → distributed systems.

**Global Scalability** → Multi-region deployment is key for performance.

**Optimize Costs** → Use **auto-scaling & cold storage** for cost efficiency.

**Balance Consistency & Availability** → Decide between **strong vs. eventual consistency** based on business needs.

# Qs for an Interview

### Conceptual Questions

1. Why do NoSQL databases scale horizontally better than relational databases?
2. What trade-offs do NoSQL databases make to achieve scalability and high availability?
3. How does the CAP theorem influence the design of NoSQL databases?
4. What are the key differences between BASE and ACID properties, and when would you choose one over the other?
5. Why do most NoSQL databases discourage the use of joins, and how do they handle relationships instead?
6. In what scenarios would you **not** choose a NoSQL database over a relational database?
7. How does schema flexibility in NoSQL databases impact data modeling and application design?
8. Explain how replication and partitioning differ in NoSQL databases. How do they contribute to scalability and fault tolerance?

### Practical and Real-World Scaling Scenarios

1. Imagine you are designing a system for a **global e-commerce platform** where product searches need to be fast and scalable. What type of NoSQL database would you choose and why?
2. Your NoSQL database has reached a **scalability bottleneck** due to uneven shard distribution. How would you diagnose and resolve the issue?
3. How would you design a **multi-region distributed NoSQL database** for an application serving users worldwide, considering factors like latency, consistency, and fault tolerance?
4. If a NoSQL database is experiencing **hotspot issues** (one shard receiving significantly more traffic than others), what strategies can be applied to mitigate this?
5. Your **NoSQL database performance degrades over time** as data grows. What factors would you investigate, and how would you optimize query performance?
6. If your application has **high write throughput**, which NoSQL database would you choose, and what techniques would you use to ensure data durability and consistency?
7. You are working on a **real-time analytics platform** that processes large volumes of event logs. Which type of NoSQL database would you choose, and how would you scale it efficiently?

### Advanced and Design-Based Questions

1. How would you implement a **custom sharding strategy** in a NoSQL database for a social media platform with billions of users?
2. What are the pros and cons of using **consistent hashing** for sharding in NoSQL databases?
3. How does a **leaderless replication model** (e.g., Cassandra) compare with a **primary-secondary replication model** (e.g., MongoDB)?
4. How would you design a **NoSQL-based real-time messaging system** that needs low latency and high availability?
5. How do NoSQL databases like Cassandra and DynamoDB handle **eventual consistency**? What are the implications for developers?
6. You need to migrate a large-scale application from a relational database to a NoSQL database. What factors would you consider, and how would you approach the migration?
7. Explain how secondary indexes impact performance in NoSQL databases and when they should be avoided.
8. How do NoSQL databases implement **multi-tenancy** to ensure data isolation for different customers?
9. If you were designing a **recommendation engine** that must handle personalized user preferences at scale, which NoSQL database would you pick, and why?
10. In a **serverless application architecture**, how would you handle database scaling while keeping costs low?

### Failure and Recovery Scenarios

1. What happens if a shard in a NoSQL database becomes **unavailable**? How does the system recover, and what techniques can prevent data loss?
2. How do NoSQL databases handle **network partitions**, and how does this impact application behavior?
3. If an entire **data center goes down**, how can a distributed NoSQL database continue serving users with minimal downtime?
4. Your NoSQL database is experiencing **high read latency** despite having multiple read replicas. What could be causing this, and how would you fix it?
5. How would you design a backup and disaster recovery strategy for a **distributed NoSQL database**?

### Comparative Analysis and Trade-offs

1. Compare the **sharding mechanisms** in MongoDB, Cassandra, and DynamoDB.
2. How does indexing differ between relational and non-relational databases?
3. In what cases would a **hybrid approach** (using both SQL and NoSQL databases) be beneficial?
4. How do databases like **Firebase and DynamoDB** achieve automatic scaling without requiring manual sharding?
5. If you had to choose between **Cassandra and MongoDB** for a time-series database, which would you prefer and why?
