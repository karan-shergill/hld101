# System Dealing With Contention / Race Condition 

- [Context: Race Condition](#context-race-condition)
- [Database-Level Locking (Pessimistic Concurrency Control)](#database-level-locking-pessimistic-concurrency-control)
    - [What is it?](#what-is-it)
    - [How It Helps?](#how-it-helps)
    - [How Pessimistic Locking Works?](#how-pessimistic-locking-works)
    - [Use Cases (When to Use Pessimistic Locking)?](#use-cases-when-to-use-pessimistic-locking)
    - [Example: Pessimistic Locking in SQL](#example-pessimistic-locking-in-sql)
    - [Using `LOCK IN SHARE MODE` (Read Lock)](#using-lock-in-share-mode-read-lock)
    - [Example: Pessimistic Locking in Code Level](#example-pessimistic-locking-in-code-level)
    - [Best Practices for Pessimistic Locking](#best-practices-for-pessimistic-locking)
    - [Pros & Cons of Pessimistic Locking](#pros--cons-of-pessimistic-locking)
    - [When NOT to Use Pessimistic Locking?](#when-not-to-use-pessimistic-locking)
- [Optimistic Concurrency Control (OCC) - Versioning](#optimistic-concurrency-control-occ---versioning)
    - [What is it?](#what-is-it)
    - [How It Helps?](#how-it-helps)
    - [How Optimistic Concurrency Control Works?](#how-optimistic-concurrency-control-works)
    - [Use Cases (When to Use OCC)?](#use-cases-when-to-use-occ)
    - [Example: Optimistic Locking in SQL](#example-optimistic-locking-in-sql)
    - [Example: Optimistic Locking in NoSQL (DynamoDB)](#example-optimistic-locking-in-nosql-dynamodb)
    - [Example: Optimistic Locking At Code Level](#example-optimistic-locking-at-code-level)
    - [Best Practices for Optimistic Locking](#best-practices-for-optimistic-locking)
    - [Pros & Cons of Optimistic Concurrency Control](#pros--cons-of-optimistic-concurrency-control)
    - [Comparison: Optimistic vs. Pessimistic Locking](#comparison-optimistic-vs-pessimistic-locking)
    - [When NOT to Use Optimistic Locking?](#when-not-to-use-optimistic-locking)
- [**Compare-and-Swap (CAS) in NoSQL (e.g., DynamoDB, MongoDB)**](#compare-and-swap-cas-in-nosql-eg-dynamodb-mongodb)
    - [What is Compare-and-Swap (CAS)?](#what-is-compare-and-swap-cas)
    - [How CAS Helps in NoSQL Databases?](#how-cas-helps-in-nosql-databases)
    - [CAS in NoSQL Databases](#cas-in-nosql-databases)
    - [Example of Optimistic Locking in Java](#example-of-optimistic-locking-in-java)
    - [Use Cases (When to Use CAS?)](#use-cases-when-to-use-cas)
    - [Best Practices](#best-practices)
    - [Pros & Cons of CAS](#pros--cons-of-cas)
    - [When NOT to Use CAS?](#when-not-to-use-cas)
    - [Similarities Between CAS and Optimistic Locking](#similarities-between-cas-and-optimistic-locking)
- [**P**essimistic **vs Optimistic Locking (Code Level vs. Database Level)**](#pessimistic-vs-optimistic-locking-code-level-vs-database-level)
    - [**When to Use P**essimistic **Locking on Code Level vs. Database Level?**](#when-to-use-pessimistic-locking-on-code-level-vs-database-level)
    - [**When to Use Optimistic Locking on Code Level vs. Database Level?**](#when-to-use-optimistic-locking-on-code-level-vs-database-level)
    - [Quick Decision Guide: When to Use Which Locking Mechanism?](#quick-decision-guide-when-to-use-which-locking-mechanism)
- [Distributed Locks Using Redis](#distributed-locks-using-redis)
    - [What is a Distributed Lock?](#what-is-a-distributed-lock)
    - [Why Use Redis for Distributed Locks?](#why-use-redis-for-distributed-locks)
    - [How Redis Distributed Lock Works?](#how-redis-distributed-lock-works)
    - [Implementation in Redis (Using `SET NX EX`)](#implementation-in-redis-using-set-nx-ex)
    - [How Redis Locks Help?](#how-redis-locks-help)
    - [Use Cases (When to Use Redis Distributed Locks)?](#use-cases-when-to-use-redis-distributed-locks)
    - [Best Practices for Redis Distributed Locks](#best-practices-for-redis-distributed-locks)
    - [Problems with Basic Redis Locking & Solutions](#problems-with-basic-redis-locking--solutions)
    - [Advanced Distributed Locking: The Redlock Algorithm](#advanced-distributed-locking-the-redlock-algorithm)
    - [**How Redlock Works?**](#how-redlock-works)
    - [**Implementing Redlock in Python**](#implementing-redlock-in-python)
    - [Pros & Cons of Redis Distributed Locks](#pros--cons-of-redis-distributed-locks)
    - [When NOT to Use Redis Locks?](#when-not-to-use-redis-locks)
- [Distributed Locks Using ZooKeeper](#distributed-locks-using-zookeeper)
    - [What is ZooKeeper?](#what-is-zookeeper)
    - [How ZooKeeper Implements Distributed Locks](#how-zookeeper-implements-distributed-locks)
    - [How It Helps?](#how-it-helps)
    - [Use Cases (When to Use ZooKeeper Locks?)](#use-cases-when-to-use-zookeeper-locks)
    - [Best Practices](#best-practices)
    - [Pros & Cons of ZooKeeper Locks](#pros--cons-of-zookeeper-locks)
    - [ZooKeeper Distributed Lock Example (Using Apache Curator)](#zookeeper-distributed-lock-example-using-apache-curator)
    - [When NOT to Use ZooKeeper Locks?](#when-not-to-use-zookeeper-locks)
- [Distributed Locks Using etcd](#distributed-locks-using-etcd)
    - [What is etcd?](#what-is-etcd)
    - [What is a Distributed Lock in etcd?](#what-is-a-distributed-lock-in-etcd)
    - [How etcd Helps in Distributed Locking?](#how-etcd-helps-in-distributed-locking)
    - [Use Cases (When to Use etcd for Locking?)](#use-cases-when-to-use-etcd-for-locking)
    - [Best Practices for etcd Locks](#best-practices-for-etcd-locks)
    - [Pros & Cons of etcd Locks](#pros--cons-of-etcd-locks)
    - [etcd Distributed Lock Example (Using Java)](#etcd-distributed-lock-example-using-java)
    - [When NOT to Use etcd for Locking?](#when-not-to-use-etcd-for-locking)
    - [Comparison: etcd vs. Redis vs. ZooKeeper for Distributed Locks](#comparison-etcd-vs-redis-vs-zookeeper-for-distributed-locks)
- [Comparison of Redis vs. ZooKeeper vs. Etcd for Distributed Locking](#comparison-of-redis-vs-zookeeper-vs-etcd-for-distributed-locking)
    - [Redis for Distributed Locks](#redis-for-distributed-locks)
    - [ZooKeeper for Distributed Locks](#zookeeper-for-distributed-locks)
    - [Etcd for Distributed Locks](#etcd-for-distributed-locks)
    - [Which One Should You Choose?](#which-one-should-you-choose)
- [**Eventual Consistency with CQRS & Event Sourcing**](#eventual-consistency-with-cqrs--event-sourcing)
    - [What is Eventual Consistency?](#what-is-eventual-consistency)
    - [What is CQRS (Command Query Responsibility Segregation)?](#what-is-cqrs-command-query-responsibility-segregation)
    - [What is Event Sourcing?](#what-is-event-sourcing)
    - [How CQRS + Event Sourcing Enable Eventual Consistency?](#how-cqrs--event-sourcing-enable-eventual-consistency)
    - [Use Cases (When to Use CQRS + Event Sourcing)?](#use-cases-when-to-use-cqrs--event-sourcing)
    - [Best Practices](#best-practices)
    - [Pros & Cons of Eventual Consistency with CQRS & Event Sourcing](#pros--cons-of-eventual-consistency-with-cqrs--event-sourcing)
    - [When NOT to Use CQRS + Event Sourcing?](#when-not-to-use-cqrs--event-sourcing)
- [Best Race Condition Prevention Strategies by Use Case](#best-race-condition-prevention-strategies-by-use-case)
    - [Summary](#summary)
- [Multiple Database Nodes](#multiple-database-nodes)
    - [Two-Phase Commit (2PC)](#two-phase-commit-2pc)
    - [Distributed Locks](#distributed-locks)
    - [Saga Pattern](#saga-pattern)
        - [What is it?](#what-is-it)
        - [Why Do We Need Saga Pattern?](#why-do-we-need-saga-pattern)
        - [How It Helps?](#how-it-helps)
        - [How Saga Pattern Works?](#how-saga-pattern-works)
        - [Types of Saga Pattern](#types-of-saga-pattern)
        - [Implementing Compensating Transactions](#implementing-compensating-transactions)
        - [Use Cases (When to Use Saga Pattern)?](#use-cases-when-to-use-saga-pattern)
        - [Choreography vs Orchestration: Which to Choose?](#choreography-vs-orchestration-which-to-choose)
        - [Best Practices for Saga Pattern](#best-practices-for-saga-pattern)
        - [Challenges and Limitations](#challenges-and-limitations)
        - [When NOT to Use Saga Pattern?](#when-not-to-use-saga-pattern)
        - [Saga Pattern with Event Sourcing](#saga-pattern-with-event-sourcing)
        - [Tools and Frameworks for Saga Pattern](#tools-and-frameworks-for-saga-pattern)
        - [Summary](#summary)
- [Architecture Patterns & Concurrency Control: Complete Comparison](#architecture-patterns--concurrency-control-complete-comparison)
    - [Architecture Types Overview](#architecture-types-overview)
        - [1. Single Backend Server + Single Database](#1-single-backend-server--single-database)
        - [2. Multiple Backend Servers + Single Database](#2-multiple-backend-servers--single-database)
        - [3. Multiple Backend Servers + Multiple Databases (Sharded/Partitioned)](#3-multiple-backend-servers--multiple-databases-shardedpartitioned)
        - [4. Multiple Backend Servers + Multiple Databases + Read Replicas](#4-multiple-backend-servers--multiple-databases--read-replicas)
        - [5. Microservices Architecture (Multiple Services + Multiple Databases)](#5-microservices-architecture-multiple-services--multiple-databases)
    - [Concurrency Control Mechanisms: When to Use What](#concurrency-control-mechanisms-when-to-use-what)
        - [Decision Matrix by Architecture](#decision-matrix-by-architecture)
        - [Detailed Guidance by Architecture](#detailed-guidance-by-architecture)
    - [Decision Tree: Choosing the Right Approach](#decision-tree-choosing-the-right-approach)
    - [Real-World Scenario Recommendations](#real-world-scenario-recommendations)
        - [Scenario 1: Movie Ticket Booking System](#scenario-1-movie-ticket-booking-system)
        - [Scenario 2: E-commerce Platform](#scenario-2-e-commerce-platform)
        - [Scenario 3: Banking System](#scenario-3-banking-system)
        - [Scenario 4: Social Media Feed](#scenario-4-social-media-feed)
    - [Performance Characteristics](#performance-characteristics)
        - [Latency Comparison (Approximate)](#latency-comparison-approximate)
    - [Common Anti-Patterns to Avoid](#common-anti-patterns-to-avoid)
    - [Summary: Quick Reference Guide](#summary-quick-reference-guide)
    - [Final Recommendations](#final-recommendations)

# Context: Race Condition

Race conditions like seat blocking in a movie booking app appear in **many high-level system design (HLD) interview scenarios** where multiple users compete for the same resource. Here are **some common scenarios** in HLD interviews where similar concurrency control issues arise:

1. Movie ticket seat booking / blocking
2. Stock Trading Systems (Order Matching)
3. E-commerce Flash Sales & Inventory Management
4. Airline Ticket Booking Systems
5. Online Hotel Room Booking
6. Food Delivery App (Restaurant Order Limits)
7. Ride-Hailing Apps (Uber, Lyft)
8. Online Gaming (Matchmaking & Resource Allocation)
9. Banking & Payment Processing (Double Spending Prevention)

# Database-Level Locking (Pessimistic Concurrency Control)

### What is it?

**Pessimistic Concurrency Control (PCC)** is a **locking mechanism** used in databases to prevent race conditions when multiple users try to access or modify the same data simultaneously.

- It **locks the data** (rows, tables, or even an entire database) while a transaction is in progress.
- Other transactions that try to access the locked data must **wait** until the lock is released.

[**Understanding Pessimistic Locking with Mutex**](https://youtu.be/4F-WiPFrPsA)

### How It Helps?

It ensures **strong consistency** by preventing:

1. **Dirty Reads** → Reading uncommitted data.
2. **Lost Updates** → Two transactions modifying the same data, leading to inconsistent updates.
3. **Phantom Reads** → When a transaction sees new rows due to another transaction's insert.

### How Pessimistic Locking Works?

When a transaction wants to read or update a row:

- It **acquires a lock** on that row (`SELECT ... FOR UPDATE` in SQL).
- Other transactions trying to **modify or read** the same row must **wait** until the first transaction releases the lock.
- Once the first transaction commits or rolls back, the lock is released.

### Use Cases (When to Use Pessimistic Locking)?

You should use pessimistic locking when:

1. **High Contention Scenarios** → Many users are competing for the same resource (e.g., seat reservation, stock trading).
2. **Strict Consistency is Required** → Banking, payments, or inventory systems where conflicts **must** be avoided.
3. **Long-Running Transactions** → When transactions take time, and optimistic locking may cause too many retries.
4. **Avoiding Data Corruption** → When conflicting writes can lead to inconsistent state (e.g., concurrent financial transactions).

### Example: Pessimistic Locking in SQL

**Using `SELECT ... FOR UPDATE`**

This prevents other transactions from modifying the same row while the current transaction is in progress.

```sql
BEGIN TRANSACTION;

-- Lock the seat row (blocks others from modifying)
SELECT * FROM seats
WHERE seat_id = 'A1'
FOR UPDATE;

-- If seat is available, book it
UPDATE seats
SET status = 'booked', user_id = '123'
WHERE seat_id = 'A1';

COMMIT;
```

**What Happens?**

- Other users trying to **select the same seat** will have to **wait** until the transaction finishes.
- Ensures **no two users can book the same seat**.

### Using `LOCK IN SHARE MODE` (Read Lock)

This allows **reading but prevents updates**.

```sql
BEGIN TRANSACTION;

-- Read the seat details but prevent updates
SELECT * FROM seats WHERE seat_id = 'A1' LOCK IN SHARE MODE;

-- Other users can read but cannot modify this seat
COMMIT;
```

### Example: Pessimistic Locking in Code Level

```java
// Synchronized block ensures only one thread can modify balance at a time
public void withdraw(double amount) {
    synchronized (this) {
        if (balance < amount) {
            throw new RuntimeException("Insufficient funds");
        }
        balance -= amount;
        System.out.println(Thread.currentThread().getName() + " withdrew " + amount + ". New balance: " + balance);
    }
}
```

### Best Practices for Pessimistic Locking

1. **Use Row-Level Locks Instead of Table Locks** → Table locks block all users, reducing system concurrency.
2. **Keep Transactions Short** → Long locks slow down the system and increase deadlocks.
3. **Use Proper Indexing** → Speeds up locked row access and avoids unnecessary locks.
4. **Set a Lock Timeout** → Avoid indefinite blocking. Example in MySQL:

```sql
SET innodb_lock_wait_timeout = 10; -- Timeout after 10 seconds
```

1. **Avoid Locks on Frequently Accessed Tables** → Could cause bottlenecks.

### Pros & Cons of Pessimistic Locking

| Aspect | Pros | Cons |
| --- | --- | --- |
| **Data Consistency** | Guarantees strong consistency (no dirty reads, lost updates) | Slows down transactions due to waiting for locks |
| **Concurrency** | Prevents conflicts in high-contention scenarios | Limits parallelism, reduces system throughput |
| **Simplicity** | Easier to implement compared to Optimistic Locking | Can lead to **deadlocks** if not handled properly |
| **Use Case Suitability** | Best for **high-conflict** operations (e.g., banking, reservations) | Not ideal for **highly concurrent systems** |

### When NOT to Use Pessimistic Locking?

1. If your system has **high concurrency**, it can cause **slow performance**.
2. If **read operations** are frequent and updates are rare → Use **Optimistic Locking** instead.
3. If your database must **scale horizontally** (distributed NoSQL like DynamoDB).

# Optimistic Concurrency Control (OCC) - Versioning

### What is it?

**Optimistic Concurrency Control (OCC)** is a concurrency management technique that allows multiple transactions to proceed **without locking the data upfront**. Instead of using **locks**, OCC checks for **conflicts before committing changes** by comparing a **version number or timestamp**.

- If the **data has changed** since the transaction started → The update **fails**, and the system **retries**.
- If the data is **unchanged**, the update **succeeds**.

[**Optimistic Locking - What, When, Why, and How?**](https://youtu.be/eMotoFvgdUo)

[**Optimistic Locking clearly explained**](https://youtu.be/d41JuPT_Wls)

### How It Helps?

OCC helps in **high-concurrency environments** by allowing **simultaneous reads and writes** without blocking other users.

It ensures **data consistency** while improving **performance and scalability** compared to **Pessimistic Locking**.

It **prevents race conditions** where:

1. **Two users try to update the same data at the same time**.
2. **A transaction reads outdated data and updates it incorrectly**.

### How Optimistic Concurrency Control Works?

**Step-by-Step Process:**

1. **Read Data** → Fetch the current value along with its **version number** or **timestamp**.
2. **Process Data** → Make changes based on the retrieved data.
3. **Check Version Before Update** → When committing the update, check if the version has changed.
    - **If the version is the same** → Update succeeds.
    - **If the version has changed** → Update fails, and the transaction **must retry**.

### Use Cases (When to Use OCC)?

Optimistic Locking is best when:

1. **Read-heavy systems with fewer writes** (E.g., e-commerce, social media, dashboards).
2. **High-concurrency environments** where locks would slow down performance.
3. **Situations where conflicts are rare** (E.g., user profile updates, online forms).
4. **Distributed databases** that do not support strict locking.
5. **Microservices with eventual consistency** (E.g., NoSQL, DynamoDB).

### Example: Optimistic Locking in SQL

Let's consider a **seat booking system** where multiple users may try to book the same seat.

**Read the Seat Data (With Version Number)**

```sql
SELECT seat_id, status, version FROM seats WHERE seat_id = 'A1';
```

**Update the Seat Only If Version Matches**

```sql
UPDATE seats
SET status = 'booked', version = version + 1
WHERE seat_id = 'A1' AND version = 5; -- Only update if version is still 5
```

**What Happens?**

- If another user has already booked the seat (version changes from 5 to 6), this update **fails**.
- The system **retries the transaction**, fetching the latest version and trying again.

### Example: Optimistic Locking in NoSQL (DynamoDB)

In **DynamoDB**, we use **Conditional Expressions** to check if data has changed before updating.

```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Seats')

response = table.update_item(
    Key={'seat_id': 'A1'},
    UpdateExpression="SET status = :booked, version = version + 1",
    ConditionExpression="version = :expected_version",
    ExpressionAttributeValues={':booked': 'booked', ':expected_version': 5}
)
```

If the **ConditionExpression** fails, the system retries.

### Example: Optimistic Locking At Code Level

```java
AtomicReference<Integer> counter = new AtomicReference<>(0);

public void increment() {
    while (true) {
        int current = counter.get();
        int updated = current + 1;
        if (counter.compareAndSet(current, updated)) {
            break; // Successful update
        }
    }
}
```

```java
@Entity
class Account {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Version // Enables optimistic locking
    private int version;

    private double balance;
}
```

### Best Practices for Optimistic Locking

1. **Use Version Numbers Instead of Timestamps** → Timestamps may lead to conflicts due to clock drift.
2. **Implement a Retry Mechanism** → If a conflict occurs, retry a limited number of times.
3. **Limit Data Updates in a Single Transaction** → Reduce the risk of multiple retries.
4. **Use Exponential Backoff for Retries** → Avoid overloading the system with immediate retries.
5. **Avoid OCC for High-Write Systems** → If conflicts are frequent, use **Pessimistic Locking** instead.

### Pros & Cons of Optimistic Concurrency Control

| Aspect | Pros | Cons |
| --- | --- | --- |
| **Performance** | High concurrency, no locks | May need multiple retries |
| **Scalability** | Works well in distributed systems | More conflicts in high-write workloads |
| **Consistency** | Ensures data consistency without blocking | Can cause temporary inconsistencies |
| **Best for** | Read-heavy, low-conflict environments | High-contention, write-heavy systems |

### Comparison: Optimistic vs. Pessimistic Locking

| Feature | Optimistic Locking | Pessimistic Locking |
| --- | --- | --- |
| **Approach** | Detect conflicts and retry | Lock data to prevent conflicts |
| **Performance** | Faster (no blocking) | Slower (blocks other transactions) |
| **Use Case** | Read-heavy, low-conflict systems | High-contention, write-heavy systems |
| **Risk** | More retries | Deadlocks |
| **Example** | E-commerce checkout, NoSQL | Banking, inventory systems |

### When NOT to Use Optimistic Locking?

1. If **conflicts are frequent**, OCC will cause **too many retries**, slowing the system.
2. If your system **handles critical transactions (e.g., financial systems)** where retrying is risky.
3. If **transactions are long-running**, causing **higher chances of conflicts**.

# **Compare-and-Swap (CAS) in NoSQL (e.g., DynamoDB, MongoDB)**

### What is Compare-and-Swap (CAS)?

Compare-and-Swap (CAS) is an **optimistic concurrency control mechanism** used in NoSQL databases to prevent race conditions when multiple users try to update the same data.

- Instead of **locking** data, CAS ensures that an update only happens **if the existing value has not changed**.
- If another process modifies the value **before your update**, the update **fails**, and you need to retry.

**How It Works:**

1. **Read a value** from the database along with a **version number** (or timestamp).
2. **Attempt to update** the value **only if** the version is unchanged.
3. **If another process modified it**, your update **fails**, and you must retry.

### How CAS Helps in NoSQL Databases?

1. **Avoids race conditions** without using locks.
2. **Improves performance** since there's no blocking.
3. **Ensures atomicity** in distributed environments.
4. **Works well with distributed NoSQL databases** (DynamoDB, MongoDB, CouchDB).

### CAS in NoSQL Databases

**CAS in DynamoDB (AWS)**

DynamoDB supports CAS via **Conditional Writes** using `ConditionExpression`.

🔥 **Example:** Ensuring an item’s price updates **only if it hasn’t changed**

```python
import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Products')

response = table.update_item(
    Key={'ProductID': '123'},
    UpdateExpression='SET Price = :newPrice',
    ConditionExpression='Price = :expectedPrice',
    ExpressionAttributeValues={
        ':newPrice': 200,
        ':expectedPrice': 150  # Ensure no one else changed it
    }
)
```

**If the `Price` is still `150`, it updates to `200`. Otherwise, it fails!**

**CAS in MongoDB (Using `findOneAndUpdate` with Versioning)**

MongoDB uses a **version field (`_version` or `updatedAt`)** to implement CAS.

**Example:** Ensuring only one process updates a document

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client["mydatabase"]
collection = db["products"]

result = collection.find_one_and_update(
    {'_id': 1, 'version': 2},  # Ensure version is still 2
    {'$set': {'price': 200, 'version': 3}},  # Update and increment version
    return_document=True
)
```

**If another process updated `version` before you, the update fails!**

### Example of Optimistic Locking in Java

Code-Level Optimistic Locking (Using `AtomicReference`)

```java
import java.util.concurrent.atomic.AtomicReference;

class OptimisticLockingAtomic {
    private final AtomicReference<Integer> counter = new AtomicReference<>(0);

    public void increment() {
        while (true) { // Retry loop
            int current = counter.get(); 
            int updated = current + 1;
            if (counter.compareAndSet(current, updated)) { // CAS operation
                System.out.println("Updated to: " + updated);
                break; // Successful update, exit loop
            }
        }
    }

    public static void main(String[] args) {
        OptimisticLockingAtomic obj = new OptimisticLockingAtomic();
        obj.increment();
    }
}

```

Database-Level Optimistic Locking (Using `@Version` in JPA)

```java
import jakarta.persistence.*;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Entity
class Account {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Version // Enables optimistic locking
    private int version;  

    private double balance;

    // Getters and Setters
    public double getBalance() { return balance; }
    public void setBalance(double balance) { this.balance = balance; }
}

@Repository
interface AccountRepository extends JpaRepository<Account, Long> {}

@Service
class AccountService {
    @Autowired private AccountRepository repository;

    @Transactional
    public void withdraw(Long accountId, double amount) {
        Account account = repository.findById(accountId).orElseThrow();
        account.setBalance(account.getBalance() - amount);
        repository.save(account); // If version changed, OptimisticLockException is thrown
    }
}
```

### Use Cases (When to Use CAS?)

1. **Optimistic concurrency control** → Prevents race conditions without locking.
2. **High-throughput systems** → E-commerce (inventory updates), finance (banking transactions).
3. **Distributed environments** → Ensures atomic updates across microservices.
4. **User profile updates** → Prevents overwriting changes if multiple updates happen.

### Best Practices

1. **Always include a version number (`updatedAt`, `_version`)** → Ensures proper CAS behavior.
2. **Use exponential backoff retries** → To handle failures efficiently.
3. **For high-contention scenarios, consider locks** → CAS may lead to frequent retries.
4. **For DynamoDB, use Conditional Writes** → Efficient and prevents extra reads.
5. **For MongoDB, use `findOneAndUpdate`** → Ensures atomicity.

### Pros & Cons of CAS

| Aspect | Pros | Cons |
| --- | --- | --- |
| **Performance** | Non-blocking, avoids locks | Can fail under high contention |
| **Scalability** | Works well in distributed systems | May require frequent retries |
| **Concurrency Control** | Prevents race conditions | Not suitable for long-running transactions |
| **Best For** | Optimistic updates, distributed apps | High-contention updates |

### When NOT to Use CAS?

1. If **updates frequently conflict**, causing too many retries.
2. If **transactions require strong consistency**, use **pessimistic locking**.
3. If **low-latency is critical**, as retries can add overhead.

### Similarities Between CAS and Optimistic Locking

| Feature | Compare-and-Swap (CAS) | Optimistic Locking |
| --- | --- | --- |
| **Concurrency Control Type** | Non-blocking | Non-blocking |
| **Locks Held?** | No | No |
| **How it Works** | Compares expected value before updating | Compares a version/timestamp before updating |
| **Retry Required?** | Yes (if conflict) | Yes (if conflict) |
| **Used In?** | NoSQL DBs (Redis, DynamoDB, Cassandra) | Relational DBs (SQL, JPA `@Version`) |

# **P**essimistic **vs Optimistic Locking (Code Level vs. Database Level)**

Both Pessimistic and Optimistic locking can be done on code level as well as database level. We should know and be clear when to user which one.

Code Example: [**Spring Boot | Optimistic & Pessimistic Locking Explained with Concurrent Movie Seat Booking Example**](https://youtu.be/lR6y-0RC5cc)

### **When to Use P**essimistic **Locking on Code Level vs. Database Level?**

If you're running a **single-server application**, you can use **synchronized blocks** or **`ReentrantLock`** to enforce pessimistic locking **at the code level** since all threads are within the same JVM. This avoids database overhead while ensuring thread safety.

However, in a **multi-server (distributed) system**, `synchronized` **won’t work across different instances**, because each server has its own JVM. In that case, you need **database-level pessimistic locking** (e.g., `PESSIMISTIC_WRITE` in JPA) to ensure consistency across multiple nodes.

| **Aspect** | **`synchronized` Block** | **Database-Level Lock (`PESSIMISTIC_WRITE`)** |
| --- | --- | --- |
| **Scope** | Application-level (JVM) | Database-level |
| **Use Case** | In-memory objects | Persistent data (DB records) |
| **Concurrency Handling** | Blocks threads inside JVM | Blocks transactions in DB |
| **Performance** | Fast (no DB overhead) | Slower (requires DB locks) |
| **Drawback** | Doesn't work across distributed systems | Can cause deadlocks if not handled properly |

### **When to Use Optimistic Locking on Code Level vs. Database Level?**

Optimistic Locking can be implemented at **two levels**—**Code Level** (in-memory) and **Database Level** (persistent data). The choice depends on where your data lives and the architecture of your system.

| **Scenario** | **Use Code-Level (`AtomicReference`)** | **Use Database-Level (`@Version` in JPA)** |
| --- | --- | --- |
| **Single-server app (one JVM)** | ✅ Yes | ❌ No (not needed) |
| **Multi-server app (distributed DB)** | ❌ No | ✅ Yes |
| **Data stored in memory (cache, list)** | ✅ Yes | ❌ No |
| **Data stored in a relational DB** | ❌ No | ✅ Yes |
| **Fast, lightweight updates** | ✅ Yes | ❌ No |
| **Ensuring no overwrites in DB** | ❌ No | ✅ Yes |
| **Frequent reads, rare writes** | ✅ Yes | ✅ Yes |
| **Frequent writes (high contention)** | ❌ No (use Pessimistic Locking) | ❌ No (use Pessimistic Locking) |

### Quick Decision Guide: When to Use Which Locking Mechanism?

| **Scenario** | **Pessimistic Locking - Code Level** (`synchronized` / `ReentrantLock`) | **Pessimistic Locking - Database Level** (`PESSIMISTIC_WRITE`) | **Optimistic Locking - Code Level** (`AtomicReference`) | **Optimistic Locking - Database Level** (`@Version` in JPA) |
| --- | --- | --- | --- | --- |
| **Single-server application (one JVM)** | ✅ Yes | ❌ No (not needed) | ✅ Yes | ❌ No (not needed) |
| **Multi-server (distributed DB, microservices)** | ❌ No (locks don’t work across servers) | ✅ Yes | ❌ No (does not work across instances) | ✅ Yes |
| **Frequent writes (high contention)** | ✅ Yes | ✅ Yes | ❌ No (too many retries) | ❌ No (too many retries) |
| **Frequent reads, rare writes** | ❌ No | ❌ No | ✅ Yes | ✅ Yes |
| **Ensuring strict consistency (e.g., bank transactions)** | ✅ Yes | ✅ Yes | ❌ No | ❌ No |
| **Fast, lightweight updates** | ❌ No (blocking is expensive) | ❌ No | ✅ Yes | ✅ Yes |
| **Preventing lost updates in DB (e.g., e-commerce stock updates)** | ❌ No (not database-related) | ✅ Yes | ❌ No | ✅ Yes |
| **Multiple threads modifying in-memory objects** | ✅ Yes | ❌ No | ✅ Yes | ❌ No |
| **Data stored in-memory (caching, counters, game state)** | ✅ Yes | ❌ No | ✅ Yes | ❌ No |
| **Data stored in a relational DB** | ❌ No | ✅ Yes | ❌ No | ✅ Yes |
| **Works well in high-contention scenarios** | ✅ Yes | ✅ Yes | ❌ No | ❌ No |
| **Works well in low-contention scenarios** | ❌ No | ❌ No | ✅ Yes | ✅ Yes |
1. **Pessimistic Locking (Code Level)**
    - ✅ **Use when:** Single-server, in-memory shared data, and high-contention writes.
    - ⚠️ **Avoid when:** Multi-server or distributed databases.
    - **Example:** `synchronized`, `ReentrantLock`.
2. **Pessimistic Locking (Database Level)**
    - ✅ **Use when:** Multi-server, database transactions require strict locking.
    - ⚠️ **Avoid when:** Read-heavy systems (it blocks other transactions).
    - **Example:** `PESSIMISTIC_WRITE` in JPA.
3. **Optimistic Locking (Code Level)**
    - ✅ **Use when:** Single-server, low-contention updates, and in-memory objects.
    - ⚠️ **Avoid when:** High contention, frequent write conflicts.
    - **Example:** `AtomicReference`, `compareAndSet()`.
4. **Optimistic Locking (Database Level)**
    - ✅ **Use when:** Multi-server, database transactions with rare conflicts.
    - ⚠️ **Avoid when:** High contention updates (use pessimistic locking instead).
    - **Example:** `@Version` in JPA.

**Rule of Thumb:**

- **Single-server, in-memory?** → Use **Code-Level Locking**.
- **Multi-server, database transactions?** → Use **Database-Level Locking**.
- **Frequent write conflicts?** → Use **Pessimistic Locking**.
- **Mostly reads, rare writes?** → Use **Optimistic Locking**.

# Distributed Locks Using Redis

### What is a Distributed Lock?

A **distributed lock** is a mechanism that prevents multiple instances of a distributed system from performing conflicting operations **at the same time**.

Since multiple servers may process the same request concurrently, a **distributed lock ensures that only one instance can perform a critical operation at a time**.

### Why Use Redis for Distributed Locks?

Redis is **fast**, **lightweight**, and **supports atomic operations**, making it an ideal choice for distributed locking. Redis locks help prevent **race conditions** in **distributed environments** where multiple processes compete for the same resource.

### How Redis Distributed Lock Works?

**Basic Locking Mechanism:**

1. A process attempts to acquire a **lock** by setting a unique key in Redis with an expiration time.
2. If the key **does not exist**, the process **gets the lock**.
3. If the key **already exists**, the process **waits** or retries.
4. Once the process **completes its operation**, it **releases the lock**.

### Implementation in Redis (Using `SET NX EX`)

- `NX` → Ensures the key is **only set if it does not already exist**.
- `EX` → Sets an **expiration time** to avoid deadlocks if the process crashes.

```java
import redis.clients.jedis.Jedis;

public class RedisLockExample {
    private static final String LOCK_KEY = "seat_A1_lock";
    private static final String LOCK_VALUE = "locked";
    private static final int LOCK_EXPIRY = 10; // Expiry in seconds

    public static void main(String[] args) {
        try (Jedis jedis = new Jedis("localhost", 6379)) { // Connect to Redis
            boolean lockAcquired = acquireLock(jedis);

            if (lockAcquired) {
                try {
                    // Critical section: Booking Seat A1
                    System.out.println("✅ Lock acquired! Processing booking...");
                    Thread.sleep(5000); // Simulate processing time
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    releaseLock(jedis);
                }
            } else {
                System.out.println("❌ Lock not acquired, retrying...");
            }
        }
    }

    private static boolean acquireLock(Jedis jedis) {
        String result = jedis.set(LOCK_KEY, LOCK_VALUE, "NX", "EX", LOCK_EXPIRY);
        return "OK".equals(result);
    }

    private static void releaseLock(Jedis jedis) {
        jedis.del(LOCK_KEY);
        System.out.println("🔓 Lock released!");
    }
}
```

**What Happens?**

- If another process **already holds the lock**, it must **wait or retry**.
- If a process **crashes**, the lock expires automatically **after 10 seconds**.

### How Redis Locks Help?

- **Prevents race conditions** → Ensures only one process modifies a resource at a time.
- **Increases consistency in distributed systems** → No two nodes can simultaneously process the same task.
- **Reduces contention** → Lightweight, non-blocking mechanism improves system performance.

### Use Cases (When to Use Redis Distributed Locks)?

1. Booking Systems (Seats, Tickets, Hotels, Flights)
    1. Prevents **double booking** in **distributed** environments.
    2. Ensures only **one user** gets a specific seat.
2. E-commerce (Flash Sales, Order Placement)
    1. Prevents multiple users from purchasing the **same limited stock** item.
    2. Ensures **correct inventory management**.
3. Leader Election in Distributed Systems
    1. Ensures **only one leader node** in a **cluster** (e.g., Kubernetes, Apache Kafka).
4. Rate Limiting & API Request Throttling
    1. Prevents **duplicate transactions** (e.g., retrying a payment twice).
5. Distributed Job Scheduling
    1. Ensures **one worker** processes a **job** at a time in a job queue.
    2. Prevents **duplicate job execution** in microservices.

### Best Practices for Redis Distributed Locks

1. **Use Short Expiration Times (`EX` option)** → Avoid deadlocks in case of failures.
2. **Ensure Atomicity (`SET NX EX`)** → Prevents race conditions while setting locks.
3. **Release Locks Using the Correct Identifier** → To prevent accidental deletions.
4. **Implement Auto-Renewal for Long Operations** → To prevent premature expiration.
5. **Use Unique Lock Keys** → Avoid conflicts between different operations.
6. **Handle Failures Gracefully** → Implement retry logic for lock acquisition failures.

### Problems with Basic Redis Locking & Solutions

Problem 1: What if a Process Crashes Before Releasing the Lock?

**Solution:** Use **lock expiration (`EX`)** to automatically free the lock.

Problem 2: What if the Lock Expires Before the Process Completes?

**Solution:**

- Use **"lock extension"** (renew lock if the process is still active).
- Implement **Redlock Algorithm**.

### Advanced Distributed Locking: The Redlock Algorithm

**Why is Redlock Needed?**

- Basic Redis locks work well but **may fail in distributed environments** (e.g., network partitions).
- **Redlock** (by Redis creator Salvatore Sanfilippo) ensures **better safety** in distributed locks.

### **How Redlock Works?**

1. **Multiple Redis Instances** → Use **5 Redis nodes** for redundancy.
2. **Acquire Locks on Majority Nodes** → A process must acquire **locks on at least 3 out of 5 nodes**.
3. **Set Expiry Time & Check Clock Skew** → Ensures no process accidentally gets an expired lock.
4. **Release Locks** → If the process finishes, it releases locks on **all nodes**.

### **Implementing Redlock in Python**

```java
import org.redisson.Redisson;
import org.redisson.api.RLock;
import org.redisson.api.RedissonClient;
import org.redisson.config.Config;

import java.util.concurrent.TimeUnit;

public class RedisRedlockExample {
    public static void main(String[] args) {
        // Step 1: Connect to multiple Redis instances (Redlock Setup)
        Config config = new Config();
        config.useReplicatedServers()
                .addNodeAddress("redis://127.0.0.1:6379", 
                                "redis://127.0.0.1:6380", 
                                "redis://127.0.0.1:6381");

        RedissonClient redisson = Redisson.create(config);
        RLock lock = redisson.getLock("my_critical_section");

        try {
            // Step 2: Try acquiring the distributed lock
            boolean lockAcquired = lock.tryLock(0, 10, TimeUnit.SECONDS);

            if (lockAcquired) {
                try {
                    System.out.println("✅ Lock acquired! Processing...");
                    Thread.sleep(5000); // Simulate task
                } finally {
                    lock.unlock();
                    System.out.println("🔓 Lock released!");
                }
            } else {
                System.out.println("❌ Lock not acquired!");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            redisson.shutdown();
        }
    }
}

```

**Why Redlock?**

1. Works across **multiple Redis nodes** (high availability).
2. Prevents **single point of failure**.
3. Safer than basic `SET NX EX` locks.

### Pros & Cons of Redis Distributed Locks

| Feature | Pros | Cons |
| --- | --- | --- |
| **Performance** | Fast, low-latency | Overhead in high-traffic systems |
| **Scalability** | Works well in distributed environments | Requires careful expiration management |
| **Fault Tolerance** | Redlock improves reliability | Basic locks may fail in network partitions |
| **Ease of Use** | Simple to implement | Must handle retries & failures properly |
| **Use Case Suitability** | Best for distributed systems, microservices, and job scheduling | Not ideal for heavy-write, multi-region applications |

### When NOT to Use Redis Locks?

1. If **data consistency is mission-critical** → Use **database transactions instead**.
2. If **network failures are frequent**, basic Redis locks may fail.
3. If **operations take too long**, the lock might **expire too soon**.
4. If you **don’t have a reliable Redis cluster**, single-node Redis is a **single point of failure**.

# Distributed Locks Using ZooKeeper

### What is ZooKeeper?

Apache **ZooKeeper** is a **distributed coordination service** that helps manage **configuration, synchronization, and leader election** in distributed systems. It ensures **reliable, consistent, and fault-tolerant** coordination between multiple services or nodes.

**Think of ZooKeeper as a "centralized brain" for managing distributed applications.**

[**Introduction to Apache zookeeper**](https://youtu.be/gXJMiDaLIP8)

### How ZooKeeper Implements Distributed Locks

ZooKeeper **creates a lock by using ephemeral sequential znodes**.

**Steps to Acquire a Lock:**

1. **Client creates an ephemeral sequential znode** in a predefined path (`/locks`).
2. **ZooKeeper assigns a unique sequence number** to the znode (e.g., `/locks/lock-0001`).
3. **Client checks if it has the lowest sequence number**:
    - ✅ **If Yes → Lock acquired**
    - ❌ **If No → Watch the next lowest node and wait**
4. **When the lock is released (znode deleted), the next client gets notified and acquires the lock.**

[**Distributed Lock using Zookeeper**](https://youtu.be/a-zatYN1Lx0?list=PL1F54YqZCTdx4JmaKY99bb9hoD5gHS9bm)

### How It Helps?

1. **Prevents race conditions** → Ensures only one client modifies the resource at a time.
2. **Automatic failure recovery** → If a process crashes, its **ephemeral node disappears**, freeing the lock.
3. **Scalability** → Handles multiple clients efficiently.

### Use Cases (When to Use ZooKeeper Locks?)

1. **Leader Election** → Ensure only one process is the leader.
2. **Distributed Coordination** → Ensure tasks are executed in **order**.
3. **Transaction Management** → Prevent **double spending** in payment systems.
4. **Service Discovery** → Ensure only **one active service instance** per task.
5. **Distributed Queues** → Ensure tasks execute in a **controlled manner**.

### Best Practices

1. **Use ephemeral znodes** → Ensures the lock is **automatically released if a process crashes**.
2. **Set a session timeout** → Prevent locks from being held indefinitely.
3. **Implement retry logic** → In case of failures, retry acquiring the lock.
4. **Use lock contention metrics** → Avoid bottlenecks caused by excessive waiting.
5. **Prefer short-lived locks** → Holding locks for too long **reduces system throughput**.

### Pros & Cons of ZooKeeper Locks

| Aspect | Pros | Cons |
| --- | --- | --- |
| **Failure Recovery** | Auto-releases locks if process dies | Requires ZooKeeper cluster to be highly available |
| **Scalability** | Supports thousands of locks simultaneously | Not as fast as Redis-based locks |
| **Strong Consistency** | Uses **ZAB protocol**, ensuring order | More overhead compared to simpler locking mechanisms |
| **Reliability** | Works well for leader election & coordination | Can be overkill for simple locking needs |
| **Best For** | Distributed coordination, leader election, queueing | Not ideal for **low-latency, high-speed locks** |

### ZooKeeper Distributed Lock Example (Using Apache Curator)

```java
import org.apache.curator.framework.CuratorFramework;
import org.apache.curator.framework.recipes.locks.InterProcessMutex;

public class ZookeeperDistributedLockExample {
    public static void main(String[] args) throws Exception {
        CuratorFramework client = ZookeeperClient.create(); // Assume initialized client
        InterProcessMutex lock = new InterProcessMutex(client, "/locks/my_lock");

        if (lock.acquire(10, TimeUnit.SECONDS)) {
            try {
                System.out.println("Lock acquired, processing...");
                Thread.sleep(5000); // Simulate task
            } finally {
                lock.release();
                System.out.println("Lock released.");
            }
        } else {
            System.out.println("Could not acquire lock.");
        }
    }
}
```

**Explanation:**

- A process acquires the lock **if available**.
- If the lock **is already held**, it waits for its turn.
- Lock **is automatically released** when the process finishes.

### When NOT to Use ZooKeeper Locks?

1. If you need **high-speed, low-latency locks** → Use **Redis-based locks** instead.
2. If your system **does not need strong consistency** → Use **Optimistic Concurrency (CAS)** instead.
3. If you have **limited infrastructure** → ZooKeeper **requires a quorum of nodes to work reliably**.

# Distributed Locks Using etcd

### What is etcd?

etcd is a **highly available, distributed key-value store** used for coordination and service discovery in distributed systems. It provides **strong consistency** and is commonly used for **leader election, distributed locks, and configuration management**.

[**What is etcd?**](https://youtu.be/OmphHSaO1sE)

### What is a Distributed Lock in etcd?

A **distributed lock in etcd** ensures that only **one process at a time** can access a critical resource in a **distributed system**.

It is implemented using etcd’s **lease and key mechanisms**:

1. **A process creates a key in etcd** representing the lock.
2. **The key is associated with a lease (TTL)** → If the process crashes, etcd **automatically deletes** the key.
3. **Other processes try to acquire the lock** by checking the key’s existence.
4. **When the process finishes, it releases the lock** by deleting the key.

### How etcd Helps in Distributed Locking?

1. **Ensures mutual exclusion** → Prevents race conditions in distributed systems.
2. **Automatic lock release** → If a process crashes, the lease expires, and the lock is freed.
3. **Highly available** → Replicated across multiple nodes, ensuring fault tolerance.
4. **Low-latency & scalable** → Optimised for high-speed lock acquisition in cloud-native environments.

### Use Cases (When to Use etcd for Locking?)

1. **Leader Election** → Ensures only one active leader in a distributed cluster.
2. **Service Discovery** → Prevent multiple instances of the same service running at the same time.
3. **Database Failover Mechanisms** → Ensures a single active database instance in a failover scenario.
4. **Microservices Coordination** → Prevents multiple services from performing the same operation simultaneously.
5. **Distributed Job Scheduling** → Ensures that a task runs on only one node in a distributed system.

### Best Practices for etcd Locks

1. **Always use leases (TTL-based locks)** → Ensures automatic lock cleanup if a process crashes.
2. **Use exponential backoff for retries** → Prevents excessive contention on etcd.
3. **Monitor lock status using etcd watch API** → Helps detect unexpected failures.
4. **Use short-lived locks** → Holding locks for too long can lead to performance bottlenecks.
5. **Ensure etcd cluster is highly available** → Deploy with at least **3 nodes** for fault tolerance.

### Pros & Cons of etcd Locks

| Aspect | Pros | Cons |
| --- | --- | --- |
| **Failure Recovery** | Automatic lock expiration via TTL | Requires a highly available etcd cluster |
| **Scalability** | Handles thousands of locks efficiently | Slower than in-memory solutions like Redis |
| **Strong Consistency** | Uses **Raft consensus** for reliability | Higher latency due to consensus overhead |
| **Availability** | Works across multiple data centers | Requires maintenance for large-scale deployments |
| **Best For** | Leader election, service coordination, database failover | Not ideal for extremely high-speed locking |

### etcd Distributed Lock Example (Using Java)

```java
import io.etcd.jetcd.Client;
import io.etcd.jetcd.Lease;
import io.etcd.jetcd.kv.PutResponse;
import io.etcd.jetcd.options.PutOption;

import java.nio.charset.StandardCharsets;
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.TimeUnit;

public class EtcdDistributedLock {
    private static final String ETCD_ENDPOINT = "http://localhost:2379";
    private static final String LOCK_KEY = "/distributed-lock";

    public static void main(String[] args) throws Exception {
        try (Client client = Client.builder().endpoints(ETCD_ENDPOINT).build()) {
            Lease leaseClient = client.getLeaseClient();
            
            // Create a lease with TTL (10 seconds)
            long leaseId = leaseClient.grant(10).get().getID();

            // Try to acquire the lock by setting a key with the lease
            CompletableFuture<PutResponse> putFuture = client.getKVClient().put(
                    io.etcd.jetcd.ByteSequence.from(LOCK_KEY, StandardCharsets.UTF_8),
                    io.etcd.jetcd.ByteSequence.from("LOCKED", StandardCharsets.UTF_8),
                    PutOption.newBuilder().withLeaseId(leaseId).build()
            );

            putFuture.get(); // Wait for operation to complete
            System.out.println("🔒 Lock acquired!");

            // Simulate some work while holding the lock
            TimeUnit.SECONDS.sleep(5);

            // Release the lock by revoking the lease
            leaseClient.revoke(leaseId).get();
            System.out.println("🔓 Lock released!");
        }
    }
}
```

**Explanation:**

- A process **creates a session with a TTL** (10 seconds).
- It **acquires the lock** by creating a key in etcd.
- If the process crashes, **the TTL expires**, and etcd **automatically releases the lock**.
- After work is done, **the lock is released manually**.

### When NOT to Use etcd for Locking?

1. If you need **ultra-low-latency locks**, use **Redis-based locks** instead.
2. If **your system can tolerate eventual consistency**, use **CAS (Compare-and-Swap)** instead.
3. If you need **hierarchical locks** (locks with structured ordering), ZooKeeper is a better fit.

### Comparison: etcd vs. Redis vs. ZooKeeper for Distributed Locks

| Feature | **etcd** | **Redis** | **ZooKeeper** |
| --- | --- | --- | --- |
| **Consistency Model** | Strong (Raft) | Eventual | Strong (ZAB) |
| **Speed** | Slower than Redis | Fastest (in-memory) | Medium |
| **Failure Handling** | TTL-based auto-release | Can lose locks if not properly implemented | Ephemeral nodes auto-release |
| **Scalability** | Good, but slower than Redis | Best for high-speed locking | Good, but requires more maintenance |
| **Use Case** | Leader election, service coordination | Low-latency, high-speed locks | Leader election, queueing, distributed coordination |

# Comparison of Redis vs. ZooKeeper vs. Etcd for Distributed Locking

| Feature | **Redis** | **ZooKeeper** | **Etcd** |
| --- | --- | --- | --- |
| **Lock Type** | Lease-based (temporary locks) | Persistent and ephemeral locks | Lease-based with TTL |
| **Lock Persistence** | Non-persistent (expires after TTL) | Persistent (until released) | Non-persistent (expires after TTL) |
| **Consensus Algorithm** | None (single-node or Redlock for HA) | **ZAB (ZooKeeper Atomic Broadcast)** | **Raft (Leader-based consensus)** |
| **Failure Tolerance** | Moderate (Redlock improves HA) | High (Strong consistency with leader election) | High (Raft ensures strong consistency) |
| **Performance** | Extremely fast, in-memory | Moderate (reads fast, writes slower) | Moderate (reads fast, writes slower) |
| **Scalability** | Scales well with replication | Scales well but needs a quorum | Scales well but has write limitations |
| **Consistency Model** | Eventual consistency (unless using Redlock) | Strong consistency (CP system) | Strong consistency (CP system) |
| **Use Case** | Fast, lightweight locks for high-throughput systems | Distributed coordination (e.g., leader election) | Strongly consistent locking (e.g., Kubernetes, distributed systems) |
| **Ease of Setup** | Simple, single Redis node or cluster | More complex, requires ZooKeeper quorum | Requires cluster setup |
| **Best For** | Caching, high-speed locking, short-lived locks | Leader election, distributed state management | Kubernetes, distributed key-value storage |

### Redis for Distributed Locks

✅ **Best For:**

- High-performance, low-latency applications
- Lightweight locking in **microservices**
- **Short-lived locks** (e.g., seat booking, API throttling)

❌ **Limitations:**

- No built-in consensus (Redlock adds complexity)
- Expiry-based locks can cause issues if they expire too soon

### ZooKeeper for Distributed Locks

✅ **Best For:**

- **Leader election** in distributed systems
- **Long-lived locks** where strong consistency is required
- Coordinating **distributed services** (e.g., Kafka, Hadoop)

❌ **Limitations:**

- Requires a **ZAB quorum** (usually 3+ nodes)
- Slower than Redis (due to disk writes & consistency checks)

### Etcd for Distributed Locks

✅ **Best For:**

- **Cloud-native applications** (e.g., Kubernetes, CoreDNS)
- **Highly consistent** distributed locks
- **Lease-based locks** with automatic renewal

❌ **Limitations:**

- Slower than Redis due to **Raft consensus overhead**
- **Write throughput is limited** (Raft needs leader confirmation)

### Which One Should You Choose?

- **For Speed & Simplicity → Redis**
- **For High Consistency & Coordination → ZooKeeper**
- **For Cloud-Native, Kubernetes Workloads → Etcd**

# **Eventual Consistency with CQRS & Event Sourcing**

### What is Eventual Consistency?

Eventual Consistency is a **data consistency model** in distributed systems where updates **do not propagate instantly** but will **eventually synchronize** across nodes.

- Unlike **strong consistency** (where all nodes have the latest data immediately), eventual consistency allows **temporary inconsistencies**.
- It is widely used in **high-performance, scalable architectures** like **NoSQL databases, microservices, and distributed applications**.

### What is CQRS (Command Query Responsibility Segregation)?

CQRS is an architectural pattern that **separates**:

1. **Commands (Write operations)** → Modify the system (e.g., "Book a Ticket").
2. **Queries (Read operations)** → Retrieve data (e.g., "Get Available Seats").

**Why separate Read & Write models?**

- Allows **different database structures** optimized for reads vs. writes.
- Supports **eventual consistency** between read and write databases.

**Example: Ticket Booking System with CQRS**

- **Command Side (Writes to the database)**: A user books a ticket.
- **Query Side (Reads from a different store)**: The UI retrieves available seats.
- **Eventual Consistency**: The **query store updates asynchronously** after the write completes.

### What is Event Sourcing?

Event Sourcing is a design pattern where **state changes are stored as a sequence of immutable events** instead of updating the database directly.

- Instead of saving the **final state**, the system stores **all past events** (e.g., "Seat Reserved", "Seat Released").
- The current state is **reconstructed** by replaying events.

**How It Works?**

1. **User Action → Generates an Event**
2. **Event Stored in an Event Log**
3. **Other Services Listen to the Event & Update Read Models**

💡 **Key Advantage**:

- Ensures **eventual consistency** → Read models are updated **asynchronously**.
- Provides **auditability** → Every change is recorded as an event.

### How CQRS + Event Sourcing Enable Eventual Consistency?

1. **Writes (Commands)** → Persist events in an **event store**.
2. **Event Processing** → A background process updates **read models** asynchronously.
3. **Reads (Queries)** → Query the **read model** (which may lag behind).

**Example: E-Commerce Order System**

- User places an **order** → "Order Placed" event stored.
- Warehouse system **listens to event** → Updates stock count asynchronously.
- UI fetches **available stock**, which **might be slightly delayed**.

This means **users might briefly see outdated stock levels** until updates propagate.

### Use Cases (When to Use CQRS + Event Sourcing)?

1. **High-Throughput Systems** → Banking, e-commerce, stock trading.
2. **Event-Driven Microservices** → Systems where **different services react to events**.
3. **Auditing & Compliance** → Systems needing a **full history of changes**.
4. **Distributed Applications** → Ensures consistency across **multiple services/databases**.
5. **Scalable Read Models** → Read-heavy applications needing **fast, optimized queries**.

### Best Practices

1. **Ensure Events Are Idempotent** → Avoid duplicate processing.
2. **Use Eventual Consistency Where Speed Matters** → Prioritize fast writes over immediate reads.
3. **Implement Event Replay Mechanism** → Enable reprocessing if needed.
4. **Use a Message Broker (Kafka, RabbitMQ)** → Decouple components processing events.
5. **Optimize Read Models for Queries** → Use caching or NoSQL for faster lookups.

### Pros & Cons of Eventual Consistency with CQRS & Event Sourcing

| Aspect | Pros | Cons |
| --- | --- | --- |
| **Performance** | Faster writes, scales well | Read delays due to async updates |
| **Scalability** | Optimized read/write separation | Complexity in maintaining multiple stores |
| **Auditability** | Full event history stored | Large event logs need cleanup |
| **Resilience** | Works well in distributed systems | Eventual consistency may confuse users |
| **Best For** | Microservices, distributed systems, auditing | Real-time transactions needing strict consistency |

### When NOT to Use CQRS + Event Sourcing?

1. If **strong consistency** is required → Use **synchronous transactions** instead.
2. If **low-latency reads** are needed → Queries may return outdated data.
3. If **system complexity** is a concern → Managing events and read models adds overhead.

# Best Race Condition Prevention Strategies by Use Case

| **Use Case** | **Best Race Condition Prevention Strategy** | **Why This Works Best?** |
| --- | --- | --- |
| **1. Movie Ticket Seat Booking / Blocking** | **Distributed Locks (Redis, ZooKeeper, Etcd)** | Ensures **only one user can lock a seat** at a time, preventing double booking. |
| **2. Stock Trading Systems (Order Matching)** | **Optimistic Concurrency Control (CAS, Versioning)** | Trading requires **high speed & concurrency**. CAS ensures atomic updates without locking. |
| **3. E-commerce Flash Sales & Inventory Management** | **Database-Level Locking (Pessimistic Concurrency Control)** | Prevents overselling by **locking rows** during inventory update. |
| **4. Airline Ticket Booking Systems** | **Distributed Locks (Redis + TTL)** | Holds a seat for **a short duration** while the payment is processed. Prevents overbooking. |
| **5. Online Hotel Room Booking** | **CQRS + Event Sourcing (Eventual Consistency)** | Updates hotel availability asynchronously to scale **high read & write loads**. |
| **6. Food Delivery App (Restaurant Order Limits)** | **Compare-and-Swap (CAS) with NoSQL** | Ensures **restaurant capacity updates atomically** without locks. |
| **7. Ride-Hailing Apps (Uber, Lyft)** | **Distributed Locks (ZooKeeper/Etcd) OR CAS** | Ensures **one driver is assigned per ride** without conflicts. |
| **8. Online Gaming (Matchmaking & Resource Allocation)** | **Distributed Locks (Redis with TTL) OR CAS** | Locks players to a game instance, prevents duplicate assignments. |
| **9. Banking & Payment Processing (Double Spending Prevention)** | **Pessimistic Locking + Transactional Databases** | Prevents race conditions in financial transactions where **strong consistency** is required. |

### Summary

- **Use Distributed Locks** when a resource **must be reserved** temporarily (e.g., seats, ride-hailing, gaming).
- **Use Optimistic Concurrency (CAS)** when **performance matters** and conflicts are rare (e.g., trading, food delivery).
- **Use Pessimistic Locking** when **data integrity is critical** (e.g., banking, inventory management).
- **Use CQRS + Event Sourcing** for **high-scale read-heavy applications** (e.g., hotel bookings).

# Multiple Database Nodes

All the approaches we've covered so far work within a single database. But what happens when you need to coordinate updates across multiple databases? This is where things get significantly more complex.

Consider a bank transfer where Alice and Bob have accounts in different databases. Maybe your bank grew large enough that you had to shard user accounts across multiple databases. Alice's account lives in Database A while Bob's account lives in Database B. Now you can't use a single database transaction to handle the transfer. Database A needs to debit $100 from Alice's account while Database B needs to credit $100 to Bob's account. Both operations must succeed or both must fail. If Database A debits Alice but Database B fails to credit Bob, money disappears from the system.
You have several options for distributed coordination, each with different trade-offs:

## Two-Phase Commit (2PC)

The classic solution is two-phase commit, where your transfer service acts as the coordinator managing the transaction across multiple database participants. The coordinator (your service) asks all participants to prepare the transaction in the first phase, then tells them to commit or abort in the second phase based on whether everyone has successfully prepared.

Critically, the coordinator must write to a persistent log before sending any commit or abort decisions. This log records which participants are involved and the current state of the transaction. Without this log, coordinator crashes create unrecoverable situations where participants don't know whether to commit or abort their prepared transactions.

Note: Keeping transactions open across network calls is extremely dangerous. Those open transactions hold locks on Alice's and Bob's account rows, blocking any other operations on those accounts. If your coordinator service crashes, those transactions stay open indefinitely, potentially locking the accounts forever. Production systems add timeouts to automatically rollback prepared transactions after 30-60 seconds, but this creates other problems like legitimate slow operations might get rolled back, causing the transfer to fail even when it should have succeeded.

The prepare phase is where each database does all the work except the final commit. Database A starts a transaction, verifies Alice has sufficient funds, places a hold on $100, but doesn't commit yet. The changes are made but not permanent, and other transactions can't see them. Database B starts a transaction, verifies Bob's account exists, prepares to add $100, but doesn't commit yet. In SQL terms, this looks like:

```sql
-- Database A during prepare phase
BEGIN TRANSACTION;
SELECT balance FROM accounts WHERE user_id = 'alice' FOR UPDATE;
-- Check if balance >= 100
UPDATE accounts SET balance = balance - 100 WHERE user_id = 'alice';
-- Transaction stays open, waiting for coordinator's decision

-- Database B during prepare phase  
BEGIN TRANSACTION;
SELECT * FROM accounts WHERE user_id = 'bob' FOR UPDATE;
-- Verify account exists and is active
UPDATE accounts SET balance = balance + 100 WHERE user_id = 'bob';
-- Transaction stays open, waiting for coordinator's decision
```

If both databases can prepare successfully, your service tells them to commit their open transactions. If either fails, both roll back their open transactions.

Two-phase commit guarantees atomicity across multiple systems, but it's expensive and fragile. If your service crashes between prepare and commit, both databases are left with open transactions in an uncertain state. If any database is slow or unavailable, the entire transfer blocks. Network partitions can leave the system in an inconsistent state.

## Distributed Locks

For simpler coordination needs, you can use distributed locks. Instead of coordinating complex transactions, you just ensure only one process can work on a particular resource at a time across your entire system.

For our bank transfer, you could acquire locks on both Alice's and Bob's account IDs before starting any operations. This prevents concurrent transfers from interfering with each other:

Distributed locks can be implemented with several technologies, each with different characteristics:

**Redis with TTL** - Redis provides atomic operations with automatic expiration, making it ideal for distributed locks. The SET command with expiration atomically creates a lock that Redis will automatically remove after the TTL expires. This eliminates the need for cleanup jobs since Redis handles expiration in the background. The lock is distributed because all your application servers can access the same Redis instance and see consistent state. When the lock expires or is explicitly deleted, the resource becomes available again. The advantage is speed and simplicity. Redis operations are very fast and the TTL handles cleanup automatically. The disadvantage is that Redis becomes a single point of failure, and you need to handle scenarios where Redis is unavailable.

**Database columns** - You can implement distributed locks using your existing database by adding status and expiration columns to track which resources are locked. This approach keeps everything in one place and leverages your database's ACID properties to ensure atomicity when acquiring locks. A background job periodically cleans up expired locks, though you need to handle race conditions between the cleanup job and users trying to extend their locks. The advantage is consistency with your existing data and no additional infrastructure. The disadvantage is that database operations are slower than cache operations, and you need to implement and maintain cleanup logic.

**ZooKeeper/etcd** - These are purpose-built coordination services designed specifically for distributed systems. They provide strong consistency guarantees even during network partitions and leader failures. ZooKeeper uses ephemeral nodes that automatically disappear when the client session ends, providing natural cleanup for crashed processes. Both systems use consensus algorithms (Raft for etcd, ZAB for ZooKeeper) to maintain consistency across multiple nodes.

The advantage is robustness. These systems are designed to handle the complex failure scenarios that Redis and database approaches struggle with. The disadvantage is operational complexity, as you need to run and maintain a separate coordination cluster.

Distributed locks aren't just for technical coordination either, they can dramatically improve user experience by preventing contention before it happens. Instead of letting users compete for the same resource, create intermediate states that give temporary exclusive access.

Consider Ticketmaster seat reservations. When you select a seat, it doesn't immediately go from "available" to "sold." Instead, it goes to a "reserved" state that gives you time to complete payment while preventing others from selecting the same seat. The contention window shrinks from the entire purchase process (5 minutes) to just the reservation step (milliseconds).

The same pattern appears everywhere. Uber sets driver status to "pending_request," e-commerce sites put items "on hold" in shopping carts, and meeting room booking systems create temporary holds.

The advantage is simplicity compared to complex transaction coordination. The disadvantage is that distributed locks can become bottlenecks under high contention, and you need to handle lock timeouts and failure scenarios.

## Saga Pattern

### What is it?

The **Saga Pattern** is a design pattern for managing **distributed transactions** across multiple microservices without using traditional two-phase commit (2PC). Instead of locking resources across services, a saga breaks down a transaction into a **sequence of local transactions**, where each service updates its own database and publishes an event or message to trigger the next step.

If any step fails, the saga executes **compensating transactions** to undo the changes made by previous steps, ensuring eventual consistency.

### Why Do We Need Saga Pattern?

In distributed systems and microservices architectures:

1. **No Distributed ACID Transactions** → Each microservice has its own database, making traditional ACID transactions across services impractical.
2. **Avoid Distributed Locks** → Two-phase commit (2PC) blocks resources and doesn't scale well.
3. **Maintain Data Consistency** → Need to ensure all services either complete successfully or rollback changes.
4. **Handle Failures Gracefully** → Services can fail independently, requiring a recovery mechanism.

### How It Helps?

The Saga Pattern provides:

1. **Eventual Consistency** → All services eventually reach a consistent state.
2. **Better Scalability** → No distributed locks or blocking operations.
3. **Fault Tolerance** → Automatic compensation (rollback) on failures.
4. **Service Independence** → Each service manages its own transactions.

### How Saga Pattern Works?

A saga consists of:

1. **Local Transactions** → Each service performs its own database transaction.
2. **Events/Messages** → Services communicate progress through events or messages.
3. **Compensating Transactions** → Reverse operations that undo changes if a step fails.

**Example Flow: E-commerce Order Processing**

```
Success Flow:
1. Order Service: Create Order (Status: PENDING)
2. Payment Service: Process Payment
3. Inventory Service: Reserve Items
4. Shipping Service: Schedule Delivery
5. Order Service: Update Order (Status: CONFIRMED)

Failure Flow (if Inventory fails):
1. Order Service: Create Order ✅
2. Payment Service: Process Payment ✅
3. Inventory Service: Reserve Items ❌ FAILED
4. Payment Service: Refund Payment (Compensating Transaction)
5. Order Service: Cancel Order (Compensating Transaction)
```

### Types of Saga Pattern

There are two main implementation approaches:

#### 1. Choreography-Based Saga

Services **communicate through events** without a central coordinator. Each service listens to events and knows what to do next.

**How It Works:**

- Each service publishes events after completing its transaction
- Other services subscribe to these events and react accordingly
- No central control; services are loosely coupled

**Example: Order Processing with Choreography**

```
1. Order Service creates order → publishes "OrderCreated" event
2. Payment Service listens to "OrderCreated" → processes payment → publishes "PaymentCompleted" event
3. Inventory Service listens to "PaymentCompleted" → reserves items → publishes "ItemsReserved" event
4. Shipping Service listens to "ItemsReserved" → schedules delivery → publishes "ShippingScheduled" event

If Inventory fails:
3. Inventory Service publishes "InventoryFailed" event
4. Payment Service listens to "InventoryFailed" → refunds payment → publishes "PaymentRefunded" event
5. Order Service listens to "PaymentRefunded" → cancels order
```

**Visual Diagram:**

```
┌─────────────────┐
│  Order Service  │
└────────┬────────┘
         │ publishes: OrderCreated
         ▼
    ┌─────────┐
    │  Event  │
    │   Bus   │
    └────┬────┘
         │
    ┌────┴─────────────────┬────────────────────┐
    ▼                      ▼                    ▼
┌──────────────┐   ┌──────────────┐   ┌────────────────┐
│   Payment    │   │  Inventory   │   │    Shipping    │
│   Service    │   │   Service    │   │    Service     │
└──────┬───────┘   └──────┬───────┘   └────────┬───────┘
       │                  │                     │
       │ PaymentCompleted │ ItemsReserved       │ ShippingScheduled
       └──────────────────┴─────────────────────┘
                          │
                          ▼
                    ┌─────────┐
                    │  Event  │
                    │   Bus   │
                    └─────────┘
```

**Pros:**
- Simple to implement for small workflows
- Services are loosely coupled
- No single point of failure

**Cons:**
- Hard to track overall saga state
- Complex to debug and monitor
- Circular dependencies can occur
- Difficult to understand the workflow

#### 2. Orchestration-Based Saga

A **central orchestrator** (saga coordinator) tells each service what to do and when. The orchestrator manages the entire workflow.

**How It Works:**

- Orchestrator sends commands to services
- Services execute and respond back to orchestrator
- Orchestrator decides next steps based on responses
- Orchestrator triggers compensating transactions on failures

**Example: Order Processing with Orchestration**

```
1. Order Orchestrator → Order Service: Create Order
2. Order Orchestrator → Payment Service: Process Payment
3. Order Orchestrator → Inventory Service: Reserve Items
4. Order Orchestrator → Shipping Service: Schedule Delivery
5. Order Orchestrator → Order Service: Confirm Order

If Inventory fails:
3. Inventory Service → Order Orchestrator: Reservation Failed
4. Order Orchestrator → Payment Service: Refund Payment
5. Order Orchestrator → Order Service: Cancel Order
```

**Visual Diagram:**

```
                    ┌──────────────────────┐
                    │  Order Orchestrator  │
                    │  (Saga Coordinator)  │
                    └──────────┬───────────┘
                               │
        ┌──────────────────────┼──────────────────────┐
        │                      │                      │
        ▼                      ▼                      ▼
┌──────────────┐      ┌──────────────┐      ┌────────────────┐
│   Payment    │      │  Inventory   │      │    Shipping    │
│   Service    │      │   Service    │      │    Service     │
└──────┬───────┘      └──────┬───────┘      └────────┬───────┘
       │                     │                       │
       │ Response            │ Response              │ Response
       └─────────────────────┴───────────────────────┘
                             │
                             ▼
                    ┌──────────────────────┐
                    │  Order Orchestrator  │
                    └──────────────────────┘
```

**Pros:**
- Clear workflow visibility
- Easier to monitor and debug
- Centralized logic for saga management
- Better control over the process

**Cons:**
- Orchestrator can become a single point of failure
- Orchestrator can become complex
- Services are coupled to orchestrator

### Implementing Compensating Transactions

**Key Principles:**

1. **Idempotency** → Compensating transactions must be idempotent (can be retried safely)
2. **Order Matters** → Compensations execute in reverse order of the saga steps
3. **Not Always Rollback** → Sometimes compensation is notification (e.g., send apology email)

**Example: Payment Compensation**

```python
# Forward Transaction
def process_payment(order_id, amount):
    payment_id = charge_credit_card(amount)
    save_payment_record(order_id, payment_id, "COMPLETED")
    return payment_id

# Compensating Transaction
def refund_payment(order_id):
    payment = get_payment_record(order_id)
    if payment.status == "COMPLETED":
        refund_credit_card(payment.payment_id)
        update_payment_record(payment.payment_id, "REFUNDED")
```

### Use Cases (When to Use Saga Pattern)?

Use Saga Pattern when:

1. **Microservices Architecture** → Multiple services with separate databases
2. **Long-Running Transactions** → Operations that span multiple services and take time
3. **No 2PC Available** → When you can't use distributed transactions
4. **Business Processes** → Order processing, booking systems, payment workflows
5. **Need Eventual Consistency** → Immediate consistency not required

**Common Use Cases:**

- **E-commerce Order Processing** → Order → Payment → Inventory → Shipping
- **Hotel Booking System** → Reserve room → Process payment → Send confirmation
- **Flight Booking** → Reserve seat → Payment → Ticket generation → Email confirmation
- **Money Transfer** → Debit account A → Credit account B

### Choreography vs Orchestration: Which to Choose?

| Aspect | Choreography | Orchestration |
| --- | --- | --- |
| **Control** | Decentralized (no coordinator) | Centralized (orchestrator manages) |
| **Coupling** | Loose coupling between services | Services coupled to orchestrator |
| **Complexity** | Simple for few services, complex for many | Complex orchestrator, simple services |
| **Visibility** | Hard to track overall workflow | Easy to track and monitor |
| **Debugging** | Difficult to trace | Easier to debug |
| **Single Point of Failure** | No | Yes (orchestrator) |
| **Best For** | Simple workflows, few services | Complex workflows, many services |

**Rule of Thumb:**
- Use **Choreography** for 2-3 services with simple workflows
- Use **Orchestration** for 4+ services or complex business logic

### Best Practices for Saga Pattern

1. **Design Idempotent Operations** → All transactions and compensations should be idempotent
2. **Use Unique Transaction IDs** → Track saga execution with correlation IDs
3. **Implement Timeout Handling** → Set timeouts for each step to prevent hanging
4. **Log Everything** → Comprehensive logging for debugging
5. **Use Message Queues** → Ensure reliable message delivery (Kafka, RabbitMQ)
6. **Handle Partial Failures** → Design for scenarios where compensation might fail
7. **Monitor Saga State** → Track progress and failures in real-time
8. **Test Compensation Logic** → Regularly test rollback scenarios

### Challenges and Limitations

1. **Lack of Isolation** → Other transactions can see intermediate states
2. **Complexity** → Harder to implement than simple transactions
3. **Debugging Difficulty** → Distributed nature makes troubleshooting complex
4. **Eventual Consistency** → Not suitable for operations requiring immediate consistency
5. **Compensation Logic** → Not all operations can be easily reversed (e.g., sending emails)

### When NOT to Use Saga Pattern?

1. **Single Database** → Use traditional ACID transactions
2. **Strong Consistency Required** → When eventual consistency is not acceptable
3. **Simple Operations** → Overhead not worth it for simple workflows
4. **Real-Time Systems** → When you need immediate consistency

### Saga Pattern with Event Sourcing

Combining Saga with Event Sourcing provides even better traceability:

```python
# Event-Sourced Saga
class OrderSagaEvents:
    ORDER_CREATED = "OrderCreated"
    PAYMENT_PROCESSED = "PaymentProcessed"
    PAYMENT_FAILED = "PaymentFailed"
    INVENTORY_RESERVED = "InventoryReserved"
    INVENTORY_FAILED = "InventoryFailed"
    SHIPPING_SCHEDULED = "ShippingScheduled"
    ORDER_CONFIRMED = "OrderConfirmed"
    ORDER_CANCELLED = "OrderCancelled"

# Store all events for audit trail
event_store.append({
    'saga_id': saga_id,
    'event_type': OrderSagaEvents.ORDER_CREATED,
    'timestamp': datetime.now(),
    'data': order_data
})
```

### Tools and Frameworks for Saga Pattern

1. **Temporal** → Workflow orchestration engine
2. **Apache Camel** → Integration framework with saga support
3. **Axon Framework** → Java framework for CQRS and Saga
4. **Eventuate Tram** → Microservices framework with saga orchestration
5. **Netflix Conductor** → Workflow orchestration engine
6. **Camunda** → BPM platform for orchestration

### Summary

The Saga Pattern is essential for managing distributed transactions in microservices architectures. It trades immediate consistency for scalability and fault tolerance, using compensating transactions to maintain eventual consistency. Choose **choreography** for simple workflows and **orchestration** for complex business processes with multiple services.

# Architecture Patterns & Concurrency Control: Complete Comparison

This section provides a comprehensive comparison of different system architectures and which concurrency control mechanisms work best for each scenario.

## Architecture Types Overview

### 1. Single Backend Server + Single Database

**Characteristics:**
- All requests handled by one application instance
- Single database instance
- All code runs in the same JVM/process
- Simplest architecture, no distributed coordination needed

**Concurrency Challenges:**
- Multiple threads competing for same resources within one server
- Race conditions between threads
- Database-level conflicts from concurrent transactions

**Best For:**
- Small applications
- Internal tools
- Prototypes and MVPs
- Low-traffic systems (< 1000 concurrent users)

### 2. Multiple Backend Servers + Single Database

**Characteristics:**
- Load balancer distributes requests across multiple application servers
- All servers share a single database
- Database becomes the coordination point
- Most common architecture for medium-scale applications

**Concurrency Challenges:**
- Race conditions across different server instances
- Database is the single source of truth but also a bottleneck
- Code-level locks (synchronized, ReentrantLock) don't work across servers
- All coordination must happen at database or external distributed lock level

**Best For:**
- Medium-scale web applications
- Applications with read-heavy workloads
- Systems requiring strong consistency
- Traffic: 1K - 100K concurrent users

### 3. Multiple Backend Servers + Multiple Databases (Sharded/Partitioned)

**Characteristics:**
- Multiple application servers
- Multiple database instances (sharded by user ID, region, etc.)
- Data distributed across databases
- More complex coordination needed

**Concurrency Challenges:**
- Transactions that span multiple databases
- No single database can coordinate across shards
- Network partitions can cause inconsistencies
- Distributed transaction complexity

**Best For:**
- Large-scale applications
- Global applications (geo-distributed)
- High-throughput systems
- Traffic: 100K+ concurrent users

### 4. Multiple Backend Servers + Multiple Databases + Read Replicas

**Characteristics:**
- Multiple application servers
- Primary databases for writes
- Read replicas for read operations
- Replication lag introduces eventual consistency

**Concurrency Challenges:**
- Read-after-write consistency issues
- Replication lag
- Failover scenarios
- Stale reads from replicas

**Best For:**
- Read-heavy applications (90%+ reads)
- Analytics dashboards
- Content platforms (blogs, news sites)
- Social media feeds

### 5. Microservices Architecture (Multiple Services + Multiple Databases)

**Characteristics:**
- Independent services with separate databases (database per service pattern)
- Services communicate via APIs/events
- No shared database access between services
- Eventual consistency is the norm

**Concurrency Challenges:**
- Cross-service transactions
- Maintaining consistency across service boundaries
- No ACID guarantees across services
- Complex failure scenarios

**Best For:**
- Large organizations with multiple teams
- Complex business domains
- Systems requiring independent scaling
- High availability requirements

## Concurrency Control Mechanisms: When to Use What

### Decision Matrix by Architecture

| Architecture | Pessimistic Locking | Optimistic Locking | Two-Phase Commit | Distributed Locks | Saga Pattern | CQRS + Event Sourcing |
|---|---|---|---|---|---|---|
| **Single Backend + Single DB** | ✅ Perfect | ✅ Perfect | ❌ Not needed | ❌ Overkill | ❌ Overkill | ❌ Overkill |
| **Multiple Backend + Single DB** | ✅ Excellent | ✅ Excellent | ❌ Not needed | ⚠️ Optional | ❌ Not needed | ⚠️ Optional |
| **Multiple Backend + Multiple DB** | ⚠️ Per DB only | ⚠️ Per DB only | ✅ Suitable | ✅ Required | ✅ Preferred | ✅ Excellent |
| **Multiple Backend + Replicas** | ⚠️ Primary only | ✅ Good | ⚠️ Complex | ✅ Required | ⚠️ Optional | ✅ Excellent |
| **Microservices** | ❌ Insufficient | ❌ Insufficient | ⚠️ Avoid | ✅ Required | ✅ Perfect | ✅ Perfect |

### Detailed Guidance by Architecture

#### Single Backend Server + Single Database

**Available Options:**
1. **Pessimistic Locking (Code-Level)**
   - Use `synchronized` blocks or `ReentrantLock`
   - Works perfectly since all threads in same JVM
   - Simple and effective
   - Example: Blocking a seat during booking

2. **Pessimistic Locking (Database-Level)**
   - Use `SELECT ... FOR UPDATE`
   - Ensures database-level consistency
   - Better for transactions spanning multiple queries
   - Example: Bank account transfers

3. **Optimistic Locking (Code-Level)**
   - Use `AtomicReference` or `compareAndSet()`
   - Good for low-contention scenarios
   - Fast and non-blocking
   - Example: Updating user preferences

4. **Optimistic Locking (Database-Level)**
   - Use version columns with `@Version`
   - Allows concurrent reads
   - Retry on conflict
   - Example: E-commerce product updates

**When to Use What:**
- **High contention (many users competing for same resource)** → Pessimistic Locking
- **Low contention (conflicts are rare)** → Optimistic Locking
- **Critical operations (banking, payments)** → Database-level Pessimistic Locking
- **Fast operations (counters, simple updates)** → Code-level Optimistic Locking

**Example Scenario: Movie Seat Booking**
```java
// Pessimistic approach (database-level)
@Transactional
public void bookSeat(String seatId, String userId) {
    Seat seat = seatRepository.findByIdWithLock(seatId); // SELECT ... FOR UPDATE
    if (seat.isAvailable()) {
        seat.setStatus("BOOKED");
        seat.setUserId(userId);
        seatRepository.save(seat);
    }
}

// Optimistic approach (database-level)
@Entity
class Seat {
    @Version
    private int version;
    // ... other fields
}

@Transactional
public void bookSeat(String seatId, String userId) {
    Seat seat = seatRepository.findById(seatId);
    if (seat.isAvailable()) {
        seat.setStatus("BOOKED");
        seat.setUserId(userId);
        seatRepository.save(seat); // Throws OptimisticLockException if version changed
    }
}
```

#### Multiple Backend Servers + Single Database

**Key Constraint:** Code-level locks don't work across different servers. Must coordinate through database or external system.

**Available Options:**

1. **Database-Level Pessimistic Locking**
   - ✅ **Best Choice for Strong Consistency**
   - Works across all servers since database is shared
   - Prevents conflicts at the source
   - Example: `SELECT ... FOR UPDATE`

2. **Database-Level Optimistic Locking**
   - ✅ **Best Choice for High Concurrency**
   - Version-based conflict detection
   - Better performance than pessimistic
   - Works across all servers
   - Example: JPA `@Version` annotation

3. **Distributed Locks (Optional Enhancement)**
   - Redis/etcd for cross-server coordination
   - Useful for application-level locking before hitting database
   - Reduces database contention
   - Example: Lock user session during checkout

**When to Use What:**
- **Financial transactions, inventory updates** → Database Pessimistic Locking
- **User profile updates, content editing** → Database Optimistic Locking
- **Multi-step workflows (reserve → process → confirm)** → Distributed Locks + Database Locking
- **Reducing database load** → Distributed Locks (Redis) before database operations

**Example Scenario: E-commerce Inventory Management**
```java
// Database-level pessimistic locking (recommended)
@Transactional
public boolean purchaseProduct(Long productId, int quantity) {
    Product product = productRepository.findByIdWithLock(productId); // SELECT ... FOR UPDATE
    if (product.getStock() >= quantity) {
        product.setStock(product.getStock() - quantity);
        productRepository.save(product);
        return true;
    }
    return false;
}

// With distributed lock for additional coordination
public boolean purchaseProductWithRedisLock(Long productId, int quantity) {
    String lockKey = "product_lock_" + productId;
    
    if (redisLock.tryLock(lockKey, 10)) { // Try to acquire Redis lock
        try {
            // Now perform database operation with pessimistic lock
            return purchaseProduct(productId, quantity);
        } finally {
            redisLock.unlock(lockKey);
        }
    }
    return false; // Couldn't acquire lock
}
```

#### Multiple Backend Servers + Multiple Databases (Sharded)

**Key Constraint:** No single database can coordinate. Operations may span multiple database instances.

**Available Options:**

1. **Two-Phase Commit (2PC)**
   - For cross-database ACID transactions
   - Expensive and blocking
   - Coordinator failure can lock resources
   - Use only when absolutely necessary
   - Example: Money transfer across sharded accounts

2. **Distributed Locks (Essential)**
   - ✅ **Required for Resource Coordination**
   - Redis Redlock, ZooKeeper, or etcd
   - Coordinate access across shards
   - Example: Ensure only one server processes a job

3. **Saga Pattern**
   - ✅ **Preferred over 2PC**
   - Eventual consistency with compensation
   - Better fault tolerance
   - More scalable
   - Example: Order processing across services

4. **CQRS + Event Sourcing**
   - ✅ **Best for Complex Domains**
   - Separate read/write models
   - Event-driven consistency
   - Excellent auditability
   - Example: Banking transaction history

**When to Use What:**
- **Cross-shard ACID required (rare)** → Two-Phase Commit
- **Resource coordination** → Distributed Locks (ZooKeeper/etcd)
- **Business workflows** → Saga Pattern (Orchestration)
- **Event-driven systems** → Saga Pattern (Choreography) + Event Sourcing
- **Complex read/write patterns** → CQRS + Event Sourcing
- **High-scale applications** → Avoid 2PC, use Saga + Distributed Locks

**Example Scenario: Bank Transfer Across Shards**

```java
// Option 1: Two-Phase Commit (expensive, avoid if possible)
@Transactional
public void transferMoney2PC(String fromAccount, String toAccount, BigDecimal amount) {
    // Coordinator manages 2PC across Database A and Database B
    TransactionCoordinator coordinator = new TransactionCoordinator();
    
    // Phase 1: Prepare on both databases
    boolean db1Ready = coordinator.prepare(databaseA, () -> {
        debitAccount(fromAccount, amount);
    });
    
    boolean db2Ready = coordinator.prepare(databaseB, () -> {
        creditAccount(toAccount, amount);
    });
    
    // Phase 2: Commit or Abort
    if (db1Ready && db2Ready) {
        coordinator.commit(); // Commit both
    } else {
        coordinator.abort(); // Rollback both
    }
}

// Option 2: Saga Pattern (preferred)
public void transferMoneySaga(String fromAccount, String toAccount, BigDecimal amount) {
    String sagaId = UUID.randomUUID().toString();
    
    try {
        // Step 1: Debit from account
        debitAccount(fromAccount, amount);
        publishEvent(new MoneyDebitedEvent(sagaId, fromAccount, amount));
        
        // Step 2: Credit to account
        creditAccount(toAccount, amount);
        publishEvent(new MoneyCreditedEvent(sagaId, toAccount, amount));
        
        // Step 3: Confirm transaction
        publishEvent(new TransferCompletedEvent(sagaId));
        
    } catch (Exception e) {
        // Compensation: Rollback debit
        compensateDebit(fromAccount, amount);
        publishEvent(new TransferFailedEvent(sagaId, e.getMessage()));
    }
}

// Option 3: With Distributed Lock
public void transferMoneyWithLock(String fromAccount, String toAccount, BigDecimal amount) {
    // Acquire locks on both accounts (alphabetically to avoid deadlock)
    List<String> accounts = Arrays.asList(fromAccount, toAccount);
    Collections.sort(accounts);
    
    try (DistributedLock lock1 = lockService.acquireLock(accounts.get(0));
         DistributedLock lock2 = lockService.acquireLock(accounts.get(1))) {
        
        // Perform transfer within locked section
        transferMoneySaga(fromAccount, toAccount, amount);
    }
}
```

#### Microservices Architecture

**Key Constraint:** Services have independent databases. No shared state. Cross-service consistency is complex.

**Available Options:**

1. **Distributed Locks (Required)**
   - ✅ **Essential for Service Coordination**
   - Prevent duplicate processing
   - Leader election
   - Use ZooKeeper or etcd (not Redis for critical operations)

2. **Saga Pattern**
   - ✅ **Primary Pattern for Cross-Service Transactions**
   - Orchestration for complex workflows
   - Choreography for simple event chains
   - Compensating transactions for rollback

3. **CQRS + Event Sourcing**
   - ✅ **Ideal for Microservices**
   - Services publish events
   - Read models built from events
   - Eventual consistency across services
   - Perfect audit trail

4. **Two-Phase Commit**
   - ⚠️ **Generally Avoided**
   - Violates microservice independence
   - Creates tight coupling
   - Reduces availability
   - Use only for critical operations (rare)

**When to Use What:**
- **Order processing, booking workflows** → Saga Pattern (Orchestration)
- **Event-driven updates** → Saga Pattern (Choreography) + Event Sourcing
- **Complex business domains** → CQRS + Event Sourcing
- **Preventing duplicate processing** → Distributed Locks (ZooKeeper)
- **Leader election** → Distributed Locks (ZooKeeper/etcd)
- **Cross-service strong consistency (rare)** → Two-Phase Commit (last resort)

**Example Scenario: E-commerce Order Processing**

```java
// Saga Pattern with Orchestration (recommended)
@Service
public class OrderSagaOrchestrator {
    
    public void processOrder(OrderRequest request) {
        String sagaId = UUID.randomUUID().toString();
        
        try {
            // Step 1: Create Order
            String orderId = orderService.createOrder(request);
            sagaStateStore.save(sagaId, "ORDER_CREATED", orderId);
            
            // Step 2: Process Payment
            PaymentResult payment = paymentService.processPayment(orderId, request.getAmount());
            sagaStateStore.save(sagaId, "PAYMENT_COMPLETED", payment.getPaymentId());
            
            // Step 3: Reserve Inventory
            inventoryService.reserveItems(orderId, request.getItems());
            sagaStateStore.save(sagaId, "INVENTORY_RESERVED", orderId);
            
            // Step 4: Schedule Shipping
            shippingService.scheduleDelivery(orderId);
            sagaStateStore.save(sagaId, "SHIPPING_SCHEDULED", orderId);
            
            // Complete
            orderService.confirmOrder(orderId);
            sagaStateStore.save(sagaId, "COMPLETED", orderId);
            
        } catch (PaymentFailedException e) {
            // Compensation: Cancel order
            orderService.cancelOrder(orderId);
            sagaStateStore.save(sagaId, "COMPENSATED", "Payment failed");
            
        } catch (InventoryNotAvailableException e) {
            // Compensation: Refund payment and cancel order
            paymentService.refundPayment(payment.getPaymentId());
            orderService.cancelOrder(orderId);
            sagaStateStore.save(sagaId, "COMPENSATED", "Inventory unavailable");
        }
    }
}

// CQRS + Event Sourcing (for complex scenarios)
@Service
public class OrderEventSourcingService {
    
    public void processOrderWithEvents(OrderRequest request) {
        String orderId = UUID.randomUUID().toString();
        
        // Publish command event
        eventStore.append(new OrderCreatedEvent(orderId, request));
        
        // Other services listen to events and react
        // Payment Service listens to OrderCreatedEvent
        // Inventory Service listens to PaymentCompletedEvent
        // Shipping Service listens to InventoryReservedEvent
    }
}

// Event Handlers in different services
@EventHandler
public void onOrderCreated(OrderCreatedEvent event) {
    // Payment Service processes payment
    PaymentResult result = processPayment(event.getOrderId(), event.getAmount());
    eventStore.append(new PaymentCompletedEvent(event.getOrderId(), result.getPaymentId()));
}

@EventHandler
public void onPaymentCompleted(PaymentCompletedEvent event) {
    // Inventory Service reserves items
    reserveInventory(event.getOrderId());
    eventStore.append(new InventoryReservedEvent(event.getOrderId()));
}
```

## Decision Tree: Choosing the Right Approach

```
START: What's your architecture?
│
├─ Single Backend + Single DB
│  │
│  └─ High contention (many users competing)?
│     ├─ YES → Pessimistic Locking (Database-level)
│     └─ NO → Optimistic Locking (Code or Database-level)
│
├─ Multiple Backends + Single DB
│  │
│  └─ What's more important?
│     ├─ Strong consistency → Pessimistic Locking (Database-level)
│     ├─ High throughput → Optimistic Locking (Database-level)
│     └─ Multi-step workflow → Distributed Locks + Database Locking
│
├─ Multiple Backends + Multiple DBs (Sharded)
│  │
│  └─ Transaction spans databases?
│     ├─ YES, ACID required → Two-Phase Commit (use cautiously)
│     ├─ YES, eventual consistency OK → Saga Pattern
│     └─ NO, same-DB operations → Pessimistic/Optimistic + Distributed Locks
│
└─ Microservices
   │
   └─ What are you coordinating?
      ├─ Cross-service transaction → Saga Pattern (Orchestration)
      ├─ Event-driven workflow → Saga Pattern (Choreography) + Event Sourcing
      ├─ Resource access → Distributed Locks (ZooKeeper/etcd)
      ├─ Complex domain → CQRS + Event Sourcing
      └─ Simple operations → Each service manages own transactions
```

## Real-World Scenario Recommendations

### Scenario 1: Movie Ticket Booking System

**Architecture:** Multiple Backend + Single DB (initially) → Multiple Backend + Multiple DBs (at scale)

**Phase 1 (Small Scale - Single DB):**
- Use **Database-Level Pessimistic Locking** for seat reservation
- Simple `SELECT ... FOR UPDATE` on seat rows
- Locks held only during booking transaction

**Phase 2 (Medium Scale - Single DB with Read Replicas):**
- **Pessimistic Locking** for writes (seat booking)
- Read replicas for browsing available seats
- Distributed Cache (Redis) for seat availability

**Phase 3 (Large Scale - Sharded DBs by Theater/Region):**
- **Distributed Locks** (Redis) for temporary seat reservation
- **Database Pessimistic Locking** for final booking
- Saga Pattern if booking spans services (payment, notification)

```java
// Phase 1: Simple pessimistic locking
@Transactional
public void bookSeat(String seatId, String userId) {
    Seat seat = seatRepository.lockAndFind(seatId);
    if (seat.isAvailable()) {
        seat.book(userId);
    }
}

// Phase 3: Distributed lock + Saga
public void bookSeatDistributed(String seatId, String userId) {
    // Step 1: Acquire distributed lock for temporary hold
    if (distributedLock.tryLock("seat_" + seatId, 300)) { // 5 min hold
        try {
            // Step 2: Reserve seat in database
            Seat seat = seatRepository.lockAndFind(seatId);
            seat.reserve(userId);
            
            // Step 3: Process payment (saga)
            sagaOrchestrator.processBooking(seatId, userId);
            
        } finally {
            distributedLock.unlock("seat_" + seatId);
        }
    }
}
```

### Scenario 2: E-commerce Platform

**Architecture:** Microservices with Multiple Databases

**Services:**
- Order Service (Order DB)
- Payment Service (Payment DB)
- Inventory Service (Inventory DB - sharded by product)
- Shipping Service (Shipping DB)

**Recommended Approach:**
- **Saga Pattern (Orchestration)** for order processing workflow
- **Distributed Locks (etcd)** for inventory coordination across shards
- **CQRS** for product catalog (write to primary, read from optimized read models)
- **Event Sourcing** for order history and audit trail

```java
// Saga Orchestrator
@Service
public class OrderOrchestrator {
    
    @Transactional
    public OrderResult processOrder(OrderRequest request) {
        // Create saga instance
        Saga saga = sagaFactory.create("ORDER_PROCESSING");
        
        saga.addStep(
            // Forward transaction
            () -> orderService.createOrder(request),
            // Compensation
            (orderId) -> orderService.cancelOrder(orderId)
        );
        
        saga.addStep(
            () -> paymentService.processPayment(saga.getOrderId(), request.getAmount()),
            (paymentId) -> paymentService.refund(paymentId)
        );
        
        saga.addStep(
            () -> inventoryService.reserveItems(saga.getOrderId(), request.getItems()),
            (reservationId) -> inventoryService.releaseItems(reservationId)
        );
        
        saga.addStep(
            () -> shippingService.schedule(saga.getOrderId()),
            (shippingId) -> shippingService.cancel(shippingId)
        );
        
        return saga.execute();
    }
}
```

### Scenario 3: Banking System

**Architecture:** Multiple Backend + Sharded Databases (by account range or region)

**Critical Requirement:** Strong consistency, ACID guarantees

**Recommended Approach:**
- **Database Pessimistic Locking** within same shard
- **Two-Phase Commit** for cross-shard transfers (unavoidable)
- **Distributed Locks (ZooKeeper)** for coordination
- **Event Sourcing** for audit trail (regulatory requirement)

```java
// Same-shard transfer: Simple pessimistic locking
@Transactional
public void transferWithinShard(String fromAccount, String toAccount, BigDecimal amount) {
    Account from = accountRepository.lockAndFind(fromAccount);
    Account to = accountRepository.lockAndFind(toAccount);
    
    from.debit(amount);
    to.credit(amount);
    
    // Event sourcing for audit
    eventStore.append(new TransferCompletedEvent(from.getId(), to.getId(), amount));
}

// Cross-shard transfer: Two-Phase Commit (necessary evil)
@Transactional
public void transferAcrossShards(String fromAccount, String toAccount, BigDecimal amount) {
    // Use distributed transaction coordinator
    GlobalTransaction gtx = transactionManager.begin();
    
    try {
        // Phase 1: Prepare both databases
        Database fromDB = getDatabaseForAccount(fromAccount);
        Database toDB = getDatabaseForAccount(toAccount);
        
        fromDB.prepare(() -> debitAccount(fromAccount, amount));
        toDB.prepare(() -> creditAccount(toAccount, amount));
        
        // Phase 2: Commit
        gtx.commit();
        
        // Event sourcing
        eventStore.append(new CrossShardTransferEvent(fromAccount, toAccount, amount));
        
    } catch (Exception e) {
        gtx.rollback();
        throw new TransferFailedException(e);
    }
}
```

### Scenario 4: Social Media Feed

**Architecture:** Microservices with Multiple Databases + Caching Layer

**Characteristics:** Read-heavy (99% reads), eventual consistency acceptable

**Recommended Approach:**
- **CQRS + Event Sourcing** for feed generation
- **Optimistic Locking** for user posts (low contention)
- **No distributed locks needed** (eventual consistency OK)
- **Saga Pattern (Choreography)** for content moderation workflow

```java
// Write side (Command): Create post
@Service
public class PostCommandService {
    
    public void createPost(PostRequest request) {
        // Optimistic locking for user posts
        Post post = new Post(request);
        post.setVersion(0);
        
        postRepository.save(post);
        
        // Publish event for read model update
        eventBus.publish(new PostCreatedEvent(post.getId(), post.getContent(), post.getAuthorId()));
    }
}

// Read side (Query): Generate feed
@Service
public class FeedQueryService {
    
    @EventHandler
    public void onPostCreated(PostCreatedEvent event) {
        // Update read model asynchronously
        List<String> followers = followerService.getFollowers(event.getAuthorId());
        
        for (String follower : followers) {
            // Update each follower's feed cache
            feedCache.addToFeed(follower, event.getPostId());
        }
    }
    
    public Feed getUserFeed(String userId) {
        // Read from optimized cache/read model
        return feedCache.getFeed(userId);
    }
}
```

## Performance Characteristics

### Latency Comparison (Approximate)

| Mechanism | Average Latency | Use Case |
|---|---|---|
| Code-level Optimistic (AtomicReference) | < 1 μs | In-memory counters |
| Code-level Pessimistic (synchronized) | < 10 μs | In-memory critical sections |
| Database Optimistic Locking | 1-10 ms | Low-contention updates |
| Database Pessimistic Locking | 5-20 ms | High-contention updates |
| Distributed Locks (Redis) | 1-5 ms | Cross-server coordination |
| Distributed Locks (ZooKeeper) | 10-50 ms | Strong consistency needs |
| Distributed Locks (etcd) | 10-50 ms | Cloud-native coordination |
| Two-Phase Commit | 50-200 ms | Cross-database ACID |
| Saga Pattern | 100-500 ms | Cross-service workflows |
| CQRS Read | 1-10 ms | Reading from read model |
| CQRS Write | 50-200 ms | Writing to event store |

## Common Anti-Patterns to Avoid

#### ❌ Anti-Pattern 1: Using 2PC Everywhere in Microservices

**Problem:** Kills availability and scalability

**Solution:** Use Saga Pattern instead

#### ❌ Anti-Pattern 2: No Timeout on Distributed Locks

**Problem:** Deadlocks when process crashes

**Solution:** Always set TTL on locks

#### ❌ Anti-Pattern 3: Pessimistic Locking for Read-Heavy Workloads

**Problem:** Blocks readers unnecessarily

**Solution:** Use Optimistic Locking or CQRS

#### ❌ Anti-Pattern 4: Optimistic Locking for High-Contention Resources

**Problem:** Too many retries, poor performance

**Solution:** Use Pessimistic Locking or Distributed Locks

#### ❌ Anti-Pattern 5: Using Code-Level Locks in Multi-Server Setup

**Problem:** Locks don't work across servers

**Solution:** Use Database-Level Locking or Distributed Locks

#### ❌ Anti-Pattern 6: Not Handling Partial Failures in Saga

**Problem:** Inconsistent state, no compensation

**Solution:** Design idempotent compensating transactions

#### ❌ Anti-Pattern 7: Strong Consistency Everywhere

**Problem:** Sacrifices availability and performance

**Solution:** Use eventual consistency where acceptable (CQRS)

## Summary: Quick Reference Guide

| Your Situation | Choose This | Why |
|---|---|---|
| Single server, high contention | Pessimistic Locking (DB) | Simple, effective, strong consistency |
| Single server, low contention | Optimistic Locking | Better performance, less blocking |
| Multiple servers, single DB, critical data | Pessimistic Locking (DB) | Works across servers, strong consistency |
| Multiple servers, single DB, high throughput | Optimistic Locking (DB) | High concurrency, retry on conflict |
| Multiple servers, multiple DBs, cross-DB transaction | Saga Pattern | Avoids 2PC complexity, fault-tolerant |
| Microservices, workflow coordination | Saga Pattern (Orchestration) | Clear workflow, easy to monitor |
| Microservices, event-driven | Saga Pattern (Choreography) + Event Sourcing | Loose coupling, scalable |
| Need temporary resource reservation | Distributed Locks (Redis) | Fast, with automatic expiration |
| Need high-reliability coordination | Distributed Locks (ZooKeeper/etcd) | Strong consistency, fault-tolerant |
| Read-heavy system | CQRS + Event Sourcing | Optimized read models, scales reads |
| Financial system, cross-shard | Two-Phase Commit (reluctantly) | Only when ACID is non-negotiable |
| Need audit trail | Event Sourcing | Complete history of all changes |

## Final Recommendations

1. **Start Simple:** Use the simplest approach that meets your requirements. Don't use Saga if a database transaction suffices.

2. **Scale Gradually:** 
   - Begin with Pessimistic/Optimistic Locking
   - Add Distributed Locks when you scale horizontally
   - Adopt Saga Pattern when you move to microservices
   - Implement CQRS when read/write patterns diverge

3. **Match Pattern to Architecture:**
   - Single DB → Pessimistic/Optimistic Locking
   - Multiple DBs → Distributed Locks + Saga
   - Microservices → Saga + Event Sourcing + CQRS

4. **Consider Trade-offs:**
   - Strong consistency vs. Availability (CAP theorem)
   - Latency vs. Consistency
   - Complexity vs. Scalability

5. **Monitor and Adapt:** Measure actual contention, latency, and throughput. Change approaches based on real data.

6. **Prepare for Failure:** Always design for partial failures, network partitions, and system crashes. This is not optional in distributed systems.

