# Scheduling & Resource Management

> Questions to know about how Kubernetes decides where Pods run and how it protects node health.

## Scheduler

- What factors does the scheduler consider when placing a Pod (resource requests, affinity/anti-affinity, taints/tolerations, topology spread)?
- What's the difference between a resource request and a resource limit, and what happens when a Pod exceeds each (CPU throttling vs OOMKill)?
- What's the difference between node affinity, Pod affinity, and Pod anti-affinity — when would you use anti-affinity (spread replicas across nodes/zones)?
- What are taints and tolerations, and how do they let a node repel Pods unless explicitly tolerated?

## Health & Probes

- What's the difference between a liveness probe, a readiness probe, and a startup probe?
- What happens when a liveness probe fails vs when a readiness probe fails (restart vs removal from Service endpoints)?
- Why can a bad readiness probe configuration cause a rolling update to hang, or a healthy Pod to receive no traffic?

## Interview Usage

- How would you explain "why did my Pod get evicted" in terms of node resource pressure?
- How would you use anti-affinity and topology spread to make a design resilient to a single AZ failure?
