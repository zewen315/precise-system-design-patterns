# Network Infrastructure

> The building blocks combined in a system design. See `../components/` for deep dives on `load-balancer.md`, `cdn.md`, `api-gateway.md`, and `rate-limiter.md` — this file is the networking-layer view of how they fit together.

## Reverse Proxy & Load Balancer

- What's the difference between a forward proxy (acts on behalf of the client, hiding the client from the server) and a reverse proxy (acts on behalf of the server, hiding backends from the client)?
- L4 vs L7 load balancing — see `../components/load-balancer.md` for the deep dive. From a pure networking angle, what does an L4 load balancer actually touch (IP/TCP headers only, often via DSR or NAT) vs an L7 one (terminates the connection and reads HTTP)?
- What is Direct Server Return (DSR), and how does it let a load balancer handle very high inbound throughput by only ever seeing request traffic, not response traffic?

## Service Discovery

- What problem does service discovery solve in a dynamic environment where instances are constantly created/destroyed (hardcoded IPs go stale almost immediately)?
- What's the difference between client-side discovery (the client queries a registry and picks an instance itself) and server-side discovery (the client just calls a fixed endpoint — e.g. a load balancer — which looks up healthy instances)?
- How does DNS-based service discovery work inside a cluster (e.g. Kubernetes' `my-service.namespace.svc.cluster.local` — see `../kubernetes/networking.md`)?
- What is a service registry (Consul, etcd, ZooKeeper — see `../components/zookeeper.md`), and how do instances register/deregister and stay listed (heartbeats/TTL leases)?

## Global Load Balancing & Failover

- What's the difference between DNS-based global load balancing (see `dns.md`) and anycast-based (many data centers announce the same IP via BGP, and routing — not DNS — picks the nearest one, see `routing-protocols.md`)?
- What's the difference between active health checking (the LB probes backends itself) and passive health checking (the LB infers health from real traffic failures)?
- What is a split-brain failover scenario, and why is fully automatic failover risky without care (both sides can end up believing they're primary)?

## Firewalls, ACLs, Security Groups

- What's the difference between a stateless ACL (evaluates each packet independently, so you must explicitly allow both directions) and a stateful firewall/security group (tracks connections and automatically allows return traffic)?
- What is the principle of least privilege applied to network access (default-deny, only open what's needed), and why do cloud security groups usually default-deny inbound?
- What's the difference between a network-level firewall (IP/port rules) and a web application firewall (WAF, which inspects HTTP payloads for attack patterns)?

## VPN & Private Connectivity

- What problem does a VPN solve (extending a private network over an untrusted one, via an encrypted tunnel)?
- What's the difference between a site-to-site VPN and a client VPN?
- What is a VPC peering connection, and how does it differ from a VPN (peering is routed directly over the cloud provider's backbone — no encryption tunnel or public internet transit)?
- What is a transit gateway/hub, and what problem does it solve when many VPCs all need to talk to each other (avoiding an unmanageable full mesh of individual peering connections)?

## Service Mesh

- What problem does a service mesh solve that a plain load balancer/API gateway doesn't (uniform east-west traffic control — retries, timeouts, mTLS, observability — between every service, without each service implementing it itself)?
- What is the sidecar pattern, and how does a proxy (e.g. Envoy) attached to every service instance intercept and manage its traffic transparently?
- What's the difference between the data plane (the sidecar proxies actually moving traffic) and the control plane (the component configuring all the sidecars, e.g. Istio's control plane)?
- What's the operational cost of a service mesh (added latency per hop, more moving parts, another thing to debug), and when is it worth it vs overkill?

## Interview / SRE Usage

- How would you decide whether a design needs an API gateway, a service mesh, both, or neither?
- How would you explain the request path for a call from the internet to an internal microservice, naming every network component it passes through?
