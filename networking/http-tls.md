# HTTP & TLS

## HTTP Fundamentals

- What is idempotency, and which HTTP methods are supposed to be idempotent (`GET`, `PUT`, `DELETE`) vs not (`POST`) — why does that matter for safe retries?
- What's the difference between `PUT` and `PATCH`?
- What are the main status code classes (2xx success, 3xx redirect, 4xx client error, 5xx server error), and why does that classification matter for monitoring/alerting (alert on 5xx rate, not on 4xx)?
- What's the difference between `401 Unauthorized` and `403 Forbidden`?

## HTTP/1.1 → HTTP/2 → HTTP/3

- What is head-of-line blocking, and how does it show up at HTTP/1.1 (one request in flight per connection at a time, hence multiple parallel connections as a workaround) and still at HTTP/2 (one lost TCP packet blocks every multiplexed stream, since they share one TCP connection)?
- What does HTTP/2 add over HTTP/1.1 (binary framing, multiplexing multiple streams over one connection, header compression via HPACK, server push)?
- How does HTTP/3 (built on QUIC — see `transport.md`) fix TCP-level head-of-line blocking (independent streams at the transport layer, so a lost packet only blocks its own stream)?
- What's the operational cost of HTTP/2's multiplexing — why can one slow or misbehaving stream on a shared connection still affect others sharing it?

## Connections

- What is keep-alive, and why does it matter for latency (amortizing the TCP handshake + TLS handshake across many requests)?
- What is connection pooling, and why do backend clients maintain a pool instead of opening a new connection per request?
- What's the operational risk of a load balancer/service holding idle keep-alive connections open (resource usage, and stale connections against a backend that already closed its end)?

## TLS

- What problem does TLS solve (confidentiality, integrity, and authentication of the connection)?
- What is the TLS handshake, at a high level (negotiate protocol version/cipher, authenticate the server via its certificate, establish shared symmetric keys)?
- Why does TLS 1.3 reduce the handshake to one round trip (vs two for TLS 1.2)? What is 0-RTT session resumption, and what's its security trade-off (replay attacks on the first, unauthenticated flight of data)?
- What's the difference between asymmetric and symmetric encryption, and why does TLS use asymmetric crypto only to establish the connection, then switch to symmetric for the actual data (asymmetric is far more computationally expensive)?

## Certificates & PKI

- What is a certificate, and what does a Certificate Authority (CA) actually vouch for (that the public key belongs to the named domain)?
- What is the chain of trust (leaf cert → intermediate → root), and why do browsers/OSes ship with a fixed set of trusted root CAs?
- What is certificate pinning, and why has it fallen out of favor for public-facing services (the operational risk of a mismanaged pin locking out legitimate clients)?
- What is mTLS (mutual TLS), and why is it common inside a service mesh (both client and server authenticate each other, not just the server)?
- Why is a forgotten certificate expiry one of the most common self-inflicted production outages?

## SNI & TLS Termination

- What is SNI (Server Name Indication), and what problem does it solve (letting one IP serve TLS for multiple hostnames/certificates, since the server needs to know which cert to present before the handshake reveals the HTTP `Host` header)?
- What is TLS termination, and why do load balancers typically terminate TLS rather than pass encrypted traffic straight through (offloading CPU-heavy crypto, enabling L7 routing which requires decrypted content)?
- What's the difference between TLS termination and TLS passthrough, and when would you need passthrough (the backend needs the client's actual TLS session, e.g. for mTLS all the way to the backend)?

## Interview / SRE Usage

- A cert expired and caused an outage — how would you prevent recurrence (automated renewal, expiry monitoring/alerting well ahead of time)?
- How would you debug "TLS handshake failures spiking" (cert chain issue, a cipher/protocol mismatch after a client or server upgrade, SNI misconfiguration)?
- Why would you terminate TLS at the edge/CDN but re-encrypt (or use mTLS) to the origin, rather than running plaintext internally?
