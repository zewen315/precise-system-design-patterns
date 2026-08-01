# DNS

## Core Concepts

- What problem does DNS solve — a hierarchical, distributed, cacheable mapping from names to addresses (and other resource records)?
- What is the resolution chain (stub resolver → recursive resolver → root → TLD → authoritative), and what does each step actually answer?
- What's the difference between a recursive resolver and an authoritative nameserver?

## Record Types

- `A` / `AAAA` — IPv4/IPv6 address.
- `CNAME` — alias to another name; why can't a CNAME coexist with other records at the same name?
- `MX` — mail routing.
- `TXT` — arbitrary text (SPF/DKIM, domain ownership verification).
- `NS` — delegation to another nameserver.
- `SOA` — zone metadata (serial number, refresh/retry/expire timers).
- `SRV` — service location (host + port), used for service discovery.

## Caching & TTL

- What's the trade-off between a long TTL (fewer lookups, but slow to propagate changes) and a short TTL (fast failover, but more load on authoritative servers and more lookup latency)?
- Why can a DNS change appear "not to have taken effect" long after you changed the record (stale caches at resolvers along the path, some ignoring your TTL, or simply not yet expired)?
- What is negative caching (caching an NXDOMAIN), and why does it also need a TTL (via the SOA record)?

## DNS-Based Traffic Management

- How does DNS-based load balancing work (returning different/rotated `A` records), and why is it coarse-grained compared to an L4/L7 load balancer (no health awareness, and client/resolver caching ignores real-time backend health)?
- What is GeoDNS, and how does it route users to a nearby region based on the resolver's location?
- Why is DNS failover slow compared to load-balancer-level failover (bounded by TTL, and by resolvers that don't respect TTL faithfully)?
- What is anycast DNS, and how does routing — not DNS itself — get a query to the nearest of many identical servers sharing one IP (see `routing-protocols.md`)?

## Security

- What is DNS spoofing/cache poisoning, and what does DNSSEC add to prevent it (cryptographic signatures on records)?
- What is DNS over HTTPS/TLS (DoH/DoT), and what privacy problem does it address (plain DNS queries are visible to anyone on the path)?
- What is a DNS amplification attack, and why does DNS's UDP-based, small-request/large-response nature make it an effective DDoS vector?

## Interview / SRE Usage

- How would you debug "this service is unreachable" when you suspect DNS (`dig`, `dig +trace`, checking TTL, querying multiple resolvers directly)?
- Why would you lower TTLs *before* a planned migration/cutover, and raise them again after?
- How would you design DNS for a multi-region active-active service (GeoDNS + health checks, vs anycast, vs a global load balancer in front)?
- What's the blast radius if your DNS provider has an outage, and how do you mitigate it (a secondary DNS provider, multiple NS delegations)?
