# Cache

1. Caching is the process of storing copies of data in a high-speed storage layer (cache) so that future requests for the same data can be served faster. The cache acts as an intermediary between the application and a slower storage layer, such as a database or an external API.
2. What cache help to avoid? Network I/O, Disk I/O and expensive computations
3. Caching is not just about storing data in fast storage; **it’s about knowing when to update or remove it.**
4. Key benefits - Why introduce cache in our system?
    1. **Availability:** Cache provides data from the closest or least busy locations.
    2. **Scalability:** Cache handles increased traffic and reducing the load on backend servers.
    3. **Performance:** Resulting in fewer read/write operations performed on the database, caching helps improve performance.
    4. **Reduced load:** By offloading the read operations, the cache protects the backend from slower performance and potential crashes during peak loads.
5. While caching offers significant benefits in many scenarios, it’s important to recognize that caching might not be essential for all systems and certainly for all layers.
    1. Google Search - relies heavily on caching to deliver instant search results and optimize performance for billions of users worldwide.
    2. Social media apps - leverage caching extensively to provide a seamless user experience
    3. Medical report application - containing sensitive user information might not require caching for performance reasons. Instead, the focus is on ensuring data security and access control mechanisms.
    4. Banking system - For someone’s account related private data, caching might not be require

## Cache State

### Cold

1. A cold cache is empty or has very little relevant data.
2. This state occurs when the cache is freshly initialized
3. Characteristics: Low Hit Rate, High Latency and High Backend Load
4. Example: After restarting a Redis instance, the cache is empty

### Warm

1. A warm cache contains some data but may not yet have a significant portion of the relevant or frequently accessed data.
2. It’s transitioning from a cold to a hot state.
3. Characteristics: Moderate Hit Rate, Improving Latency and Decreasing Backend Load

### Hot

1. A hot cache is fully populated with the most frequently accessed or relevant data, achieving optimal performance.
2. Characteristics: High Hit Rate, Low Latency and Low Backend Load

## Caching Strategies

### Cache Aside (Lazy Loading)

- Application checks the cache first.
- If data is not in the cache, it is fetched from the database and then written to the cache.
- Characteristics: On-demand caching; low write intensity; eventual consistency.
- Pros: Ensures only frequently accessed data is cached.
- Cons: Initial access to uncached data is slow.
- Use Case: User profile data access

### **Write Through(Eager population)**

- Data is written to the cache and the database simultaneously.
- Characteristics: Strong consistency; higher write latency.
- Pros: Ensures consistency between cache and database.
- Cons: Slower writes.
- Use Case: E-commerce shopping carts

### Write Back (Write Behind)

- Data is written to the cache first and later written to the database asynchronously.
- Characteristics: High write throughput; eventual consistency acceptable.
- Pros: Fast writes.
- Cons: Risk of data loss if the cache fails before syncing.
- Use Case: Analytics and event logging

### Read Through

- Application interacts only with the cache, which fetches data from the database when needed.
- Combines aspects of cache aside and write-through.
- Characteristics: Simplified cache logic; predictable read patterns.
- Use Case: Metadata in streaming platforms like Netflix

## Cache Eviction Policies

1. What happen it i don’t set Cache Eviction Policies? Without eviction policies, the cache can grow indefinitely, eventually consuming all available memory. This may cause the system to crash or start rejecting new write requests due to memory limits
2. What will happen if stored key in cache has no expiration(TTL)? Applications might serve incorrect or stale data to users, leading to inconsistencies with the underlying source of truth (e.g., database)
3. Cache Eviction Policies:
    - Least Recently Used (LRU): Evicts the least recently accessed data.
    - Least Frequently Used (LFU): Evicts the least frequently accessed data.
    - First In, First Out (FIFO): Evicts the oldest data.
    - Time-to-Live (TTL): Data expires after a set duration.

## Where to Use Caching in System Design

1. Database Query Results: Cache frequently queried or computationally expensive results.
2. API Responses: Cache external API responses to avoid rate limits and reduce latency.
3. Session Management: Cache user sessions for quick access.
4. Content Delivery: Cache static content like images, videos, or web assets.

## Caching **Tools and Technologies**

1. In-Memory Caches: Redis, Memcached.
2. CDNs: Cloudflare, Akamai, AWS CloudFront.
3. Database Caching: Query caching, materialized views.
4. Application-Level Libraries: Guava Cache (Java), CacheManager (Spring), etc.

