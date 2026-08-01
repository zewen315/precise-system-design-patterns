# Redis (Cache)

> Questions to know before using Redis as a building block in a system design interview.

## Core Concepts

- Why is Redis single-threaded for command execution, and how does that avoid locking overhead?
- What data structures does Redis support beyond simple key-value (list, hash, set, sorted set, stream, HyperLogLog, bitmap), and what is each used for?
- What's the difference between cache-aside, write-through, and write-back caching, and what are the trade-offs of each?
- What is cache stampede (thundering herd), and how do you prevent it (locking, request coalescing, jittered TTLs)?
- What is a hot key, and how do you mitigate it (local caching, key splitting, read replicas)?

## Memory & Eviction

- What eviction policies does Redis support (LRU, LFU, TTL-based, random), and how do you choose?
- How do you estimate memory usage / size a Redis deployment for a given workload?
- What happens when Redis hits `maxmemory` and no keys are evictable?

## Persistence & Durability

- What's the difference between RDB snapshotting and AOF, and what durability guarantee does each give?
- Why is Redis not durable by default, and what's the risk of data loss on a crash?
- What's the trade-off of `fsync` frequency in AOF (always/everysec/no)?

## Replication & Clustering

- How does Redis primary-replica replication work, and is it synchronous or asynchronous?
- What is Redis Sentinel, and what problem does it solve (automatic failover)?
- What is Redis Cluster, and how does it shard data across nodes (hash slots)?
- Why can Redis Cluster lose acknowledged writes during failover (async replication + failover)?
- What happens during a partial network partition in Redis Cluster?

## Consistency & Invalidation

- Why is cache invalidation considered one of the hard problems in computer science?
- How do you keep a cache consistent with the source of truth (TTL, write-time invalidation, pub/sub invalidation)?
- What's the risk of serving stale data, and when is it acceptable vs unacceptable?

## Beyond Caching

- How would you use Redis as a message broker (pub/sub vs streams), and how does that differ from Kafka?
- What is a distributed lock in Redis (`SETNX`/Redlock), and what are the known criticisms of Redlock's safety guarantees?
- When would you use Redis as a primary datastore instead of just a cache, and what do you give up?

## Failure Modes & Interview Usage

- How do you handle a Redis outage in your design — what's the fallback (degrade to DB read, circuit breaker)?
- When would you choose Memcached over Redis, or vice versa?
- How would you justify adding a cache layer, and what latency/QPS numbers make the case?
