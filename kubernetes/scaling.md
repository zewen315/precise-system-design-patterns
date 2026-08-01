# Scaling & Rollouts

> Questions to know about scaling and safely deploying changes in Kubernetes.

## Autoscaling

- What is the Horizontal Pod Autoscaler (HPA), and what metrics can it scale on (CPU, memory, custom/external metrics)?
- What's the difference between HPA and the Vertical Pod Autoscaler (VPA), and why can't you safely run both on CPU/memory at once?
- What is the Cluster Autoscaler, and how does it add/remove nodes based on unschedulable Pods vs underutilized nodes?
- What's the cold-start problem with autoscaling (new Pods/nodes take time to become ready), and how do you mitigate it (over-provisioning, predictive scaling)?

## Safe Rollouts

- What is a PodDisruptionBudget, and what problem does it solve (protecting availability during voluntary disruptions like node drains)?
- How does a rolling update's `maxSurge`/`maxUnavailable` control the pace and risk of a deploy?
- What is a canary deployment, and how would you implement one on Kubernetes (traffic splitting via Ingress/service mesh, or a partial replica rollout)?
- How do you roll back a bad deployment quickly, and what does Kubernetes track to make that possible (revision history)?

## Interview Usage

- How would you explain autoscaling trade-offs (cost vs latency during traffic spikes) in a system design interview?
- How would you design a safe rollout strategy for a stateful vs a stateless service?