## Caching Challenges and How to Deal with Them

### **Cache Invalidation**

Ensuring that outdated data is removed or replaced with fresh data in the cache.

Solutions:

- Time-to-Live (TTL): Set an expiration time for cache entries.
- Write Through/Write Back Strategies: Update the cache when data is modified in the database.
- Event-Driven Invalidation: Use database triggers or messaging systems to invalidate specific cache keys.

### **Cache Consistency**

Maintaining synchronization between the cache and the underlying data source.

Types of Consistency:

- Strong Consistency: Cache is always in sync with the database.
- Eventual Consistency: Cache may lag behind but eventually reflects the database state.

Solutions:

- Write Through Strategy: Update cache and database simultaneously.
- Read Through Strategy: Fetch fresh data from the database when needed.
- Invalidate on Writes: Clear cache entries when the database is updated.

### **Cold Start**

When a cache is empty (e.g., after initialization or a restart), leading to high latency and load on the backend.

Solutions:

- Cache Pre-Warming: Preload frequently accessed data into the cache during startup.
- Lazy Loading: Populate the cache only when data is requested.

## **Cache Stampede**

Occurs when multiple requests simultaneously query the backend because of a cache miss or expired data.

Solutions:

- Request Coalescing: Allow only one request to fetch the data and populate the cache while others wait.
- Locking Mechanisms: Use distributed locks to prevent multiple backend requests.

### **Cache Penetration**

Frequent requests for non-existent keys lead to unnecessary load on the backend.

Solutions:

- Negative Caching: Cache responses for non-existent keys with a short TTL.
- Input Validation: Validate and filter invalid or nonsensical requests at the application level.

### **Cache Avalanche**

A large number of cache keys expire simultaneously, causing a sudden surge in backend load.

Solutions:

- Staggered Expiration: Randomize TTLs for cache entries to prevent mass expirations.
- Pre-Warming: Gradually refill the cache with critical data before expiration.
- Backup Caching: Maintain a secondary lower-priority cache to serve requests if the primary cache is unavailable.

## Horizontal and Vertical Scaling

### Vertical Scaling (Scaling Up)

Adding more resources (e.g., CPU, RAM, or storage) to an existing cache server.

1. **Increase Memory:**
    - Allocate more RAM to the cache server to store a larger dataset.
    - E.g., upgrading a server from 16 GB to 64 GB RAM.
2. **Upgrade Hardware:**
    - Use faster processors for lower latency.
    - Use SSDs for faster read/write operations if persistent caching is required.
3. **Drawbacks:**
    - Limited by the physical capacity of a single machine (e.g., memory or CPU limits).
    - A single point of failure: if the cache server goes down, the entire system is impacted.
    - Scaling stops once the server's maximum capacity is reached.

### Horizontal Scaling (Scaling Out)

Adding more cache servers to distribute the load across multiple nodes.

1. **Partitioning Data:**
    - Use **sharding** to distribute data across multiple servers.
    - **Consistent Hashing:** Ensures even data distribution and minimal data movement when nodes are added/removed.
    - E.g., Redis Cluster automatically partitions data based on hash slots.
2. **Replication:**
    - Replicate data across multiple servers for fault tolerance.
    - E.g., Redis Master-Slave replication.
3. **Load Balancing:**
    - Use a load balancer to distribute client requests evenly across cache nodes.
    - Load balancers or client-side libraries (e.g., Redis Sentinel) manage which cache node to query.
4. **Dynamic Node Management:**
    - Add or remove nodes dynamically to adjust to changing traffic patterns.
    - E.g., Elasticache on AWS allows auto-scaling of Redis clusters.

**Benefits:**

- Infinite scalability (theoretically) by adding more nodes.
- Fault tolerance: if one node fails, others can handle the load.
- Avoids single points of failure.

**Drawbacks:**

- Increased complexity in managing distributed data.
- Potential for **data consistency issues** across nodes.
- **Network latency** may increase when accessing remote nodes.

## Scaling cache from 100 to 1 billion users

### **Phase 1: Small Scale (100–10,000 Users)**

**Characteristics:** Low traffic, limited budget, simple architecture.

**Goals:** Improve latency and reduce load on the backend with minimal complexity.

