# Redis Use Cases

- [Redis as a Cache](#redis-as-a-cache)
    - [How is Redis Used for Caching?](#how-is-redis-used-for-caching)
    - [Deep Dive into Redis Caching](#deep-dive-into-redis-caching)
    - [Pros of Using Redis for Caching](#pros-of-using-redis-for-caching)
    - [Cons of Using Redis for Caching](#cons-of-using-redis-for-caching)
    - [Real-Life Examples of Redis Caching](#real-life-examples-of-redis-caching)
- [Redis as Session Storage](#redis-as-session-storage)
    - [How is Redis Used for Session Storage?](#how-is-redis-used-for-session-storage)
    - [Deep Dive into Redis Session Storage](#deep-dive-into-redis-session-storage)
    - [Pros of Using Redis for Session Storage](#pros-of-using-redis-for-session-storage)
    - [Cons of Using Redis for Session Storage](#cons-of-using-redis-for-session-storage)
    - [Real-Life Examples of Redis for Session Storage](#real-life-examples-of-redis-for-session-storage)
- [Redis for Rate Limiting](#redis-for-rate-limiting)
    - [How is Redis Used for Rate Limiting?](#how-is-redis-used-for-rate-limiting)
    - [Deep Dive into Redis Rate Limiting](#deep-dive-into-redis-rate-limiting)
    - [Pros of Using Redis for Rate Limiting](#pros-of-using-redis-for-rate-limiting)
    - [Cons of Using Redis for Rate Limiting](#cons-of-using-redis-for-rate-limiting)
    - [Real-Life Examples of Redis Rate Limiting](#real-life-examples-of-redis-rate-limiting)
- [Redis for Leaderboards](#redis-for-leaderboards)
    - [How is Redis Used for Leaderboards?](#how-is-redis-used-for-leaderboards)
    - [Deep Dive into Redis Leaderboards](#deep-dive-into-redis-leaderboards)
    - [Advanced Leaderboard Use Cases](#advanced-leaderboard-use-cases)
    - [Pros of Using Redis for Leaderboards](#pros-of-using-redis-for-leaderboards)
    - [Cons of Using Redis for Leaderboards](#cons-of-using-redis-for-leaderboards)
    - [Real-Life Examples of Redis Leaderboards](#real-life-examples-of-redis-leaderboards)
- [Redis for Counting (Views, Likes, Upvotes)](#redis-for-counting-views-likes-upvotes)
    - [How is Redis Used for Counting?](#how-is-redis-used-for-counting)
    - [Deep Dive into Redis Counting Mechanisms](#deep-dive-into-redis-counting-mechanisms)
    - [How Redis Manages Counters?](#how-redis-manages-counters)
    - [Pros of Using Redis for Counting](#pros-of-using-redis-for-counting)
    - [Cons of Using Redis for Counting](#cons-of-using-redis-for-counting)
    - [Real-Life Examples of Redis Counting](#real-life-examples-of-redis-counting)
- [Redis for Distributed Locks](#redis-for-distributed-locks)
    - [How is Redis Used for Distributed Locks?](#how-is-redis-used-for-distributed-locks)
    - [Deep Dive into Redis Distributed Locks](#deep-dive-into-redis-distributed-locks)
    - [How Redis Implements Distributed Locks?](#how-redis-implements-distributed-locks)
    - [Pros of Using Redis for Distributed Locks](#pros-of-using-redis-for-distributed-locks)
    - [Cons of Using Redis for Distributed Locks](#cons-of-using-redis-for-distributed-locks)
    - [Real-Life Examples of Redis Distributed Locks](#real-life-examples-of-redis-distributed-locks)
- [Redis for Geospatial Indexing (Finding Nearby Users, Location-Based Services)](#redis-for-geospatial-indexing-finding-nearby-users-location-based-services)
    - [How is Redis Used for Geospatial Indexing?](#how-is-redis-used-for-geospatial-indexing)
    - [Deep Dive into Redis Geospatial Indexing](#deep-dive-into-redis-geospatial-indexing)
    - [How Redis Stores Geospatial Data?](#how-redis-stores-geospatial-data)
    - [Pros of Using Redis for Geospatial Indexing](#pros-of-using-redis-for-geospatial-indexing)
    - [Cons of Using Redis for Geospatial Indexing](#cons-of-using-redis-for-geospatial-indexing)
    - [Real-Life Examples of Redis Geospatial Indexing](#real-life-examples-of-redis-geospatial-indexing)
- [Redis for Pub/Sub Messaging (Real-Time Chat, Notifications, Event Broadcasting)](#redis-for-pubsub-messaging-real-time-chat-notifications-event-broadcasting)
    - [How is Redis Used for Pub/Sub Messaging?](#how-is-redis-used-for-pubsub-messaging)
    - [Deep Dive into Redis Pub/Sub](#deep-dive-into-redis-pubsub)
    - [How Redis Pub/Sub Works?](#how-redis-pubsub-works)
    - [Advanced Pub/Sub with Redis Streams (For Persistent Messaging)](#advanced-pubsub-with-redis-streams-for-persistent-messaging)
    - [Pros of Using Redis for Pub/Sub Messaging](#pros-of-using-redis-for-pubsub-messaging)
    - [Cons of Using Redis for Pub/Sub Messaging](#cons-of-using-redis-for-pubsub-messaging)
    - [Real-Life Examples of Redis Pub/Sub Messaging](#real-life-examples-of-redis-pubsub-messaging)
- [Redis for Job Queues (Background Tasks, Asynchronous Processing)](#redis-for-job-queues-background-tasks-asynchronous-processing)
    - [How is Redis Used for Job Queues?](#how-is-redis-used-for-job-queues)
    - [Deep Dive into Redis Job Queues](#deep-dive-into-redis-job-queues)
    - [How Redis Implements Job Queues?](#how-redis-implements-job-queues)
    - [Pros of Using Redis for Job Queues](#pros-of-using-redis-for-job-queues)
    - [Cons of Using Redis for Job Queues](#cons-of-using-redis-for-job-queues)
    - [Real-Life Examples of Redis Job Queues](#real-life-examples-of-redis-job-queues)
- [Redis for Real-Time Analytics (Live Dashboards, Log Processing)](#redis-for-real-time-analytics-live-dashboards-log-processing)
    - [How is Redis Used for Real-Time Analytics?](#how-is-redis-used-for-real-time-analytics)
    - [Deep Dive into Redis for Real-Time Analytics](#deep-dive-into-redis-for-real-time-analytics)
    - [How Redis Enables Real-Time Analytics?](#how-redis-enables-real-time-analytics)
    - [Pros of Using Redis for Real-Time Analytics](#pros-of-using-redis-for-real-time-analytics)
    - [Cons of Using Redis for Real-Time Analytics](#cons-of-using-redis-for-real-time-analytics)
    - [Real-Life Examples of Redis for Real-Time Analytics](#real-life-examples-of-redis-for-real-time-analytics)
- [Redis as a Primary Database](#redis-as-a-primary-database)
    - [How is Redis Used as a Primary Database?](#how-is-redis-used-as-a-primary-database)
    - [Deep Dive into Redis as a Primary Database](#deep-dive-into-redis-as-a-primary-database)
    - [How Redis Works as a Primary Database?](#how-redis-works-as-a-primary-database)
    - [Pros of Using Redis as a Primary Database](#pros-of-using-redis-as-a-primary-database)
    - [Cons of Using Redis as a Primary Database](#cons-of-using-redis-as-a-primary-database)
    - [Real-Life Examples of Redis as a Primary Database](#real-life-examples-of-redis-as-a-primary-database)
- [Bloom Filter in Redis](#bloom-filter-in-redis)
    - [Pros Of having Bloom Filter in Redis](#pros-of-having-bloom-filter-in-redis)
    - [Cons Of having Bloom Filter in Redis](#cons-of-having-bloom-filter-in-redis)
- [Summary](#summary)
- [Code Implementation Examples](#code-implementation-examples)
    - [Redis - Caching](#redis---caching)
    - [Redis - Session Storage](#redis---session-storage)
    - [Redis - Rate Limiting](#redis---rate-limiting)
    - [Redis - Leaderboard](#redis---leaderboard)
    - [Redis - Counting (Views, Likes, Upvotes)](#redis---counting-views-likes-upvotes)
    - [Redis - Distributed Locks](#redis---distributed-locks)
    - [Redis - Geospatial Indexing](#redis---geospatial-indexing)
    - [Redis - Pub/Sub Messaging](#redis---pubsub-messaging)
    - [Redis - Job Queues (Background Tasks)](#redis---job-queues-background-tasks)
    - [Redis - Bloom Filter](#redis---bloom-filter)

[**Redis Use Cases Examples in the Real-World**](https://www.coderbased.com/p/redis-use-cases-examples-in-the-real?open=false#%C2%A7geolocation-distance-driver-distance-and-attendance-system)

[**Top 5 Redis Use Cases**](https://youtu.be/a4yX7RUgTxI)

# Redis as a Cache

### How is Redis Used for Caching?

Redis is one of the most popular choices for caching due to its in-memory data storage, low-latency access, and rich data structures. It is commonly used to store frequently accessed data to reduce database queries, improve response times, and scale applications efficiently.

### Deep Dive into Redis Caching

1. **Basic Workflow**:
    - A client requests data.
    - The application first checks Redis for the data (cache lookup).
    - If found, Redis returns the data (cache hit).
    - If not found (cache miss), the application fetches the data from the database and stores it in Redis before returning it.
2. [**Common Caching Strategies**](https://tinyurl.com/2a5xdx3t):
    - **Write-Through Cache**: Data is written to both Redis and the database at the same time.
    - **Write-Around Cache**: Data is written directly to the database, and Redis is only updated when a read request occurs.
    - **Write-Back (Write-Behind) Cache**: Data is written to Redis first, and asynchronously updated in the database.
    - **Lazy Loading (Cache-aside)**: Data is loaded into Redis only when requested and missing (most commonly used).
    - **Read-Through Cache**: Similar to Lazy Loading, but the application always interacts with Redis, and Redis handles loading from the database.
3. **Eviction Policies**:
    - Redis offers various eviction policies to remove old data and make space for new data, such as:
        - **LRU (Least Recently Used)**: Removes the least accessed items.
        - **LFU (Least Frequently Used)**: Removes the least frequently used items.
        - **TTL-based Expiry**: Removes data based on time-to-live (TTL) settings.
        - **Random Eviction**: Removes any random key when memory is full.
4. **Persistence in Caching**:
    - While caching is typically transient, Redis supports persistence options like **RDB (Redis Database) snapshots** and **AOF (Append-Only File)** logging to recover cached data if needed.
5. **Data Structures Used**:
    - **String**: Basic key-value caching.
    - **Hash**: Store objects as key-value pairs.
    - **List**: Store ordered data like recent items.
    - **Set & Sorted Set**: Store unique values and leaderboard-like data.
    - **Bitmap & HyperLogLog**: Efficient memory usage for large-scale counting.

### Pros of Using Redis for Caching

1. **Ultra-Fast Performance** – Redis operates in memory and provides sub-millisecond latency.
2. **Reduced Database Load** – Offloads frequent reads from databases, improving database performance.
3. **Scalability** – Redis can handle millions of operations per second and supports clustering.
4. **Flexible Expiry Mechanisms** – Supports TTLs, automatic eviction, and LRU policies.
5. **Atomic Operations** – Supports transactions and atomic commands for efficient updates.
6. **Supports Advanced Data Structures** – More than just key-value caching (Lists, Hashes, Sets, etc.).
7. **High Availability** – Supports replication, failover, and persistence options.

### Cons of Using Redis for Caching

1. **Memory Cost** – Redis is an in-memory store, making it expensive compared to disk-based databases.
2. **Data Loss Risk** – If persistence is not enabled, data loss occurs upon crashes or reboots.
3. **Manual Cache Invalidation** – Keeping the cache in sync with the database can be complex.
4. **Single-threaded for Commands** – Although fast, Redis is single-threaded for command execution, which may cause bottlenecks in extreme cases.
5. **Limited Querying Capabilities** – Redis lacks advanced query support compared to databases.

### Real-Life Examples of Redis Caching

1. **Facebook** – Uses Redis to cache social media feed data and friend suggestions.
2. **Twitter** – Caches tweets, timelines, and trending topics using Redis.
3. **Netflix** – Uses Redis to cache user recommendations and metadata to improve streaming performance.
4. **GitHub** – Caches API responses and repository metadata.
5. **Amazon** – Uses Redis to cache product catalog details and shopping cart data.
6. **Slack** – Caches messages, user data, and real-time chat updates.
7. **Uber** – Caches driver locations and estimated fare calculations.

# Redis as Session Storage

### How is Redis Used for Session Storage?

Redis is widely used for managing user sessions in web applications due to its low-latency, high-throughput, and persistent in-memory storage. Since sessions involve frequent reads and writes, Redis ensures quick access to user data, improving performance and scalability.

### Deep Dive into Redis Session Storage

**1. Why Use Redis for Session Storage?**

- Traditional session storage methods (e.g., cookies, relational databases) can be slow and difficult to scale.
- Redis offers an **in-memory** solution that allows near-instantaneous access to user session data.
- It supports **auto-expiration**, ensuring that inactive sessions are removed automatically.
- Redis can **persist sessions**, preventing session loss during server restarts.

**2. How It Works (Session Lifecycle in Redis)**

1. **User Logs In** – The application generates a unique **session ID**.
2. **Session Data Stored in Redis** – This includes user authentication details, preferences, and temporary state data.
3. **Subsequent Requests** – The application retrieves the session data from Redis using the session ID.
4. **Session Expiry/Logout** – Redis automatically removes expired sessions based on TTL or when the user logs out.

**3. Redis Features for Session Management**

- **TTL (Time-To-Live)** – Sessions can be set to expire after a predefined period (e.g., `EXPIRE session_id 3600` for 1 hour).
- **LRU Eviction Policy** – Least Recently Used sessions are evicted first when memory is full. (Example - Netflix let you keep 4 instances active at a time)
- **Persistence Options** –
    - **RDB (Snapshot Persistence)** – Saves session data at intervals.
    - **AOF (Append-Only File)** – Logs each write operation for recovery.
- **Replication & High Availability** – Redis supports master-replica setups to ensure reliability.
- **Data Structures for Session Storage**:
    - **String** – Simple session key-value storage.
    - **Hash** – Efficient storage of structured session data (e.g., `{session_id: {user_id: 123, cart: [item1, item2]}}`).
    - **List** – Useful for storing user activity history.

**4. Session Storage Architectures**

- **Centralized Session Store** – All application servers retrieve and update sessions from a single Redis instance.
- **Distributed Session Store** – Multiple Redis nodes store session data, supporting horizontal scaling.
- **Hybrid Approach** – Combines Redis with other session storage methods for redundancy.

### Pros of Using Redis for Session Storage

1. **Ultra-Fast Read & Write Speed** – Ideal for real-time session handling.
2. **Automatic Expiry & Cleanup** – Sessions expire automatically, reducing stale data.
3. **Scalability** – Supports distributed storage and clustering for large-scale applications.
4. **Persistence Options** – Prevents data loss in case of crashes.
5. **Multi-Platform Support** – Works with Node.js, Python, Java, PHP, etc.
6. **Reduces Database Load** – Keeps session data separate from the primary database.

### Cons of Using Redis for Session Storage

1. **Memory Intensive** – In-memory storage can be costly compared to disk-based alternatives.
2. **Potential Data Loss** – If persistence isn’t enabled, sessions may be lost on crashes.
3. **Session Synchronization Challenges** – Requires careful configuration in multi-region setups.
4. **Write Bottlenecks in a Single Instance** – Needs sharding or clustering for high throughput.

### Real-Life Examples of Redis for Session Storage

1. **Netflix** – Uses Redis to store user session states and authentication tokens for seamless streaming.
2. **Twitter** – Manages logged-in user sessions and maintains active session tracking.
3. **Instagram** – Stores temporary user session data to enable fast logins and interactions.
4. **Slack** – Manages real-time authentication sessions for users and bots.
5. **E-Commerce Platforms (Amazon, Shopify, Flipkart, etc.)** – Store cart sessions, user preferences, and login states.
6. **Ride-Sharing Apps (Uber, Lyft)** – Store driver and rider session states for real-time matching.

# Redis for Rate Limiting

### How is Redis Used for Rate Limiting?

Redis is widely used for **rate limiting** to control the number of requests users, IPs, or services can make within a specific time frame. This helps protect APIs, prevent abuse, and ensure fair resource usage. Redis's fast in-memory operations and atomic commands make it an ideal choice for enforcing rate limits efficiently.

### Deep Dive into Redis Rate Limiting

**1. Why Use Redis for Rate Limiting?**

- **High-speed, low-latency** enforcement of limits.
- **Atomic operations** ensure correctness even in distributed environments.
- **Flexible data structures** (strings, hashes, sorted sets) allow different rate-limiting strategies.
- **Auto-expiry (TTL support)** ensures old rate limits are cleared automatically.

**2. Rate Limiting Strategies in Redis**

**(A) Fixed Window Counter**

- **Concept**: Count requests in a fixed time window (e.g., 100 requests per minute).
- **Implementation**:
    1. Use a Redis key like `rate:user:1234:2025-02-15-14:05` (hour-minute bucket).
    2. Increment the count with `INCR` when a request arrives.
    3. Set TTL to match the window length (e.g., 60s).
- **Downside**: Traffic spikes at window boundaries can lead to uneven distribution.

**(B) Sliding Window Log**

- **Concept**: Store request timestamps and allow requests within a rolling time window.
- **Implementation**:
    1. Store timestamps in a Redis **sorted set (ZSET)** with the request time as the score.
    2. Remove timestamps older than the time window (`ZREMRANGEBYSCORE`).
    3. Count the remaining timestamps to determine if a request should be allowed.
- **Advantage**: Smoother distribution compared to fixed windows.
- **Downside**: More memory usage due to storing timestamps.

**(C) Sliding Window Counter (Leaky Bucket Algorithm)**

- **Concept**: Instead of storing each request, maintain a **decreasing** counter that smoothens bursty traffic.
- **Implementation**:
    1. Store a counter with a timestamp.
    2. Decay the count over time (like a dripping bucket).
    3. Increment the counter on each request.
- **Advantage**: More memory-efficient than the log approach.
- **Use Case**: Preventing bursty traffic without harsh cutoff limits.

**(D) Token Bucket Algorithm**

- **Concept**: Each user has a "bucket" that refills at a fixed rate and consumes tokens per request.
- **Implementation**:
    1. Use a Redis key to store the remaining tokens.
    2. Refill tokens at regular intervals using `INCR`.
    3. Deduct tokens per request using `DECR`.
- **Advantage**: Allows occasional bursts while enforcing long-term limits.
- **Use Case**: API rate limiting, network traffic shaping.

### Pros of Using Redis for Rate Limiting

1. **Fast Execution** – In-memory operations make enforcement real-time.
2. **Atomicity** – Commands like `INCR`, `ZADD`, and `EXPIRE` ensure safe concurrent updates.
3. **Auto-Expiry Support** – Avoids stale data accumulation.
4. **Distributed Capability** – Works well with Redis Cluster and Redis Sentinel.
5. **Flexible Strategies** – Supports multiple algorithms based on use cases.

### Cons of Using Redis for Rate Limiting

1. **Memory Consumption** – Storing request logs (Sliding Window Log) can be expensive.
2. **Single Point of Failure** – If Redis crashes and persistence is off, rate limits reset.
3. **Complexity in Synchronization** – In multi-region setups, rate limits may need Redis replication strategies.
4. **Potential Thundering Herd Problem** – A sudden spike when limits reset requires mitigation strategies.

### Real-Life Examples of Redis Rate Limiting

1. **API Rate Limiting (GitHub, Twitter, OpenAI, Stripe, AWS, etc.) -** Controls API request limits to prevent excessive traffic from a single user.
2. **Login Brute-Force Protection (Facebook, Google, Microsoft, etc.) -** Limits failed login attempts to prevent credential stuffing attacks.
3. **DDoS Mitigation (Cloudflare, AWS Shield, Akamai) -** Rate limits requests from suspicious IPs to reduce attack impact.
4. **Web Scraping Prevention (News Websites, E-commerce Platforms) -** Prevents bots from making excessive requests to extract data.
5. **Messaging and Chat Systems (Slack, WhatsApp, Discord) -** Limits message sending frequency to prevent spam.
6. **Online Voting & Polling (Surveys, Contests, Elections) -** Prevents fraudulent multiple submissions.
7. **Gaming Servers (Fortnite, Call of Duty, Riot Games) -** Rate limits game requests to ensure fair matchmaking and prevent flooding.

# Redis for Leaderboards

### How is Redis Used for Leaderboards?

Redis is extensively used for **leaderboards** due to its support for **sorted sets (ZSETs)**, which allow ranking items (players, scores, etc.) efficiently. This enables real-time updates and retrieval of rankings with **O(log N)** operations, making Redis an ideal choice for gaming, social platforms, and competitive applications.

### Deep Dive into Redis Leaderboards

**1. Why Use Redis for Leaderboards?**

- **Real-time updates** – New scores instantly adjust rankings.
- **Efficient queries** – Fetch top rankings, player positions, and percentile rankings in milliseconds.
- **Automatic sorting** – Redis's **sorted set (ZSET)** maintains order by score.
- **Low-latency performance** – Even with millions of players, Redis handles leaderboards efficiently.

**2. How Redis Stores and Manages Leaderboards?**

Redis **Sorted Sets (ZSETs)** store items with a score while keeping them **automatically sorted**.

**Basic Operations for a Leaderboard**

| Operation | Redis Command | Description |
| --- | --- | --- |
| Add/Update Score | `ZADD leaderboard 5000 "player123"` | Inserts or updates a player’s score |
| Get Rank | `ZRANK leaderboard "player123"` | Fetches a player's rank (0-based) |
| Get Score | `ZSCORE leaderboard "player123"` | Fetches a player’s score |
| Get Top N Players | `ZREVRANGE leaderboard 0 9 WITHSCORES` | Retrieves the top 10 players |
| Get Players in a Score Range | `ZRANGEBYSCORE leaderboard 1000 5000` | Retrieves players with scores between 1000-5000 |
| Remove Player | `ZREM leaderboard "player123"` | Removes a player from the leaderboard |
| Get Player’s Rank with Score | `ZREVRANK leaderboard "player123" WITHSCORES` | Retrieves rank and score of a player |

### Advanced Leaderboard Use Cases

**A. Global & Regional Leaderboards**

- Maintain different leaderboards for **global, country, or region-based** rankings.
- Example:
    
    ```bash
    ZADD global_leaderboard 5000 "player123"
    ZADD US_leaderboard 4800 "player123"
    ```
    

**B. Weekly / Monthly Leaderboards (Time-based Leaderboards)**

- Store scores in a leaderboard with a **timestamp** suffix (e.g., `leaderboard_2025_02_15`).
- Use **Redis expiration (TTL)** to remove old leaderboards automatically:
    
    ```bash
    EXPIRE leaderboard_2025_02_15 2592000  # Expire after 30 days
    ```
    

**C. Multi-Category Leaderboards**

- Store different types of rankings (e.g., "Kills", "Win Rate", "XP Gained") in separate ZSETs:
    
    ```bash
    ZADD kills_leaderboard 150 "player123"
    ZADD xp_leaderboard 5000 "player123"
    ```
    

**D. Pagination & Custom Views**

- Use `ZREVRANGE` with offset and limit to fetch **paginated results**:
    
    ```bash
    ZREVRANGE leaderboard 0 9 WITHSCORES  # Get top 10 players
    ZREVRANGE leaderboard 10 19 WITHSCORES  # Get next 10 players
    ```
    

**E. Player-Specific Insights**

- Get player's percentile ranking:
    
    ```bash
    ZCARD leaderboard  # Get total players
    ZREVRANK leaderboard "player123"  # Get player's rank
    ```
    
    - Percentile = `(1 - (rank / total_players)) * 100`

### Pros of Using Redis for Leaderboards

1. **Real-time Updates** – Scores are automatically ranked without manual sorting.
2. **Fast Queries** – `O(log N)` time complexity makes rank lookups efficient.
3. **Scalability** – Handles millions of leaderboard entries easily.
4. **Expiration Support** – Can auto-remove old leaderboards (weekly/monthly resets).
5. **Flexibility** – Supports global, regional, and multi-metric leaderboards.
6. **Atomic Operations** – Ensures consistency in ranking updates.

### Cons of Using Redis for Leaderboards

1. **Memory Consumption** – Large leaderboards require significant memory.
2. **Lack of Complex Queries** – No support for advanced filtering like SQL (e.g., filtering by country *and* XP).
3. **Data Loss Risk** – If persistence (RDB/AOF) is not enabled, leaderboard data may be lost on crashes.
4. **Limited Aggregation** – Aggregating multiple leaderboards (e.g., merging XP and Kills rankings) is complex.

### Real-Life Examples of Redis Leaderboards

**Gaming & Esports**

1. **Fortnite & Call of Duty** – Track top players in global leaderboards.
2. **League of Legends** – Ranks players based on MMR (Matchmaking Rating).
3. **PUBG & Apex Legends** – Shows **real-time ranking** for kills, wins, and damage dealt.

**Social Media & Live Streaming**

1. **TikTok & Instagram Live** – Ranks top viewers or donors in a live session.
2. **Twitch & YouTube Gaming** – Tracks top streamers by followers and donations.

**E-Commerce & Engagement**

1. **Amazon & eBay** – Ranks top-selling products.
2. **Duolingo & Khan Academy** – Shows top learners based on XP gained.

**Fitness & Challenges**

1. **Strava & Peloton** – Ranks users based on miles run, calories burned, or workout sessions.

# Redis for Counting (Views, Likes, Upvotes)

### How is Redis Used for Counting?

Redis is widely used for real-time counting in applications that track views, likes, upvotes, retweets, and other counters. Thanks to its **in-memory storage**, **atomic operations**, and **high throughput**, Redis efficiently handles massive concurrent increments with minimal latency.

### Deep Dive into Redis Counting Mechanisms

**1. Why Use Redis for Counting?**

- **Fast Increments & Reads** – Unlike relational databases, Redis can handle millions of updates per second.
- **Atomic Operations** – Ensures consistency in concurrent environments.
- **Efficient Storage** – Uses compact data structures like **strings and hashes** to store counters.
- **Scalability** – Works well in distributed systems with Redis Cluster.

### How Redis Manages Counters?

**(A) Basic Counters with Strings**

Redis **strings** are commonly used for simple counters like **page views, likes, or upvotes**.

- **Increment a Counter**:
    
    ```bash
    INCR post:123:views  # Increments post 123's view count by 1
    ```
    
- **Increment by a Specific Value**:
    
    ```bash
    INCRBY post:123:likes 10  # Adds 10 likes
    ```
    
- **Get Current Counter Value**:
    
    ```bash
    GET post:123:views
    ```
    

**(B) Hashes for Multiple Counters Per Entity**

Instead of creating multiple keys, Redis **hashes** store multiple counters in a single key.

- **Increment Multiple Counters Efficiently**:
    
    ```bash
    HINCRBY post:123 stats views 1
    HINCRBY post:123 stats likes 5
    ```
    
- **Retrieve All Counters for a Post**:**Example Output:**
    
    ```bash
    HGETALL post:123
    ```
    
    ```
    {"views": "5000", "likes": "300"}
    ```
    

**(C) Expiring Counters (Time-Based Counting)**

- Useful for tracking **hourly/daily views or likes** using **Time-Series Keys**.
- Example: Store view counts per day:
    
    ```bash
    INCR "views:post:123:2025-02-15"
    EXPIRE "views:post:123:2025-02-15" 86400  # Expire after 1 day
    ```
    
- Retrieve counts over time:
    
    ```bash
    MGET views:post:123:2025-02-15 views:post:123:2025-02-14
    ```
    

**(D) Unique Counting with HyperLogLog (Approximate Counting)**

For counting unique users (e.g., **unique views, unique voters**), Redis **HyperLogLog** provides memory-efficient **approximate counting**.

- **Add Unique User to Counter**:
    
    ```bash
    PFADD unique_views:post:123 user_789
    ```
    
- **Get Estimated Unique Count**:
    
    ```bash
    PFCOUNT unique_views:post:123
    ```
    

**(E) Leaderboards for Counting-Based Rankings**

Using **sorted sets (ZSETs)**, you can rank posts or users based on views, likes, or upvotes.

- **Increase Score for a Post** (e.g., likes/upvotes):
    
    ```bash
    ZINCRBY trending_posts 1 "post:123"
    ```
    
- **Get Top Posts by Likes**:
    
    ```bash
    ZREVRANGE trending_posts 0 9 WITHSCORES  # Top 10 posts
    ```
    

### Pros of Using Redis for Counting

1. **Blazing-Fast Increments** – Handles millions of updates per second.
2. **Atomic Operations** – Prevents race conditions in concurrent writes.
3. **Memory Efficiency** – Uses small-sized keys and data structures (Hashes, HyperLogLog).
4. **Auto-Expiry Support** – Avoids unnecessary data buildup for time-sensitive counters.
5. **Distributed Scaling** – Works with **Redis Cluster** for large-scale applications.

### Cons of Using Redis for Counting

1. **Volatile Data** – Without persistence (`AOF` or `RDB`), data loss may occur on crashes.
2. **Memory Usage** – Large-scale counters (e.g., per-user per-hour tracking) can grow memory usage.
3. **Approximate Counts with HyperLogLog** – Not 100% accurate (though suitable for large-scale unique counts).
4. **No Advanced Querying** – Unlike SQL, Redis doesn’t support complex filters (e.g., "top liked posts with >100 comments").

### Real-Life Examples of Redis Counting

**Social Media Platforms**

1. **Facebook & Instagram** – Track post likes, shares, and views in real-time.
2. **Twitter** – Counts retweets, likes, and engagement metrics.
3. **Reddit** – Handles upvotes/downvotes dynamically for ranking posts.

**Content Streaming & News Websites**

1. **YouTube & Netflix** – Track video views and like counts.
2. **The New York Times & Medium** – Count article reads, shares, and reactions.

**E-Commerce & Marketplaces**

1. **Amazon & eBay** – Track product views and wishlist counts.
2. **Booking.com & Airbnb** – Counts how many times a hotel or listing was viewed.

**Online Voting & Surveys**

1. **Google Forms & Typeform** – Count votes in polls and surveys in real-time.

# Redis for Distributed Locks

### How is Redis Used for Distributed Locks?

Redis is widely used for implementing **distributed locks** in a multi-node environment where multiple processes or servers need **mutual exclusion** while accessing shared resources. Using **SETNX (SET if Not Exists)** and **Expiration (TTL)**, Redis provides **lightweight, fast, and atomic** locking mechanisms to prevent race conditions.

### Deep Dive into Redis Distributed Locks

**1. Why Use Redis for Distributed Locks?**

- **Atomic Operations** – Redis commands like `SETNX` and `GETSET` ensure locks are acquired atomically.
- **High Performance** – Lock operations execute in **O(1)** time, making them extremely fast.
- **Auto-Expiration** – Prevents deadlocks if a process crashes without releasing the lock.
- **Cross-Instance Synchronization** – Ensures consistency across distributed systems.

### How Redis Implements Distributed Locks?

**A. Basic Lock Mechanism**

A Redis **lock** is simply a key with a short expiration time to prevent deadlocks.

- **Acquire a Lock**:
    
    ```bash
    SET lock:resource_123 "client_1" NX EX 10  # Lock expires in 10 seconds
    ```
    
    - `NX` → Ensures the key is only set if it does not already exist (i.e., prevents multiple clients from acquiring the lock).
    - `EX 10` → Sets the lock expiration to 10 seconds to prevent indefinite locks.
- **Release a Lock (If Owned by the Same Client)**:
    
    ```bash
    DEL lock:resource_123  # Remove lock
    ```
    
- **Check Lock Ownership Before Deleting** (Lua script for atomic check-and-delete):
    
    ```lua
    if redis.call("GET", KEYS[1]) == ARGV[1] then
        return redis.call("DEL", KEYS[1])
    else
        return 0
    end
    ```
    
    - This ensures that a client cannot **delete a lock it doesn’t own**.

**B. Redlock Algorithm (Highly Reliable Locking in Distributed Systems)**

For **fault tolerance across multiple Redis nodes**, Redis Labs introduced **Redlock**. It ensures that a lock is **only acquired if a majority of Redis instances agree**.

**Steps to Implement Redlock**

1. **Try to acquire the lock on multiple Redis nodes** (typically 5 nodes).
2. **If a majority (e.g., 3/5 nodes) grant the lock**, it is considered acquired.
3. **If the lock is not acquired on a majority, release it from all nodes**.
4. **Ensure lock expiration time is consistent to prevent indefinite locks**.

**Redlock Python Example**

```python
import redis
import time

def acquire_lock(client, lock_key, lock_value, ttl=10):
    return client.set(lock_key, lock_value, nx=True, ex=ttl)

def release_lock(client, lock_key, lock_value):
    lua_script = """
    if redis.call("GET", KEYS[1]) == ARGV[1] then
        return redis.call("DEL", KEYS[1])
    else
        return 0
    end
    """
    return client.eval(lua_script, 1, lock_key, lock_value)

client = redis.StrictRedis(host='localhost', port=6379, decode_responses=True)

lock_key = "lock:resource"
lock_value = "client_1"

if acquire_lock(client, lock_key, lock_value):
    print("Lock acquired")
    time.sleep(5)  # Simulate processing time
    release_lock(client, lock_key, lock_value)
    print("Lock released")
else:
    print("Lock already acquired by another client")
```

### Pros of Using Redis for Distributed Locks

1. **Fast Lock Acquisition** – O(1) time complexity ensures near-instant locking.
2. **Auto-Expiration Prevents Deadlocks** – Locks expire if the process crashes.
3. **Scales Across Multiple Servers** – Ensures mutual exclusion in distributed environments.
4. **Atomicity & Consistency** – SETNX ensures that only one client acquires a lock.
5. **Lightweight** – Unlike databases, Redis does not require complex transaction management.

### Cons of Using Redis for Distributed Locks

1. **Single Point of Failure (Without Redlock)** – If Redis crashes, locks may be lost.
2. **Clock Skew Issues** – In distributed environments, different Redis nodes may have unsynchronized clocks.
3. **Not a Perfect Fit for Long Locks** – Since Redis is memory-based, long-held locks are inefficient.
4. **Network Partitioning Risks** – If a Redis node is isolated, locks may be incorrectly acquired or lost.
5. **Redlock Complexity** – Requires **multiple Redis instances** and **synchronization logic**.

### Real-Life Examples of Redis Distributed Locks

**1. E-Commerce & Online Transactions - Amazon, Flipkart, Shopify** – Prevent double-checkout issues by ensuring that an item is locked when a customer starts payment.

**2. Banking & Payment Systems - Stripe, PayPal** – Prevents duplicate payments by locking transaction IDs.

**3. Ticket Booking Systems - BookMyShow, Ticketmaster** – Ensures that only one user can book a seat at a time.

**4. Job Scheduling & Task Queues - Celery, Sidekiq, Resque** – Prevents multiple workers from processing the same job.

**5. Inventory Management - Walmart, Instacart** – Prevents overselling by ensuring an item is locked during checkout.

# Redis for Geospatial Indexing (Finding Nearby Users, Location-Based Services)

### How is Redis Used for Geospatial Indexing?

Redis provides built-in **geospatial indexing** capabilities through its **GEO commands**, enabling fast and efficient **location-based queries**. It is widely used for **finding nearby users, places, delivery tracking, ride-sharing, and location-based recommendations**.

### Deep Dive into Redis Geospatial Indexing

**Why Use Redis for Geospatial Queries?**

- **Efficient Geospatial Data Storage** – Stores latitude & longitude as **Geohashes** for fast lookups.
- **Optimized for Proximity Searches** – Uses **sorted sets (ZSETs)** for fast retrieval.
- **Atomic Updates & Reads** – Ensures consistency in concurrent location updates.
- **Scalable** – Works with **Redis Cluster** for large-scale applications.

### How Redis Stores Geospatial Data?

Redis stores geospatial data using **Geohashes**, which represent locations as compact **base-32 encoded strings**. These are stored in **sorted sets**, allowing efficient **proximity searches**.

**A. Adding Locations to Redis**

- **Store a User’s Location** (latitude, longitude) under a key:
    
    ```bash
    GEOADD users:locations 13.4050 52.5200 user_1  # Berlin
    GEOADD users:locations -74.0060 40.7128 user_2  # New York
    GEOADD users:locations 103.8198 1.3521 user_3  # Singapore
    ```
    
    - Uses **latitude and longitude** to store users' locations.
    - The underlying **ZSET (sorted set)** enables fast lookups.

**B. Finding Nearby Users (Proximity Search)**

- **Find users within a radius** (e.g., 50 km of Berlin):
    
    ```bash
    GEORADIUS users:locations 13.4050 52.5200 50 km
    ```
    
    **Example Output:**
    
    ```
    1) "user_1"
    ```
    
- **Find users with distance information**:
    
    ```bash
    GEORADIUS users:locations 13.4050 52.5200 50 km WITHDIST
    ```
    
    **Output Example:**
    
    ```
    1) "user_1" "0.0"  # user_1 is exactly at the given coordinates
    ```
    

**C. Finding Nearby Users by Member Name (Faster Query)**

- **Find nearby users using an existing user’s location**:
    
    ```bash
    GEORADIUSBYMEMBER users:locations user_1 50 km
    ```
    
    - **Advantage:** No need to manually provide latitude & longitude.

**D. Getting Distance Between Two Users**

- **Measure distance between two users**:**Output Example:**
    
    ```bash
    GEODIST users:locations user_1 user_2 km
    ```
    
    ```
    "6389.67"  # Distance in kilometers between Berlin and New York
    ```
    

**E. Getting Geohashes for Locations**

- **Retrieve the geohash representation of a location**:**Output Example:**
    
    ```bash
    GEOHASH users:locations user_1
    ```
    
    ```
    "u33dbf7"  # Geohash of Berlin
    ```
    
    - **Geohashes enable hierarchical searching** (prefix-based matching for nearby locations).

### Pros of Using Redis for Geospatial Indexing

1. **Fast Lookups** – Redis uses sorted sets (ZSETs) for efficient spatial queries.
2. **Simple & Built-In Support** – No need for an external GIS library.
3. **Scales Well** – Works with Redis Cluster for large-scale location data.
4. **Efficient Storage** – Uses Geohashes to compactly store coordinates.
5. **Atomic Updates** – Ensures consistent real-time location tracking.

### Cons of Using Redis for Geospatial Indexing

1. **No Polygon or Complex Geospatial Queries** – Cannot handle **bounding boxes, custom-shaped areas, or intersections** (unlike PostGIS).
2. **No Altitude Support** – Only stores latitude & longitude (not elevation).
3. **Limited Precision** – Geohash precision varies based on zoom level.
4. **Not Ideal for Huge Datasets** – For massive spatial datasets, a dedicated **spatial database** (e.g., PostGIS, Google S2) might be better.

### Real-Life Examples of Redis Geospatial Indexing

**1. Ride-Sharing & Delivery Services**

1. **Uber, Lyft, Ola, Bolt** – Find nearby drivers for users.
2. **DoorDash, Swiggy, Uber Eats** – Locate the closest delivery agents.

**2. Location-Based Social Apps**

1. **Tinder, Bumble** – Match users based on location proximity.
2. **Snapchat, Instagram Stories** – Show nearby users & stories.

**3. E-Commerce & Store Locators**

1. **Amazon, Walmart, Instacart** – Find nearest delivery warehouses.
2. **Google Maps, Apple Maps** – Show closest stores or service centers.

**4. Emergency Services & Logistics**

1. **911 Dispatch, Fire Departments** – Locate the nearest available ambulance.
2. **FedEx, UPS, DHL** – Optimize delivery route planning.

# Redis for Pub/Sub Messaging (Real-Time Chat, Notifications, Event Broadcasting)

### How is Redis Used for Pub/Sub Messaging?

Redis **Publish/Subscribe (Pub/Sub)** provides a lightweight and efficient messaging system for **real-time event-driven applications** like **chat apps, notifications, and event broadcasting**. It enables message producers to **publish** messages to channels, while consumers **subscribe** to receive relevant messages in real-time.

### Deep Dive into Redis Pub/Sub

**Why Use Redis for Pub/Sub?**

- **Low-Latency Messaging** – Redis operates in-memory, making it extremely fast.
- **Scales Horizontally** – Can be extended with Redis Cluster for distributed event handling.
- **Simple API** – Lightweight, requiring minimal setup compared to complex messaging systems like Kafka or RabbitMQ.
- **Real-Time Communication** – Ideal for instant updates, like live notifications and chat messages.

### How Redis Pub/Sub Works?

**A. Basic Publish/Subscribe Flow**

- **Publishers** send messages to a **channel**.
- **Subscribers** listening on that channel receive messages instantly.
- Messages are **not persisted**, meaning if a subscriber is offline, it will miss messages.

**B. Publishing Messages**

- Send a message to a channel:
    
    ```bash
    PUBLISH chat_room_1 "Hello, Redis Pub/Sub!"
    ```
    
    - This sends **"Hello, Redis Pub/Sub!"** to all subscribers listening on `chat_room_1`.

**C. Subscribing to a Channel**

- Clients can **subscribe** to receive messages from a channel:
    
    ```bash
    SUBSCRIBE chat_room_1
    ```
    
    - Any new messages published to `chat_room_1` will be **instantly pushed** to the subscriber.

**Example Output:**

```
1) "message"
2) "chat_room_1"
3) "Hello, Redis Pub/Sub!"
```

**D. Pattern-Based Subscriptions (Wildcard Matching)**

- Subscribe to multiple channels using a pattern:
    
    ```bash
    PSUBSCRIBE chat_room_*  # Matches chat_room_1, chat_room_2, etc.
    ```
    
    - Useful for multi-room chat applications.

**E. Unsubscribing from a Channel**

- To **unsubscribe** from a specific channel:
    
    ```bash
    UNSUBSCRIBE chat_room_1
    ```
    
- To **unsubscribe from all pattern-matched channels**:
    
    ```bash
    PUNSUBSCRIBE chat_room_*
    ```
    

### Advanced Pub/Sub with Redis Streams (For Persistent Messaging)

**Problem with Basic Pub/Sub:**

- **Messages are not stored** – If a subscriber disconnects, it misses messages.
- **No message acknowledgment or retries** – No guarantee that a message is received.

**Solution: Redis Streams!**

- Unlike traditional Pub/Sub, **Redis Streams** store messages persistently.
- **Consumers can replay missed messages** (useful for offline users).

**Example – Adding a Message to a Redis Stream:**

```bash
XADD notifications * message "User123 liked your post"
```

- `XADD` appends the message to the `notifications` stream with a unique ID.

### Pros of Using Redis for Pub/Sub Messaging

1. **Blazing Fast** – In-memory operations enable real-time messaging.
2. **Lightweight & Simple** – No complex brokers or configurations needed.
3. **Scalable** – Works well for small-to-medium-scale distributed systems.
4. **Pattern Matching** – Supports flexible subscriptions using wildcards.
5. **Event Broadcasting** – Ideal for push notifications, real-time feeds, and dashboards.

### Cons of Using Redis for Pub/Sub Messaging

Here’s the numbered list for your new points:

1. **No Message Persistence** – Messages are lost if subscribers are offline (use **Redis Streams** for persistence).
2. **No Message Acknowledgment** – No built-in retry mechanism if a message is dropped.
3. **Limited Scalability for High Throughput** – Not ideal for massive-scale event-driven systems (Kafka, RabbitMQ may be better).
4. **Network Partition Risks** – If a Redis node fails, Pub/Sub messages may be lost.

### Real-Life Examples of Redis Pub/Sub Messaging

**1. Real-Time Chat Applications - WhatsApp, Slack, Discord** – Redis Pub/Sub powers live chat rooms and instant messaging.

**2. Push Notifications & Live Updates**

- **Facebook, Twitter, Instagram** – Notify users instantly about likes, messages, and mentions.
- **Stock Market Apps** – Deliver real-time stock price changes.

**3. Event Broadcasting in Microservices - Netflix, Uber, DoorDash** – Microservices use Redis Pub/Sub to communicate event updates.

**4. Multiplayer Gaming - Fortnite, PUBG, Call of Duty** – Use Pub/Sub to sync game states in real-time.

**5. IoT (Internet of Things) Applications - Smart Homes, Wearable Devices** – Devices publish sensor data, and subscribers process it in real-time.

# Redis for Job Queues (Background Tasks, Asynchronous Processing)

### How is Redis Used for Job Queues?

Redis is widely used as a **message broker** and **task queue** for **background job processing**. It enables **asynchronous execution** of tasks, improving application **performance and scalability** by offloading time-consuming operations to worker processes.

### Deep Dive into Redis Job Queues

**Why Use Redis for Job Queues?**

- **Fast In-Memory Processing** – Jobs are queued and processed quickly.
- **Scalability** – Supports multiple workers consuming tasks in parallel.
- **Reliable Delivery** – Ensures messages are processed **at least once**.
- **Simple API** – Uses Redis lists (`LPUSH`, `RPUSH`, `BLPOP`, `BRPOP`) for queueing.

### How Redis Implements Job Queues?

**A. Basic Redis Queue with Lists**

Redis **lists** act as FIFO (First-In-First-Out) queues.

- **Producer adds a job (task) to the queue:**
    
    ```bash
    LPUSH job_queue "task_1"
    LPUSH job_queue "task_2"
    ```
    
    - `LPUSH` adds tasks (`task_1`, `task_2`) to the **left** of the queue.
- **Worker (consumer) retrieves and processes jobs:**
    
    ```bash
    BRPOP job_queue 0
    ```
    
    - `BRPOP` waits for a task and **removes it from the queue** once processed.

**Example Execution:**

```
1) "job_queue"
2) "task_1"
```

**B. Ensuring Job Reliability with Two-Queue Pattern**

- To avoid losing tasks during crashes, a **work queue** pattern is used:
    - Move jobs from `job_queue` to `processing_queue` before execution.
    - If processing fails, the job can be **re-added** for retry.

```bash
RPOPLPUSH job_queue processing_queue
```

- `RPOPLPUSH` moves a job from `job_queue` to `processing_queue`.
- After processing, remove it from `processing_queue`:
    
    ```bash
    LREM processing_queue 1 "task_1"
    ```
    

**C. Job Prioritization Using Multiple Queues**

- High-priority jobs are placed in a separate queue.
    
    ```bash
    LPUSH high_priority_queue "urgent_task"
    LPUSH job_queue "normal_task"
    ```
    
- Workers check the **high-priority queue first** before normal jobs.

**D. Distributed Job Processing with Multiple Workers**

- Multiple worker nodes fetch tasks from Redis **simultaneously**, allowing **horizontal scaling**.
- Example: **Celery with Redis as a broker** (Python-based job queue system).

### Pros of Using Redis for Job Queues

1. **Blazing Fast** – In-memory processing ensures low-latency task execution.
2. **Scalable** – Multiple workers can consume jobs concurrently.
3. **Reliable** – Can implement retries and processing acknowledgments.
4. **Simple Implementation** – Requires minimal setup compared to RabbitMQ or Kafka.
5. **Supports Priority Queues** – Using multiple lists for urgent vs normal jobs.

### Cons of Using Redis for Job Queues

1. **No Built-In Job Persistence** – If Redis crashes, jobs can be lost (unless backed up).
2. **No Dead Letter Queue (DLQ)** – Failed jobs need manual reprocessing.
3. **Limited Visibility & Monitoring** – Lacks advanced tracking features found in Kafka/RabbitMQ.
4. **Memory Constraints** – Large job queues can fill up Redis memory quickly.

### Real-Life Examples of Redis Job Queues

**1. Background Task Processing - Django Celery, Laravel Queue, Sidekiq (Ruby)** – Offload time-consuming tasks like sending emails.

**2. Asynchronous Workflows in Web Apps**

- **Gmail, Outlook** – Queue and send email notifications in the background.
- **E-commerce (Amazon, Shopify, Flipkart)** – Process orders and payments asynchronously.

**3. Video Processing & Image Resizing - YouTube, Instagram, TikTok** – Encode videos and resize images in the background.

**4. Data Processing & Analytics - Google Analytics, Mixpanel** – Process massive logs asynchronously using Redis queues.

**5. Chat & Notification Systems - WhatsApp, Slack, Discord** – Redis queues handle real-time message delivery.

# Redis for Real-Time Analytics (Live Dashboards, Log Processing)

### How is Redis Used for Real-Time Analytics?

Redis is widely used for **real-time data aggregation**, **stream processing**, and **fast query execution** in **live dashboards and log processing**. Its in-memory architecture allows for **low-latency reads/writes**, making it ideal for tracking and analyzing rapidly changing data streams.

### Deep Dive into Redis for Real-Time Analytics

**Why Use Redis for Real-Time Analytics?**

- **In-Memory Speed** – Delivers sub-millisecond response times.
- **Efficient Data Structures** – Uses **Sorted Sets**, **HyperLogLog**, and **Streams** for analytics.
- **Scalable & Distributed** – Works with Redis Cluster and Redis Sentinel for high availability.
- **Low Overhead** – Unlike databases, Redis doesn’t require expensive disk I/O.

### How Redis Enables Real-Time Analytics?

**A. Real-Time Counters (Using Redis INCR/DECR)**

Redis supports atomic **counters**, useful for tracking **page views, active users, and event counts**.

- **Increment a page view counter:**
    
    ```bash
    INCR page_views
    ```
    
- **Get current page views:**
    
    ```bash
    GET page_views
    ```
    
- **Use EXPIRE for time-limited tracking** (e.g., hourly stats):
    
    ```bash
    SETEX hourly_views 3600 0
    ```
    

**B. Aggregating Metrics with Sorted Sets (ZSETs)**

Redis **Sorted Sets (ZSETs)** are ideal for **leaderboards, trending topics, and ranked analytics**.

- **Track top website referrals:**
    
    ```bash
    ZINCRBY referrers 1 "google.com"
    ZINCRBY referrers 1 "twitter.com"
    ```
    
- **Retrieve top 5 referrers:**
    
    ```bash
    ZREVRANGE referrers 0 4 WITHSCORES
    ```
    

**C. Real-Time Stream Processing (Redis Streams)**

**Redis Streams** handle high-throughput, **ordered event processing**, useful for **log aggregation, clickstream analytics, and monitoring dashboards**.

- **Producer adds an event to a stream:**
    
    ```bash
    XADD logs * user_id 123 event "click"
    ```
    
- **Consumer reads events in real-time:**
    
    ```bash
    XREAD BLOCK 0 STREAMS logs $
    ```
    

**D. Unique Visitors Estimation (HyperLogLog)**

HyperLogLog is a **probabilistic data structure** used for estimating **unique visitors with low memory usage**.

- **Track unique visitors:**
    
    ```bash
    PFADD unique_users "user_1"
    PFADD unique_users "user_2"
    ```
    
- **Estimate total unique users:**
    
    ```bash
    PFCOUNT unique_users
    ```
    

**E. Session Tracking (Hash Data Structure)**

Redis **Hashes** efficiently store and query session data for active users.

- **Store user session info:**
    
    ```bash
    HSET user_session:123 status "active" last_seen "2025-02-15T12:00:00"
    ```
    
- **Retrieve session details:**
    
    ```bash
    HGETALL user_session:123
    ```
    

### Pros of Using Redis for Real-Time Analytics

1. **Lightning-Fast Processing** – In-memory operations ensure sub-millisecond latency.
2. **Scalability** – Supports millions of real-time events with Redis Cluster.
3. **Versatile Data Structures** – Optimized for counters, logs, rankings, and session tracking.
4. **Low Memory Usage** – HyperLogLog estimates large datasets with minimal space.
5. **Supports Streaming Analytics** – Redis Streams process high-throughput logs.

### Cons of Using Redis for Real-Time Analytics

1. **Volatile Data** – Data can be lost if not persisted (use **AOF or RDB snapshots**).
2. **Limited Querying Capabilities** – Not a full-fledged analytics database like ClickHouse or Druid.
3. **Memory Constraints** – Large datasets require careful memory management.
4. **No Complex Joins** – Unlike SQL databases, Redis lacks relational querying.

### Real-Life Examples of Redis for Real-Time Analytics

**1. Live Dashboards (Real-Time Metrics) - Google Analytics, Mixpanel, Amplitude** – Track user engagement, session durations, and active users.

**2. Log Processing & Event Monitoring - ELK Stack (Elasticsearch, Logstash, Kibana) + Redis** – Redis Streams buffer logs before processing.

**3. Social Media Trends & Leaderboards - Twitter, Instagram, TikTok** – Track trending hashtags and most-liked posts using Sorted Sets.

**4. Financial Market Data & Fraud Detection**

- **Stock Trading Platforms (Robinhood, Bloomberg)** – Process millions of price updates per second.
- **Fraud Detection (Visa, PayPal)** – Analyze transaction patterns in real-time.

**5. E-Commerce & Recommendation Engines - Amazon, Flipkart, Shopify** – Track live product views, purchases, and user recommendations.

# Redis as a Primary Database

### How is Redis Used as a Primary Database?

Redis can serve as a **primary database** when applications need **low-latency data access**, **high throughput**, and **simple key-value storage**. Unlike traditional relational databases, Redis stores data **in-memory**, making it suitable for real-time applications that require **microsecond-level response times**.

### Deep Dive into Redis as a Primary Database

**Why Use Redis as a Primary Database?**

- **Performance** – Sub-millisecond response times.
- **Scalability** – Redis Cluster enables horizontal scaling.
- **Flexible Data Structures** – Supports key-value, lists, sets, sorted sets, hashes, streams, and geospatial data.
- **Persistence Options** – AOF (Append-Only File) and RDB (Redis Database) for data durability.
- **High Availability** – Redis Sentinel and Redis Cluster provide automatic failover and replication.

### How Redis Works as a Primary Database?

**A. Data Storage with Persistence Mechanisms**

- **RDB (Redis Database File)** – Periodic snapshots of the dataset.
- **AOF (Append-Only File)** – Logs every write operation for durability.
- **Hybrid Mode (AOF + RDB)** – Provides both snapshots and logs for redundancy.

**Example: Enabling AOF for durability**

```bash
redis-cli CONFIG SET appendonly yes
```

**B. Data Modeling with Redis**

Redis supports multiple data types for structured storage.

1. **Key-Value Store (String)**
    - Ideal for session management and caching.
    
    ```bash
    SET user:123 "John Doe"
    GET user:123
    ```
    
2. **Hashes (Structured Data)**
    - Stores user profiles or product details.
    
    ```bash
    HSET user:123 name "John Doe" age "30"
    HGETALL user:123
    ```
    
3. **Lists (Queue-like Operations)**
    - Used for task queues and recent activity logs.
    
    ```bash
    LPUSH notifications "You have a new message!"
    LRANGE notifications 0 -1
    ```
    
4. **Sets (Unique Elements, Tags, Relationships)**
    - Useful for tracking unique users, categories, or user interests.
    
    ```bash
    SADD user:123:tags "sports" "technology"
    SMEMBERS user:123:tags
    ```
    
5. **Sorted Sets (Leaderboard, Ranking Systems)**
    - Used for rating systems and prioritization.
    
    ```bash
    ZADD leaderboard 100 "player1"
    ZREVRANGE leaderboard 0 9 WITHSCORES
    ```
    

**C. High Availability with Replication & Clustering**

- **Redis Sentinel** – Ensures high availability with automated failover.
- **Redis Cluster** – Distributes data across multiple nodes for horizontal scaling.

**Example: Enabling Replication for Read Scaling**

```bash
SLAVEOF <master-ip> <master-port>
```

### Pros of Using Redis as a Primary Database

1. **Blazing Fast Read/Write Performance** – Sub-millisecond latency.
2. **Scalability** – Redis Cluster supports sharding and replication.
3. **Flexible Data Structures** – Supports key-value, lists, hashes, streams, and more.
4. **Persistence Mechanisms** – AOF and RDB ensure data durability.
5. **Built-in Pub/Sub** – Supports event-driven architectures.
6. **Low Latency Geospatial Queries** – Optimized for real-time location services.

### Cons of Using Redis as a Primary Database

1. **Memory Constraints** – Being an in-memory database, large datasets require expensive RAM.
2. **Lacks Complex Querying** – No support for SQL-style joins or ACID transactions.
3. **Data Persistence Overhead** – AOF and RDB add storage and CPU overhead.
4. **Not Ideal for Large Datasets** – Scaling beyond RAM limits requires Redis on Flash or external storage.
5. **Limited Security Features** – No built-in access control mechanisms beyond basic authentication.

### Real-Life Examples of Redis as a Primary Database

**1. Real-Time Leaderboards & Gaming - Fortnite, PUBG, Clash of Clans** – Uses Redis Sorted Sets for ranking players.

**2. Session Management & Authentication - Slack, Discord, Zoom** – Uses Redis to store active user sessions and login states.

**3. Financial & Trading Applications - Stock Market Platforms (Robinhood, Bloomberg)** – Tracks live stock prices and trading orders.

**4. IoT Data Processing & Real-Time Analytics - Tesla, Smart Home Systems** – Uses Redis Streams to process millions of sensor data points per second.

**5. Social Media & Chat Applications - Twitter, Instagram, WhatsApp** – Uses Redis Pub/Sub for real-time notifications and messaging.

# Bloom Filter in Redis

**Redis supports Bloom Filters** natively using the **RedisBloom** module. Bloom filters are probabilistic data structures that efficiently test **set membership** with minimal memory. They are widely used for **fast existence checks** (e.g., detecting duplicate requests, caching lookups, spam filtering).

### Pros Of having Bloom Filter in Redis

1. **Fast Membership Check** → Constant time (`O(1)`) for insert & lookup.
2. **Memory Efficient** → Uses **bitwise hashing** instead of storing full data.
3. **Scalable** → Supports **millions of entries** with minimal RAM.
4. **Distributed & Persistent** → Works across Redis clusters.

### Cons Of having Bloom Filter in Redis

1. **False Positives** → A Bloom filter **might** return `true` for non-existent items.
2. **No Deletion** → Cannot remove elements once added.
3. **Larger Size for Accuracy** → Lowering the false-positive rate **increases memory usage**.

# Summary

Here’s a **comprehensive summary table** of all Redis use cases discussed in this session:

| **Use Case** | **How Redis is Used?** | **Pros** | **Cons** | **Real-Life Examples** |
| --- | --- | --- | --- | --- |
| **Caching** | Stores frequently accessed data in-memory for fast retrieval. | ✅ Sub-millisecond latency  ✅ Reduces database load  ✅ Scales horizontally | ❌ Cache invalidation is tricky  ❌ Requires careful eviction strategy | 📱 **Instagram** (User feed caching)  🎵 **Spotify** (Music metadata caching) |
| **Session Storage** | Stores user session data (tokens, login states) for fast access. | ✅ Fast session retrieval  ✅ Stateless authentication  ✅ Auto-expiry for sessions | ❌ Requires persistence for durability  ❌ Scaling sessions across regions is complex | 💬 **Slack, Discord** (User presence, session states)  🛒 **Amazon, Shopify** (Shopping cart sessions) |
| **Rate Limiting** | Throttles API requests using counters & sliding windows. | ✅ Prevents abuse & DDOS  ✅ Atomic increments for accurate limits  ✅ Lightweight & fast | ❌ Memory consumption for tracking  ❌ Doesn't track long-term quotas | 🔐 **Cloudflare** (API rate limiting)  🏦 **Banking APIs** (Transaction rate limits) |
| **Leaderboards** | Uses **Sorted Sets (ZSETs)** to rank scores. | ✅ Real-time ranking  ✅ Efficient range queries  ✅ Automatic sorting | ❌ High memory usage for large leaderboards  ❌ No built-in aggregation | 🎮 **Fortnite, PUBG** (Player rankings)  🎵 **Spotify** (Top songs ranking) |
| **Counting (Views, Likes, Upvotes)** | Uses **INCR, HyperLogLog** for analytics & counters. | ✅ Fast atomic counters  ✅ HyperLogLog reduces memory usage  ✅ Ideal for real-time metrics | ❌ Estimations in HyperLogLog are not exact  ❌ Persistence overhead for frequent updates | 📊 **Google Analytics, Mixpanel** (Real-time analytics)  👍 **Reddit, YouTube** (Upvotes, views tracking) |
| **Distributed Locks** | Uses **SETNX, EXPIRE, Redlock** for mutual exclusion. | ✅ Prevents race conditions  ✅ Lightweight & fast  ✅ Works across multiple servers | ❌ Not 100% reliable (needs Redlock)  ❌ Requires expiration handling | 🛍 **E-commerce** (Prevent double checkout)  🏦 **Banking** (Prevent duplicate transactions) |
| **Geospatial Indexing** | Uses **GEOADD, GEODIST, GEORADIUS** for location-based queries. | ✅ Fast location lookups  ✅ Efficient storage  ✅ Supports radius queries | ❌ No complex geospatial joins  ❌ Limited geospatial indexing | 🚗 **Uber, Lyft** (Find nearby drivers)  🍔 **Food Delivery Apps** (Find restaurants nearby) |
| **Pub/Sub Messaging** | Uses **PUBLISH/SUBSCRIBE** for real-time event broadcasting. | ✅ Real-time notifications  ✅ Low-latency messaging  ✅ Simple implementation | ❌ No message persistence  ❌ Not ideal for complex messaging | 💬 **WhatsApp, Slack** (Real-time chat)  📢 **Stock Market Tickers** |
| **Job Queues (Background Tasks)** | Uses **Lists (LPUSH, BRPOP)** or **Streams** to queue tasks. | ✅ Asynchronous processing  ✅ Reliable task queues  ✅ Low-latency processing | ❌ No built-in retries  ❌ Needs external worker management | 📧 **Email Sending (Gmail, Mailchimp)**  🎬 **Video Processing (YouTube, Netflix)** |
| **Real-Time Analytics** | Uses **INCR, HyperLogLog, Streams** for live dashboards. | ✅ Fast real-time aggregation  ✅ Efficient event processing  ✅ Supports high-throughput logging | ❌ Memory-heavy for large-scale analytics  ❌ No advanced querying like SQL | 📈 **Stock Trading (Robinhood, Bloomberg)**  🔍 **Fraud Detection (PayPal, Visa)** |
| **Primary Database** | Stores **all** data in-memory with persistence. | ✅ Ultra-fast queries  ✅ Flexible data structures  ✅ High availability with clustering | ❌ Memory constraints  ❌ No complex querying (no joins) | 🎮 **Gaming (Session states, leaderboards)**  💬 **Chat apps (WhatsApp, Slack)** |

# Code Implementation Examples

### Redis - Caching

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;
import org.springframework.web.bind.annotation.*;

import java.util.concurrent.TimeUnit;

@SpringBootApplication
public class RedisCacheApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisCacheApp.class, args);
    }

    // Configure Redis connection using Jedis
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new JedisConnectionFactory();
    }

    // Configure RedisTemplate for key-value operations
    @Bean
    public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, String> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        template.setKeySerializer(new StringRedisSerializer());  // Serialize keys as Strings
        template.setValueSerializer(new StringRedisSerializer()); // Serialize values as Strings
        return template;
    }
}

@RestController
@RequestMapping("/cache")
class CacheController {
    private final RedisTemplate<String, String> redisTemplate;

    public CacheController(RedisTemplate<String, String> redisTemplate) {
        this.redisTemplate = redisTemplate;
    }

    // Store key-value pair in Redis with 10-minute expiration
    @PostMapping("/{key}/{value}")
    public String setCache(@PathVariable String key, @PathVariable String value) {
        redisTemplate.opsForValue().set(key, value, 10, TimeUnit.MINUTES);  // Set key with TTL
        return "Cached: " + key;
    }

    // Retrieve value from Redis cache
    @GetMapping("/{key}")
    public String getCache(@PathVariable String key) {
        String value = redisTemplate.opsForValue().get(key);
        return value != null ? "Value: " + value : "Key not found!";
    }

    // Remove key-value pair from Redis
    @DeleteMapping("/{key}")
    public String deleteCache(@PathVariable String key) {
        redisTemplate.delete(key);
        return "Deleted: " + key;
    }
}
```

### Redis - Session Storage

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.session.data.redis.config.annotation.web.http.EnableRedisHttpSession;
import org.springframework.web.bind.annotation.*;

import javax.servlet.http.HttpSession;

@SpringBootApplication
@EnableRedisHttpSession(maxInactiveIntervalInSeconds = 1800) // Enables Redis-backed session storage (30 min timeout)
public class RedisSessionApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisSessionApp.class, args);
    }

    // Configure Redis connection using Jedis
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new JedisConnectionFactory();
    }
}

@RestController
@RequestMapping("/session")
class SessionController {

    // Store data in Redis-backed session
    @PostMapping("/set/{key}/{value}")
    public String setSessionData(@PathVariable String key, @PathVariable String value, HttpSession session) {
        session.setAttribute(key, value); // Store in session (Redis will handle storage)
        return "Session Key '" + key + "' stored with value: " + value;
    }

    // Retrieve data from Redis-backed session
    @GetMapping("/get/{key}")
    public String getSessionData(@PathVariable String key, HttpSession session) {
        Object value = session.getAttribute(key);
        return value != null ? "Session Value: " + value : "No session data found for key: " + key;
    }

    // Remove session data
    @DeleteMapping("/remove/{key}")
    public String removeSessionData(@PathVariable String key, HttpSession session) {
        session.removeAttribute(key);
        return "Session Key '" + key + "' removed.";
    }
}
```

### Redis - Rate Limiting

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.web.bind.annotation.*;
import redis.clients.jedis.Jedis;

import java.time.Instant;

@SpringBootApplication
public class RedisRateLimiterApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisRateLimiterApp.class, args);
    }

    // Configure Redis connection using Jedis
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        return new JedisConnectionFactory();
    }
}

@RestController
@RequestMapping("/rate-limit")
class RateLimiterController {
    private static final String REDIS_KEY_PREFIX = "rate_limit:";
    private static final int MAX_TOKENS = 10;  // Max requests allowed
    private static final int REFILL_RATE = 1;  // Tokens added per second

    private final Jedis jedis = new Jedis("localhost", 6379); // Connect to Redis

    @GetMapping("/{userId}")
    public String accessResource(@PathVariable String userId) {
        String key = REDIS_KEY_PREFIX + userId;
        long currentTime = Instant.now().getEpochSecond();

        // Get current token bucket info
        String[] tokenData = jedis.hmget(key, "tokens", "last_refill").toArray(new String[0]);
        int tokens = tokenData[0] != null ? Integer.parseInt(tokenData[0]) : MAX_TOKENS;
        long lastRefill = tokenData[1] != null ? Long.parseLong(tokenData[1]) : currentTime;

        // Refill tokens
        long elapsedTime = currentTime - lastRefill;
        int newTokens = Math.min(MAX_TOKENS, tokens + (int) (elapsedTime * REFILL_RATE));

        // Check if request is allowed
        if (newTokens > 0) {
            jedis.hset(key, "tokens", String.valueOf(newTokens - 1));
            jedis.hset(key, "last_refill", String.valueOf(currentTime));
            jedis.expire(key, 60);  // Set TTL to prevent unused data buildup
            return "Request allowed. Remaining tokens: " + (newTokens - 1);
        } else {
            return "Rate limit exceeded. Try again later.";
        }
    }
}
```

### Redis - Leaderboard

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.bind.annotation.*;
import redis.clients.jedis.Jedis;

import java.util.Set;
import java.util.stream.Collectors;

@SpringBootApplication
public class RedisLeaderboardApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisLeaderboardApp.class, args);
    }
}

@RestController
@RequestMapping("/leaderboard")
class LeaderboardController {
    private static final String LEADERBOARD_KEY = "game_leaderboard";  // Redis Sorted Set key
    private final Jedis jedis = new Jedis("localhost", 6379); // Connect to Redis

    // Add or update a player's score in the leaderboard
    @PostMapping("/add/{player}/{score}")
    public String addScore(@PathVariable String player, @PathVariable double score) {
        jedis.zadd(LEADERBOARD_KEY, score, player);  // Add player with score
        return "Score updated: " + player + " -> " + score;
    }

    // Get the top N players from the leaderboard
    @GetMapping("/top/{count}")
    public Set<String> getTopPlayers(@PathVariable int count) {
        return jedis.zrevrangeWithScores(LEADERBOARD_KEY, 0, count - 1)
                .stream()
                .map(entry -> entry.getElement() + " - " + entry.getScore())
                .collect(Collectors.toSet());
    }

    // Get the rank of a specific player
    @GetMapping("/rank/{player}")
    public String getPlayerRank(@PathVariable String player) {
        Long rank = jedis.zrevrank(LEADERBOARD_KEY, player);
        return rank != null ? player + " is ranked #" + (rank + 1) : "Player not found.";
    }
}
```

### Redis - Counting (Views, Likes, Upvotes)

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import redis.clients.jedis.Jedis;

@SpringBootApplication
public class RedisCountingApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisCountingApp.class, args);
    }
}

@RestController
@RequestMapping("/count")
class CounterController {
    private static final String VIEW_KEY_PREFIX = "views:";
    private static final String LIKE_KEY_PREFIX = "likes:";
    private static final String UPVOTE_KEY_PREFIX = "upvotes:";
    
    private final Jedis jedis = new Jedis("localhost", 6379); // Connect to Redis

    // Increment view count for a post
    @PostMapping("/view/{postId}")
    public String incrementView(@PathVariable String postId) {
        long views = jedis.incr(VIEW_KEY_PREFIX + postId);
        return "Post " + postId + " has " + views + " views.";
    }

    // Increment like count for a post
    @PostMapping("/like/{postId}")
    public String incrementLike(@PathVariable String postId) {
        long likes = jedis.incr(LIKE_KEY_PREFIX + postId);
        return "Post " + postId + " has " + likes + " likes.";
    }

    // Increment upvote count for a comment
    @PostMapping("/upvote/{commentId}")
    public String incrementUpvote(@PathVariable String commentId) {
        long upvotes = jedis.incr(UPVOTE_KEY_PREFIX + commentId);
        return "Comment " + commentId + " has " + upvotes + " upvotes.";
    }

    // Get current counts for a post
    @GetMapping("/stats/{postId}")
    public String getPostStats(@PathVariable String postId) {
        long views = Long.parseLong(jedis.get(VIEW_KEY_PREFIX + postId) != null ? jedis.get(VIEW_KEY_PREFIX + postId) : "0");
        long likes = Long.parseLong(jedis.get(LIKE_KEY_PREFIX + postId) != null ? jedis.get(LIKE_KEY_PREFIX + postId) : "0");
        return "Post " + postId + " Stats -> Views: " + views + ", Likes: " + likes;
    }
}
```

### Redis - Distributed Locks

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import redis.clients.jedis.Jedis;

import java.util.UUID;

@SpringBootApplication
public class RedisDistributedLockApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisDistributedLockApp.class, args);
    }
}

@RestController
@RequestMapping("/lock")
class DistributedLockController {
    private static final String LOCK_KEY = "resource_lock";
    private static final int LOCK_EXPIRY = 10; // Lock expiry time in seconds

    private final Jedis jedis = new Jedis("localhost", 6379); // Connect to Redis

    // Acquire lock before processing a critical task
    @PostMapping("/acquire")
    public String acquireLock() {
        String lockValue = UUID.randomUUID().toString(); // Unique lock identifier

        // Attempt to acquire the lock using SETNX with an expiration time
        String result = jedis.set(LOCK_KEY, lockValue, "NX", "EX", LOCK_EXPIRY);
        if ("OK".equals(result)) {
            return "Lock acquired successfully: " + lockValue;
        } else {
            return "Failed to acquire lock. Resource is locked.";
        }
    }

    // Release lock after processing
    @PostMapping("/release/{lockValue}")
    public String releaseLock(@PathVariable String lockValue) {
        // Ensure that the lock being released belongs to the requester
        if (lockValue.equals(jedis.get(LOCK_KEY))) {
            jedis.del(LOCK_KEY);
            return "Lock released successfully.";
        } else {
            return "Lock release failed. Invalid or expired lock.";
        }
    }
}
```

Redlock Example

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;

import java.util.List;
import java.util.UUID;

@SpringBootApplication
public class RedisRedlockApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisRedlockApp.class, args);
    }
}

@RestController
@RequestMapping("/redlock")
class RedlockController {
    private final List<JedisPool> redisInstances = List.of(
        new JedisPool("localhost", 6379),  // First Redis instance
        new JedisPool("localhost", 6380),  // Second Redis instance
        new JedisPool("localhost", 6381)   // Third Redis instance
    );

    private static final String LOCK_KEY = "distributed_lock";
    private static final int LOCK_EXPIRY = 10_000; // 10 seconds (in milliseconds)
    private static final int QUORUM = 2; // Majority of instances

    // Acquire Redlock
    @PostMapping("/acquire")
    public String acquireLock() {
        String lockValue = UUID.randomUUID().toString(); // Unique lock identifier
        int acquiredCount = 0;
        long startTime = System.currentTimeMillis();

        // Try to acquire lock on each Redis instance
        for (JedisPool pool : redisInstances) {
            try (Jedis jedis = pool.getResource()) {
                String result = jedis.set(LOCK_KEY, lockValue, "NX", "PX", LOCK_EXPIRY);
                if ("OK".equals(result)) {
                    acquiredCount++;
                }
            }
        }

        // Check if we acquired the majority of locks
        if (acquiredCount >= QUORUM) {
            return "Lock acquired successfully: " + lockValue;
        } else {
            releaseLock(lockValue); // Rollback if failed
            return "Failed to acquire lock.";
        }
    }

    // Release Redlock
    @PostMapping("/release/{lockValue}")
    public String releaseLock(@PathVariable String lockValue) {
        int releasedCount = 0;

        // Try to release the lock from all instances
        for (JedisPool pool : redisInstances) {
            try (Jedis jedis = pool.getResource()) {
                if (lockValue.equals(jedis.get(LOCK_KEY))) {  // Verify lock ownership
                    jedis.del(LOCK_KEY);
                    releasedCount++;
                }
            }
        }

        return (releasedCount >= QUORUM) ? "Lock released successfully." : "Lock release failed.";
    }
}
```

### Redis - Geospatial Indexing

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.*;
import redis.clients.jedis.Jedis;
import redis.clients.jedis.GeoCoordinate;
import redis.clients.jedis.GeoRadiusResponse;
import redis.clients.jedis.params.GeoRadiusParam;

import java.util.List;

@SpringBootApplication
public class RedisGeospatialApp {
    public static void main(String[] args) {
        SpringApplication.run(RedisGeospatialApp.class, args);
    }
}

@RestController
@RequestMapping("/geo")
class GeoSpatialController {
    private static final String GEO_KEY = "locations"; // Redis key for geospatial data
    private final Jedis jedis = new Jedis("localhost", 6379); // Connect to Redis

    // Add a location with latitude & longitude
    @PostMapping("/add/{name}/{lat}/{lon}")
    public String addLocation(@PathVariable String name, @PathVariable double lat, @PathVariable double lon) {
        jedis.geoadd(GEO_KEY, lon, lat, name);
        return "Added location: " + name + " at [" + lat + ", " + lon + "]";
    }

    // Find locations within a given radius
    @GetMapping("/nearby/{lat}/{lon}/{radius}")
    public String findNearby(@PathVariable double lat, @PathVariable double lon, @PathVariable double radius) {
        List<GeoRadiusResponse> results = jedis.georadius(GEO_KEY, lon, lat, radius, redis.clients.jedis.args.GeoUnit.KM, GeoRadiusParam.geoRadiusParam().withDist());

        StringBuilder response = new StringBuilder("Nearby locations:\n");
        for (GeoRadiusResponse res : results) {
            response.append(res.getMemberByString()).append(" - ").append(res.getDistance()).append(" km\n");
        }

        return response.toString();
    }

    // Get the distance between two locations
    @GetMapping("/distance/{place1}/{place2}")
    public String getDistance(@PathVariable String place1, @PathVariable String place2) {
        Double distance = jedis.geodist(GEO_KEY, place1, place2, redis.clients.jedis.args.GeoUnit.KM);
        return distance != null ? "Distance between " + place1 + " and " + place2 + " is " + distance + " km" : "Locations not found.";
    }

    // Get the GeoHash of a location
    @GetMapping("/geohash/{name}")
    public String getGeoHash(@PathVariable String name) {
        List<String> hash = jedis.geohash(GEO_KEY, name);
        return "GeoHash for " + name + ": " + hash;
    }

    // Get the latitude and longitude of a location
    @GetMapping("/position/{name}")
    public String getPosition(@PathVariable String name) {
        List<GeoCoordinate> coordinates = jedis.geopos(GEO_KEY, name);
        return coordinates != null && !coordinates.isEmpty() ? "Coordinates for " + name + ": " + coordinates.get(0).getLatitude() + ", " + coordinates.get(0).getLongitude() : "Location not found.";
    }
}
```

### Redis - Pub/Sub Messaging

```java
import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPubSub;

// Publisher Class
class RedisPublisher {
    private final Jedis jedis;
    private final String channel;

    public RedisPublisher(String redisHost, int redisPort, String channel) {
        this.jedis = new Jedis(redisHost, redisPort); // Connect to Redis
        this.channel = channel;
    }

    public void publishMessage(String message) {
        jedis.publish(channel, message); // Publish message to channel
        System.out.println("Published: " + message);
    }
}

// Subscriber Class
class RedisSubscriber extends JedisPubSub {
    @Override
    public void onMessage(String channel, String message) {
        System.out.println("Received on " + channel + ": " + message);
    }
}

// Main Class to Run Publisher & Subscriber
public class RedisPubSubApp {
    public static void main(String[] args) throws InterruptedException {
        String redisHost = "localhost";
        int redisPort = 6379;
        String channel = "notifications";

        // Start Subscriber in a New Thread
        new Thread(() -> {
            try (Jedis jedis = new Jedis(redisHost, redisPort)) {
                RedisSubscriber subscriber = new RedisSubscriber();
                System.out.println("Subscribed to channel: " + channel);
                jedis.subscribe(subscriber, channel); // Listen for messages
            }
        }).start();

        // Wait for Subscriber to Start
        Thread.sleep(2000);

        // Publish Messages
        RedisPublisher publisher = new RedisPublisher(redisHost, redisPort, channel);
        publisher.publishMessage("Hello, Redis Pub/Sub!");
        publisher.publishMessage("New Event: User Joined");
        publisher.publishMessage("Breaking News: Redis is awesome!");
    }
}
```

### Redis - Job Queues (Background Tasks)

```java
import redis.clients.jedis.Jedis;

// Producer (Adds Jobs to Queue)
class JobProducer {
    private final Jedis jedis;
    private final String queueName;

    public JobProducer(String redisHost, int redisPort, String queueName) {
        this.jedis = new Jedis(redisHost, redisPort); // Connect to Redis
        this.queueName = queueName;
    }

    public void enqueueJob(String jobData) {
        jedis.lpush(queueName, jobData); // Push job to queue
        System.out.println("Job Enqueued: " + jobData);
    }
}

// Worker (Processes Jobs from Queue)
class JobWorker {
    private final Jedis jedis;
    private final String queueName;

    public JobWorker(String redisHost, int redisPort, String queueName) {
        this.jedis = new Jedis(redisHost, redisPort); // Connect to Redis
        this.queueName = queueName;
    }

    public void processJobs() {
        System.out.println("Worker Started - Waiting for jobs...");
        while (true) {
            String job = jedis.brpop(0, queueName).get(1); // Blocking pop
            System.out.println("Processing Job: " + job);
            try {
                Thread.sleep(2000); // Simulate task processing time
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Job Completed: " + job);
        }
    }
}

// Main Class to Run Producer & Worker
public class RedisJobQueueApp {
    public static void main(String[] args) throws InterruptedException {
        String redisHost = "localhost";
        int redisPort = 6379;
        String queueName = "jobQueue";

        // Start Worker in a New Thread
        new Thread(() -> {
            JobWorker worker = new JobWorker(redisHost, redisPort, queueName);
            worker.processJobs();
        }).start();

        // Wait for Worker to Start
        Thread.sleep(2000);

        // Produce Jobs
        JobProducer producer = new JobProducer(redisHost, redisPort, queueName);
        producer.enqueueJob("Task 1: Email Notification");
        producer.enqueueJob("Task 2: Generate Report");
        producer.enqueueJob("Task 3: Data Processing");
    }
}
```

### Redis - Bloom Filter

```java
import redis.clients.jedis.Jedis;
import redis.clients.jedis.bloom.BFInsertParams;
import redis.clients.jedis.bloom.RedisBloom;

import java.util.Arrays;

public class RedisBloomFilter {
    private final RedisBloom redisBloom;
    private final String filterName;

    public RedisBloomFilter(String redisHost, int redisPort, String filterName) {
        Jedis jedis = new Jedis(redisHost, redisPort);
        this.redisBloom = new RedisBloom(jedis);
        this.filterName = filterName;

        // Create Bloom Filter with expected size & false positive rate
        redisBloom.bfReserve(filterName, 0.01, 1000);
    }

    // Add an element to the Bloom Filter
    public void addElement(String element) {
        redisBloom.bfAdd(filterName, element);
        System.out.println("Added: " + element);
    }

    // Check if an element **might** exist in the Bloom Filter
    public boolean checkElement(String element) {
        boolean exists = redisBloom.bfExists(filterName, element);
        System.out.println("Exists (" + element + ")? " + exists);
        return exists;
    }

    public static void main(String[] args) {
        RedisBloomFilter bloomFilter = new RedisBloomFilter("localhost", 6379, "myBloomFilter");

        // Add Elements
        bloomFilter.addElement("user123");
        bloomFilter.addElement("user456");

        // Check Existence
        bloomFilter.checkElement("user123");  // True (definitely present)
        bloomFilter.checkElement("user789");  // False or might be True (false positive)
    }
}
```
