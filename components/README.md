# Components

> The recurring building blocks used to assemble a system design, with the important questions to know about each before an interview.

## Data Storage

* [postgres.md](postgres.md) — Relational database: ACID, indexing, replication, sharding.
* [nosql.md](nosql.md) — Key-value / document / wide-column stores: CAP, partitioning, consistency.
* [object-storage.md](object-storage.md) — Blob storage (S3-style): durability, multipart upload, pre-signed URLs.

## Caching & Messaging

* [redis.md](redis.md) — Cache: eviction, persistence, clustering, invalidation.
* [kafka.md](kafka.md) — Message queue / event streaming: partitions, delivery guarantees, consumer groups.

## Search & Coordination

* [elasticsearch.md](elasticsearch.md) — Search & indexing: inverted index, relevance, sharding.
* [zookeeper.md](zookeeper.md) — Coordination / consensus: leader election, distributed locks, watches.
* [distributed-lock.md](distributed-lock.md) — Mutual exclusion across processes: fencing tokens, Redlock.

## Networking & Delivery

* [load-balancer.md](load-balancer.md) — Traffic distribution: L4 vs L7, algorithms, health checks.
* [cdn.md](cdn.md) — Content delivery: caching strategy, invalidation, origin shielding.
* [api-gateway.md](api-gateway.md) — Single entry point: auth, routing, rate limiting.
* [websocket.md](websocket.md) — Real-time delivery: long-lived connections, fan-out, scaling.

## Scaling Patterns

* [consistent-hashing.md](consistent-hashing.md) — Minimal-remap partitioning: hash ring, virtual nodes.
* [sharding.md](sharding.md) — Data partitioning: shard key choice, resharding, cross-shard queries.
* [rate-limiter.md](rate-limiter.md) — Traffic shaping: token/leaky bucket, distributed enforcement.

Each file follows the same structure: core concepts, internals/architecture, trade-offs, failure modes, and how the component shows up in an interview — matching the [repository's philosophy](../README.md#philosophy) of understanding *why* over memorizing answers.