1. **Introduce Basic Caching:**
    - Use **in-memory caching** on the application server (e.g., local cache like Guava in Java or in-memory dictionaries in Python).
    - Cache **frequently accessed static data** (e.g., configurations, static metadata).
2. **Caching Strategy:**
    - Use a **Cache Aside** strategy to populate the cache on demand.
    - Set a **Time-to-Live (TTL)** to prevent stale data.
3. **Data to Cache:**
    - Static assets like configuration files or commonly used lookups (e.g., country codes).
    - User session data for faster authentication.

### **Phase 2: Medium Scale (10,000–1 Million Users)**

**Characteristics:** Increased traffic, higher database queries, more dynamic data.

**Goals:** Reduce database load, handle dynamic content efficiently, and scale horizontally.

1. **Move to a Centralized Cache:**
    - Deploy a centralized **remote in-memory caching solution** like Redis or Memcached.
    - This ensures multiple application servers can share the same cache.
2. **Caching Layers:**
    - **Frontend Caching:** Use Content Delivery Networks (CDNs) for static assets like images, JavaScript, CSS.
    - **Database Query Results:** Cache frequently accessed query results (e.g., top 10 trending posts).
3. **Caching Strategies:**
    - Use a combination of **Cache Aside** and **Write Through** for dynamic data.
    - Introduce **negative caching** to handle invalid requests.
4. **Scalability Considerations:**
    - Horizontal scaling of caching servers.
    - Monitor cache performance (hit/miss rates, memory usage).

### **Phase 3: Large Scale (1 Million–100 Million Users)**

**Characteristics:** Diverse user base, higher concurrency, more complex data flows.

**Goals:** Maintain low latency, high availability, and support geographically distributed users.

1. **Distributed Caching:**
    - Use a **distributed caching system** (e.g., Redis Cluster) to handle large-scale data storage and fault tolerance.
    - Shard the cache data to scale horizontally.
2. **Geographical Caching:**
    - Deploy **regional caching** layers to bring data closer to users (e.g., edge caches).
    - Use a multi-region setup for global coverage.
3. **Caching Layers:**
    - **Application-Level Caching:** Store computed data (e.g., processed API responses).
    - **Database Query Caching:** Cache joins, aggregations, and other expensive queries.
    - **Session and User Data Caching:** Use distributed caches for session management (e.g., Redis with sticky sessions).
4. **Consistency Management:**
    - Use eviction policies like **Least Recently Used (LRU)** to manage memory efficiently.
    - Implement strategies for **cache invalidation** and **cache warming.**

### **Phase 4: Hyper Scale (100 Million–1 Billion Users)**

**Characteristics:** Massive scale, diverse workloads, high availability requirements.

**Goals:** Minimize latency globally, optimize cost, and ensure resilience.

1. **Global Caching Infrastructure:**
    - Use **Content Delivery Networks (CDNs)** to cache static and semi-dynamic content closer to users.
    - Implement **global distributed caching** for user-specific data with geo-replication.
2. **Hierarchical Caching:**
    - **Edge Caching:** Cache content at the edge (e.g., Cloudflare, Akamai).
    - **Regional Caching:** Deploy region-specific caches for localized data.
    - **Central Caching:** Use a central distributed cache for global synchronization.
3. **Advanced Techniques:**
    - **Read-Through and Write-Back Strategies:** Improve consistency and reduce backend writes.
    - **Dynamic Data Caching:** Use AI/ML to predict and pre-cache popular data.
    - **Partitioning and Sharding:** Scale horizontally by partitioning cache keys.
4. **Monitoring and Resilience:**
    - Use observability tools to monitor hit rates, latency, and resource usage.
    - Employ fault-tolerant systems (e.g., multi-master Redis clusters).

### **Final Architecture for 1 Billion Users**

| **Layer** | **Type of Cache** | **Example Use Cases** |
| --- | --- | --- |
| **Frontend Layer** | CDN Cache | Images, videos, CSS, and JS assets. |
| **Regional Layer** | Geo-Distributed Cache | User-specific data, local trending data. |
| **Application Layer** | In-Memory Distributed Cache (e.g., Redis) | API responses, session data, computed results. |
| **Database Layer** | Query Cache | Expensive queries, aggregations. |

## Drawback to caching at every level of a system/architecture?

Caching at every level of a system or architecture may sound like an effective strategy for performance optimization, but it introduces several drawbacks and challenges.

