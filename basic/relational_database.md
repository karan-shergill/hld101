# Relational Database

A relational database organizes data into **tables** (rows and columns) and uses a **structured schema** to define the relationships between those tables. It relies on **SQL (Structured Query Language)** for querying and managing the data.

# Transaction

A database transaction is a sequence of one or more operations (such as SQL queries) performed as a single, logical unit of work on a database. A transaction is designed to ensure that either all operations are completed successfully or none of them take effect, preserving the integrity and consistency of the database.

### How a Transaction Works

1. **Start the Transaction**: This marks the beginning of a set of operations.
2. **Execute Operations**: Perform database operations like inserting, updating, or deleting data.
3. **Commit or Rollback:** **Commit -** If all operations are successful, the changes are saved permanently. **Rollback -** If any operation fails, all changes are undone.

### Example

```sql
-- Start a transaction
BEGIN TRANSACTION;

-- Step 1: Deduct $500 from Account A
UPDATE accounts
SET balance = balance - 500
WHERE account_id = 'A';

-- Step 2: Add $500 to Account B
UPDATE accounts
SET balance = balance + 500
WHERE account_id = 'B';

-- Commit the transaction if all operations succeed
COMMIT;

-- One can keep the check & business logic in service as well 
```

### **Types of Transactions**

1. **Single Transaction**: One operation (e.g., inserting a record).
2. **Batch Transaction**: Multiple operations grouped together.

### **Transactions Real-World Examples**

1. **E-commerce**: Place an order only if payment is successful and inventory is available.
2. **Ticket Booking**: Reserve a seat and confirm payment as a single transaction.
3. **Banking**: Ensure fund transfers are completed entirely or not at all.

NOTE: To keep the control at developer’s hand multiple statement transaction is prohibited by keeping the business logic in the service itself.

# Atomicity: A in ACID

The **A in ACID** stands for **Atomicity**. It ensures that a database transaction is treated as a single **atomic unit of work**. Either **all operations** within the transaction are completed successfully, or **none of them** are applied to the database.

### Key Points About Atomicity

1. A transaction can involve multiple operations, such as writing to multiple tables.
2. If any part of the transaction fails (e.g., due to a crash or error), the database will roll back all changes made during the transaction.
3. Atomicity ensures **no partial updates** are applied to the database, maintaining data integrity.

### **Real-Life Importance of Atomicity**

- **E-commerce**: Deduct inventory stock only if the payment is successful.
- **Ticket Booking**: Reserve a seat only if payment is confirmed; otherwise, release it for others.

### SQL example

```sql
-- Start a transaction
BEGIN TRANSACTION;

-- Step 1: Deduct $500 from Account A
UPDATE accounts
SET balance = balance - 500
WHERE account_id = 'A';

-- Step 2: Add $500 to Account B
UPDATE accounts
SET balance = balance + 500
WHERE account_id = 'B';

-- Commit the transaction if all operations succeed
COMMIT;
```

# Consistency: C in ACID

The **C in ACID** stands for **Consistency**. It ensures that a database moves from one **valid state** to another after a transaction is completed. Consistency guarantees that **rules, constraints, and integrity of the database are not violated** at any point.

### **Key Points About Consistency**

1. Consistency ensures that all data written to the database complies with predefined rules, such as:
    - Primary key constraints.
    - Foreign key constraints.
    - Unique constraints.
    - Check constraints or triggers.
2. If a transaction violates any constraint, it is **rolled back**, and the database remains in its original state.
3. Consistency depends on the database schema, constraints, and logic defined during design.

### **Example of Consistency**

**Scenario: E-commerce Inventory Management**

Consider an online store that sells items. The system has the following constraints:

1. The inventory count of a product **cannot go below zero**.
2. Orders should only be processed if there is enough inventory.

**Steps Without Consistency**

1. A transaction deducts 5 items from the inventory of Product A (current stock: 3).
2. This violates the rule (inventory < 0).
3. Without consistency, the inventory could end up in an invalid state (e.g., -2).

**Steps With Consistency**

1. A transaction tries to deduct 5 items from Product A's inventory (current stock: 3).
2. The database checks the constraint (`inventory >= 0`).
3. Since this violates the rule, the transaction is **rolled back**.
4. The inventory remains consistent, with no invalid states.

### SQL Example

```sql
-- Table structure with a constraint
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    inventory_count INT CHECK (inventory_count >= 0)
);

-- Begin transaction
BEGIN TRANSACTION;

-- Attempt to deduct 5 items from inventory
UPDATE products
SET inventory_count = inventory_count - 5
WHERE product_id = 1;

-- If the constraint is violated, rollback
COMMIT; -- Will fail if inventory_count < 0

```

# Constraints, Triggers, and Cascades in Consistency (C of ACID)

**Constraints, Triggers, and Cascades** are key mechanisms in relational databases to maintain **Consistency** as part of the **ACID** properties. They help enforce rules and integrity of data, ensuring that the database remains in a valid state during and after transactions.

### Constraints

Constraints are **rules** enforced at the database schema level to ensure data integrity and validity. They prevent invalid data from being entered into the database.

**Types of Constraints**

1. **Primary Key**:
    - Ensures each row in a table has a **unique identifier**.
    - Example: A `user_id` must be unique and not null.
    - Violation: Adding two rows with the same `user_id`.
2. **Foreign Key**:
    - Enforces a **relationship between two tables** by ensuring that a column matches the primary key in another table.
    - Example: An `order_id` in an `Orders` table must exist in the `Products` table.
    - Violation: Deleting a product that still has associated orders.
3. **Unique**:
    - Ensures that a column (or combination of columns) contains only unique values.
    - Example: Emails in a `Users` table must be unique.
