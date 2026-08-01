# Networking (Service, Ingress, DNS)

> Questions to know about how traffic reaches a Pod in Kubernetes.

## Core Concepts

- Why does every Pod need a stable network identity even though Pods themselves are ephemeral — what problem does a Service solve?
- What's the difference between the `ClusterIP`, `NodePort`, and `LoadBalancer` Service types?
- How does kube-proxy route traffic to a Service's backing Pods (iptables/IPVS rules)?
- How does Kubernetes DNS let a Pod reach a Service by name (`my-service.my-namespace.svc.cluster.local`)?

## Ingress & Load Balancing

- What is an Ingress, and how does it differ from a Service (L7 routing, host/path-based rules, a single external LB fronting many services — see `components/load-balancer.md`)?
- What is an Ingress controller, and why does Ingress do nothing without one installed?
- How does an external `LoadBalancer` Service typically map to a cloud provider's load balancer?

## CNI & Pod Networking

- What is the Container Network Interface (CNI), and what problem does it solve (pluggable networking implementations)?
- What is the "flat network" model in Kubernetes — why can every Pod reach every other Pod by IP without NAT?
- What is a NetworkPolicy, and how does it restrict traffic between Pods, given the default is "allow all"?

## Interview Usage

- How would you explain the full path of a request from the internet to a Pod (LB → Ingress → Service → Pod)?
- When would you mention Kubernetes networking in a system design interview vs treat it as an implementation detail?
