# Workloads (Pod, Deployment, StatefulSet, DaemonSet, Job)

> Questions to know about Kubernetes workload types.

## Pods

- What is a Pod, and why can it contain multiple containers (sidecar pattern)?
- What is a sidecar container, and what's it commonly used for (log shipping, service mesh proxy, config reload)?
- What is an init container, and how does it differ from a sidecar?
- What happens to a Pod's IP address and local state when it restarts or is rescheduled?

## Controllers

- What's the difference between a ReplicaSet and a Deployment, and why do you almost always use a Deployment directly?
- What is a rolling update, and how does a Deployment perform one without downtime (`maxSurge`/`maxUnavailable`)?
- What's the difference between a Deployment and a StatefulSet — why do stateful workloads (databases) need stable identity and storage?
- What is a DaemonSet, and when would you use one (log collector, node monitoring agent on every node)?
- What's the difference between a Job and a CronJob, and how does a Job guarantee completion?

## Interview Usage

- How would you decide whether a service in your design should be a Deployment vs a StatefulSet?
- How would you explain zero-downtime deploys to an interviewer who asks "how do you roll out a new version safely"?
