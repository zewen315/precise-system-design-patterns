# Postgres (Relational Database)

> Questions to know before using a relational database as a building block in a system design interview.

## Core Concepts

- What is ACID and how does Postgres guarantee each property?
- When do you choose a relational database over a NoSQL store, and what are you optimizing for (joins, transactions, ad-hoc queries)?
- What is a B-tree index and why is it the default index type?
- What's the difference between a clustered and a non-clustered index? Does Postgres have clustered indexes?
- What is the write-ahead log (WAL), and why does every write go through it before touching the actual data files?
- What is MVCC (multi-version concurrency control), and how does it let readers avoid blocking writers?

## Storage & Indexing

- How does a heap table differ from an index-organized table?
- What is a covering index, and how does it avoid an extra heap fetch?
- When would you use a B-tree vs a hash index vs a GIN/GiST index?
- What is index bloat, and why does it happen under MVCC?
- What does `VACUUM` do, and why is it necessary in Postgres specifically?
- What's the cost of adding an index — how does it affect write throughput?

## Transactions & Concurrency

- What are the four SQL isolation levels, and which anomalies does each prevent (dirty read, non-repeatable read, phantom read)?
- What isolation level does Postgres use by default, and how does its "repeatable read" differ from the SQL standard's?
- What is a deadlock, and how does the database detect and resolve one?
- What's the difference between optimistic and pessimistic locking, and when would you use `SELECT ... FOR UPDATE`?
- What causes a serialization failure under `SERIALIZABLE`, and how should the application handle it (retry)?

## Replication & High Availability

- What's the difference between synchronous and asynchronous replication, and what does each cost you (latency vs durability)?
- What is streaming replication, and how does a replica stay caught up via WAL shipping?
- What is replication lag, and what problems does it cause for read replicas (e.g., read-your-writes)?
- How does failover work — what triggers a promotion, and what's the risk of split-brain?
- What's the difference between logical and physical replication, and when would you use each?

## Scaling

- What's the difference between vertical scaling, read replicas, and sharding for a relational database?
- What is connection pooling (e.g., PgBouncer), and why does Postgres need it at scale?
- What is table partitioning, and how does it differ from sharding?
- Why is sharding a relational database hard — what breaks (joins, foreign keys, cross-shard transactions, unique constraints)?
- How do you pick a shard key, and what happens with a bad choice (hot shard)?

## Failure Modes & Operations

- If the primary crashes mid-transaction, how does WAL replay guarantee durability on restart?
- How do you detect and fix an N+1 query problem?
- Why can a long-running transaction block `VACUUM` and cause bloat?
- How would you diagnose a sudden latency spike (`pg_stat_activity`, `EXPLAIN ANALYZE`, lock waits)?
- What does connection exhaustion look like, and how do you prevent it?

## Interview Usage

- How do you justify choosing Postgres over a NoSQL store for a given design?
- How do you estimate whether a single instance can handle the required QPS/storage, and when do you say "we need to shard"?
- How would you design multi-tenant isolation in Postgres (schema-per-tenant vs row-level `tenant_id`)?
- How do you keep strong consistency across services when Postgres is the source of truth?
