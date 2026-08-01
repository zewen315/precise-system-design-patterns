# Kubernetes Notes

> Notes on container orchestration, from container fundamentals to cluster scheduling, networking, and failure modes.

Kubernetes shows up in two different flavors of interview: as a passing mention in a system design interview ("we'd containerize this and run it on Kubernetes behind an HPA") and as the deep subject of an infrastructure/SRE interview. This directory covers both, and each file calls out which depth is expected where.

## What You'll Find

* Containers & Docker fundamentals
* Cluster architecture (control plane vs worker nodes)
* Workloads (Pod, Deployment, StatefulSet, DaemonSet, Job)
* Networking (Service, Ingress, DNS, CNI)
* Scheduling & resource management
* Scaling & safe rollouts
* Storage & configuration
* Failure modes & debugging

## Philosophy

Each file is written as the important questions to know before an interview, matching the format used in [components/](../components/README.md):

* Why does this piece exist?
* How does it actually work under the hood?
* What's the failure mode, and how do you debug it?
* How deep should you go with this in a system design interview vs an infra/SRE interview?

## Repository Structure

```text
kubernetes/
├── overview.md
├── docker.md
├── workloads.md
├── networking.md
├── scheduling.md
├── scaling.md
├── storage.md
└── failure-modes.md
```
