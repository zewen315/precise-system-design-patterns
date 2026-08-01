# Sharding (Data Partitioning)

> Questions to know before sharding a datastore as part of a system design interview.

## Core Concepts

- What problem does sharding solve that a single database instance can't (write throughput, storage beyond one machine)?
- What's the difference between sharding and replication, and why do most systems need both?
- What are the common sharding strategies (range-based, hash-based, directory/lookup-based), and what are the trade-offs of each (hot spots vs efficient range queries)?

## Choosing a Shard Key

- What makes a shard key "good" (even distribution, aligns with query patterns) vs "bad" (hot shard, forces cross-shard queries)?
- How do you handle a unique constraint or auto-incrementing ID across shards?

## Cross-Shard Operations

- What happens to a query that needs data from multiple shards (e.g., a JOIN or aggregate) — how do you handle scatter-gather?
- How do you handle transactions that span multiple shards (two-phase commit, sagas, or avoiding them by design)?

## Resharding

- What is resharding, and why is it operationally risky (live data migration, dual writes, cutover)?
- How does consistent hashing reduce the amount of data that needs to move during resharding compared to naive modulo sharding?
- What is a hot shard, and how do you detect and mitigate it (splitting the shard, better key, caching)?

## Interview Usage

- How do you decide, in an interview, whether a design needs sharding at all vs whether a single primary plus read replicas is enough?
- How would you talk through the shard key choice out loud, including the failure mode of a bad choice?
