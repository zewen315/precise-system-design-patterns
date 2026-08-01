# Failure Modes & Debugging

> Questions to know about diagnosing problems in a Kubernetes cluster.

## Common Failures

- What does `CrashLoopBackOff` mean, and how do you debug it (`kubectl logs`, `kubectl describe pod`, previous container logs)?
- What does `ImagePullBackOff` mean, and what are the common causes (wrong tag, private registry auth, network policy)?
- What does `OOMKilled` mean, and how do you distinguish it from a liveness probe failure?
- What does `Pending` mean for a Pod, and what are the common causes (insufficient resources, unsatisfied affinity/taints, no matching node)?
- What is node pressure eviction, and what triggers it (disk, memory, PID pressure)?

## Cluster-Level Issues

- What happens to a node's Pods if the kubelet stops reporting (node `NotReady`) — how long before Pods are rescheduled?
- How do you debug a Service that isn't routing traffic (check Endpoints, selector labels, kube-proxy rules)?
- What is a "noisy neighbor" problem, and how do resource requests/limits and QoS classes (Guaranteed/Burstable/BestEffort) address it?

## Interview Usage

- If asked "how would you debug a service that's returning 502s intermittently in Kubernetes," what's your systematic approach?
- How would you explain the blast-radius difference between a bad Deployment rollout and a bad node?
