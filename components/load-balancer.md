# Load Balancer

> Questions to know before using a load balancer as a building block in a system design interview.

## Core Concepts

- What problem does a load balancer solve (distribute traffic, hide backend topology, enable horizontal scaling)?
- What's the difference between a Layer 4 (transport) and a Layer 7 (application) load balancer, and what can L7 do that L4 can't (routing by path/header, SSL termination)?
- What is a health check, and how does the load balancer decide to add/remove a backend?

## Algorithms

- What load balancing algorithms exist (round robin, least connections, weighted, IP hash, consistent hashing), and when would you choose each?
- What's the difference between client-side load balancing and a centralized proxy?
- What is DNS-based load balancing, and why is it coarse-grained (TTL caching, no health awareness)?

## Operational Concerns

- What is SSL/TLS termination, and why do it at the load balancer instead of every backend?
- What is sticky session (session affinity), and why does it conflict with even load distribution and horizontal scaling?
- What is connection draining, and why is it needed during deploys/scale-down?
- What is slow start, and how does it avoid a thundering herd on a newly added backend?

## High Availability & Scaling

- How do you make the load balancer itself highly available (avoid a single point of failure — floating IP/VRRP, anycast, multiple LBs behind DNS)?
- What happens when the load balancer becomes the bottleneck, and how do you scale it?
- How would you load balance across multiple regions/data centers (global load balancing, GeoDNS, anycast)?

## Interview Usage

- What's the difference between a load balancer and a reverse proxy, and how do they overlap?
- How do you decide between L4 and L7 for a given design, and what's the latency/flexibility trade-off?
- How would you explain where the load balancer sits relative to the API gateway and CDN in a request path?
