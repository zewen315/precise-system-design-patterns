# Kubernetes Overview

> Questions to know about Kubernetes architecture before an infrastructure/SRE interview.

## Core Concepts

- What problem does Kubernetes solve that Docker alone doesn't (scheduling, self-healing, scaling, service discovery across many hosts)?
- What's the difference between a container and a Pod, and why does Kubernetes schedule Pods rather than raw containers?
- What is declarative configuration, and how does the reconciliation loop (desired state vs actual state) drive everything in Kubernetes?
- What's the difference between the control plane and worker nodes?

## Control Plane

- What does the API server do, and why is it the only component that talks to etcd directly?
- What is etcd, and why does Kubernetes store all cluster state in it (see `components/zookeeper.md` for the consensus concepts behind it)?
- What does the scheduler do, and how does it decide which node a Pod runs on?
- What does the controller manager do, and what is a "controller" (e.g., the Deployment controller, ReplicaSet controller)?
- What happens to already-running Pods if the entire control plane goes down?

## Worker Node

- What does the kubelet do, and how does it use the container runtime (containerd/CRI-O) to run containers?
- What does kube-proxy do, and how does it implement Service networking on each node?
- What is the Container Runtime Interface (CRI), and why did Kubernetes move away from a Docker-specific integration?

## Interview Usage

- How would you explain "what happens when you run `kubectl apply -f deployment.yaml`" end to end?
- Why do interviewers ask about Kubernetes in a system design interview at all — what signal are they actually checking for (deployment/scaling awareness), and what's overkill detail?
- What's the right depth to go into Kubernetes in a product-focused system design interview vs an infra/SRE interview?
