# Consistent Hashing

> Questions to know before using consistent hashing as a building block in a system design interview.

## Core Concepts

- What problem does consistent hashing solve compared to naive `hash(key) % N` sharding?
- Why does modulo-based hashing cause almost all keys to remap when a node is added or removed, and how does consistent hashing minimize that?
- How does the hash ring work — how do you map both nodes and keys onto the same ring, and how do you find the owner of a key?

## Mechanics

- What's the problem with a small number of nodes on the ring (uneven load distribution), and how do virtual nodes solve it?
- How do you handle node failure on the ring, and what happens to the keys that were owned by the failed node?
- How does consistent hashing support replication (e.g., "the next N nodes clockwise own replicas of this key")?

## Interview Usage

- What systems use consistent hashing in practice (Cassandra, DynamoDB, Redis Cluster's hash slots, CDNs, load balancers)?
- What's the trade-off between consistent hashing and simpler range-based partitioning?
- What is rendezvous hashing (highest random weight), and how does it compare as an alternative?
- Could you explain consistent hashing on a whiteboard in under two minutes if asked?