4. **Check**:
    - Adds a custom rule to enforce specific conditions.
    - Example: `balance >= 0` for a `BankAccount` table.
    - Violation: Setting the balance to a negative value.
5. **Not Null**:
    - Ensures that a column cannot contain null values.
    - Example: A `username` cannot be null in a `Users` table.

**SQL example:**

```sql
CREATE TABLE Users (
    user_id INT PRIMARY KEY,
    email VARCHAR(255) UNIQUE,
    age INT CHECK (age >= 18),
    country VARCHAR(50) NOT NULL
);
```

### Triggers

Triggers are **automated actions** executed by the database in response to specific events, such as inserts, updates, or deletions. They are often used to enforce consistency by validating or modifying data.

**When to Use Triggers**

1. **Data Validation**:
    - Enforce business rules that cannot be expressed with constraints.
    - Example: Preventing salary updates that reduce it by more than 20%.
2. **Auditing**:
    - Log changes to critical tables automatically.
    - Example: Record every update made to the `balance` column in a `BankAccounts` table.
3. **Automatic Updates**:
    - Maintain consistency across related tables.
    - Example: When a product is deleted, set the status of all associated orders to "canceled."

**SQL Example**

```sql
CREATE TRIGGER update_audit_log
AFTER UPDATE ON BankAccounts
FOR EACH ROW
BEGIN
    INSERT INTO AuditLog (account_id, old_balance, new_balance, updated_at)
    VALUES (OLD.account_id, OLD.balance, NEW.balance, NOW());
END;
```

### Cascades

Cascades are mechanisms tied to **foreign key relationships** that define how changes in one table propagate to another. Cascades ensure consistency when related data is modified or deleted.

**Types of Cascades**

1. **ON DELETE CASCADE**:
    - If a parent row is deleted, all related rows in the child table are also deleted.
    - Example: Deleting a user also deletes their associated orders.
2. **ON UPDATE CASCADE**:
    - If a primary key in the parent table is updated, the corresponding foreign key in the child table is also updated.
3. **SET NULL**:
    - If a parent row is deleted, the foreign key in the child table is set to `NULL`.
    - Example: Deleting a category sets its associated products' `category_id` to `NULL`.
4. **RESTRICT**:
    - Prevents deletion or updates of parent rows if there are related child rows.
    - Example: Deleting a product fails if it still has associated orders.

