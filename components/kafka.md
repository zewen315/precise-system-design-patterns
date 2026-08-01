# Kafka (Message Queue / Event Streaming)

> Questions to know before using Kafka (or a message queue in general) as a building block in a system design interview.

## Core Concepts

- What problem does a message queue solve that a direct synchronous call doesn't (decoupling, backpressure, buffering, fan-out)?
- What's the difference between a traditional message queue (RabbitMQ/SQS) and a distributed log (Kafka)?
- What is a topic, a partition, and an offset, and how do they relate to ordering guarantees?
- Why is ordering only guaranteed within a partition, not across a topic?
- How do you choose a partition key, and what happens if you pick a bad one (hot partition)?

## Architecture

- What is a consumer group, and how does Kafka rebalance partitions across its members?
- What is the role of the broker, and how does the in-sync replica set (ISR) protect against broker failure?
- What do "leader" and "follower" mean for a partition, and what happens when a leader fails?
- Why did Kafka move away from ZooKeeper (KRaft), and what was ZooKeeper's original role?

## Delivery Guarantees

- What does `acks=0/1/all` mean, and how does it trade off latency vs durability?
- What are at-most-once, at-least-once, and exactly-once delivery, and how does Kafka achieve each?
- How do idempotent producers and transactions give exactly-once semantics?
- What's the difference between committing a consumer offset before vs after processing a message (at-most-once vs at-least-once)?
- What is a dead-letter queue, and why do you need one?

## Scaling & Operations

- How does Kafka handle backpressure when consumers are slower than producers?
- What is log compaction, and when would you use a compacted topic vs a normal one?
- How do you decide retention (time-based vs size-based) for a topic?
- What happens when you add partitions or brokers — how does the cluster rebalance?
- What causes consumer lag, and how do you monitor and alert on it?
- What happens if all replicas for a partition go down (unclean leader election)?

## Interview Usage

- Kafka vs SQS vs RabbitMQ — when would you pick each in a design?
- How would you use Kafka for event sourcing / CQRS, and what are the trade-offs?
- How do you size a Kafka cluster (partitions, throughput per partition, replication factor)?
- How would you use Kafka to decouple a write path from a slow downstream (search indexing, notifications, analytics)?
