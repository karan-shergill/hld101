# Revision Table

| Topic | Learnings |
| --- | --- |
| [Relational Databases](https://github.com/karan-shergill/hld101/blob/main/notes/relational_database.md) | `Transaction`, `A:Atomicity`, `C:Consistency`, `I:Isolation`, `D:Durability`, `4 types of Isolation levels`, `Vertical Scaling`, `Horizontal Scaling`, `Replication`, `Sharding`, `Partitioning`, `Scaling DB with ACID`, `Indexing`, `Challenges in horizontally scaling RDBMS & how to fix it` |
| [Non Relational Databases](https://github.com/karan-shergill/hld101/blob/main/notes/non_relational_databases.md) | `SQL Vs NoSQL`, `Why Non-Relational Databases (NoSQL) scales well Horizontalally`, `ACID vs BASE`, `NoSQL DB & Joins`, `Types of NoSQL DBs`, `Partitioning`, `Sharding`, `Sharding in SQL Vs NoSQL DBs`, `Indexing` , `How to pick right NoSQL DB` |
| [Database Scaling Strategies](https://github.com/karan-shergill/hld101/blob/main/notes/database_scaling_strategies.md) | `Indexing`, `Materialized Views`, `Denormalization`, `Vertical scaling`, `Caching`, `Replication`, `Sharding`, `When to use which strategies` |
| [Cache](https://github.com/karan-shergill/hld101/blob/main/notes/cache.md) | `Why needed`, `Cache State`, `Caching Strategies`, `Cache Eviction Policies`, `Where to Use Caching in System Design`, `Caching Tools`, `Caching Challenges and How to Deal with Them`, `How to scale cache`, `Drawback to caching at every level of a system`, `Distributes Cache` |
| [Race Condition Solutions](https://github.com/karan-shergill/hld101/blob/main/notes/race_condition_solutions.md) | `Pessimistic CC`, `Optimistic CC`, `Compare-and-Swap (CAS)`, `Pessimistic vs Optimistic Locking (Code Level vs. Database Level)`, `Distributed lock using Redis`, `Redis Redlock Algo`, `Distributed lock using Zookeeper`, `Distributed lock using ETCD` |
| [Fetch Nearby Using Geospatial Data](https://github.com/karan-shergill/hld101/blob/main/notes/fetch_nearby_using_geospatial_data.md) | `Geospatial Data`, `Geohash`, `Quadtree`, `Google S2`, `Uber H3`, `Hilbert Curve`, `Z-order Curve` , `PostgreSQL + PostGIS`, `MySQL (InnoDB + Spatial Indexes)`, `MongoDB (Geospatial Indexes)`, `Redis (Geospatial Indexing)`, `Google BigQuery (GIS Functions)` |
| [Authentication & Authorisation](https://github.com/karan-shergill/hld101/blob/main/notes/authentication_and_authorisation.md) | `Cookies`, `Session (ID-based Authentication)`, `JWT (Token-based Authentication)`, `OAuth 2.0`, `OpenID Connect (OIDC)`, `Type of tokens`, `SAML`, `Single Sign-on (SSO)`, `LDAP`, `Authentication, Authorization` |
| [Redis](https://github.com/karan-shergill/hld101/blob/main/notes/redis.md) | `Redis vs. Traditional Databases`, `Why is Redis Single-Threaded`, `How is Redis so Fast`, `Redis Data Structures`, `Strings (Binary-Safe)`, `Lists (Ordered Collection)`, `Sets (Unique Collection, Unordered)`, `Hashes (Key-Value Store within Redis)`, `Sorted Sets (Ordered Collection with Scores)`, `Redis Replication - Single Node`, `Read-Only Mode in Replicas`, `Failover & Handling Master Failures`, `Redis Sentinel`, `Redis Clustering – Distributed Scaling`, `Working of Sharding (Data Partitioning)`, `How Does Redis Cluster Route Requests`, `Failover & Recovery in Redis Cluster`, `RDB (Redis Database Backup) working`, `AOF (Append-Only File) working`, `RDB vs AOF`, `Distributed Locking in Redis` , `Atomic Locking`, `Redlock Algorithm`, `Hot-Key Problem in Redis`, `Mitigation for Hot-Key Problem` |
| [Redis Use Cases](https://github.com/karan-shergill/hld101/blob/main/notes/redis_use_cases.md) | `Redis as a Cache`, `Caching Strategies`, `Eviction Policies`, `Redis as Session Storage`, `Redis for Rate Limiting`, `Rate Limiting Strategies in Redis - 4 types`, `Redis for Leaderboards`, `Redis for Counting (Views, Likes, Upvotes)`, `Redis for Distributed Locks`, `Redlock Algorithm`, `Redis for Geospatial Indexing`, `What are possible Geospatial Queries`, `Redis for Pub/Sub Messaging`, `Redis for Job Queues`, `Redis for Real-Time Analytics`, `Redis as a Primary Database` , `Implementation thought for each` |


# Topics I need to deep dive
1. Design User Rating and Review System
3. Bases on database size what desisions one takes
4. Choosing right database
5. How Search bar works with filters?
6. ~~Where all I can use Redis in my system?~~
7. ~~Good API design practice for interviews~~
8. Database keys
9. Database table design
