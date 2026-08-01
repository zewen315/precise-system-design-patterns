# Network Troubleshooting (SRE Playbook)

> Practical, systematic debugging — ties together every layer above and the tools in `../linux/tools/network.md`.

## The Layered Debugging Mental Model

- Work through the stack methodically rather than guessing: link/L2 → IP/routing → transport (TCP/UDP) → TLS → application (HTTP/DNS).
- Why is "can I ping it" a weak first signal (ICMP is often blocked/deprioritized even when the actual service is healthy, and a successful ping doesn't confirm the port/service you actually care about works)?
- What's a better first step than ping for "is this service reachable" (`curl -v`, `nc -zv`, checking the real port and protocol)?

## Connectivity & Routing

- Host unreachable — checklist: local interface up (`ip addr`)? → default route present (`ip route`)? → can you reach the gateway? → `traceroute`/`mtr` to see where it stops.
- Reachable from some places but not others — what does that pattern suggest (asymmetric routing, a firewall/security group rule scoped to specific source ranges, a path-specific issue)?
- What is asymmetric routing, and why can it cause a stateful firewall to drop legitimate return traffic it never saw the outbound leg of?

## Latency & Packet Loss

- How do you distinguish "slow DNS" from "slow connect" from "slow TLS" from "slow application response" for a single slow request (`curl -w` timing breakdown, or a packet capture)?
- `mtr`/`ping` shows loss at an intermediate hop but not the destination — does that matter? Often no — many routers deprioritize ICMP addressed to themselves while forwarding fine; only sustained loss *at the destination* is conclusive.
- Consistent, low packet loss vs bursty loss — what different root causes do they suggest (link-layer error rate/congestion vs a flapping route or an overloaded device)?
- How would you tell a real network latency spike apart from an application pause (e.g. GC) that just *looks* like network from the outside?

## TCP-Level Debugging

- A `tcpdump` capture shows retransmissions — what does that tell you about where the problem likely is (loss on the path, not application logic)?
- Connections stuck in `SYN_SENT` — what does that mean (the handshake's `SYN` isn't getting a response: firewall drop, backend down, or a full backlog)?
- Lots of connections in `CLOSE_WAIT` on your service — what does that mean (your application accepted the peer's close but never closed its own end — usually an app-level leaked socket)?
- See `../linux/network.md` for the TCP state machine and `../linux/tools/network.md` for the `ss`/`tcpdump` command reference.

## DNS Debugging

- `dig +trace` — walk the resolution chain yourself to find exactly which step is wrong.
- Works with `dig @8.8.8.8` but not with the default resolver — what does that isolate (your local/configured resolver specifically, not the record itself)?
- Intermittent DNS failures — common causes (a flaky resolver somewhere in the chain, UDP packet loss, or a response exceeding UDP size and mishandling the fallback to TCP).

## TLS Debugging

- `openssl s_client -connect host:443 -servername host` — manually inspect the handshake and the presented certificate chain.
- Handshake succeeds but the client rejects the cert — checklist (expired cert, hostname mismatch, missing intermediate in the chain, client's trust store missing the root).
- A spike in TLS handshake failures right after a deploy — first thing to check (did the cert/chain change, did the minimum TLS version or cipher suite list change)?

## Load Balancer / Proxy Layer

- 502/503/504 from a load balancer — what does each imply (502: backend sent a malformed/no response; 503: LB or backend explicitly refusing, e.g. no healthy backends; 504: backend accepted the request but never responded in time)?
- Health checks pass but real traffic fails — why can that happen (the health check hits a different, simpler endpoint/path, or a different protocol/port, than real traffic does)?
- Why can enabling keep-alive between a load balancer and backends change failure behavior (a backend silently closing an idle "kept-alive" connection races with the LB trying to reuse it — the classic stale-connection 502)?

## Interview / SRE Usage

- Walk through how you'd debug "users report our API is slow, but our server-side latency metrics look normal" (points at network path, DNS, TLS, or the client — not app code).
- Walk through how you'd debug "some percentage of requests fail intermittently, with no pattern in the logs" (looks like packet loss/infra flakiness rather than app logic — where do you look next).
- What's your first move when paged for "service unreachable," before you've formed any hypothesis?
