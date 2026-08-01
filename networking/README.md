# Networking Notes

> Computer networking from an SRE/backend perspective — the layers below every system design diagram — plus the network-engineering fundamentals (BGP, OSPF, anycast) that explain how the internet actually routes traffic.

## What You'll Find

* Layer 2: Ethernet, ARP, switches, VLANs, DHCP, MTU
* Layer 3: IPv4/IPv6, CIDR, routing tables, NAT, ECMP
* Transport: TCP/UDP mechanics, congestion control, QUIC
* DNS and HTTP/TLS
* Network infrastructure: load balancers, service discovery, VPNs, service mesh
* Routing protocols: BGP, OSPF, autonomous systems, anycast, SDN
* A layered SRE troubleshooting playbook

## Philosophy

Each file is written as the important questions to know before an interview or an incident, matching the format used in [components/](../components/README.md) and [kubernetes/](../kubernetes/README.md):

* What problem does this layer/protocol solve, and what would break without it?
* How does it actually work on the wire?
* What's the failure mode, and how do you tell it apart from a failure at a different layer?
* Where's the line between "an SRE should know this" and "that's a network engineer's job" — and this repo covers both, explicitly, since the audience includes both.

## Repository Structure

```text
networking/
├── overview.md            # index — topic outline, links into every file below
├── local-network.md         # Layer 2: MAC, ARP, switches, VLANs, DHCP, MTU
├── ip-routing.md              # Layer 3: IPv4/IPv6, CIDR, routing tables, NAT, ECMP
├── transport.md                 # TCP/UDP mechanics, congestion control, QUIC
├── dns.md                         # resolution, record types, caching, DNS-based traffic mgmt
├── http-tls.md                      # HTTP/1.1-2-3, TLS handshake, certs/PKI, mTLS
├── infrastructure.md                  # load balancers, service discovery, VPN, service mesh
├── routing-protocols.md                 # BGP, OSPF, AS/peering/transit, anycast, SDN
└── troubleshooting.md                     # layered SRE debugging playbook
```

`../linux/network.md` covers the host/kernel side (sockets, epoll, the NIC/DMA path) that this directory builds on top of; `../components/` covers the specific building blocks (load balancer, CDN, API gateway, rate limiter) referenced throughout.
