# Transport Protocols (TCP & UDP)

> Protocol-level mechanics. See `../linux/network.md` for the kernel/socket-API side (epoll, buffers, blocking I/O).

## TCP vs UDP

- What does TCP guarantee that UDP doesn't (reliable, ordered, connection-oriented delivery with flow/congestion control), and what does that cost (handshake latency, head-of-line blocking)?
- Why would you deliberately choose UDP despite no reliability guarantee (lower latency, no head-of-line blocking, room for the application's own partial reliability — DNS, video/voice, QUIC)?
- What is a TCP/UDP port, and why are ports 0–1023 "well-known"/privileged?

## TCP Handshake & Teardown

- Three-way handshake: `SYN` → `SYN-ACK` → `ACK` — what does each side agree on (initial sequence numbers, window size, MSS)?
- Teardown: `FIN`/`ACK` in each direction — why can one side keep sending after the other has sent `FIN` (a half-closed connection)?
- What is a SYN flood, and why does it exploit the asymmetry of the handshake (the server allocates state after just a `SYN`, before the connection is confirmed)? What does the SYN cookies defense do about it?

## Sequence Numbers & Reliability

- How does TCP detect and retransmit lost data (sequence numbers + ACKs + a retransmission timeout)?
- What is a duplicate ACK, and how does fast retransmit use it to recover faster than waiting for a timeout?
- What is TCP's sliding window, and how does it allow multiple unacknowledged segments in flight instead of stop-and-wait?

## Flow Control vs Congestion Control

- What's the difference between flow control (protecting the *receiver* from being overwhelmed — the advertised window) and congestion control (protecting the *network* from being overwhelmed — the congestion window)?
- What is slow start, and why does TCP ramp up its sending rate exponentially at first instead of jumping straight to full speed?
- What is congestion avoidance, and how do algorithms differ (Reno's additive-increase/multiplicative-decrease; Cubic, Linux's long-time default; BBR, which is model-based instead of purely loss-based)?
- Why does BBR behave very differently from loss-based algorithms on links that are lossy but not actually congested (e.g. wifi, satellite) — it doesn't treat packet loss alone as a signal to back off?

## Nagle's Algorithm & Delayed ACK

- What does Nagle's algorithm do (batch small writes before sending, to avoid tiny packets dominated by header overhead), and why does it interact badly with delayed ACK (each side waits on the other, producing stalls of up to ~200ms)?
- Why do latency-sensitive applications set `TCP_NODELAY` to disable Nagle's algorithm?

## MSS & MTU

- What is MSS (maximum segment size), and how does it relate to MTU (MSS ≈ MTU minus IP/TCP header overhead)?
- What is MSS clamping, and why is it used at a network boundary (e.g. a VPN or tunnel) where the effective MTU is smaller than usual?

## QUIC / HTTP-3 (brief)

- What problem does QUIC solve that TCP+TLS can't (head-of-line blocking across independent streams, and folding the TCP and TLS handshakes into fewer round trips)?
- Why does QUIC run over UDP instead of being a brand-new transport protocol (deployability — middleboxes and OSes already know how to pass UDP)?
- What is connection migration in QUIC, and why does it help mobile clients switching networks (wifi → cellular) without dropping the connection?

## Interview / SRE Usage

- A connection is randomly slow, and a packet capture shows retransmissions — what does that tell you about where the problem lives (loss on the path, not application logic)?
- Why can a single very slow/lossy TCP connection throttle an entire application's throughput even when bandwidth is available (a collapsing congestion window, single-stream limits)?
- How would you decide between one long-lived TCP connection and connection pooling/multiplexing for a service-to-service call pattern?
