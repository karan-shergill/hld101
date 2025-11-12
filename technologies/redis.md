# Redis

- [What is Redis?](#what-is-redis)
    - [Key Features](#key-features)
- [Redis vs. Traditional Databases (Relational & NoSQL)](#redis-vs-traditional-databases-relational--nosql)
    - [When to Use Redis Over Traditional Databases?](#when-to-use-redis-over-traditional-databases)
- [Why is Redis Single-Threaded?](#why-is-redis-single-threaded)
    - [How Redis’s Event-Driven Architecture Works?](#how-rediss-event-driven-architecture-works)
    - [Why & How is Redis Fast?](#why--how-is-redis-fast)
    - [Does Single-Threading Limit Redis Performance?](#does-single-threading-limit-redis-performance)
- [Redis Data Structures](#redis-data-structures)
  - [Strings (Binary-Safe)](#strings-binary-safe)
    - [Description](#description)
    - [Commands](#commands)
  - [Use Cases Using Redis Strings (Binary-Safe)](#use-cases-using-redis-strings-binary-safe)
    - [Caching](#caching)
    - [Session Storage](#session-storage)
    - [Counters](#counters)
    - [Rate Limiting](#rate-limiting)
    - [Feature Flags](#feature-flags)
    - [Storing Simple Key-Value Data](#storing-simple-key-value-data)
    - [Real-Time Analytics](#real-time-analytics)
    - [Distributed Locking](#distributed-locking)
  - [Lists (Ordered Collection)](#lists-ordered-collection)
    - [Description](#description)
    - [Commands](#commands)
  - [Use Cases Using Redis Lists (Ordered Collection)](#use-cases-using-redis-lists-ordered-collection)
    - [Message Queues](#message-queues)
    - [Activity Feeds](#activity-feeds)
    - [Job Scheduling](#job-scheduling)
    - [Recent Items](#recent-items)
    - [Leaderboard History](#leaderboard-history)
  - [Sets (Unique Collection, Unordered)](#sets-unique-collection-unordered)
    - [Description](#description)
    - [Commands](#commands)
  - [Use Cases Using Sets (Unique Collection, Unordered)](#use-cases-using-sets-unique-collection-unordered)
    - [Intersection and Union Operations](#intersection-and-union-operations)
    - [Random Selection](#random-selection)
    - [Blacklists or Blocklists](#blacklists-or-blocklists)
    - [Unique Event Tracking](#unique-event-tracking)
    - [Tagging Systems](#tagging-systems)
    - [Friend Lists or Followers](#friend-lists-or-followers)
  - [Hashes (Key-Value Store within Redis)](#hashes-key-value-store-within-redis)
    - [Description](#description)
    - [Commands](#commands)
  - [Use Cases Using Hashes (Key-Value Store within Redis)](#use-cases-using-hashes-key-value-store-within-redis)
    - [Storing User Profiles](#storing-user-profiles)
    - [Product Catalog](#product-catalog)
    - [Configuration Management](#configuration-management)
    - [Counters](#counters)
    - [Shopping Cart](#shopping-cart)
    - [Game Leaderboards](#game-leaderboards)
  - [Sorted Sets (Ordered Collection with Scores)](#sorted-sets-ordered-collection-with-scores)
    - [Description](#description)
    - [Commands](#commands)
  - [Use Cases Using Redis Sorted Sets](#use-cases-using-redis-sorted-sets)
    - [Leaderboards](#leaderboards)
    - [Priority Queues](#priority-queues)
    - [Time-Series Data](#time-series-data)
    - [Geospatial Indexing](#geospatial-indexing)
  - [Summary: When to Use Which Data Structure?](#summary-when-to-use-which-data-structure)
- [Redis Replication](#redis-replication)
    - [What is Redis Replication?](#what-is-redis-replication)
    - [How Redis Replication Works](#how-redis-replication-works)
    - [How Replication Works Internally?](#how-replication-works-internally)
    - [Read-Only Mode in Replicas](#read-only-mode-in-replicas)
    - [Failover & Handling Master Failures](#failover--handling-master-failures)
    - [Replication Limitations](#replication-limitations)
    - [What is Redis Sentinel?](#what-is-redis-sentinel)
    - [Why is Redis Sentinel Needed?](#why-is-redis-sentinel-needed)
    - [Failover Scenarios](#failover-scenarios)
    - [Comparison](#comparison)
- [Redis Clustering – Distributed Scaling](#redis-clustering--distributed-scaling)
    - [What is Redis Cluster?](#what-is-redis-cluster)
    - [Why Do We Need Redis Cluster?](#why-do-we-need-redis-cluster)
    - [Key Features of Redis Cluster](#key-features-of-redis-cluster)
    - [How Redis Cluster Works?](#how-redis-cluster-works)
    - [How Does Redis Cluster Route Requests?](#how-does-redis-cluster-route-requests)
    - [Failover & Recovery in Redis Cluster](#failover--recovery-in-redis-cluster)
    - [Redis Cluster vs. Redis Sentinel](#redis-cluster-vs-redis-sentinel)
    - [Limitations of Redis Cluster](#limitations-of-redis-cluster)
- [Redis Persistence – RDB & AOF for Data Durability](#redis-persistence--rdb--aof-for-data-durability)
    - [Why Does Redis Need Persistence?](#why-does-redis-need-persistence)
    - [RDB (Redis Database Backup) – Snapshot Persistence](#rdb-redis-database-backup--snapshot-persistence)
    - [AOF (Append-Only File) – Operation Logging](#aof-append-only-file--operation-logging)
    - [RDB vs AOF – Key Differences](#rdb-vs-aof--key-differences)
- [Distributed Locking in Redis – Handling Race Conditions](#distributed-locking-in-redis--handling-race-conditions)
    - [Why Do We Need a Distributed Lock?](#why-do-we-need-a-distributed-lock)
    - [Simple Redis Lock Using SETNX](#simple-redis-lock-using-setnx)
    - [Limitations of SETNX](#limitations-of-setnx)
    - [Safer Locking with SET + EX (Atomic Locking)](#safer-locking-with-set--ex-atomic-locking)
    - [Redlock Algorithm – The Distributed Locking Solution](#redlock-algorithm--the-distributed-locking-solution)
    - [When to Use Which Locking Mechanism?](#when-to-use-which-locking-mechanism)
- [Hot-Key Problem in Redis – Identification & Mitigation](#hot-key-problem-in-redis--identification--mitigation)
    - [What is a Hot Key in Redis?](#what-is-a-hot-key-in-redis)
    - [How to Identify Hot Keys?](#how-to-identify-hot-keys)
    - [Mitigating 1: Distribute Load with Key Sharding](#mitigating-1-distribute-load-with-key-sharding)
    - [Mitigating 2: Cache Replication with Read Replicas](#mitigating-2-cache-replication-with-read-replicas)
    - [Mitigating 3: Use Local Caching (Layered Caching)](#mitigating-3-use-local-caching-layered-caching)
    - [Mitigating 4: Use a TTL (Time-To-Live) Strategy](#mitigating-4-use-a-ttl-time-to-live-strategy)
    - [Mitigating 5: Implement Request Coalescing (Batching Requests)](#mitigating-5-implement-request-coalescing-batching-requests)
    - [Which Strategy Should You Use?](#which-strategy-should-you-use)

# What is Redis?

Redis (Remote Dictionary Server) is an **in-memory, key-value data store** known for **high-speed performance, low latency, and flexibility**. It is often used for caching, real-time analytics, messaging, and as a primary NoSQL database in some cases.

### Key Features

- **In-memory storage:** Data is stored in RAM, making it extremely fast.
- **Persistence options:** Supports snapshotting (RDB) and Append-Only File (AOF).
- **Data structures:** Supports Strings, Lists, Sets, Hashes, Sorted Sets, Streams, and more.
- **Pub/Sub messaging:** Allows event-driven architectures.
- **Replication & Clustering:** Supports master-slave replication and sharding for scalability.
- **Atomic operations:** Most commands are executed atomically.
- **Lightweight & efficient:** Written in C for optimal performance.

# Redis vs. Traditional Databases (Relational & NoSQL)

| Feature | Redis | Traditional Databases (MySQL, PostgreSQL) |
| --- | --- | --- |
| **Data Storage** | In-memory (RAM) | Disk-based (with caching layers) |
| **Speed** | Extremely fast (sub-millisecond) | Slower due to disk I/O |
| **Data Model** | Key-Value (NoSQL) | Relational (SQL-based, tables, joins) |
| **Persistence** | Optional (RDB & AOF) | Always persistent |
| **Scalability** | Horizontal scaling (sharding, clustering) | Vertical scaling (replication, partitioning) |
| **Use Cases** | Caching, real-time data, leaderboards, rate limiting | Transactional systems, complex queries |
| **ACID Compliance** | Not fully ACID (except transactions with Lua) | Fully ACID-compliant |
| **Query Language** | Redis CLI (command-based) | SQL (structured queries) |

### When to Use Redis Over Traditional Databases?

1. **Caching layer** – Store frequently accessed data to reduce database load.
2. **Real-time analytics** – Leaderboards, live stats, dashboards.
3. **Session management** – Store user sessions for web apps.
4. **Rate limiting** – Prevent abuse in APIs.
5. **Message queues** – Use Redis Pub/Sub for event-driven systems.

# Why is Redis Single-Threaded?

Redis is **single-threaded** for executing commands to **ensure simplicity and avoid concurrency issues** like race conditions. The key reasons for this design choice are:

1. **Avoids Locking Overhead**
    - Multi-threaded databases require locks to manage concurrent writes, leading to contention and slow performance.
    - Redis processes one command at a time, avoiding the need for locks.
2. **Efficient Use of CPU & Memory**
    - Redis operations are **in-memory**, so CPU is not the bottleneck.
    - Most Redis operations are **O(1) or O(log N)** in time complexity, making them super fast.
3. **Pipeline Optimization:** Redis batches multiple commands together using **pipelining**, reducing network round trips.
4. **Event-Driven, Non-Blocking I/O:** Uses **epoll/kqueue** (multiplexed I/O) to handle thousands of connections efficiently.

### How Redis’s Event-Driven Architecture Works?

1. **Single-Threaded Execution**
    - Redis runs a single-threaded event loop, avoiding thread management overhead.
    - This ensures **atomic execution** of commands, preventing race conditions.
2. **Multiplexed I/O Handling**
    - Uses **epoll (Linux), kqueue (BSD/macOS), select/poll (older systems)** for **non-blocking, asynchronous I/O**.
    - Can handle thousands of connections without thread context switching.
3. **Event Loop Mechanism**
    - Events are queued and processed one at a time:
        - **File Events:** Client requests (e.g., SET, GET) handled via TCP sockets.
        - **Time Events:** Periodic tasks (e.g., persistence, memory eviction).
4. **Pub/Sub Messaging:** Redis uses a **publish-subscribe model** for real-time messaging and event broadcasting.

### Why & How is Redis Fast?

Redis achieves **sub-millisecond response times** due to the following factors:

1. **In-Memory Storage (RAM-based)**
    - Unlike disk-based databases, Redis keeps data in RAM, eliminating disk I/O latency.
    - Example: Retrieving a key from RAM (~100ns) vs. disk (~10ms).
2. **Optimized Data Structures**
    - Redis uses **efficient algorithms** (e.g., Hash Tables, Skip Lists) to minimize lookup time.
    - Example: Hash lookups in Redis are **O(1)**, making key-value retrievals extremely fast.
3. **Single-Threaded Execution (No Context Switching)**
    - Avoids CPU overhead caused by context switching and thread management.
    - Each command is processed **atomically**, ensuring consistency.
4. **Asynchronous I/O with Event Loop:** Uses **epoll (Linux) or kqueue (BSD/macOS)** for handling thousands of client connections **without blocking**.
5. **Pipelining (Batch Execution):** Allows sending multiple commands in one request, reducing network latency.
6. **Efficient Memory Management (Mempool & Copy-On-Write)**
    - Redis uses a **memory allocator (jemalloc)** to minimize fragmentation.
    - For persistence (AOF & RDB), it uses **Copy-On-Write (COW)** to avoid blocking writes.

### Does Single-Threading Limit Redis Performance?

Not really! Redis can handle **millions of operations per second** on a single CPU core.

However, for very high workloads, Redis offers:

1. **Multi-threaded I/O in Redis 6.0+** – Improves network request handling.
2. **Sharding with Redis Cluster** – Distributes data across multiple nodes.

# Redis Data Structures

Redis provides **highly optimized** data structures that enable fast and efficient operations. Each structure is designed for specific **use cases** to maximize performance.

## Strings (Binary-Safe)

### Description

- The simplest data type, storing **text or binary data** (up to 512MB).
- Supports atomic operations like **increment/decrement**, **append**, and **substring retrieval**.

### Commands

```bash
SET mykey "Hello, Redis!"  # Store a key-value pair
GET mykey # Retrieve the value of a key
DEL mykey # Delete a key
EXISTS mykey # Check if a key exists. Returns 1 - key exists, 0 - doesn't

SET counter 10
INCR counter # Increment the integer value of a key by 1
DECR counter # Decrement the integer value of a key by 1
INCRBY counter 5 # Increment the integer value of a key by a specified amount
DECRBY counter 3 # Decrement the integer value of a key by a specified amount

SET mykey "Hello"
APPEND mykey ", Redis!" # Append a value to an existing key
STRLEN mykey # Get the length of the value stored at a key

# Set multiple key-value pairs in one command.
MSET key1 "value1" key2 "value2" key3 "value3"
# Get the values of multiple keys
MGET key1 key2 key3

# Set a key with a value and an expiration time (in seconds)
SETEX mykey 10 "Hello"
TTL mykey # Get the time-to-live (TTL) of a key

SET mykey "Hello"
GETSET mykey "World" # Set a new value for a key and return the old value

# Set the value of a key only if it does not already exist
SETNX mykey "Hello"
# Set multiple keys only if none of them exist
MSETNX key1 "value1" key2 "value2"

# Get a substring of the value stored at a key
SET mykey "Hello, Redis!"
GETRANGE mykey 0 4 # Returns "Hello"

# Overwrite part of a string at a key
SET mykey "Hello, Redis!"
SETRANGE mykey 7 "World" # The value of mykey becomes "Hello, World!"
```

## Use Cases Using Redis Strings (Binary-Safe)

### Caching

Redis strings are widely used for caching data to reduce the load on primary databases or expensive computations.

**Example**:

- Cache the results of database queries or API calls.
- Store HTML fragments or rendered pages for quick retrieval.

```bash
SET user:123:profile "<html>...<html>"
EXPIRE user:123:profile 3600  # Cache for 1 hour
```

### Session Storage

Redis strings can store user session data for web applications, enabling fast access and scalability.

**Example**: Store session data like user ID, preferences, or authentication tokens.

```bash
SET session:abc123 "{userId: 123, loggedIn: true}"
EXPIRE session:abc123 1800  # Session expires after 30 minutes
```

### Counters

Redis strings are ideal for implementing counters, such as tracking page views, likes, or votes.

**Example**: Increment a counter for page views or user actions.

```bash
INCR page:home:views
INCRBY user:123:likes 5
```

### Rate Limiting

Redis strings can be used to implement rate-limiting mechanisms to control the number of requests a user can make within a specific time window.

**Example**: Track the number of requests from a user and block them if they exceed the limit.

```bash
INCR user:123:requests
EXPIRE user:123:requests 60  # Reset counter after 60 seconds
```

### Feature Flags

Redis strings can store feature flags or configuration settings that control the behavior of an application.

**Example**: Enable or disable features dynamically.

```bash
SET feature:new_design "enabled"
GET feature:new_design
```

### Storing Simple Key-Value Data

Redis strings are perfect for storing simple key-value pairs, such as user preferences, settings, or metadata.

**Example**: Store user preferences or application settings.

```bash
SET user:123:theme "dark"
SET app:config:max_users "1000"
```

### Real-Time Analytics

Redis strings can be used to track real-time metrics, such as the number of active users, clicks, or events.

**Example**: Track the number of active users on a website.

```bash
INCR active_users
DECR active_users  # When a user leaves
```

### Distributed Locking

Redis strings can be used to implement distributed locks, ensuring that only one process can access a resource at a time.

**Example**: Acquire a lock using `SETNX` (Set if Not Exists).

```bash
SETNX lock:resource1 "locked"
EXPIRE lock:resource1 10  # Lock expires after 10 seconds
```

## Lists (Ordered Collection)

### Description

- A **linked list** of ordered elements.
- Can act as a **queue (FIFO)** or **stack (LIFO)**.
- Supports operations like **pushing, popping, trimming, and range queries**.

### Commands

```bash
# Insert one or more elements at the head (left) of the list
LPUSH mylist "world"
LPUSH mylist "hello"

# Insert one or more elements at the tail (right) of the list
RPUSH mylist "redis"
RPUSH mylist "rocks"

RPOP mylist # Remove and return the first element (head) of the list
RPOP mylist # Remove and return the last element (tail) of the list

LLEN mylist # Get the length of the list
LRANGE mylist 0 3 #  Get a range of elements from the list
LINDEX mylist 0 # Get an element by its index in the list
LSET mylist 1 "Redis" # Set the value of an element at a specific index

# Insert an element before or after a specific value in the list
LINSERT mylist BEFORE "Redis" "awesome"
# Remove elements from the list
LREM mylist 1 "awesome" # Removes the first occurrence of "awesome" from the list

LTRIM mylist 0 1 # Trim the list to a specific range of elements
# Remove the last element from one list and push it to another
RPOPLPUSH mylist myotherlist # Moves the last element of mylist to the head of myotherlist

LPUSHX mylist "new" # Push an element to a list only if the list exists
# Move an element from one list to another (introduced in Redis 6.2)
LMOVE mylist myotherlist LEFT RIGHT # oves the first element of mylist to the end of myotherlist
```

## Use Cases Using Redis Lists (Ordered Collection)

### Message Queues

Redis Lists are often used to implement message queues, where producers push messages to the list and consumers pop messages from the list.

**Example: Simple Message Queue**

- **Producer** pushes messages to the list:
    
    ```bash
    LPUSH myqueue "message1"
    LPUSH myqueue "message2"
    ```
    
- **Consumer** processes messages:
    
    ```bash
    RPOP myqueue
    ```
    
    - Returns `"message1"` (the oldest message).

### Activity Feeds

Redis Lists can store user activity feeds, such as recent actions or notifications, in chronological order.

**Example: User Activity Feed**

- Add user activities to the list:
    
    ```bash
    LPUSH user:123:activity "Logged in"
    LPUSH user:123:activity "Viewed profile"
    LPUSH user:123:activity "Posted a comment"
    ```
    
- Retrieve the latest activities:
    
    ```bash
    LRANGE user:123:activity 0 2
    ```
    
    - Output: `["Posted a comment", "Viewed profile", "Logged in"]`.

### Job Scheduling

Redis Lists can be used to manage a list of jobs or tasks that need to be processed by workers.

**Example: Job Queue**

- Add jobs to the queue:
    
    ```bash
    LPUSH jobqueue "job1"
    LPUSH jobqueue "job2"
    ```
    
- Workers process jobs:
    
    ```bash
    RPOP jobqueue
    ```
    
    - Returns `"job1"` (the oldest job).

### Recent Items

Redis Lists can track the most recent items, such as recently viewed products or recently searched terms.

**Example: Recently Viewed Products**

- Add products to the list:
    
    ```bash
    LPUSH user:123:recent_products "product123"
    LPUSH user:123:recent_products "product456"
    LPUSH user:123:recent_products "product789"
    ```
    
- Retrieve the last 5 viewed products:
    
    ```bash
    LRANGE user:123:recent_products 0 4
    ```
    
    - Output: `["product789", "product456", "product123"]`.

### Leaderboard History

Redis Lists can store historical data, such as changes in a leaderboard over time.

**Example: Leaderboard History**

- Add leaderboard snapshots:
    
    ```bash
    LPUSH leaderboard_history "2023-10-01: UserA=100, UserB=90"
    LPUSH leaderboard_history "2023-10-02: UserA=110, UserB=95"
    ```
    
- Retrieve the latest snapshot:
    
    ```bash
    LINDEX leaderboard_history 0
    ```
    
    - Output: `"2023-10-02: UserA=110, UserB=95"`.

## Sets (Unique Collection, Unordered)

### Description

- A **collection of unique values** (no duplicates).
- Supports **fast membership checks** and **set operations** (union, intersection, difference).

### Commands

```bash
# Add one or more members to a set
SADD myset "apple"
SADD myset "banana" "cherry"

SMEMBERS myset # Get all members of a set
SREM myset "banana" # Remove one or more members from a set
SISMEMBER myset "apple" # Check if a member exists in a set
SCARD myset # Get the number of members in a set
SPOP myset # Remove and return one or more random members from a set
SRANDMEMBER myset 2 # Get one or more random members from a set without removing them

# Get the intersection of multiple sets.
SADD set1 "apple" "banana" "cherry"
SADD set2 "banana" "cherry" "date"
SINTER set1 set2 # Returns the common elements: ["banana", "cherry"]
# Get the union of multiple sets
SUNION set1 set2 # Returns all unique elements from both sets: ["apple", "banana", "cherry", "date"]
# Get the difference between sets (elements in the first set that are not in the others)
SDIFF set1 set2 # Returns elements in set1 but not in set2: ["apple"]
# Store the intersection of multiple sets in a new set
SINTERSTORE set3 set1 set2 # Stores the intersection of set1 and set2 in set3
# Store the difference between sets in a new set
SDIFFSTORE set5 set1 set2 # Stores the difference of set1 and set2 in set5

# Move a member from one set to another.
SMOVE set1 set2 "apple" # Moves "apple" from set1 to set2
# Incrementally iterate over members of a set
SSCAN myset 0 MATCH "a*" # Returns members of myset that match the pattern "a*"
```

## Use Cases Using Sets (Unique Collection, Unordered)

### Intersection and Union Operations

Redis Sets support operations like intersection, union, and difference, which are useful for finding common or combined elements.

**Example: Find Common Interests Between Users**

- Store interests of two users:
    
    ```bash
    SADD user:123:interests "music" "movies" "sports"
    SADD user:456:interests "movies" "books" "sports"
    ```
    
- Find common interests:
    
    ```bash
    SINTER user:123:interests user:456:interests
    ```
    
    - Output: `["movies", "sports"]`.

**Example: Combine Interests of Two Users**

- Combine interests:
    
    ```bash
    SUNION user:123:interests user:456:interests
    ```
    
    - Output: `["music", "movies", "sports", "books"]`.

### Random Selection

Redis Sets can be used to pick random elements, which is useful for giveaways, recommendations, or random sampling.

**Example: Randomly Select a Winner**

- Add participants to a set:
    
    ```bash
    SADD giveaway:participants "user:123"
    SADD giveaway:participants "user:456"
    SADD giveaway:participants "user:789"
    ```
    
- Select a random winner:
    
    ```bash
    SRANDMEMBER giveaway:participants
    ```
    

### Blacklists or Blocklists

Redis Sets can be used to store blacklisted or blocked items, such as IP addresses or users.

**Example: Blocklist IP Addresses**

- Add IP addresses to a blocklist:
    
    ```bash
    SADD blocklist:ips "192.168.1.1"
    SADD blocklist:ips "192.168.1.2"
    ```
    
- Check if an IP is blocked:
    
    ```bash
    SISMEMBER blocklist:ips "192.168.1.1"
    ```
    

### Unique Event Tracking

Redis Sets can track unique events, such as unique visitors or unique actions.

**Example: Track Unique Visitors to a Website**

- Add unique visitors:
    
    ```bash
    SADD website:visitors "user:123"
    SADD website:visitors "user:456"
    ```
    
- Count unique visitors:
    
    ```bash
    SCARD website:visitors
    ```
    

### Tagging Systems

Redis Sets can be used to implement tagging systems for articles, products, or posts.

**Example: Tag Articles**

- Tag an article with multiple tags:
    
    ```bash
    SADD article:456:tags "technology"
    SADD article:456:tags "programming"
    ```
    
- Find all articles with a specific tag:
    
    ```bash
    SADD tag:technology:articles "article:456"
    SADD tag:programming:articles "article:456"
    ```
    
- Retrieve articles tagged with "technology":
    
    ```bash
    SMEMBERS tag:technology:articles
    ```
    

### Friend Lists or Followers

Redis Sets can store relationships like friends or followers in a social network.

**Example: Store Friends of a User**

- Add friends for a user:
    
    ```bash
    SADD user:123:friends "user:456"
    SADD user:123:friends "user:789"
    ```
    
- Check if two users are friends:
    
    ```bash
    SISMEMBER user:123:friends "user:456"
    ```
    
- Get all friends of a user:
    
    ```bash
    SMEMBERS user:123:friends
    ```
    

## Hashes (Key-Value Store within Redis)

### Description

- A **mini-database** inside Redis, allowing multiple key-value pairs under a single key.
- Useful for **storing objects efficiently**.

### Commands

```bash
# Set the value of a field in a hash.
HSET user:123 name "John"
HSET user:123 age 30 # Creates a hash user:123 with fields name and age

# Get the value of a field in a hash
HGET user:123 name # Returns "John"

# Set multiple field-value pairs in a hash (deprecated in Redis 4.0.0, use HSET instead)
HSET user:123 name "John" age 30 email "john@example.com"
# Get the values of multiple fields in a hash
HMGET user:123 name age # Returns ["John", "30"]

# Get all field-value pairs in a hash
HGETALL user:123 # Returns all fields and values: ["name", "John", "age", "30", "email", "john@example.com"].

# Delete one or more fields from a hash
HDEL user:123 email # Removes the email field from the hash

# Check if a field exists in a hash
HEXISTS user:123 name # Returns 1 if the field exists, 0 otherwise

# Get all fields in a hash.
HVALS user:123 # Returns all values: ["John", "30"]

# Get the number of fields in a hash
HLEN user:123 # Returns the number of fields in the hash

# Increment the integer value of a field by a specified amount.
HSET user:123 balance 100.50
HINCRBYFLOAT user:123 balance 25.75 # The value of balance becomes 126.25.

# Set the value of a field only if it does not already exist.
HSETNX user:123 name "Jane" # If the name field does not exist, it sets the value to "Jane". Otherwise, it does nothing

# Incrementally iterate over fields and values in a hash.
HSCAN user:123 0 MATCH "n*" # Returns fields and values matching the pattern "n*"
```

## Use Cases Using Hashes (Key-Value Store within Redis)

### Storing User Profiles

Redis Hashes are perfect for storing user profiles, where each field represents a property of the user.

**Example: Store User Profile**

- Add user details to a hash:
    
    ```bash
    HSET user:123 name "John Doe"
    HSET user:123 age 30
    HSET user:123 email "john@example.com"
    ```
    
- Retrieve user details:
    
    ```bash
    HGETALL user:123
    ```
    
    - Output: `["name", "John Doe", "age", "30", "email", "john@example.com"]`.

### Product Catalog

Redis Hashes can be used to store product details, such as name, price, and description.

**Example: Store Product Details**

- Add product details to a hash:
    
    ```bash
    HSET product:456 name "Smartphone"
    HSET product:456 price 599.99
    HSET product:456 description "Latest model with 128GB storage"
    ```
    
- Retrieve product details:
    
    ```bash
    HGETALL product:456
    ```
    
    - Output: `["name", "Smartphone", "price", "599.99", "description", "Latest model with 128GB storage"]`.

### Configuration Management

Redis Hashes can store configuration settings for applications or systems.

**Example: Store Application Configuration**

- Add configuration settings to a hash:
    
    ```bash
    HSET app:config max_users 1000
    HSET app:config theme "dark"
    HSET app:config timeout 30
    ```
    
- Retrieve configuration settings:
    
    ```bash
    HGETALL app:config
    ```
    

### Counters

Redis Hashes can be used to track counters for specific fields.

**Example: Track User Activity Counters**

- Add counters to a hash:
    
    ```bash
    HSET user:123:counters logins 10
    HSET user:123:counters posts 5
    HSET user:123:counters likes 20
    ```
    
- Increment a counter:
    
    ```bash
    HINCRBY user:123:counters logins 1
    ```
    
- Retrieve counters:
    
    ```bash
    HGETALL user:123:counters
    ```
    

### Shopping Cart

Redis Hashes can be used to implement a shopping cart, where each field represents a product and its quantity.

**Example: Store Shopping Cart**

- Add items to a shopping cart:
    
    ```bash
    HSET cart:123 product:456 2
    HSET cart:123 product:789 1
    ```
    
- Update item quantity:
    
    ```bash
    HINCRBY cart:123 product:456 1
    ```
    
- Retrieve cart contents:
    
    ```bash
    HGETALL cart:123
    ```
    

### Game Leaderboards

Redis Hashes can store player scores and other game-related data.

**Example: Store Player Scores**

- Add player scores to a hash:
    
    ```bash
    HSET game:leaderboard player:123 1000
    HSET game:leaderboard player:456 1500
    HSET game:leaderboard player:789 1200
    ```
    
- Update player score:
    
    ```bash
    HINCRBY game:leaderboard player:123 100
    ```
    
- Retrieve leaderboard:
    
    ```bash
    HGETALL game:leaderboard
    ```
    

## Sorted Sets (Ordered Collection with Scores)

### Description

- Like **Sets**, but each element has a **score** that defines its order.
- Enables **ranking and range queries**.

### Commands

```bash
# Add one or more members to a sorted set, or update their scores if they already exist.
ZADD leaderboard 100 "Alice"
ZADD leaderboard 200 "Bob" 300 "Charlie"

# Get a range of members from a sorted set by index (sorted by score in ascending order)
ZRANGE leaderboard 0 -1 # Returns all members in ascending order: ["Alice", "Bob", "Charlie"]
# Get a range of members from a sorted set by index (sorted by score in descending order).
ZREVRANGE leaderboard 0 -1 # Returns all members in descending order: ["Charlie", "Bob", "Alice"]

# Get members within a specific score range (ascending order)
ZRANGEBYSCORE leaderboard 100 200 # Returns members with scores between 100 and 200: ["Alice", "Bob"]
# Get members within a specific score range (descending order)
ZREVRANGEBYSCORE leaderboard 200 100 # Returns members with scores between 200 and 100: ["Bob", "Alice"]

# Get the score of a member in a sorted set
ZSCORE leaderboard "Alice"
# Get the rank (index) of a member in ascending order
ZRANK leaderboard "Bob" # Returns the rank of "Bob": 1 (0-based index)
# Get the rank (index) of a member in descending order
ZREVRANK leaderboard "Bob" # Returns the rank of "Bob": 1 (0-based index)

# Remove one or more members from a sorted set
ZREM leaderboard "Alice"
# Get the number of members in a sorted set
ZCARD leaderboard

# Count the number of members within a specific score range
ZCOUNT leaderboard 100 200 # Returns the number of members with scores between 100 and 200

# Increment the score of a member in a sorted set.
ZINCRBY leaderboard 50 "Alice"
# Remove and return the member with the highest score
ZPOPMAX leaderboard # Removes and returns "Charlie" with a score of 300
# Remove and return the member with the lowest score.
ZPOPMIN leaderboard # Removes and returns "Alice" with a score of 100

# Perform a union of multiple sorted sets and store the result in a new sorted set
ZADD set1 100 "Alice"
ZADD set2 200 "Bob"
ZUNIONSTORE result 2 set1 set2 # Stores the union of set1 and set2 in result

# Perform an intersection of multiple sorted sets and store the result in a new sorted set.
ZADD set1 100 "Alice"
ZADD set2 100 "Alice"
ZINTERSTORE result 2 set1 set2 # Stores the intersection of set1 and set2 in result

# Incrementally iterate over members of a sorted set.
ZSCAN leaderboard 0 MATCH "A*" # Returns members matching the pattern "A*"

```

## Use Cases Using Redis Sorted Sets

### Leaderboards

Redis Sorted Sets are perfect for implementing leaderboards, where users or items are ranked by their scores.

**Example: Game Leaderboard**

- Add players with their scores:
    
    ```bash
    ZADD leaderboard 1000 "Alice"
    ZADD leaderboard 1500 "Bob"
    ZADD leaderboard 1200 "Charlie"
    ```
    
- Retrieve the top 3 players:
    
    ```bash
    ZREVRANGE leaderboard 0 2 WITHSCORES
    ```
    
    - Output: `["Bob", "1500", "Charlie", "1200", "Alice", "1000"]`.
- Increment a player's score:
    
    ```bash
    ZINCRBY leaderboard 200 "Alice"
    ```
    
    - Alice's new score is `1200`

### Priority Queues

Redis Sorted Sets can be used to implement priority queues, where tasks are processed based on their priority scores.

**Example: Task Queue**

- Add tasks with their priorities:
    
    ```bash
    ZADD tasks 1 "Send emails"
    ZADD tasks 3 "Generate report"
    ZADD tasks 2 "Update database"
    ```
    
- Process the highest-priority task:
    
    ```bash
    ZPOPMAX tasks
    ```
    
    - Removes and returns `"Generate report"` with a score of `3`.

### Time-Series Data

Redis Sorted Sets can store and query time-series data, where the score represents a timestamp.

**Example: Store Event Timestamps**

- Add events with timestamps:
    
    ```bash
    ZADD events 1633046400 "User logged in"
    ZADD events 1633047000 "User made a purchase"
    ZADD events 1633047600 "User logged out"
    ```
    
- Retrieve events within a time range:
    
    ```bash
    ZRANGEBYSCORE events 1633046400 1633047600
    ```
    
    - Output: `["User logged in", "User made a purchase", "User logged out"]`.

### Geospatial Indexing

Redis Sorted Sets can be used to store and query geospatial data, where the score represents a geohash.

**Example: Nearby Locations**

- Add locations with their geohash scores:
    
    ```bash
    ZADD locations 3471579339407378 "New York"
    ZADD locations 3471579339407379 "Los Angeles"
    ZADD locations 3471579339407380 "Chicago"
    ```
    
- Retrieve locations within a geohash range:
    
    ```bash
    ZRANGEBYSCORE locations 3471579339407378 3471579339407380
    ```
    
    - Output: `["New York", "Los Angeles", "Chicago"]`.

## Summary: When to Use Which Data Structure?

| Data Structure | Best Use Cases |
| --- | --- |
| **Strings** | Caching, session storage, counters |
| **Lists** | Queues, task processing, recent history |
| **Sets** | Unique users, tags, social relations |
| **Hashes** | Storing objects (user profiles, metadata) |
| **Sorted Sets** | Leaderboards, ranking, scheduling |
| **Bitmaps** | Feature flags, activity tracking |
| **HyperLogLog** | Unique counts (web traffic, hashtags) |
| **Streams** | Event logs, real-time processing |

# Redis Replication

### What is Redis Replication?

Redis **replication** is a process where data is **copied from a primary (master) node** to one or more **replica (slave) nodes**. It is primarily used for:

- **High availability**: Ensures data redundancy in case of failure.
- **Read scalability**: Distributes read operations across multiple nodes.
- **Fault tolerance**: Helps recover data if the master crashes.

### How Redis Replication Works

- A **single master node** handles all **write operations**.
- **One or more replica nodes** maintain an **exact copy** of the master’s data.
- Replicas can **serve read requests**, reducing the master’s workload.
- Replication is **asynchronous** by default, meaning there might be a slight delay between the master and replicas.

### How Replication Works Internally?

**1. Initial Synchronization (Full Sync)**

When a replica connects to a master for the first time, the following occurs:

1. **Replica sends a request** to the master to start synchronization.
2. The master takes a **snapshot** of the dataset and saves it as an **RDB (Redis Database) file**.
3. The snapshot is sent to the replica, which **loads it into memory**.
4. Meanwhile, the master also sends **new write operations** to ensure the replica remains up-to-date.

This ensures the replica has an exact copy of the master’s data.

**2. Ongoing Synchronization (Partial Sync)**

Once the initial sync is complete, the master:

- Continuously sends **new commands (write operations)** to the replica.
- Ensures that **replica data remains up-to-date** with minimal lag.

If a replica **disconnects and reconnects**, Redis uses the **Replication Backlog** to send only the **missing updates**, avoiding a full resynchronization.

### Read-Only Mode in Replicas

By default, Redis **replicas are read-only** to prevent accidental data modifications.

- Replicas can accept **read requests**, but they **reject write requests**.
- This behavior prevents **data inconsistency** between master and replicas.

### Failover & Handling Master Failures

**Scenario: What Happens If the Master Fails?**

- By default, if the master crashes, **replicas remain operational** but cannot accept writes.
- A **manual intervention** is required to promote a replica to a new master.
- Clients need to be reconfigured to connect to the new master.

To automate failover, **Redis Sentinel** is used (explained later).

### Replication Limitations

1. **Asynchronous Replication Delay** – Replicas may lag behind the master.
2. **No Automatic Failover by Default** – Requires Redis Sentinel for automation.
3. **Master as Single Point of Failure** – If the master crashes, a manual failover is needed.
4. **Increased Memory Usage** – Replicas consume additional memory, as they maintain full copies of the dataset.

### What is Redis Sentinel?

Redis Sentinel is a **monitoring and failover system** that provides:

- **Automatic failover** if the master node fails.
- **Master election** to promote a replica as the new master.
- **Monitoring** to check the health of Redis instances.
- **Notification system** for system administrators.

Sentinel **ensures high availability** of Redis by automatically managing failures.

### Why is Redis Sentinel Needed?

Redis **replication alone is not enough** for high availability because:

❌ If the **master crashes**, replicas cannot become a master automatically.

❌ Clients need **manual reconfiguration** to connect to a new master.

✅ **Sentinel automates this process**, reducing downtime.

### Failover Scenarios

**Case 1: Master Fails**

1. Sentinel detects failure.
2. Sentinel promotes a new master from available replicas.
3. Sentinel reconfigures replicas to follow the new master.
4. Clients update automatically.

**Case 2: Replica Fails**

1. Sentinel detects failure but does **not promote a new replica** (no impact on master).
2. Clients can still read from the master or other replicas.

**Case 3: Sentinel Fails**

1. Other Sentinels continue monitoring Redis.
2. If a majority of Sentinels survive, the system remains operational.

### Comparison

**Replication vs. Clustering**

| Feature | Replication | Clustering |
| --- | --- | --- |
| **Purpose** | High availability, read scaling | Horizontal scaling, sharding |
| **Data Copying** | Full copy of master data | Data split across nodes |
| **Failover** | Manual or Sentinel-based | Automatic failover |
| **Read Scaling** | Yes (replicas handle reads) | Yes (but data is sharded) |

**Sentinel vs. Redis Clustering**

| Feature | Redis Sentinel | Redis Cluster |
| --- | --- | --- |
| **Purpose** | High availability & failover | Horizontal scaling & sharding |
| **Automatic Failover** | ✅ Yes | ✅ Yes |
| **Data Partitioning** | ❌ No (Full copy of data) | ✅ Yes (Data sharded) |
| **Replication** | ✅ Yes (Master-Replica) | ✅ Yes (Sharded replication) |
| **Complexity** | Simple | More complex |

# Redis Clustering – Distributed Scaling

### What is Redis Cluster?

Redis **Cluster** is a distributed Redis setup that:

- **Splits data across multiple nodes (sharding)**
- **Provides high availability** with automatic failover
- **Eliminates the need for a single master node**
- **Supports parallel processing** for high-performance workloads

Unlike standalone Redis, which stores all data on a **single machine**, Redis Cluster **spreads the dataset across multiple nodes**, allowing **horizontal scaling**.

### Why Do We Need Redis Cluster?

Standalone Redis works well, but it has **limitations**:

❌ **Memory Limitations** – A single node can only store as much data as its RAM allows.

❌ **Single Point of Failure** – If the master crashes, all writes stop (unless Sentinel is used).

❌ **Limited Scalability** – Read scaling is possible with replicas, but write scaling is not.

✅ **Redis Cluster solves these issues by distributing data across multiple nodes** while ensuring **high availability**.

### Key Features of Redis Cluster

**1. Sharding (Data Partitioning)**

- Redis Cluster **splits the dataset across multiple nodes**.
- **Each node stores only a portion of the total data**.
- No need for manual partitioning – Redis **automatically assigns data** to different nodes.

**2. Automatic Failover**

- If a master node fails, **a replica is automatically promoted to master**.
- No manual intervention is needed, unlike Redis Sentinel.

**3. Horizontal Scaling**

- More nodes can be added dynamically to **increase capacity**.
- Redis Cluster automatically **rebalances data** across new nodes.

**4. High Availability**

- Each **master node has at least one replica** for fault tolerance.
- If a master crashes, its **replica takes over** automatically.

### How Redis Cluster Works?

**1. Cluster Nodes: Masters & Replicas**

- **Master Nodes** – Store the actual data.
- **Replica Nodes** – Act as backups for masters (for failover).

Example:

If we have **6 nodes**, we can configure:

- **3 master nodes**
- **3 replicas** (one per master for failover)

**2. Slot-Based Data Partitioning**

- Redis Cluster divides data into **16,384 slots**.
- Each **key is assigned to a slot** based on **hashing**.
- **Each master node is responsible for a subset of slots**.

**Example:**

- **Master 1** handles slots **0 – 5,000**
- **Master 2** handles slots **5,001 – 10,000**
- **Master 3** handles slots **10,001 – 16,384**

### How Does Redis Cluster Route Requests?

**1. Clients Use Hashing to Find the Right Node**

- Redis Cluster uses **CRC16 hashing** to assign a **slot** to a key.
- The client sends the request to the **correct master** based on the key’s hash slot.

**2. Redirects Using MOVED and ASK Commands**

If a client contacts the **wrong node**, Redis returns a **MOVED** response with the correct node’s address.

- The client then re-sends the request to the correct node.

This ensures efficient data lookups across a distributed Redis setup.

### Failover & Recovery in Redis Cluster

**1. What Happens If a Master Fails?**

✅ A **replica is promoted to master** automatically.

✅ Other nodes in the cluster **update their metadata**.

✅ Clients are notified about the **new master**.

**2. What Happens If a Replica Fails?**

✅ No impact on the cluster unless the master also fails.

✅ The failed replica can be **replaced manually** or **automatically rebalanced**.

### Redis Cluster vs. Redis Sentinel

| Feature | Redis Cluster | Redis Sentinel |
| --- | --- | --- |
| **Sharding** | ✅ Yes (Data partitioning) | ❌ No (All nodes store full data) |
| **Horizontal Scaling** | ✅ Yes | ❌ No |
| **Automatic Failover** | ✅ Yes | ✅ Yes |
| **High Availability** | ✅ Yes | ✅ Yes |
| **Client Complexity** | 🔸 Needs cluster-aware clients | 🔹 Simple clients work fine |

👉 **Use Redis Sentinel if you need high availability without sharding.**

👉 **Use Redis Cluster if you need both high availability & horizontal scaling.**

### Limitations of Redis Cluster

1. **Not All Commands Are Supported** – Multi-key operations (e.g., `MGET`, `MSET`) must involve keys in the same hash slot.
2. **Asynchronous Replication** – Some data loss is possible during failover.
3. **More Complex Setup** – Requires **at least 6 nodes** for full HA (3 masters + 3 replicas).

# Redis Persistence – RDB & AOF for Data Durability

### Why Does Redis Need Persistence?

Redis is **an in-memory database**, meaning all data is stored in RAM. This makes it **super fast** but comes with risks:

❌ If Redis crashes, **all data is lost** unless it's persisted to disk.

✅ Redis offers **two persistence mechanisms** to ensure data durability:

1. **RDB (Redis Database Backup)** – Periodic snapshots.
2. **AOF (Append-Only File)** – Logs every write operation.

### RDB (Redis Database Backup) – Snapshot Persistence

**How RDB Works?**

- Takes **binary snapshots** of the database **at intervals** and saves them to disk.
- The snapshot is a compact `.rdb` file.
- Redis can **reload the snapshot** during restart.

**When to Use RDB?**

✅ **Faster restarts** – Reloading an RDB file is quicker than processing logs.

✅ **Efficient for backups** – Small, compressed file size.

✅ **Good for recovery** – Useful for full database restores.

**Downsides of RDB**

1. **Risk of Data Loss** – Since snapshots are periodic, recent writes may be lost if Redis crashes before the next snapshot.
2. **High CPU Usage During Snapshot** – Can impact performance when saving large datasets.

### AOF (Append-Only File) – Operation Logging

**How AOF Works?**

- Logs **every write operation** (`SET`, `DEL`, etc.) to a file (`appendonly.aof`).
- When Redis restarts, it **replays the AOF log** to reconstruct the dataset.

**When to Use AOF?**

✅ **Better durability** – Stores every change, so less data loss.

✅ **Safer for critical applications** – Ensures no lost writes after a crash.

**Downsides of AOF**

1. **Larger file size** – AOF grows over time (though Redis supports log rewriting).
2. **Slower than RDB** – Writing every operation to disk adds overhead.

### RDB vs AOF – Key Differences

| Feature | RDB (Snapshot) | AOF (Operation Log) |
| --- | --- | --- |
| **Speed** | ⚡ Faster on recovery | 🐢 Slower on recovery |
| **Durability** | ❌ Data loss possible | ✅ Better durability |
| **File Size** | 📦 Smaller (compressed) | 📜 Larger (logs all ops) |
| **Performance Impact** | 🏎️ Low (periodic saves) | 🚶 Higher (writes every op) |
| **Use Case** | ✅ Best for backups & fast recovery | ✅ Best for critical, durable writes |

**Which one should you use?**

1. **Use RDB** if you want **faster recovery & efficient backups**.
2. **Use AOF** if you need **higher durability & less data loss**.
3. **Use Both!** – Redis allows **RDB + AOF for best of both worlds**.

# Distributed Locking in Redis – Handling Race Conditions

### Why Do We Need a Distributed Lock?

In a distributed system, multiple processes may try to access the **same resource** simultaneously. This can lead to:

❌ **Race conditions** – Two processes modify data at the same time.

❌ **Inconsistent state** – Database integrity issues.

✅ A **distributed lock** ensures that only **one process modifies a resource at a time**.

Redis provides **efficient distributed locking** using **SETNX**, **Redlock**, and **TTL-based locks**.

### Simple Redis Lock Using SETNX

**How It Works?**

- `SETNX key value` (SET if Not Exists) creates a lock **only if it doesn’t exist**.
- If the lock exists, another process **cannot acquire it**.
- **EXPIRE sets a TTL**, preventing deadlocks if a process crashes.

**Implementation Example**

```bash
SETNX lock:resource 1  # Try to acquire lock
EXPIRE lock:resource 10  # Set expiration (avoid deadlock)
```

✅ **Only the first process succeeds. Others must retry later.**

**Releasing the Lock**

```bash
DEL lock:resource  # Release the lock when done
```

### Limitations of SETNX

1. **Not Reliable** – If the process crashes **before setting EXPIRE**, the lock stays forever.
2. **No Safety Against Expiration** – If the lock expires too soon, another process may acquire it while the first one is still working.

### Safer Locking with SET + EX (Atomic Locking)

Redis 2.8+ provides a better command:

```bash
SET lock:resource 1 EX 10 NX  # Set lock with 10s TTL (Atomic)
```

- `NX` – Ensures the lock is **only set if it doesn’t exist**.
- `EX 10` – Sets expiration time of **10 seconds** (prevents deadlocks).

**Implementation in Python (Using `redis-py`)**

```python
import redis

r = redis.Redis()

lock_acquired = r.set("lock:resource", "1", ex=10, nx=True)

if lock_acquired:
    try:
        # Perform critical operation
        print("Lock acquired, doing work...")
    finally:
        r.delete("lock:resource")  # Release the lock
else:
    print("Lock already held, try later.")

```

✅ **Atomic and safer than SETNX + EXPIRE**.

✅ **Avoids race conditions and deadlocks**.

### Redlock Algorithm – The Distributed Locking Solution

**Why SETNX Alone is Not Enough?**

- Works for **single Redis node** but **fails in distributed setups**.
- **If Redis crashes**, locks are lost.

**Redlock: A Reliable Distributed Locking Algorithm**

Redlock works by **acquiring a lock on multiple Redis nodes** to ensure consistency:

1. The client **tries to set the lock** on **at least 3 Redis instances**.
2. If a majority (at least 2 of 3) grant the lock, the client **considers the lock acquired**.
3. The client **sets an expiration** to avoid deadlocks.
4. The lock is **released from all nodes** when the process is done.

**Redlock Implementation (Python)**

```python
from redis import Redis
from redis.lock import Lock
from redis.exceptions import LockError
import time

# Connect to multiple Redis nodes
nodes = [Redis(host="redis1"), Redis(host="redis2"), Redis(host="redis3")]

# Try acquiring lock across nodes
lock = Lock(nodes[0], "resource_lock", timeout=10)

try:
    if lock.acquire(blocking=False):
        print("Lock acquired!")
        time.sleep(5)  # Perform critical work
    else:
        print("Could not acquire lock.")
finally:
    lock.release()
```

✅ Ensures **high availability & consistency**.

✅ Prevents **split-brain scenarios**.

### When to Use Which Locking Mechanism?

| Locking Method | Best For | Pros | Cons |
| --- | --- | --- | --- |
| **SETNX + EXPIRE** | Simple local locks | Fast, minimal setup | Risk of deadlocks if EXPIRE fails |
| **SET + EX + NX** | Safe atomic locks | Prevents deadlocks | Single-node only |
| **Redlock Algorithm** | Distributed locks (multi-node) | Reliable, prevents race conditions | More complex setup |
1. **SETNX + EXPIRE** is the simplest locking method but not foolproof.
2. **SET + EX + NX** is a **better atomic locking** solution.
3. **Redlock Algorithm** is the most **robust distributed locking** mechanism for multi-node Redis.

# Hot-Key Problem in Redis – Identification & Mitigation

### What is a Hot Key in Redis?

A **hot key** is a **single key that receives an extremely high number of requests**, leading to:

❌ **CPU & Memory bottlenecks** – Overloading a single Redis node.

❌ **Increased latency** – Redis struggles to serve repeated requests.

❌ **Possible failures** – If Redis can't handle the load, requests fail.

**Example:**

Imagine a **flash sale system** where the **product inventory key** (`product:123:stock`) is accessed by **millions of users at the same time**. This **one key** becomes a bottleneck.

### How to Identify Hot Keys?

**1. Monitor Slow Queries in Redis**

Run:

```bash
SLOWLOG GET 10
```

This shows **slow queries**, indicating potential hot keys.

**2. Use Redis Keyspace Stats**

```bash
INFO keyspace
```

Shows how frequently each database key is accessed.

**3. Use Redis MONITOR Command**

```bash
MONITOR
```

It logs **real-time queries**, helping spot **frequent key accesses**.

**4. Use Latency Monitoring**

```bash
LATENCY DOCTOR
```

Detects **high-latency** operations caused by hot keys.

**5. External Monitoring Tools**

1. **RedisInsight** (GUI monitoring).
2. **Grafana + Prometheus** (real-time analytics).
3. **AOF or Query Logging** (log-based analysis).

### Mitigating 1: Distribute Load with Key Sharding

Instead of **one key receiving all requests**, split the load across **multiple keys**.

**Example:** Instead of:

```bash
GET product:123:stock
```

Use **sharded keys**:

```bash
GET product:123:stock:1
GET product:123:stock:2
...
```

Then, distribute requests **randomly** or based on **user hashing**.

### Mitigating 2: Cache Replication with Read Replicas

- Add **Redis replicas** (`slaveof` or `replicaof`) to **distribute read requests**.
- Clients can **read from replicas** while writes go to the master.

✅ **Useful for read-heavy workloads** (e.g., leaderboards, inventory lookups).

### Mitigating 3: Use Local Caching (Layered Caching)

- **Store frequently accessed data in application memory (e.g., using Redis + Memcached).**
- **Use CDNs** (e.g., Cloudflare, Akamai) for caching **user-facing content**.
- **Use client-side caching** (e.g., browser cache for API responses).

✅ Reduces direct Redis hits by caching **at multiple layers**.

### Mitigating 4: Use a TTL (Time-To-Live) Strategy

- Set **short TTLs** (`EXPIRE`) for **frequently updated** keys.
- Use **randomized TTLs** to avoid cache stampedes.

Example:

```bash
SET product:123:stock 100 EX 60
```

✅ Prevents excessive load on a single key over long periods.

### Mitigating 5: Implement Request Coalescing (Batching Requests)

- Instead of **multiple users hitting Redis** for the same key, batch them.
- A **single request** fetches data, then **serves multiple users**.
- Use **message queues** (Kafka, RabbitMQ) for **bulk processing**.

### Which Strategy Should You Use?

| Scenario | Best Solution |
| --- | --- |
| **Heavy Reads** | Read Replicas / Layered Caching |
| **Flash Sales / Spikes** | Key Sharding / Request Coalescing |
| **Frequent Updates** | Write-Behind Cache / Randomized TTLs |
| **API Rate-Limited Data** | Client-Side Caching |