**SQL Example**

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(user_id)
    ON DELETE CASCADE
    ON UPDATE CASCADE
);
```

**Comparison**

| Feature | Purpose | Example Use Case |
| --- | --- | --- |
| **Constraints** | Enforce rules at the schema level. | Prevent invalid data like negative balances. |
| **Triggers** | Perform automated actions in response to events. | Audit changes or validate complex business rules. |
| **Cascades** | Manage changes to related rows automatically. | Delete a user and cascade changes to their orders. |

# Isolation: I in ACID

The **I in ACID** stands for **Isolation**. It ensures that **concurrent transactions** are executed independently, without interfering with each other, and that the final result is the same as if the transactions were executed sequentially (one after the other). This prevents issues like dirty reads, lost updates, or inconsistent data caused by simultaneous access.

### **Key Points About Isolation**

1. Transactions run as if they are the **only transaction** in the system.
2. The level of isolation determines how visible the changes from one transaction are to others before completion.
3. Isolation helps maintain consistency and prevents race conditions in a multi-user environment.

### **Issues Addressed by Isolation**

1. **Dirty Read**: A **dirty read** happens when a transaction reads data that has been modified by another transaction but not yet committed. If the other transaction rolls back, the first transaction ends up using invalid or inconsistent data.
2. **Non-Repeatable Read**: A **non-repeatable read** occurs when a transaction reads the same data multiple times during its execution but gets **different results** because another transaction modified the data in between.
3. **Phantom Read**: A **phantom read** occurs when a transaction reads a set of rows that match a condition but, on subsequent reads, **new rows are added or removed** by another transaction, altering the results.
4. **Lost Update**: Two transactions modify the same data simultaneously, causing one update to overwrite the other.

| **Issue** | **Cause** | **Example Query** | **Isolation Level to Prevent** |
| --- | --- | --- | --- |
| **Dirty Read** | Reading uncommitted changes from another transaction. | `SELECT balance FROM Accounts` | **Read Committed** |
| **Non-Repeatable Read** | Re-reading a value that another transaction has modified and committed. | `SELECT stock FROM Products` | **Repeatable Read** |
| **Phantom Read** | Re-reading rows that have been added/removed by another transaction. | `SELECT SUM(salary) FROM Employees` | **Serializable** |

### **Isolation Levels**

Relational databases implement isolation through **isolation levels** defined by the SQL standard:

1. **Read Uncommitted**:
    - Transactions can read uncommitted changes from other transactions.
    - Issues: Dirty reads, non-repeatable reads, phantom reads.
2. **Read Committed**:
    - Transactions can only read data that has been committed.
    - Issues: Non-repeatable reads, phantom reads.
3. **Repeatable Read**:
    - Transactions can read the same data multiple times and get the same result.
    - Issues: Phantom reads.
4. **Serializable**:
    - The highest level of isolation. Transactions are executed as if they are serialized (executed one after the other).
    - Prevents all issues but has performance overhead.

| **Isolation Level** | **Dirty Read** | **Non-Repeatable Read** | **Phantom Read** |
| --- | --- | --- | --- |
| **Read Uncommitted** | ✗ | ✗ | ✗ |
| **Read Committed** | ✓ | ✗ | ✗ |
| **Repeatable Read** | ✓ | ✓ | ✗ |
| **Serializable** | ✓ | ✓ | ✓ |

# Understanding Isolation Levels in Detail

Isolation levels in relational databases define how transactions interact with one another, especially in concurrent environments. They impact **read** and **write** operations, the trade-off between **consistency** and **performance**, and whether issues like dirty reads, non-repeatable reads, or phantom reads can occur.

Here's a detailed breakdown of isolation levels:

| Isolation Level | Description | Issues Prevented | Performance Impact |
| --- | --- | --- | --- |
| **Read Uncommitted** | Transactions can read uncommitted changes from other transactions. | None | **High throughput** |
| **Read Committed** | Transactions can only read committed changes. | Dirty reads | Moderate throughput |
| **Repeatable Read** | Ensures that if a transaction reads a value twice, it sees the same value both times. Prevents updates. | Dirty reads, Non-repeatable reads | Lower throughput |
| **Serializable** | Ensures full isolation, as if transactions are executed sequentially. Prevents all conflicts. | Dirty reads, Non-repeatable reads, Phantom reads | **Lowest throughput (most overhead)** |

### **Choosing the Right Isolation Level**

Choosing the right isolation level depends on the application’s requirements for **consistency** and **performance**.

**When to Use Read Uncommitted**

- Use when:
    - You don’t require strict consistency.
    - Performance is a higher priority than data accuracy.
- Example: Logging systems or analytical dashboards where stale or partial data is acceptable.

**When to Use Read Committed**

- Use when:
    - You want to avoid dirty reads but can tolerate non-repeatable or phantom reads.
- Example: Most OLTP (Online Transaction Processing) systems, such as order management or ticket booking, where data accuracy during single reads is crucial.

**When to Use Repeatable Read**

- Use when:
    - Consistent reads are required for the same data during a transaction.
    - Updates during a transaction are not allowed but new inserts (phantom reads) can be tolerated.
- Example: Financial systems where balance calculations or multi-step workflows must be consistent.

**When to Use Serializable**

- Use when:
    - You need **strict consistency** and **isolation**.
    - Preventing all concurrency issues is critical.
- Example: Critical banking systems, inventory management systems where race conditions could cause overbooking or duplicate reservations.

### **Throughput and Latency at Each Isolation Level**

| Isolation Level | **Throughput** | **Latency** | **Concurrency** | **Consistency** |
| --- | --- | --- | --- | --- |
| **Read Uncommitted** | **Highest** (no locks) | Lowest | Very High | Low |
| **Read Committed** | High | Low | High | Moderate |
| **Repeatable Read** | Moderate | Moderate | Moderate | High (except phantom) |
| **Serializable** | Lowest (most locking/blocking) | Highest | Low | Very High |

Key Trade-offs:

1. **Higher Isolation = Lower Throughput**:
    - As isolation increases, the database uses more locks or checks, reducing concurrency.
2. **Lower Isolation = Higher Latency for Fixing Errors**:
    - Lower isolation may require application-level compensations for issues like dirty reads.

### **Isolation levels** and **locks**

The concepts of **isolation levels** and **locks** are closely related in relational databases, but they are not the same.

Key Differences:

| **Aspect** | **Isolation Levels** | **Locks** |
| --- | --- | --- |
| **Definition** | Rules for transaction interaction and visibility of data. | Mechanisms to enforce isolation by restricting access to data. |
| **Purpose** | Control concurrency anomalies like dirty reads and phantom reads. | Ensure data integrity by restricting data access. |
| **Level** | High-level policy for the transaction/session. | Low-level mechanism for physical enforcement. |
| **Application** | Configured globally, per session, or per transaction. | Automatically applied by the database engine or explicitly set by users. |
| **Granularity** | Affects all data the transaction accesses (based on rules). | Applied at row, page, or table level. |
| **Relation** | Isolation levels determine when and how locks are applied. | Locks implement the behavior dictated by isolation levels. |
| **Performance Impact** | Controls trade-off between consistency and concurrency. | Can reduce concurrency by blocking resources. |

# Durability: D of ACID

The **D** in **ACID** stands for **Durability** in relational databases. **Durability** ensures that once a transaction has been **committed**, it is **permanent** and will survive any subsequent system failures, such as crashes, power outages, or hardware failures.

Durability guarantees that after a transaction has been completed and committed:

- The changes made by the transaction are **permanent**.
- Even if the system crashes or faces a failure immediately after the commit, the database will **recover** to the state it was in after the transaction was committed, without losing any committed data.

### **How Durability Is Implemented**

Databases implement durability using several techniques to ensure that committed data is stored safely:

1. **Write-Ahead Logging (WAL)**:
    - Before any changes are made to the database, the transaction details (such as which data was modified) are written to a **log file**. This log entry is stored on disk before the actual changes are applied to the database.
    - If a failure occurs, the database can replay the log to restore any committed transactions.
2. **Transaction Journaling**:
    - The database keeps a record (journal) of all committed transactions, ensuring that it can be replayed after recovery, so no data is lost.
3. **Database Checkpoints**:
    - Periodically, the database will create a **checkpoint** where the state of the database is saved to disk. If a crash happens, the database can roll back to the last checkpoint and replay the transaction logs from that point to ensure durability.
4. **Replication**:
    - In some setups, **data replication** ensures that data is replicated to multiple servers. Even if one server crashes, the data will be available from another server.

# Scaling Relational Database

Scaling a relational database refers to the process of making it capable of handling increasing loads, such as more data, more queries, or higher user traffic. Scaling can be achieved in two main ways: **Vertical Scaling** and **Horizontal Scaling**. Each approach has its own trade-offs and is suited to different types of workloads.

### **Vertical Scaling (Scaling Up)**

**Vertical Scaling** involves adding more **resources (CPU, RAM, Storage)** to the **existing database server** to improve its performance. It's essentially about upgrading the hardware of the single machine running the database.

**How Vertical Scaling Works**

- You increase the capacity of the database server by adding more **CPU cores**, **RAM**, and **disk space** to the existing system.
- With more resources, the database can handle more queries, perform more complex operations, and store larger datasets.

**Pros of Vertical Scaling**

- **Simplicity**: No need to change the application logic or the database schema.
- **No Partitioning**: You continue using the same single machine and the same database.
- **Cost-Effective for Small Loads**: For modest increases in traffic, scaling vertically can be simpler and more cost-effective.

**Cons of Vertical Scaling**

- **Limits**: There's a physical limit to how much you can scale up a single server, as you eventually hit the maximum capacity of the hardware.
- **Single Point of Failure**: If the server goes down, the entire database becomes unavailable.
- **Costly**: High-end servers with more CPU, RAM, and storage can become very expensive.

**Example**:

- **Scenario**: A company has a single database server with 16GB of RAM and a quad-core processor. As more users access the application, the database performance starts to degrade.
- **Vertical Scaling**: The company upgrades the server to 64GB of RAM and an 8-core processor to handle more queries and larger datasets.

### **Horizontal Scaling (Scaling Out)**

**Horizontal Scaling** involves adding more **database servers** to distribute the load and handle more traffic. In this approach, the database workload is divided across multiple machines, which can be in a distributed architecture.

**How Horizontal Scaling Works**

- Instead of upgrading a single server, you add **more database nodes** (servers) to spread the load.
- Each node can be responsible for a portion of the data or the queries.

# **Types of Horizontal Scaling in Databases**

Horizontal scaling involves distributing your database workload across multiple servers (also called nodes). The main methods of horizontal scaling include **Sharding**, **Replication**, and **Partitioning**. Each technique helps manage large-scale databases with high throughput and ensures that the system can handle increased traffic while maintaining high availability.

### Replication

**Replication** involves creating copies of the database (or parts of it) on multiple servers. The idea is to have one **master** (primary) node for **writes** and one or more **replica** (secondary) nodes for **reads**.

**How Replication Works:**

- **Master (Primary) Node**: Handles all write operations (INSERT, UPDATE, DELETE). It is the authoritative source of data.
- **Replica Nodes**: These are read-only copies of the master node. They replicate the data from the master and can handle read queries (SELECT).
- **Synchronization**: Replicas continuously synchronize with the master to stay up to date.

There are two types of replication:

- **Master-Slave Replication**: The **master** node accepts writes and replicates data to one or more **slave** nodes, which are read-only.
- **Master-Master Replication**: Multiple nodes can accept writes, and each node replicates its data to the other nodes. This approach is more complex but can be useful for high-availability systems.

**Example**:

- A **website** that handles user queries can use **master-slave replication**:
    - The **master** database handles all **write operations** like updating user profiles or adding new posts.
    - Multiple **replica databases** handle **read operations** such as fetching user posts, displaying profiles, etc.

**Pros of Replication**:

- **Improved Read Performance**: By offloading read operations to replica nodes, you can improve performance, especially for read-heavy workloads.
- **High Availability**: If the master database goes down, a replica can take over, ensuring no downtime (with some replication lag).
- **Fault Tolerance**: Since multiple copies of the database exist, replication provides redundancy.

**Cons of Replication**:

- **Consistency Issues**: There may be a lag in replication, leading to eventual consistency. This means that after a write operation on the master, the data might not immediately be available on the replicas.
- **Write Bottleneck**: The master node can become a bottleneck for write-heavy workloads, as all writes are funneled through a single node.
- **Complexity**: Master-master replication requires careful conflict resolution and is harder to manage than master-slave replication.

### Sharding

**Sharding** is the process of splitting a large database into smaller, more manageable **parts**, called **shards**. Each shard is stored on a separate server, and each shard is responsible for a subset of the database’s data.

**How Sharding Works:**

- **Data Distribution**: Data is divided across multiple machines using a **shard key**. The shard key determines how data is distributed. The key could be based on attributes such as **user ID**, **geographical location**, or **date**.
- **Independent Shards**: Each shard operates independently of others, which allows the database to handle more requests and data volume.
- **Sharded Data**: Each shard contains a subset of the total dataset, and typically the same schema is used on all shards, but they contain different data.

**Example**:

- A **social media application** might shard data based on **user ID**.
    - **Shards 1**: Users with IDs 1-1,000,000.
    - **Shards 2**: Users with IDs 1,000,001-2,000,000.
    - **Shards 3**: Users with IDs 2,000,001-3,000,000, etc.
    
    When a user makes a request, the database knows which shard to access based on the **user ID**, making queries faster.
    

**Pros of Sharding**:

- **Scalability**: Sharding allows you to scale the database by adding more servers as data grows.
- **Performance**: By distributing the data across multiple nodes, sharding can improve query performance.
- **Fault Isolation**: A failure in one shard doesn’t affect the others, increasing fault tolerance.

**Cons of Sharding**:

- **Complexity**: Sharding introduces complexity in application logic. Your application needs to know how to access the right shard based on the shard key.
- **Cross-Shard Queries**: Queries that need to join data across multiple shards can be slower because they require communication between different servers.
- **Rebalancing**: As data grows, you may need to **rebalance** the shards, which can be difficult and time-consuming.

### Partitioning

**Partitioning** is the practice of dividing large tables into smaller, more manageable **pieces**, called **partitions**, which can be stored on different machines or within the same machine.

There are two types of partitioning:

1. **Horizontal Partitioning** (Row-level Partitioning): Splitting a large table into smaller tables based on row data.
2. **Vertical Partitioning** (Column-level Partitioning): Splitting a table into smaller tables by columns (often used when a table has very wide rows with many columns).

**Horizontal Partitioning (Sharding) Example**:

- **Large table**: Imagine a table that stores user data with millions of records. You can partition this table based on user **geographical region**.
    - **Partition 1**: Users from **North America**.
    - **Partition 2**: Users from **Europe**.
    - **Partition 3**: Users from **Asia**.
    
    This way, data specific to each region is stored separately, and queries can be directed to the appropriate partition.
    

**Vertical Partitioning Example**:

- **Wide Table**: A large table with user information might have columns like `user_id`, `email`, `phone_number`, `profile_picture`, `account_balance`, etc.
    - **Partition 1**: `user_id`, `email`, `phone_number` (frequently accessed columns).
    - **Partition 2**: `profile_picture`, `account_balance` (less frequently accessed columns).
    
    This helps to optimize performance by separating frequently accessed data from less frequently accessed data.
    

**Pros of Partitioning**:

- **Improved Query Performance**: Partitioning large tables into smaller, more manageable chunks can speed up queries that only need to access a subset of data.
- **Efficient Storage**: Partitioning helps manage large tables by storing partitions on separate disks, potentially reducing I/O bottlenecks.
- **Fault Tolerance**: Partitioning helps isolate failures by ensuring that if one partition fails, the others remain available.

**Cons of Partitioning**:

- **Complexity**: Managing partitions can become complex, especially when choosing partition keys and ensuring balanced data distribution.
- **Cross-Partition Queries**: Queries that need data from multiple partitions can be slower, especially in horizontal partitioning, as they require accessing multiple nodes or partitions.

### **When to Use Each Scaling Technique**

1. **Replication**: Use **replication** when you need to scale reads and improve **fault tolerance**. Replication is ideal for **read-heavy** applications like websites with a large number of users where read queries (such as displaying content) are common, and you want to avoid overloading the primary database with read traffic.
2. **Sharding**: Use **sharding** when the database grows beyond the capacity of a single machine and when you need to distribute data based on a **shard key**. It’s particularly useful when you have **write-heavy** workloads and need to balance load across multiple servers.
3. **Partitioning**: Use **partitioning** when you have a **single large table** or set of tables that need to be divided into smaller, more manageable pieces. Partitioning helps optimize performance and storage, especially when dealing with **large datasets** or **wide tables**.

### **Real-Life Examples of Horizontal Scaling**

1. **Replication**: **Netflix** uses **replication** to distribute copies of their databases globally, ensuring fast reads from multiple data centers. They use **master-slave replication** for content and user data.
2. **Sharding**: **Twitter** uses **sharding** to store tweets in multiple shards, based on tweet ID or user ID, to distribute the massive amount of data and handle high write throughput.
3. **Partitioning**: **Amazon** uses **partitioning** for their product catalog. The catalog is partitioned by categories (e.g., electronics, clothing) to improve query performance when users browse products.

# Scaling relational databases horizontally while maintaining **ACID**

Scaling relational databases horizontally while maintaining **ACID** (Atomicity, Consistency, Isolation, Durability) properties is a complex challenge. When you distribute data across multiple nodes or servers (via techniques like **sharding**, **replication**, or **partitioning**), it's more difficult to ensure that all the ACID properties are still upheld.

### Atomicity

**Atomicity** ensures that a transaction is all-or-nothing; if one part of the transaction fails, the entire transaction should be rolled back to maintain database consistency.

**Challenge in Horizontal Scaling**

- In a distributed setup (e.g., across multiple shards or replicas), a transaction might span across **multiple nodes**. If one part of the transaction succeeds on one node and another part fails on a different node, ensuring **atomicity** becomes complicated.

**How to Maintain Atomicity in Horizontal Scaling**:

- **Two-Phase Commit (2PC)**: A protocol for ensuring atomicity in distributed transactions. It involves two phases:
    1. **Prepare phase**: The coordinator node asks all participant nodes whether they can commit the transaction.
    2. **Commit phase**: If all participants agree to commit, the coordinator sends the "commit" signal to all participants. Otherwise, it sends a "rollback" signal.
    - **Downside**: 2PC can introduce latency and can block if one of the nodes fails or takes too long to respond.
- **Eventual Consistency**: Some systems opt for **eventual consistency** instead of strong consistency. This may allow a transaction to "succeed" even if not all parts of it are immediately applied, but the system will eventually ensure consistency across all nodes.

**Example**:

- In a **banking application**, a money transfer transaction (debiting from one account and crediting another) might be distributed across two shards. With 2PC, the database will ensure that both actions (debit and credit) are either committed together or rolled back together.

### Consistency

**Consistency** ensures that a transaction brings the database from one valid state to another, and that all constraints (like primary keys, foreign keys, etc.) are respected.

**Challenge in Horizontal Scaling**:

- **Data Distribution**: In sharding, data is distributed across multiple nodes. Ensuring **referential integrity** (e.g., foreign key relationships) between different shards or partitions becomes a challenge. For example, if you store customer orders in one shard and customer details in another, ensuring consistency across these shards becomes complex.
- **Distributed Data**: Different nodes may be out-of-sync temporarily, leading to **inconsistent** states if one shard has stale data.

**How to Maintain Consistency in Horizontal Scaling**:

- **Distributed Transactions**: Using techniques like **Two-Phase Commit (2PC)** or **Three-Phase Commit (3PC)** ensures that the database maintains consistency even when transactions span multiple nodes.
- **Eventual Consistency**: In some systems, rather than ensuring strict consistency at all times, the system may allow **eventual consistency**. This means that while the system may be temporarily inconsistent, it will converge to a consistent state after a period of time.
- **Distributed Integrity Checks**: Some systems use distributed checks and constraints to ensure that data across different partitions/shards still adheres to relational integrity, even in distributed scenarios.

**Example**:

In a **sharded e-commerce system**, a product's inventory might be stored in one shard, while orders are stored in another. If an order is placed for a product, ensuring that the inventory in one shard is consistent with the orders in another shard requires careful coordination, often relying on distributed transactions or eventual consistency.

### Isolation

**Isolation** ensures that the operations of one transaction are not visible to other transactions until they are completed (i.e., there is no interference between concurrent transactions).

**Challenge in Horizontal Scaling**:

- **Distributed Transactions**: When a transaction spans multiple nodes, ensuring that other transactions do not see partial updates or interfere with the transaction becomes challenging.
- **Concurrency**: Distributed databases may use different isolation mechanisms for managing concurrency across shards, and managing them effectively across multiple servers is more complex than in a single-server database.

**How to Maintain Isolation in Horizontal Scaling**:

- **Distributed Locking**: Systems can use **distributed locks** to ensure that only one transaction at a time can access certain data, similar to traditional locking mechanisms in relational databases. However, this can introduce overhead and reduce concurrency.
- **Transaction Coordination**: Using **Two-Phase Locking (2PL)** or **Serializable Isolation** in a distributed environment ensures that transactions are isolated, even if they are distributed across multiple nodes. This comes at a performance cost, as more locking and coordination are involved.
- **Optimistic Concurrency Control (OCC)**: Some systems use **OCC** where transactions are allowed to execute without locking resources, but before committing, the system checks if there were conflicts. If conflicts are detected, the transaction is aborted and retried.
- **Eventual Consistency**: In distributed systems, isolation may be relaxed in favor of **eventual consistency**, where transactions may not be fully isolated but will eventually reach a consistent state.

**Example**:

In a **multi-shard database**, if two users place orders for the same product, you need to ensure that one transaction doesn't read a partially updated product inventory from a shard while the other transaction is trying to write to it. This requires **distributed isolation** mechanisms like distributed locking or using **eventual consistency** with conflict resolution.

### Durability

**Durability** ensures that once a transaction is committed, its changes are permanent, even in the case of a system crash.

**Challenge in Horizontal Scaling**:

- **Replication**: In a horizontally scaled database with replication, ensuring that committed transactions are propagated to all replicas and shards, especially in the event of a failure, can be difficult.
- **Crash Recovery**: If a transaction is committed on one node but has not been replicated to others before a crash, recovering the system to a consistent state becomes tricky.

**How to Maintain Durability in Horizontal Scaling**:

- **Write-Ahead Logging (WAL)**: Most relational databases use a **write-ahead log** to ensure that before any data is written to the database, it is first recorded in a log. This ensures that even in the event of a crash, the transaction can be recovered.
- **Replication Protocols**: Systems often use **synchronous replication** to ensure that data is replicated to multiple nodes before a transaction is considered committed. This reduces the risk of data loss in case of a node failure.
- **Durable Commit**: Some systems use a **durable commit protocol**, where a commit is only considered successful when the data is fully replicated across all replicas or shards, ensuring durability across the distributed system.
- **Quorum-based Commit**: Systems can use a **quorum-based commit** mechanism, where a transaction is only considered committed once a majority of nodes (or replicas) have acknowledged the transaction.

**Example**:

In a **distributed banking system**, a money transfer transaction may be replicated to multiple nodes for fault tolerance. To ensure durability, the transaction is only marked as "committed" once the change has been written to the log and replicated to a majority of the nodes.

### Conclusion: Maintaining ACID in Horizontally Scaled Relational Databases

- **Distributed Transactions**: Methods like **Two-Phase Commit (2PC)**, **Three-Phase Commit (3PC)**, and **Quorum-based Commit** are used to maintain atomicity and consistency when transactions span multiple nodes.
- **Replication and Consistency Models**: Some databases allow for **eventual consistency** or **strong consistency** (via synchronous replication) depending on the use case.
- **Distributed Isolation**: Mechanisms like **distributed locking** and **optimistic concurrency control** are used to manage isolation in distributed systems.
- **Fault Tolerance**: **Write-ahead logging**, **replication**, and **quorum-based commit** ensure durability in the event of failures.

# Indexing in Databases

**Indexing** is a technique used in databases to **optimize the speed of data retrieval** operations. It works by creating a data structure (usually a **B-tree**, **hash table**, or other forms of indexes) that allows for faster searches, inserts, updates, and deletes. Think of an index as a **table of contents** for a book: instead of searching through every page of the book to find a topic, you can go directly to the section you're looking for.

### **Importance of Indexing**

- **Speed Up Queries**: Indexing significantly improves the **query performance** by reducing the time required to retrieve data. Without indexes, the database would need to perform a **full table scan**, which can be slow, especially for large datasets.
- **Efficient Sorting**: Indexes help with sorting operations. Queries that order results (e.g., `ORDER BY`) can be much faster when indexes are in place.
- **Faster Joins**: Indexing is important for optimizing joins, as the database can quickly find matching rows in different tables.
- **Reduces I/O Operations**: Since indexes allow the database to quickly locate data, it minimizes the number of disk reads, which leads to a decrease in overall I/O operations.

# Stages of Database Scaling: From 100 Users to 1 Billion Users

### Stage 1: Initial Setup (100 Users)

At the start, you can get away with a **single database instance**. The main focus will be on ensuring data integrity and fast access for users. The system's architecture may look like this:

- **Database**: A **single relational database** (e.g., PostgreSQL, MySQL, or a cloud database like AWS RDS).
- **Load**: The database handles both reads and writes efficiently because the load is minimal.
- **Scaling Needs**: Minimal—just a well-designed schema and careful index management to keep queries fast.
- **ACID Properties**: Easy to maintain since it's a monolithic system with limited concurrent access.

### **Stage 2: Growth to Thousands of Users**

As the number of users grows into the thousands, some **performance issues** will start to emerge, particularly around database **write-heavy operations** (e.g., user sign-ups, profile updates). In this stage, you may need to begin scaling the database.

- **Vertical Scaling**: Initially, the primary strategy will be **vertical scaling**, where you upgrade the server that hosts the database (e.g., adding more CPU, RAM, or disk storage). However, this is limited by the hardware.
- **Database Optimization**:
    - **Indexes**: Use indexes on commonly queried columns (like user IDs, timestamps).
    - **Query Optimization**: Ensure that queries are efficient (e.g., avoid SELECT *).
- **ACID Properties**: At this stage, database consistency and isolation can be maintained easily since the database is still relatively small, and all operations can be done in a single instance.

### **Stage 3: Growth to Tens of Thousands of Users**

Once the number of users reaches tens of thousands, **vertical scaling** starts to hit its limits. Now, it’s time to think about **horizontal scaling** to accommodate growth.

- **Sharding**: Implement **sharding** to split the data into smaller chunks and store them on different servers. A common approach would be to use the **user ID** as a shard key (e.g., all users with odd IDs are stored on one shard, even IDs on another).
- **Replication**: Use **replication** to create read replicas of the database. The **master node** handles writes, while **replica nodes** handle read queries to balance the load.
- **Load Balancing**: Set up **load balancers** to distribute requests to different database replicas or shards.
- **ACID Properties**: In this phase, maintaining **atomicity** and **consistency** across multiple shards can become more complex. To ensure atomicity across multiple shards, you may use **Two-Phase Commit (2PC)** or **Eventual Consistency** based on the application’s needs. However, strict consistency might slow down performance.

### **Stage 4: Growth to Hundreds of Thousands of Users**

As the user base grows, managing large amounts of data, especially across many shards and replicas, will require even more advanced techniques.

- **Database Partitioning**: Partition large tables (e.g., user activity logs, large datasets) into smaller, more manageable pieces. Partitioning by date, user location, or other business logic is common.
- **Read-Write Splitting**: As read traffic grows, you can further **scale reads** by adding more read replicas. Writes will still go to the master database.
- **Advanced Caching**: Use caching layers (e.g., **Redis**, **Memcached**) to reduce load on the database and improve response times, especially for frequent queries (e.g., user profile data).
- **Distributed Transactions**: At this stage, implementing distributed transactions to maintain **ACID** guarantees becomes more complex. You may need to trade-off between **strong consistency** and **eventual consistency** for certain parts of the system.
- **ACID Properties**:
    - **Consistency**: This can be maintained using **distributed transactions** (e.g., 2PC) or by moving to a **strongly consistent** database solution.
    - **Isolation**: Isolation might be relaxed in favor of higher throughput if performance is a priority (i.e., using **eventual consistency**).
    - **Durability**: Ensure **write-ahead logs** are in place to recover from crashes.

### **Stage 5: Scaling to Millions of Users**

At this stage, your system must be **highly distributed**, and you will be dealing with massive data volumes.

- **Multi-Region Replication**: To handle traffic from different geographic regions, deploy databases across multiple regions with **geo-replication** to reduce latency and ensure high availability.
- **Advanced Sharding**: You will need to refine your **shard strategy**—this could include **multi-key sharding** or using more advanced techniques like **hash-based** sharding.
- **Data Warehousing**: For analytics or reporting, use a **data warehouse** to offload heavy queries that are not time-sensitive from the main transactional database.
- **Eventual Consistency and CAP Trade-Off**: At this stage, you'll likely need to make trade-offs between **consistency** and **availability**. You might use **eventual consistency** for some operations to prioritize **availability** and **partition tolerance** (based on the **CAP theorem**).
- **ACID Properties**: For some operations, you may switch from **strong consistency** to **eventual consistency** to reduce latency and improve scalability. **Isolation** and **Durability** are still important, but you may use **distributed locking** or **Quorum-based Commit** to manage these properties effectively across multiple nodes.

### **Stage 6: Scaling to Billions of Users**

At this level, your system is incredibly complex and needs to handle **massive amounts of data** across a large, globally distributed infrastructure.

- **Sharded and Partitioned Databases**: The database will likely consist of **multiple shards** across several **data centers** and **regions**. Each shard may be partitioned to further distribute the load.
- **Microservices Architecture**: The database architecture may evolve from a single monolithic database to a **microservices-based architecture** where each service has its own database, but they communicate and share data through APIs.
- **Advanced Caching**: Use highly distributed caches like **Redis Cluster** or **Memcached** to handle frequently accessed data and reduce database load.
- **ACID Properties**:
    - **Atomicity**: Maintain **atomicity** across distributed systems using advanced **distributed transaction** mechanisms or **eventual consistency**.
    - **Consistency**: For certain operations, the system will rely on **eventual consistency** where consistency across the system may not be immediate but will be eventually achieved.
    - **Isolation**: Isolation will be more relaxed, with **optimistic concurrency control** and techniques that support **high throughput** and **low latency**.
    - **Durability**: Use **multi-zone replication**, **distributed storage systems**, and **backup strategies** to ensure that committed transactions are durable.

### **Summary of Database Scaling Strategy for 1 Billion Users**

1. **Stage 1 (100 Users)**: Start with a single database instance and ensure performance with good indexing and query optimization.
2. **Stage 2 (Thousands of Users)**: Begin vertical scaling and implement basic replication.
3. **Stage 3 (Tens of Thousands)**: Introduce sharding and read-write splitting.
4. **Stage 4 (Hundreds of Thousands)**: Implement partitioning, distributed transactions, and geo-replication.
5. **Stage 5 (Millions)**: Scale horizontally with advanced sharding, microservices, and data warehousing.
6. **Stage 6 (Billions)**: Achieve scalability with multi-region databases, distributed caches, and relaxed ACID properties in some parts of the system.

### **Tuning ACID Properties at Scale**

As you scale, **tuning ACID properties** becomes a balancing act between consistency, performance, and availability. Here's how you might adjust them at each stage:

- **Atomicity**:
    - In smaller systems, atomicity can be ensured with a single database.
    - In large distributed systems, use **distributed transactions** (like **Two-Phase Commit**) to ensure that all parts of a transaction across multiple nodes are either fully committed or rolled back.
- **Consistency**:
    - Initially, you'll have **strong consistency** with a single database instance. As you scale, you may transition to **eventual consistency** for certain less-critical parts of the system (e.g., user activity data).
    - Use **quorum-based writes** or **distributed locking** to ensure consistency in a distributed system. Eventually, some parts of the system may prioritize **availability** (as per the **CAP theorem**) over consistency.
- **Isolation**:
    - Start with **serializable isolation** for strict transaction isolation in a single database instance.
    - As you scale horizontally, use techniques like **optimistic concurrency control**, **distributed locking**, or **quorum-based isolation** to manage isolation in distributed environments, relaxing isolation levels when necessary to allow for higher throughput.
- **Durability**:
    - Use **write-ahead logs (WAL)** and ensure that **replication** is synchronous to guarantee durability at scale.
    - For very high scalability, use **multi-region replication** and ensure **backup and recovery strategies** are robust.

# Qs for an Interview

1. **Schema Design**: How would you design the database schema for an e-commerce platform like Amazon? What considerations would you have for scalability, indexing, and relationships between entities?
2. **Normalization vs. Denormalization**: What are the trade-offs between normalizing and denormalizing your database? In which scenarios would you prioritize one over the other?
3. **Transactions**:
    - Explain how ACID properties are maintained in a relational database during high-concurrency operations.
    - What challenges arise in maintaining ACID properties when scaling horizontally?

### Scaling and Performance

1. **Scaling Strategies**:
    - How would you scale a relational database to handle 1 billion users? Walk through the steps from a single-instance database to a distributed system.
    - Compare vertical scaling and horizontal scaling. When would you use one over the other?
2. **Read vs. Write Optimization**: What strategies would you implement to optimize for read-heavy workloads? What about write-heavy workloads?
3. **Sharding**: What are the challenges of implementing sharding in a relational database? How would you select a shard key for a multi-tenant application?
4. **Replication**: Explain the difference between synchronous and asynchronous replication. What are the trade-offs, and how would you decide which to use?
5. **Indexing**: When would adding an index hurt performance instead of improving it? How would you monitor and tune your indexes?
6. **Latency and Throughput**: How do different isolation levels impact the throughput and latency of a database system? Provide examples of use cases for each isolation level.

### Consistency, Availability, and Partition Tolerance

1. **CAP Theorem**: Explain how the CAP theorem applies to relational databases. How would you balance consistency, availability, and partition tolerance in a globally distributed system
2. **Eventual Consistency**: In which scenarios might eventual consistency be acceptable for a relational database? How would you implement it?

### Advanced and Real-World Challenges

1. **Distributed Transactions**: How would you implement a distributed transaction across multiple relational databases while maintaining ACID properties?
2. **Caching**: What role does caching play in scaling a relational database? What are the potential pitfalls of caching, and how would you avoid them?
3. **Multi-Region Databases**: How would you design a relational database for a globally distributed application with low latency requirements?
4. **Partitioning**: How would you decide between vertical partitioning (splitting by columns) and horizontal partitioning (splitting by rows) for a large database?
5. **Hotspots in Sharding**: What are database hotspots in sharding, and how would you design your system to avoid them?
6. **Data Integrity**: In a highly scaled system, how would you ensure data integrity across shards or replicas?

### Operational Challenges

1. **Monitoring and Alerting**: How would you monitor and troubleshoot a performance bottleneck in a relational database?
2. **Schema Changes**: How would you roll out schema changes (e.g., adding a new column) in a large-scale relational database with minimal downtime?
3. **Disaster Recovery**: How would you design a backup and disaster recovery strategy for a database handling millions of transactions per second?

### **Open-Ended Design Questions**

1. **E-Commerce System**:Imagine you're designing a relational database for an e-commerce site with 1 billion users. How would you:
    - Handle flash sales where millions of users order simultaneously?
    - Ensure inventory consistency across regions?
2. **Social Media Platform**: Design a database schema for a social media platform like Twitter. How would you handle scaling for operations like user feeds and mentions?
3. **IoT System**: How would you design a relational database to handle data from billions of IoT devices sending updates in real time?
4. **ACID vs. BASE**: In which scenarios would you consider moving away from strict ACID compliance to adopt a BASE (Basically Available, Soft state, Eventual consistency) model? How would you handle the transition?

### **Hypothetical Situations**

1. **Unexpected Load**: Suppose your e-commerce system experiences 10x unexpected traffic on Black Friday. How would your relational database handle the load? What bottlenecks might occur?
2. **Data Migration**: If you were tasked with migrating a monolithic relational database to a distributed setup, how would you approach it while ensuring minimal downtime?
3. **Join Optimization**: What challenges arise when performing joins across multiple shards? How would you optimize such queries?
4. **AI/ML Integration**: How would you design your database to support AI/ML workflows, such as product recommendations, while still maintaining performance for transactional queries?
