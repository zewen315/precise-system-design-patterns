# ZooKeeper (Coordination / Consensus)

> Questions to know before using a coordination service as a building block in a system design interview.

## Core Concepts

- What problem does a coordination service solve that's hard to solve with a plain database (leader election, distributed locks, config, membership)?
- What consensus algorithm does ZooKeeper use (ZAB), and how does it relate to Paxos/Raft?
- What is a quorum, and why does ZooKeeper need an odd number of nodes (e.g., 3 or 5)?
- What's a znode, and what types exist (persistent, ephemeral, sequential)?

## Building Blocks on Top of ZooKeeper

- How do you implement leader election using ephemeral sequential znodes?
- How do you implement a distributed lock using ZooKeeper, and how does it avoid the split-brain risk of a naive lock?
- What are watches, and what's the trade-off of using them (one-time trigger, possibility of missed events between notification and re-watch)?

## Consistency

- What consistency guarantees does ZooKeeper provide (linearizable writes, sequential consistency for reads)?
- Why can a ZooKeeper read be stale, and how do you force a fresh read (`sync`)?
- What happens during a leader failure inside the ZooKeeper ensemble itself, and how does it recover?

## Interview Usage

- What's the difference between ZooKeeper, etcd, and Consul, and when would you choose each?
- Why do systems like Kafka and HBase depend on ZooKeeper, and why are some moving away from it (operational overhead, embedding Raft directly)?
- Why is ZooKeeper unsuitable as a general-purpose data store, even though it technically stores data?
- If an interviewer pushes back with "just use the database for coordination," how do you explain why that doesn't work?