1. Increased Complexity: Synchronizing data across caches at different levels is challenging. Harder to debug issues (e.g., determining which layer served stale or incorrect data). Maintenance becomes more time-consuming and error-prone.
2. Risk of Data Inconsistency: Different layers may hold outdated or conflicting versions of the same data. Cache invalidation and synchronization across levels can fail. Leads to inconsistencies between the cache and the source of truth (e.g., database). Clients may receive incorrect or stale data.
3. Higher Operational Overhead: Multiple caching layers require monitoring, maintenance, and tuning. Each layer may have distinct tools, configurations, and metrics. Increased cost of infrastructure and personnel expertise.
4. Memory and Storage Overhead: Storing similar or redundant data in multiple caches wastes memory. May require significant hardware resources or cloud budgets. Higher costs without proportional performance benefits.
5. Cache Eviction Conflicts: Different eviction policies at different layers can lead to unpredictable behavior. Leads to unnecessary cache misses and backend load.
6. Cache Stampede and Redundant Fetches: Multiple layers may independently attempt to fetch the same data from the backend after a cache miss. Backend experiences a sudden surge in traffic.
7. Difficulty in Cache Optimization: Fine-tuning TTLs, eviction policies, and caching strategies for each layer is non-trivial.
8. Increased Debugging Difficulty: Debugging application behavior becomes harder when caches at different layers behave unexpectedly. More effort is required to identify the root cause of issues.

## Qs for an Interview

### Basic Understanding of Caching

1. When would you choose not to use a cache in a system?
2. Explain the difference between strong consistency, eventual consistency, and cache consistency. How would you handle consistency in a caching layer?
3. How do you decide what data to cache in a system?

### Caching Strategies and Policies

1. What are the differences between write-through, write-back, and write-around caching? When would you use each?
2. How do you design a cache eviction policy for a system with limited memory?
3. Can you explain a situation where Least Recently Used (LRU) might not be the best eviction policy?

### Designing Caching Layers

1. How would you design a multi-layered caching system (e.g., CDN, in-memory cache, database cache) to optimize performance for a global user base?
2. What are the trade-offs between in-memory caches (like Redis) and disk-based caches (like CDN edge caches)?
3. How would you handle caching in a distributed system where nodes can go down or join dynamically?

### Cache Consistency and Invalidation

1. How would you design an efficient cache invalidation mechanism for a highly dynamic system where data changes frequently?
2. What are the challenges of maintaining cache consistency in distributed systems?
3. How would you implement cache invalidation for a multi-tenant application where users share common data?

### Handling Caching Challenges

1. How would you mitigate cache stampedes in a high-traffic system?
2. What strategies would you use to handle a cold start problem in a caching layer?
3. How do you prevent cache penetration in a system prone to malicious or random queries?
4. What steps would you take to avoid a cache avalanche when a popular cached item expires?

### Scalability and Performance

1. How would you scale a caching layer to handle a billion requests per second?
2. What are the limitations of horizontal scaling in distributed caches? How would you address them?
3. How would you measure and monitor the effectiveness of a caching layer in a system?

### Advanced Topics

1. What is cache warming, and why is it important? How would you implement it?
2. How does consistent hashing work, and why is it important for distributed caching systems?
3. What are the pros and cons of using a caching layer versus a database index for query optimization?
4. How would you implement caching in a microservices architecture with services communicating over HTTP or gRPC?
5. How do CDNs implement caching policies at the edge, and what challenges do they face in maintaining freshness for globally distributed content?

### Debugging and Real-World Scenarios

1. How would you debug a cache inconsistency issue in a multi-level caching system?
2. What would you do if a cache miss rate suddenly spikes in a production system?
3. Imagine a scenario where the caching layer is slowing down the system instead of speeding it up. What could cause this, and how would you resolve it?

### Scenario-Based Questions

1. Design a caching strategy for a real-time messaging application like WhatsApp. How would you handle ephemeral messages and ensure consistent delivery?
2. How would you use caching to improve the performance of a large-scale recommendation system?
3. Describe how you would design a caching solution for an e-commerce website where prices and inventory frequently change.

### Open-Ended Brainstorming

1. What are some innovative uses of caching beyond traditional use cases like reducing latency or database load?
2. How would you implement a cache for a system where some data must never be stale (e.g., financial transactions)?
3. What trade-offs would you make when designing a cache for a system with limited memory, highly variable workloads, and high fault-tolerance requirements?
