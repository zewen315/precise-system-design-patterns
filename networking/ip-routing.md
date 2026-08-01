# IP & Routing (Layer 3)

## IPv4 vs IPv6

- What problem does IPv6 actually solve (address space: 2^32 vs 2^128), and why did IPv4 exhaustion take so long to force the issue?
- What are the practical differences beyond address length (no mandatory NAT, no router-level fragmentation, built-in autoconfiguration/SLAAC, a simplified header)?
- Why has IPv6 adoption been slow despite exhaustion (NAT extended IPv4's usable life; dual-stack and compatibility costs; "if it isn't visibly broken")?

## CIDR & Subnetting

- What does CIDR notation (`10.0.0.0/24`) actually encode — network bits vs host bits?
- How do you compute usable hosts in a `/24` vs a `/16` vs a `/30`?
- Why is a `/30` the smallest practical subnet for a point-to-point link (2 usable addresses after network + broadcast)?
- Why does cloud VPC design lean on subnetting (isolating tiers, one subnet per AZ, controlling blast radius)?

## Routing Tables & Forwarding

- What is a routing table, and what does each entry contain (destination network, next hop, interface, metric)?
- What is longest prefix match, and why does a router always prefer the most specific matching route over a broader default route?
- What is a default route (`0.0.0.0/0`), and why does almost every host/router have one?
- `ip route show`, `route -n` — inspect a routing table.

## TTL & ICMP

- What is TTL (time to live), and what problem does it solve (stopping a misrouted packet from looping forever)?
- How does `traceroute` actually work — sending packets with increasing TTL and reading the "TTL exceeded" ICMP reply from each hop along the way?
- What is ICMP used for beyond ping (Destination Unreachable, Time Exceeded, Redirect, Fragmentation Needed)?
- Why do some networks block ICMP entirely, and what does that break (Path MTU Discovery, traceroute visibility)?

## Fragmentation

- What is IP fragmentation, and why can routers fragment an oversized packet in IPv4, while in IPv6 only the sending host may fragment?
- Why is fragmentation generally avoided in modern high-performance networking (per-fragment overhead, one lost fragment fails the whole packet, security/evasion concerns)?

## NAT

- What problem does NAT (Network Address Translation) solve — letting many private-IP hosts share one public IP?
- How does NAT track translations (a table of private IP:port ↔ public IP:port mappings)?
- What's the difference between source NAT (SNAT, typical for outbound traffic) and destination NAT (DNAT, typical for inbound/load balancing)?
- Why does NAT break protocols that embed IP addresses in their payload (e.g., legacy FTP), and why do modern protocols avoid that?
- What is NAT traversal / hole punching, and why do peer-to-peer protocols (WebRTC, some VoIP) need it?
- In cloud terms, what's the difference between a NAT gateway and a public IP attached directly to an instance?

## ECMP

- What is ECMP (equal-cost multi-path), and how does it let a router spread traffic across several equally-good paths instead of picking just one?
- How does a router avoid reordering packets within a single flow while still load-balancing across paths (hashing on the 5-tuple: src/dst IP, src/dst port, protocol)?
- Why can ECMP hashing cause uneven load if a small number of flows dominate traffic (elephant flows)?

## Interview / SRE Usage

- How would you debug "this host can't reach that host" systematically, layer by layer (link → ARP → routing → firewall → application)?
- Why might `traceroute` show `* * *` for a hop that's actually fine (that router simply doesn't respond to probes, or a firewall drops them)?
- How would you explain the trade-off of NAT for a Kubernetes cluster's pod networking vs a flat, routable pod network (see `../kubernetes/networking.md`)?
