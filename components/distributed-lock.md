# Distributed Lock

> Questions to know before using a distributed lock as a building block in a system design interview.

## Core Concepts

- What problem does a distributed lock solve (mutual exclusion across processes/machines, e.g., preventing duplicate job execution)?
- Why is a distributed lock fundamentally harder than an in-process mutex (no shared memory, network partitions, clock skew)?

## Implementations

- How do you implement a lock using a database row (`SELECT ... FOR UPDATE` or a unique constraint), and what's the risk if the holder crashes without releasing it?
- How do you implement a lock in Redis (`SET key value NX PX ttl`), and why does it need a TTL if the holder crashes?
- What is the "lock expires while the client is still working" problem, and how does a fencing token solve it?
- What is Redlock, and what are the known criticisms of its safety guarantees under GC pauses/clock drift (Martin Kleppmann's critique)?
- How does ZooKeeper/etcd implement a safer distributed lock using ephemeral nodes and sessions?

## Correctness

- What is a fencing token, and how does it prevent a "zombie" holder (one that thinks it still owns the lock after expiry) from corrupting shared state?
- What's the difference between a lock for correctness (must never be violated) and a lock for efficiency (best-effort, e.g., avoiding duplicate work) — does that change which implementation is "good enough"?

## Interview Usage

- How would you design idempotent job processing so the result is still correct even if two workers briefly run at once?
- When would you avoid a distributed lock entirely and use an idempotent/CAS-based approach instead?
