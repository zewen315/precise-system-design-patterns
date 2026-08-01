# Routing Protocols & Internet Routing (Network Engineering)

> Beyond most SRE scope day-to-day, but essential for understanding how the internet actually routes traffic, debugging multi-region/multi-cloud connectivity, and network engineer interviews.

## IGP vs EGP

- What's the difference between an Interior Gateway Protocol (routing *within* one organization's network, e.g. OSPF) and an Exterior Gateway Protocol (routing *between* independent networks, e.g. BGP)?
- What is an Autonomous System (AS), and why does the internet's routing model assume each organization operates one or more, each identified by a globally unique AS number?

## OSPF (Interior)

- What type of algorithm does OSPF use (link-state — every router builds a full map of the topology and computes shortest paths itself via Dijkstra)?
- How does link-state differ from distance-vector (e.g. RIP) — every router sees the *whole* topology instead of just "which neighbor is closest for each destination"?
- What is an OSPF area, and why do large networks split into areas (limiting how far a topology change has to propagate and be recomputed)?
- Why is OSPF generally not used across organizational boundaries (it assumes a single trust domain/administrative authority)?

## BGP (Exterior)

- What type of algorithm does BGP use (path-vector — each router advertises the full AS path it would take, not a link-state map), and why is that necessary at internet scale (the full topology is far too large to hold as link-state everywhere, and organizations don't want to expose or trust each other's internals)?
- What does a BGP route advertisement actually contain (a prefix, the AS path to reach it, and other attributes)?
- What's the difference between eBGP (between different autonomous systems) and iBGP (within the same AS, used to distribute externally-learned routes internally)?
- How does BGP pick the best route when multiple paths exist (a defined attribute precedence — local preference, AS path length, origin type, MED, and more; "shortest AS path" is a common simplification, not the whole story)?
- How is a BGP peering session established (a TCP connection on port 179 between routers)?
- What's the difference between peering (two networks exchange only routes to each other's own traffic, typically settlement-free) and transit (paying a provider to carry your traffic to the entire rest of the internet)?
- What is route aggregation/summarization, and why does it keep the global routing table from growing unmanageably large (advertising one covering prefix instead of many small ones)?

## BGP Failure Modes

- What is a BGP route leak, and how does it happen (a network accidentally advertises routes it shouldn't — e.g. leaking a full routing table to a peer — becoming an unintended transit path)?
- What is a BGP hijack, and why is it possible without extra safeguards (any AS can announce a prefix it doesn't own; BGP by default trusts advertisements)?
- What is RPKI (Resource Public Key Infrastructure), and how does it let networks cryptographically validate that an AS is authorized to originate a given prefix?
- What is route flapping, and why can it destabilize the wider internet (each flap propagates as an update to every router that heard the route)? What does route flap damping do about it?
- Recall a real-world BGP incident (a major provider's route leak or hijack causing a large-scale outage) — what made it possible, and what would have prevented it?

## Anycast

- What is anycast, and how does BGP make it work (the same IP prefix is announced from many physical locations; each router's normal best-path selection sends traffic to the topologically/AS-path-nearest one)?
- Why is anycast common for DNS root servers and CDN/DDoS-mitigation edge nodes (resilience — an outage at one site doesn't take down the shared IP, traffic just reroutes to the next nearest site)?
- What's a limitation of anycast for stateful, long-lived connections (a BGP route change mid-connection can silently redirect the same IP to a different physical server, breaking the connection)?

## SDN & Modern Data Center Networking (brief)

- What is Software-Defined Networking (SDN), and what does it change (separating the control plane from the data plane, so a central controller programs many switches' forwarding behavior)?
- What is a spine-leaf topology, and why do modern data centers use it over a traditional three-tier design (predictable latency — every leaf is exactly two hops from every other leaf — and easy horizontal scaling)?
- What is VXLAN, and what problem does it solve (extending a single L2 broadcast domain over an L3 IP network — e.g. across racks or data centers — by encapsulating Ethernet frames in UDP)?

## Interview Usage

- Where's the line for an SRE — understanding BGP's failure modes and internet-routing-dependent outages, vs actually configuring a router?
- How would you explain "why did our multi-region failover not actually reroute traffic" if the root cause turned out to be a stale/held BGP route?
- If asked "how does anycast work" in an interview, what's a concise, accurate 60-second answer?
