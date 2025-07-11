# 🏗️ High Level System Design Mastery

[![System Design](https://img.shields.io/badge/System%20Design-Expert%20Level-blue?style=for-the-badge&logo=architecture&logoColor=white)](https://github.com)
[![Distributed Systems](https://img.shields.io/badge/Distributed%20Systems-Advanced-green?style=for-the-badge&logo=network-wired&logoColor=white)](https://github.com)
[![Scalability](https://img.shields.io/badge/Scalability-Production%20Ready-orange?style=for-the-badge&logo=chart-line&logoColor=white)](https://github.com)

> **A comprehensive guide to mastering High Level System Design concepts, patterns, and best practices for building scalable, reliable, and maintainable distributed systems.**

## 🗂️ Table of Contents

- [🏗️ High Level System Design Mastery](#️-high-level-system-design-mastery)
  - [📊 Quick Overview](#-quick-overview)
  - [🗂️ Table of Contents](#️-table-of-contents)
  - [🔤 Core Terminologies](#-core-terminologies)
  - [🏛️ System Design Fundamentals](#️-system-design-fundamentals)
    - [🌐 Networking & Communication](#-networking--communication)
    - [⚖️ Load Management & Distribution](#️-load-management--distribution)
    - [🗄️ Data Storage & Management](#️-data-storage--management)
    - [🔗 Integration & Messaging](#-integration--messaging)
    - [🏗️ Architecture Patterns](#️-architecture-patterns)
    - [🔐 Security & Authentication](#-security--authentication)
    - [📡 Communication Protocols](#-communication-protocols)
    - [🛠️ Infrastructure & Deployment](#️-infrastructure--deployment)
    - [📈 Advanced Concepts](#-advanced-concepts)
  - [📚 Study Guide & Best Practices](#-study-guide--best-practices)

---

## 🔤 Core Terminologies

### 📈 **System Quality Attributes**
| 🎯 **Concept** | 📝 **Definition** | 🔗 **Resources** |
|-------------|------------------|------------------|
| **🔄 Availability** | The measure of a system's ability to be operational and accessible when required for use | [[Video](https://youtu.be/LdvduBxZRLs)] [[Article](https://blog.bytebytego.com/p/how-do-we-design-for-high-availability)] |
| **🛡️ Reliability** | The ability of a system to consistently perform its intended functions without failure over a specified period | [[Video](https://youtu.be/y_Orb2euMo0?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.prepbytes.com/blog/system-design/reliability-in-system-design/)] |
| **📊 Scalability** | The capability of a system to handle increased load or expand in capacity without compromising performance | [[Video](https://youtu.be/-CAuaHEJD8E?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.educative.io/answers/scalability-in-system-design)] |
| **🔄 Maintainability** | The ease with which a system can be updated, repaired, or improved over its lifecycle | [[Video](https://youtu.be/epf1eLbbBl8?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://medium.com/oolooroo/crafting-highly-maintainable-systems-practices-patterns-and-principles-part-1-163202fee62c)] |

### 🎯 **Performance Metrics**
| 🎯 **Concept** | 📝 **Definition** | 🔗 **Resources** |
|-------------|------------------|------------------|
| **⚡ Latency** | The time delay between a request being made and the corresponding response being received | [[Video](https://youtu.be/YSL8wJNLA58?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.enjoyalgorithms.com/blog/latency-in-system-design)] |
| **📊 Throughput** | The rate at which a system processes requests or completes tasks, typically measured in units per time | [[Video](https://youtu.be/IhemzDuCwgU)] [[Article](https://www.enjoyalgorithms.com/blog/throughput-in-system-design)] |
| **📈 Percentile Latency** | p99, p95, p50 latency representing performance at various percentiles of requests | [[Video](https://youtu.be/BrzhaXSEWy8)] [[Article](https://medium.com/@djsmith42/how-to-metric-edafaf959fc7)] |
| **📋 SLA, SLO & SLI** | Service Level Agreement, Objectives, and Indicators for measuring service quality | [[Video](https://youtu.be/HZwmF47OPMk?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.blameless.com/blog/sla-vs-slo)] |

### 🔧 **Data Consistency & Transactions**
| 🎯 **Concept** | 📝 **Definition** | 🔗 **Resources** |
|-------------|------------------|------------------|
| **🔄 Consistency** | The property ensuring all nodes have the same data at the same time across distributed systems | [[Video](https://youtu.be/e_HjDt6AA8s?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://medium.com/javarevisited/system-design-1-consistency-in-distributed-systems-a-fundamental-challenge-023616039819)] |
| **⚛️ Atomicity** | Operations within a transaction are completed entirely or not at all, maintaining consistency | [[Video](https://youtu.be/w4rGJ9QylNU?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.freecodecamp.org/news/acid-databases-explained/)] |
| **🔒 Isolation** | Transactions operate independently without interference from other concurrent transactions | [[Video](https://youtu.be/U-4tjScxc_Q?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.freecodecamp.org/news/acid-databases-explained/)] |
| **💾 Durability** | Once a transaction is committed, its changes are permanent and survive system failures | [[Video](https://youtu.be/FWuK7uhlYBs?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.freecodecamp.org/news/acid-databases-explained/)] |

### 🛡️ **Fault Handling & Recovery**
| 🎯 **Concept** | 📝 **Definition** | 🔗 **Resources** |
|-------------|------------------|------------------|
| **⚠️ Faults** | Defects or issues in system components that have potential to cause errors | [[Video](https://youtu.be/l1wBr-MZnPQ?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.baeldung.com/cs/distributed-systems-fault-failure)] |
| **💥 Failures** | Events where a system fails to perform its intended function, causing service disruption | [[Video](https://youtu.be/l1wBr-MZnPQ?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.baeldung.com/cs/distributed-systems-fault-failure)] |
| **🔄 Failover** | Automatic switch to backup system when primary system fails, ensuring continuous operation | [[Video](https://youtu.be/02pg0GJky9I?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://cloudpatterns.org/mechanisms/failover_system/)] |
| **⛓️ Cascading Failure** | Chain reaction of failures in interconnected parts leading to widespread system outage | [[Video](https://youtu.be/oVGOUh6-m4E?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.infoq.com/articles/anatomy-cascading-failure/)] |
| **🏥 Resiliency** | Ability to recover from failures and continue operating with minimal impact | [[Video](https://youtu.be/kS06YB_K8n0?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://insights.sei.cmu.edu/blog/system-resilience-what-exactly-is-it/)] |
| **🛡️ Fault Tolerance** | Ability to continue operating correctly even when components fail | [[Video](https://youtu.be/DNam6NbjgW4?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.cockroachlabs.com/blog/what-is-fault-tolerance/)] |

### 🔗 **System Integration & Communication**
| 🎯 **Concept** | 📝 **Definition** | 🔗 **Resources** |
|-------------|------------------|------------------|
| **⬆️⬇️ Upstream/Downstream** | Systems providing data (upstream) vs systems receiving data (downstream) | [[Video](https://youtu.be/XpZpJi4wEsY?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://reflectoring.io/upstream-downstream/)] |
| **🔄 Sync vs Async** | Real-time communication (sync) vs non-blocking communication (async) | [[Video](https://youtu.be/aCzZKlRL4q8?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://medium.com/inspiredbrilliance/patterns-for-microservices-e57a2d71ff9e)] |
| **⚖️ Fault Tolerance vs Resiliency** | Continuous operation despite faults vs recovery and adaptation after disruptions | [[Video](https://youtu.be/rpBPMFk1ah0?list=PLBgdlMZhIRmExB642Hb7JeW8aBCA-m3PS)] [[Article](https://www.ufried.com/blog/resilience_vs_fault_tolerance/)] |

---

## 🏛️ System Design Fundamentals

> **Essential concepts and patterns for building robust, scalable systems**

### 🌐 **Networking & Communication**

1. What is system design? Why is system design so important? [[link1](https://www.karanpratapsingh.com/courses/system-design/what-is-system-design)]
2. Internet Protocol(IP) [[link1](https://www.karanpratapsingh.com/courses/system-design/ip)] [[link2](https://youtu.be/pMLPG3-ulwY)]
    1. What is an IP address?
    2. Versions of IP address? IPv4 vs IPv6?
    3. Types of IP addresses? Static, Dynamic, Public & Private IP addresses?
3. Open System Interconnection (OSI) Model [[link1](https://www.karanpratapsingh.com/courses/system-design/osi-model)] [[link2](https://youtu.be/0y6FtKsg6J4)]
    1. What is the OSI model? Why it matters?
    2. What are the seven abstraction layers of the OSI model?
    3. Please Do Not Touch Steve's Pet Alligator!
4. TCP and UDP [[link1](https://www.karanpratapsingh.com/courses/system-design/tcp-and-udp)] [[link2](https://youtu.be/uwoD5YsGACg)] [[link3](https://youtu.be/P6SZLcGE4us)]
    1. Transmission Control Protocol (TCP)?
    2. User Datagram Protocol (UDP)?
    3. What is TCP Overhead?
    4. Use cases for TCP and UDP?
5. Domain Name System (DNS) [[link1](https://www.karanpratapsingh.com/courses/system-design/domain-name-system)] [[link2](https://youtu.be/27r4Bzuj5NQ)]
    1. How DNS works?
    2. Server types - DNS resolver, DNS root server, TLD nameserver & authoritative DNS server
    3. Query types - Recursive, Iterative & Non-recursive
    4. What are Subdomains, DNS Zones, DNS Caching & Reverse DNS?
6. Load balancing [[link1](https://www.karanpratapsingh.com/courses/system-design/load-balancing)] [[link2](https://youtu.be/chyZRNT7eEo)]
    1. What is Load balancing? What is a load balancer needed for?
    2. Load balancing in Layer 4 vs Layer 7? [[link1](https://youtu.be/aKMLgFVxZYk)]
    3. What are different Load balancing Routing Algorithms? Static vs Dynamic? [[link1](https://youtu.be/dBmxNsS3BGE)]
    4. How does the load balancer take care of when a server is down? [[link1](https://youtu.be/K0Ta65OqQkY)]
    5. What are other features offered by load balancers?
7. Proxy [[link1](https://www.karanpratapsingh.com/courses/system-design/proxy)]
    1. What is a proxy? Why it’s needed?
    2. What is Forward Proxy? Features and use cases?
    3. What is Reverse Proxy? Features and use cases?
    4. Load balancer vs Reverse Proxy? [[link1](https://youtu.be/4NB0NDtOwIQ)]
    5. Reverse Proxy vs API Gateway vs Load Balancer? [[link1](https://youtu.be/RqfaTIWc3LQ)]
8. Clustering [[link1](https://www.karanpratapsingh.com/courses/system-design/clustering)]
    1. What is Clustering?
    2. What are the different configurations of Clustering? Active-Active Vs Active-Passive? [[link1](https://youtu.be/d-Bfi5qywFo)]
    3. Load balancing vs Clustering?
9. Caching [[link1](https://www.karanpratapsingh.com/courses/system-design/caching)] [[link2](https://youtu.be/U3RkDLtS7uY)] [[link3](https://youtu.be/dGAgxozNWFE)]
    1. What are Cache hierarchy levels? L1, L2 & L3 difference? Cold, Warm, and Hot Caches?
    2. What are Cache hits and Cache misses?
    3. What is Cache Invalidation?
        1. Write-through cache? Pros and Cons?
        2. Write-around cache? Pros and Cons?
        3. Write-back cache? Pros and Cons?
    4. What are Cache Eviction policies? FIFO, LIFO, LRU, MRU, LFU & RR?
    5. Distributed Cache Vs Global Cache?
    6. Some common caching pitfalls and how to prohibit that from happening? [[link1](https://youtu.be/wh98s0XhMmQ)]
        1. Cache Stampede
        2. Cache Penetration
        3. Cache Avalanche
10. Content Delivery Network (CDN) [[link1](https://www.karanpratapsingh.com/courses/system-design/content-delivery-network)] [[link2](https://youtu.be/RI9np1LWzqw)]
    1. What is a CDN? Why use a CDN?
    2. How does a CDN work? What are Edge Servers?
    3. CDN types? Push CDNs vs Pull CDNs?
    4. What are the disadvantages of CDN?
11. Availability [[link1](https://www.karanpratapsingh.com/courses/system-design/availability)] [[link2](https://youtu.be/LdvduBxZRLs)] [[link3](https://youtu.be/USCCqS9MbHs)]
    1. What is Availability? What is Nine's of availability?
    2. How does Availability change in Sequence vs Parallel?
12. Scalability [[link1](https://www.karanpratapsingh.com/courses/system-design/scalability)] [[link2](https://youtu.be/dvRFHG2-uYs)]
    1. What is Scalability? When and why it’s needed?
    2. What is Vertical scaling? What are the advantages & disadvantages?
    3. What is Horizontal scaling? What are the advantages & disadvantages?
    4. How do you decide between Vertical scaling & Horizontal scaling?
13. Storage
14. Databases and DBMS [[link1](https://www.karanpratapsingh.com/courses/system-design/databases-and-dbms)] [[link2](https://youtu.be/hRulZhTtUTg?list=PLOspHqNVtKAD74tcRKW9FhETyKSaKxl3J)]
    1. What is Database and DBMS? Why it’s needed by any org at first place?
    2. What are the different components of databases? Schema, table, row & column.
15. SQL Relational databases [[link1](https://www.karanpratapsingh.com/courses/system-design/sql-databases)] [[link2](https://youtu.be/OqjJjpjDRLc)]
    1. What are relational databases? What is its structure?
    2. What is SQL? [[link1](https://youtu.be/27axs9dO7AE)] [[link2](https://youtu.be/zsjvFFKOm3c)]
    3. What is an index in a relational database? Why needed? How it works? [[link1](https://youtu.be/xXtig5uLQS4)]
16. NoSQL databases [[link1](https://www.karanpratapsingh.com/courses/system-design/nosql-databases)] [[link2](https://youtu.be/xQnIN9bW0og)]
    1. How do NoSQL databases work? [[link1](https://youtu.be/0buKQHokLK8?t=424)]
    2. What are different types of NoSQL databases and how to decide which to use? [[link1](https://youtu.be/VfcRxtBKI54)]
    3. How indexing work in non relational databases? [[link1](https://youtu.be/mTNkqMDCasI)]
17. SQL vs NoSQL databases [[link1](https://www.karanpratapsingh.com/courses/system-design/sql-vs-nosql-databases)] [[link2](https://youtu.be/ruz-vK8IesE)]
    1. How to choose the best database in a system design interview? [[link1](https://youtu.be/cODCpXtPHbQ)]
18. Database Replication [[link1](https://www.karanpratapsingh.com/courses/system-design/database-replication)] [[link2](https://youtu.be/bI8Ry6GhMSE)]
    1. What is database replication? Why it’s needed in first place?
    2. What is Master-Slave Replication? What are the pros and cons?
    3. What is Master-Master Replication? What are the pros and cons?
    4. What is leaderless replication? How it works? What are the pros and cons?
    5. What is Synchronous vs Asynchronous replication? [[link1](https://arpitbhayani.me/blogs/replication-strategies/)]
19. Indexes [[link1](https://www.karanpratapsingh.com/courses/system-design/indexes)]
20. Normalization and Denormalization [[link1](https://www.karanpratapsingh.com/courses/system-design/normalization-and-denormalization)]
    1. What are the different types of database keys? [[link1](https://youtu.be/_UZLrD_R0T4)] [[link2](https://www.guru99.com/dbms-keys.html)] [[link3](https://youtu.be/8wUUMOKAK-c)]
    2. What are different types of Dependencies? Functional, Fully-Functional, Transitive, Multivalued and Partial [[link1](https://www.javatpoint.com/dependency-in-dbms)]
    3. What are Anomalies? Give an example of update, insert, and delete Anomaly [[link1](https://www.scaler.com/topics/anomalies-in-dbms/)]
    4. What is Database Normalization - 1NF, 2NF, 3NF, 4NF, 5NF? [[link1](https://youtu.be/GFQaEYEc8_8)] [[link2](https://www.scaler.com/topics/dbms/normalization-in-dbms/)]
    5. What is the Boyce-Codd Normal Form (BCNF)? [[link1](https://youtu.be/VWnKUKH4tLg)]
    6. Pros and Cons of Database Normalization?
    7. What is Database Denormalization? What is its Pros & Cons? [[link1](https://youtu.be/4bTq0GdSeQs)] [[link2](https://www.scaler.com/topics/denormalization-in-dbms/)]
    8. Does denormalization mean reversing normalization? Justify
21. ACID and BASE consistency models [[link1](https://www.karanpratapsingh.com/courses/system-design/acid-and-base-consistency-models)]
    1. What is ACID? When to choose ACID? [[link1](https://youtu.be/GAe5oB742dw)] [[link2](https://www.freecodecamp.org/news/acid-databases-explained/)]
    2. What is BASE? When to choose BASE? [[link1](https://youtu.be/l8SoqbLNN7Q)]
    3. ACID vs BASE Trade-offs [[link1](https://aws.amazon.com/compare/the-difference-between-acid-and-base-database/)]
22. CAP Theorem [[link1](https://www.karanpratapsingh.com/courses/system-design/cap-theorem)] [[link2](https://youtu.be/BHqjEjzAicA)]
    1. What is CAP? Give a good example. [[link1](https://www.educative.io/blog/what-is-cap-theorem)]
    2. Which DBs fall under which combination?
    3. How is C(consistency) of ACID and CAP are different? [[link1](https://medium.com/@skeller88/cap-and-acid-8bbf9b45941)]
23. PACELC Theorem [[link1](https://www.karanpratapsingh.com/courses/system-design/pacelc-theorem)] [[link2](https://medium.com/distributed-systems-series/pacelc-theorem-explained-distributed-systems-series-9c509febb8f8)]
24. Transactions & its state [[link1](https://www.karanpratapsingh.com/courses/system-design/transactions)] [[link2](https://www.scaler.com/topics/transaction-state-in-dbms/)] [[link3](https://youtu.be/UkfWH3RHIJk)]
25. Distributed Transactions [[link1](https://www.karanpratapsingh.com/courses/system-design/distributed-transactions)]
    1. Why do we need distributed transactions?
    2. What is a Two-Phase commit? Pros and Cons?
    3. What is a Three-phase commit? Pros and Cons?
    4. What is a Sagas? Pros and Cons?
26. Sharding [[link1](https://www.karanpratapsingh.com/courses/system-design/sharding)] [[link2](https://youtu.be/5faMjKuB9bc)] [[link3](https://youtu.be/XP98YCr-iXQ)]
    1. What is Data Partitioning? [[link1](https://www.timescale.com/learn/data-partitioning-what-it-is-and-why-it-matters)]
    2. Horizontal Partitioning vs Vertical Partitioning? [[link1](https://blog.bytebytego.com/p/vertical-partitioning-vs-horizontal)]
    3. What is sharding? How is it different from  Partitioning? [[link1](https://youtu.be/wXvljefXyEo)]
    4. Partitioning criteria for sharding a DB?
    5. What are the advantages and disadvantages of Sharding?
    6. When to use sharding?
27. Consistent Hashing [[link1](https://www.karanpratapsingh.com/courses/system-design/consistent-hashing)] [[link2](https://highscalability.com/consistent-hashing-algorithm/)] [[link3](https://www.toptal.com/big-data/consistent-hashing)] [[link4](https://arpitbhayani.me/blogs/consistent-hashing/)] [[link5](https://youtu.be/zaRkONvyGr8)] [[link6](https://youtu.be/UF9Iqmg94tk)]
    1. Why do we need Consistent Hashing in the first place?
    2. How does Consistent Hashing work?
    3. List down Advantages & Disadvantages.
28. Database Federation [[link1](https://www.karanpratapsingh.com/courses/system-design/database-federation)]
29. N-tier architecture [[link1](https://www.karanpratapsingh.com/courses/system-design/n-tier-architecture)] [[link2](https://www.guru99.com/n-tier-architecture-system-concepts-tips.html)] [[link3](https://www.youtube.com/watch?v=xJC7ItRoEbw)]
    1. What are the different types of N-Tier architectures?
    2. What are the advantages of the different types of N-Tier architectures?
30. Message Brokers [[link1](https://www.karanpratapsingh.com/courses/system-design/message-brokers)] [[link2](https://www.ibm.com/topics/message-brokers)] [[link3](https://hasithas.medium.com/introduction-to-message-brokers-c4177d2a9fe3)] [[link4](https://www.youtube.com/watch?v=GDeK8sBwIug)] [[link5](https://www.youtube.com/watch?v=RMph4uSdDEM)]
    1. What is Message Brokers? Why it’s needed at 1st place?
    2. What are the different types of Message broker models?
    3. Message brokers vs. APIs? When to use what?
    4. Message brokers vs. event streaming platforms? What’s the difference?
    5. What are Message broker use cases?
31. Message Queues [[link1](https://www.karanpratapsingh.com/courses/system-design/message-queues)] [[link2](https://blog.bytebytego.com/p/why-do-we-need-a-message-queue)] [[link3](https://www.youtube.com/watch?v=oUJbuFMyBDk)]
    1. What are Message Queues? Why is it needed in 1st place?
    2. Explain how a message queue works with an example.
    3. What are the Benefits of Message Queues?
32. Publish-Subscribe [[link1](https://www.karanpratapsingh.com/courses/system-design/publish-subscribe)] [[link2](https://aws.amazon.com/what-is/pub-sub-messaging/)] [[link3](https://youtu.be/algmP8MGeL4)]
    1. What is pub/sub messaging?
    2. How does pub/sub messaging work?
    3. What are the features & benefits of a pub/sub-messaging system?
    4. What are the use cases of pub/sub messaging?
    5. What is the difference between message queues and pub/sub messaging? [[link1](https://systemdesignschool.io/blog/message-queue-vs-pub-sub)]
33. Enterprise Service Bus (ESB) [[link1](https://www.karanpratapsingh.com/courses/system-design/enterprise-service-bus)] [[link2](https://www.youtube.com/watch?v=eVrgMZH2jNY)] [[link3](https://aws.amazon.com/what-is/enterprise-service-bus/)]
    1. What is Enterprise Service Bus? What are the benefits?
    2. How does enterprise service bus work?
    3. What technologies are replacing enterprise service buses?
    4. What is an event bus?
34. Monoliths and Microservices [[link1](https://www.karanpratapsingh.com/courses/system-design/monoliths-microservices)] [[link2](https://www.atlassian.com/microservices/microservices-architecture/microservices-vs-monolith)] [[link3](https://youtu.be/NdeTGlZ__Do)] [[link4](https://youtu.be/lTAcCNbJ7KE)]
    1. What are Monoliths? List down its advantages & disadvantages
    2. What are Microservices? List down its advantages & disadvantages
    3. How to decide bewteen Monoliths & Microservices
    4. Benefits of having Monoliths over Microservices
    5. Benefits pf having Microservices over Monoliths
    6. What are distributed monolith? [[link1](https://medium.com/simpplr-technology/microservices-architecture-the-hard-parts-trap-of-distributed-monolith-7d707858aa32)] [[link2](https://www.gremlin.com/blog/is-your-microservice-a-distributed-monolith)]
    7. What are Service-oriented architecture (SOA)? [[link1](https://medium.com/@SoftwareDevelopmentCommunity/what-is-service-oriented-architecture-fa894d11a7ec)] [[link2](https://www.ibm.com/topics/soa)] [[link3](https://aws.amazon.com/what-is/service-oriented-architecture/)]
35. Moving from Monoliths to Microservices [[link1](https://youtu.be/rckfN7xFig0)] [[link2](https://insights.sei.cmu.edu/blog/8-steps-for-migrating-existing-applications-to-microservices/)] [[link3](https://medium.com/@puneet.vyas/how-to-migrate-from-monolithic-to-microservices-part-i-bd0645a7c529)]
36. Event-Driven Architecture (EDA) [[link1](https://www.karanpratapsingh.com/courses/system-design/event-driven-architecture)] [[link2](https://youtu.be/o2HJCGcYwoU)] [[link3](https://youtu.be/rJHTK2TfZ1I)] [[link4](https://www.ibm.com/topics/event-driven-architecture)]
    1. Event-Driven Architecture (EDA) vs Request/Response (RR)? [[link1](https://youtu.be/7fkS-18KBlw)] [[link2](https://www.designgurus.io/answers/detail/what-is-event-driven-architecture-vs-request-driven-architecture)]
    2. Events Vs Messages [[link1](https://medium.com/simpplr-technology/event-driven-architecture-the-hard-parts-events-vs-messages-0fcfc7243703)]
37. Event Sourcing [[link1](https://www.karanpratapsingh.com/courses/system-design/event-sourcing)] [[link2](https://www.youtube.com/watch?v=wPwD9CQAGsk)] [[link3](https://www.youtube.com/playlist?list=PLa7VYi0yPIH1TXGUoSUqXgPMD2SQXEXxj)] [[link4](https://www.eventstore.com/event-sourcing)]
    1. What is Event Sourcing? Pros & Cons?
    2. In what use-case we should use Event Sourcing?
    3. Event Sourcing vs Event-Driven Architecture (EDA) [[link1](https://www.youtube.com/watch?v=y1KJITitFA8)] [[link2](https://cloudnative.ly/event-driven-architectures-edas-vs-event-sourcing-c8582578e87)]
38. Command and Query Responsibility Segregation (CQRS) [[link1](https://www.karanpratapsingh.com/courses/system-design/command-and-query-responsibility-segregation)] [[link2](https://henriquesd.medium.com/the-command-and-query-responsibility-segregation-cqrs-pattern-16cb7704c809)] [[link3](https://www.youtube.com/watch?v=lg6aF5PP4Tc&t=45s)]
    1. What is Command and Query Responsibility Segregation (CQRS)? Why use it?
    2. Use-cases of Command and Query Responsibility Segregation (CQRS)
39. API Gateway [[link1](https://www.karanpratapsingh.com/courses/system-design/api-gateway)]
    1. What is an API Gateway and Why is it Useful? [[link1](https://www.freecodecamp.org/news/what-are-api-gateways/)] [[link2](https://youtu.be/6ULyxuHKxg8)] [[link3](https://youtu.be/RbMxB_Cyx6A)]
    2. Reverse Proxy vs API Gateway vs Load Balancer [[link1](https://www.youtube.com/watch?v=RqfaTIWc3LQ)] [[link2](https://youtu.be/UX11tVIkYUg)] [[link3](https://medium.com/geekculture/load-balancer-vs-reverse-proxy-vs-api-gateway-e9ec5809180c)] [[link4](https://www.linkedin.com/pulse/load-balancer-vs-reverse-proxy-api-gateway-rishikesh-roy/)] [[link5](https://medium.com/codenx/load-balancer-vs-reverse-proxy-vs-api-gateway-fcb79912abbf)]
    3. API Gateway v Service Mesh [[link1](https://youtu.be/31keFtODPvs)] [[link2](https://medium.com/microservices-in-practice/service-mesh-vs-api-gateway-a6d814b9bf56)] [[link3](https://www.getambassador.io/blog/microservices-discovery-api-gateway-vs-service-mesh)]
40. Backend For Frontend (BFF) pattern [[link1](https://youtu.be/GCx0aouuEkU)] [[link2](https://dzone.com/articles/backend-for-frontend-bff-pattern)] [[link3](https://medium.com/mobilepeople/backend-for-frontend-pattern-why-you-need-to-know-it-46f94ce420b0)] [[link4](https://aws.amazon.com/blogs/mobile/backends-for-frontends-pattern/)]
    1. What is Backend for Frontend Pattern? Why needed at 1st place?
    2. Pros & Cons for Backend for Frontend Pattern? How to decide where to use this or not? What are the trade offs?
41. Application Programming Interface(API)
    1. What are APIs? How they work? [[link1](https://youtu.be/bxuYDT-BWaI)] [[link2](https://www.postman.com/what-is-an-api/)]
    2. How to API Design? What are best practices? [[link1](https://youtu.be/FqljO9B5grM)] [[link2](https://youtu.be/_gQaygjm_hg)] [[link3](https://youtu.be/_YlYuNMTCc8)] [[link4](https://medium.com/@techworldwithmilan/rest-api-design-best-practices-2eb5e749d428)]
    3. What is Swagger? Why to use? [[link1](https://blog.hubspot.com/website/what-is-swagger)]
    4. API vs. SDK [[link1](https://youtu.be/kG-fLp9BTRo)] [[link2](https://aws.amazon.com/compare/the-difference-between-sdk-and-api/)] [[link3](https://sendbird.com/developer/tutorials/sdk-vs-api-the-similarities-and-differences-between-an-sdk-and-api)]
42. API technologies [[link1](https://www.karanpratapsingh.com/courses/system-design/rest-graphql-grpc)]
    1. SOAP APIs & everything about it [[link1](https://blog.postman.com/soap-api-definition/)] [[link2](https://stoplight.io/api-types/soap-api)] [[link3](https://youtu.be/it8ybkQuAh8)]
    2. REST APIs & everything about it [[link1](https://youtu.be/lsMQRaeKNDk)] [[link2](https://youtu.be/-mN3VyJuCjM)] [[link3](https://aws.amazon.com/what-is/restful-api/)] [[link4](https://restfulapi.net/)]
    3. SOAP vs. REST APIs [[link1](https://aws.amazon.com/compare/the-difference-between-soap-rest/)] [[link2](https://blog.hubspot.com/website/rest-vs-soap)] [[link3](https://blog.postman.com/soap-vs-rest/)]
    4. GraphQL & everything about it [[link1](https://youtu.be/eIQh02xuVw4)] [[link2](https://youtu.be/qgdiLcD2RL8)] [[link3](https://graphql.org/)] [[link4](https://www.apollographql.com/blog/what-is-graphql-introduction)] [[link5](https://hasura.io/learn/graphql/intro-graphql/what-is-graphql/)]
    5. GraphQL vs. REST APIs [[link1](https://youtu.be/PTfZcN20fro)] [[link2](https://youtu.be/yWzKJPw_VzM)] [[link3](https://hygraph.com/blog/graphql-vs-rest-apis)] [[link4](https://hasura.io/learn/graphql/intro-graphql/graphql-vs-rest/)]
    6. gRPC & everything about it [[link1](https://youtu.be/gnchfOojMk4)] [[link2](https://youtu.be/hVrwuMnCtok)] [[link3](https://www.freecodecamp.org/news/what-is-grpc-protocol-buffers-stream-architecture/)] [[link4](https://www.wallarm.com/what/the-concept-of-grpc)]
    7. gRPC vs. REST [[link1](https://aws.amazon.com/compare/the-difference-between-grpc-and-rest/)] [[link2](https://www.toptal.com/grpc/grpc-vs-rest-api)]
    8. Comparing API Architectural Styles SOAP vs REST vs GraphQL vs RPC [[link1](https://youtu.be/4vLxWqE94l4)] [[link2](https://youtu.be/NFw0HznpLlM)] [[link3](https://youtu.be/veAb1fSp1Lk)] [[link4](https://www.altexsoft.com/blog/soap-vs-rest-vs-graphql-vs-rpc/)] [[link5](https://www.velvetech.com/blog/graphql-vs-rest-choose-api/)]
43. Communication on Web [[link1](https://youtu.be/8ksWRX4xV-s)] [[link2](https://www.karanpratapsingh.com/courses/system-design/long-polling-websockets-server-sent-events)] [[link3](https://stackoverflow.com/questions/11077857/what-are-long-polling-websockets-server-sent-events-sse-and-comet)] [[link4](https://hyperskill.org/learn/step/42945)]
    1. What is Short Polling? What are its pros & cons? When to use it?
    2. What is Long Polling? What are its pros & cons? When to use it?
    3. Long Polling vs Short Polling [[link1](https://medium.com/@anoopnayak1/exploring-real-time-communication-in-web-development-short-polling-vs-long-polling-ec571f5e8af8)] [[link2](https://www.svix.com/resources/faq/long-polling-vs-short-polling/)]
    4. What is WebSocket? How its different from HTTP? What are its pros & cons? When to use it? [[link1](https://youtu.be/favi7avxIag)] [[link2](https://youtu.be/7WQ2EbXLfLI)] [[link3](https://www.ramotion.com/blog/what-is-websocket/)] [[link4](https://youtu.be/pnj3Jbho5Ck)] [[link5](https://youtu.be/G0_e02DdH7I)]
    5. What is Server-Sent Events (SSE)? What are its pros & cons? When to use it? [[link1](https://youtu.be/4HlNv1qpZFY)] [[link2](https://youtu.be/KkI3yHjKjqs)] [[link3](https://ably.com/topic/server-sent-events)] [[link4](https://www.pubnub.com/guides/server-sent-events/)]
    6. WebSockets vs Server-Sent Events [[link1](https://softwaremill.com/sse-vs-websockets-comparing-real-time-communication-protocols/)] [[link2](https://ably.com/blog/websockets-vs-sse)]
    7. What is WebHook? Why its needed at 1st place? When to use it? [[link1](https://youtu.be/mrkQ5iLb4DM)] [[link2](https://youtu.be/oQaJn6RdA3g)] [[link3](https://youtu.be/x_jjhcDrISk)] [[link4](https://mailchimp.com/marketing-glossary/webhook/)] [[link5](https://dorik.com/blog/what-is-webhook)] [[link6](https://medium.com/hookdeck/webhooks-explained-what-are-webhooks-and-how-do-they-work-90c613c055a4)]
44. Geohashing and Quadtrees [[link1](https://www.karanpratapsingh.com/courses/system-design/geohashing-and-quadtrees)]
    1. What is Geohashing? What are its use-cases? [[link1](https://youtu.be/UaMzra18TD8)] [[link2](https://www.pubnub.com/guides/what-is-geohashing/)] [[link3](https://medium.com/@bkawk/geohashing-20b282fc9655)] [[link4](https://geohash.softeng.co/)]
    2. What is Quadtrees? What are its use-cases? [[link1](https://youtu.be/OcUKFIjhKu0)] [[link2](https://www.youtube.com/watch?v=jxbDYxm-pXg)] [[link3](https://medium.com/@waleoyediran/spatial-indexing-with-quadtrees-b998ae49336)] [[link4](https://www.educative.io/answers/what-is-a-quadtree-how-is-it-used-in-location-based-services)]
45. Circuit breaker [[link1](https://www.karanpratapsingh.com/courses/system-design/circuit-breaker)] [[link2](https://medium.com/@minadev/circuit-breaker-pattern-in-microservices-9568320f2059)] [[link3](https://youtu.be/HRS9mIfiNn4)] [[link4](https://youtu.be/dJI2saoM5_k)]
46. Rate Limiting [[link1](https://www.karanpratapsingh.com/courses/system-design/rate-limiting)] [[link2](https://youtu.be/eR66m7TaV5A)] [[link3](https://youtu.be/CW4gVlU0xtU)] [[link4](https://blog.bytebytego.com/p/rate-limiting-fundamentals)]
    1. What is a Rate Limiter? Why its needed at 1st place?
    2. What are different algorithems, that can be used for Rate Limiting? [[link1](https://medium.com/@patrikkaura/the-fundamentals-of-rate-limiting-how-it-works-and-why-you-need-it-fd86d39e358d)]
47. Service Discovery [[link1](https://www.karanpratapsingh.com/courses/system-design/service-discovery)] [[link2](https://middleware.io/blog/service-discovery/)] [[link3](https://www.youtube.com/watch?v=3UDJvF0CMkQ)]
    1. What is Service Discovery? Why its needed at 1st place?
    2. Client-Side Vs Server-Side Service Discovery? Pros and Cons for both
    3. How to implement Service Discovery?
    4. What are different Service Registration Options?
48. Service Mesh [[link1](https://youtu.be/QiXK0B9FhO0)] [[link2](https://www.youtube.com/watch?v=nnxWMhy0mpA)] [[link3](https://aws.amazon.com/what-is/service-mesh/)] [[link4](https://medium.com/microservices-in-practice/service-mesh-for-microservices-2953109a3c9a)] [[link5](https://www.dynatrace.com/news/blog/what-is-a-service-mesh/)]
    1. What is a Service Mesh? Why its needed at 1st place?
    2. What are pros and cons of Service Mesh?
49. Disaster Recovery [[link1](https://www.karanpratapsingh.com/courses/system-design/disaster-recovery)] [[link2](https://medium.com/walmartglobaltech/business-continuity-disaster-recovery-in-the-microservices-world-ef2adca363df)] [[link3](https://medium.com/@tanmays309/the-bac-theorem-disaster-recovery-in-a-microservices-architecture-5ce61617ef64)] [[link4](https://www.youtube.com/watch?v=07EHsPuKXc0)]
50. Virtual Machines (VMs) and Containers [[link1](https://www.karanpratapsingh.com/courses/system-design/virtual-machines-and-containers)] [[link2](https://youtu.be/Jz8Gs4UHTO8)]
    1. What is Bare Metal?
    2. What is Virtual Machines (VM)? Why use a Virtual Machine at 1st place?
    3. What is a Hypervisor?
    4. What are Containers? Why do we need containers?
    5. Virtualization vs Containerization [[link1](https://www.atlassian.com/microservices/cloud-computing/containers-vs-vms)] [[link2](https://youtu.be/cjXI-yxqGTI)]
51. OAuth 2.0 and OpenID Connect (OIDC) [[link1](https://www.karanpratapsingh.com/courses/system-design/oauth2-and-openid-connect)] [[link2](https://developer.okta.com/docs/concepts/oauth-openid/)]
    1. What is OAuth 2.0? How it works? [[link1](https://www.youtube.com/watch?v=t18YB3xDfXI)]
    2. What is OIDC? How it works? [[link1](https://youtu.be/PsbIGfvX900)]
52. Single Sign-On (SSO) [[link1](https://www.karanpratapsingh.com/courses/system-design/single-sign-on)]
    1. What is SAML? [[link1](https://youtu.be/4ULlJEupV-I)]
    2. LDAP vs SAML: What's the Difference? [[link1](https://youtu.be/_NCcLJin30E)] [[link2](https://www.strongdm.com/blog/saml-vs-ldap)]
    3. What is SSO? How does SSO work? [[link1](https://www.youtube.com/watch?v=O1cRJWYF-g4)] [[link2](https://youtu.be/cD6pkL020q4)] [[link3](https://www.okta.com/blog/2021/02/single-sign-on-sso/)]
    4. SAML vs OAuth 2.0 and OpenID Connect (OIDC) [[link1](https://www.okta.com/identity-101/whats-the-difference-between-oauth-openid-connect-and-saml/)] [[link2](https://medium.com/ucsc-isaca-student-group/openid-vs-oauth-vs-saml-understanding-the-key-differences-b060d5bc2487)]
53. HTTP, HTTPS, SSL & TLS [[link1](https://www.karanpratapsingh.com/courses/system-design/ssl-tls-mtls)]
    1. HTTP/1 to HTTP/2 to HTTP/3 [[link1](https://youtu.be/a-sBfyiXysI)]
    2. SSL, TLS, HTTPS [[link1](https://youtu.be/j9QmMEWmcfo)]
    3. How SSL Certificate Works? [[link1](https://youtu.be/0yw-z6f7Mb4)]
54. Bloom Filters [[link1](https://systemdesign.one/bloom-filters-explained/)] [[link2](https://www.youtube.com/watch?v=V3pzxngeLqw)] [[link3](https://www.youtube.com/watch?v=Bay3X9PAX5k)] [[link4](https://www.youtube.com/watch?v=kfFacplFY4Y)]
    1. What is a Bloom filter? What is it used for?
    2. Use cases for bloom filter usage?
    3. Implementation in Java [[link1](https://richardstartin.github.io/posts/building-a-bloom-filter-from-scratch)] [[link2](https://www.baeldung.com/guava-bloom-filter)]
55. Lamport Clocks And Vector Clocks [[link1](https://medium.com/@balrajasubbiah/lamport-clocks-and-vector-clocks-b713db1890d7)] [[link2](https://medium.com/big-data-processing/vector-clocks-182007060193)] [[link3](https://flurryhead.medium.com/vector-clock-applications-965677624b94)]
56. **🌳 Merkle Trees** [[Video1](https://www.youtube.com/watch?v=qHMLy5JjbjQ)] [[Video2](https://www.youtube.com/watch?v=rsx1nt2bxf8)] [[Article1](https://www.codementor.io/blog/merkle-trees-5h9arzd3n8)] [[Article2](https://medium.com/coinmonks/merkle-trees-the-backbone-of-distributed-software-4fa0805132c6)]

---

## 📚 Study Guide & Best Practices

#### 🧠 **System Design Interview Strategy**

```
1. 📋 Requirements Gathering (5 minutes)
   ├── Functional requirements
   ├── Non-functional requirements  
   ├── Scale estimation
   └── Constraints and assumptions

2. 🎨 High-Level Design (10 minutes)
   ├── Core components identification
   ├── API design
   ├── Database schema
   └── System architecture diagram

3. 🔧 Detailed Design (15 minutes)
   ├── Component deep-dive
   ├── Data flow explanation
   ├── Algorithm choices
   └── Technology selections

4. 🎯 Scale & Optimize (10 minutes)
   ├── Bottleneck identification
   ├── Scaling strategies
   ├── Performance optimizations
   └── Monitoring and metrics
```

### 🏆 **Best Practices & Design Principles**

#### ⚡ **Performance Optimization**
- **🔍 Identify Bottlenecks Early**: Profile and monitor system performance
- **📊 Optimize for Actual Usage**: Design based on real traffic patterns
- **💾 Cache Strategically**: Implement multi-level caching where appropriate
- **🔄 Async Where Possible**: Use asynchronous processing for non-critical paths

#### 🛡️ **Reliability & Resilience**
- **🎯 Design for Failure**: Assume components will fail and plan accordingly
- **🔄 Implement Circuit Breakers**: Prevent cascade failures
- **📈 Gradual Rollouts**: Deploy changes incrementally
- **🔍 Comprehensive Monitoring**: Monitor all critical system metrics

#### 📈 **Scalability Guidelines**
- **📊 Horizontal > Vertical**: Prefer scaling out over scaling up
- **🔄 Stateless Services**: Design services to be stateless when possible
- **📝 Database Partitioning**: Use sharding for large datasets
- **⚖️ Load Distribution**: Distribute load evenly across services

### 🛠️ **Common System Design Patterns**

| 🎨 **Pattern** | 🎯 **Use Case** | 📝 **Description** |
|------------|--------------|------------------|
| **🔄 CQRS** | Read/Write Separation | Separate command and query responsibilities |
| **📅 Event Sourcing** | Audit Trail | Store events rather than current state |
| **🏗️ Microservices** | Service Decomposition | Break monolith into smaller services |
| **🌐 API Gateway** | Service Aggregation | Single entry point for client requests |
| **💾 Database per Service** | Data Isolation | Each service owns its data |
| **🔄 Saga Pattern** | Distributed Transactions | Manage long-running transactions |

### 📊 **Key Metrics to Consider**

#### 🎯 **Performance Metrics**
- **Latency**: p95, p99 response times
- **Throughput**: Requests per second (RPS)
- **Availability**: Uptime percentage
- **Error Rate**: Percentage of failed requests

#### 💰 **Business Metrics**
- **Cost per Transaction**: Infrastructure cost efficiency
- **Development Velocity**: Time to market for features
- **Operational Overhead**: Maintenance and support costs

### 🎓 **Interview Success Tips**

1. **🎯 Ask Clarifying Questions**: Don't assume requirements
2. **📊 Think Out Loud**: Explain your thought process
3. **🔄 Iterate and Improve**: Start simple, then add complexity
4. **📈 Consider Trade-offs**: Discuss pros and cons of decisions
5. **🛡️ Address Failure Scenarios**: Show you think about edge cases
6. **💬 Communicate Clearly**: Use diagrams and clear explanations

---

<div align="center">
  <h3>🏗️ Build Systems That Scale! Design with Purpose, Engineer with Precision! 🚀</h3>
  <p><em>Remember: Great systems are not built by accident – they are designed with intention and evolved with experience. 💪</em></p>
</div>
