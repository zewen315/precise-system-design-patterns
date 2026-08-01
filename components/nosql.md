# NoSQL (Key-Value / Document / Wide-Column)

> Questions to know before using a NoSQL store (Cassandra, DynamoDB, MongoDB, etc.) as a building block in a system design interview.

## Core Concepts

- What problem does NoSQL solve that drove its adoption (horizontal scalability, flexible schema, high write throughput)?
- What's the difference between key-value, document, wide-column, and graph NoSQL databases — give an example product for each?
- What is denormalization, and why do NoSQL data models favor it over normalization/joins?
- What does "schema-on-read vs schema-on-write" mean, and what's the trade-off?

## Consistency & Replication

- What is the CAP theorem, and where do most NoSQL stores land (AP vs CP)? What happens during a network partition?
- What is eventual consistency, and what are weaker-but-useful guarantees like read-your-writes or monotonic reads?
- What is a quorum read/write (e.g., Cassandra's N/R/W), and how do you tune it for consistency vs latency/availability?
- How do these stores handle write conflicts across replicas (last-write-wins, vector clocks)?

## Partitioning

- How does a wide-column store choose a partition key, and why does a bad choice create a hot partition?
- How does auto-sharding work, and how does the store rebalance when you add a node?
- What is a secondary index in a NoSQL store, and why is it more limited/expensive than in a relational database?

## Trade-offs vs SQL

- What query limitations do you accept (no joins, limited ad-hoc querying), and how does that force you to model access patterns upfront?
- When would you choose a NoSQL store over Postgres in a design, and what do you give up (transactions, joins, ad-hoc queries)?
- How would you evolve a schema in a NoSQL store where there's no enforced schema?

## Interview Usage

- How do you decide between DynamoDB-style key-value, Cassandra-style wide-column, and MongoDB-style document for a given access pattern?
- How would you model a design's data so that every required query is a single-partition lookup?
