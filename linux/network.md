# Network (Kernel Networking & Sockets)

> OS-level networking concepts. See `tools/network.md` for the CLI tools and `../networking/` for network-engineering-focused material.

## Sockets

- What is a socket — the kernel's abstraction for one endpoint of a network connection, exposed as just another file descriptor?
- What's the difference between a socket, a port, and a connection (a connection is uniquely identified by the 4-tuple: source IP, source port, destination IP, destination port, plus protocol)?
- Why can a server listen on a single port but handle many simultaneous clients (each accepted connection gets its own socket/fd; the listening socket just hands out new ones)?

## Connection Lifecycle

- `socket()` → `bind()` → `listen()` → `accept()` on the server; `socket()` → `connect()` on the client.
- What does `bind()` actually do (associate a socket with a local address/port), and why is it optional for clients (the kernel picks an ephemeral port automatically)?
- What is the TCP three-way handshake (`SYN` → `SYN-ACK` → `ACK`), and where does `accept()` return relative to it (after the handshake completes, pulling an already-established connection off the kernel's backlog)?
- What is the backlog queue, and what happens to new connection attempts when it's full?
- What are the TCP states (`LISTEN`, `SYN_SENT`, `ESTABLISHED`, `FIN_WAIT`, `TIME_WAIT`, `CLOSE_WAIT`, ...), and why does `TIME_WAIT` matter operationally (the socket can't be immediately reused, which matters for servers with high connection churn)?
- `ss -tan` — see live connections and their states (see `tools/network.md`).

## Blocking vs Non-Blocking I/O

- What does a blocking `read()`/`write()` do when no data is available (the calling thread sleeps until the kernel wakes it)?
- What's the cost of the naive "one thread per connection" model at high connection counts (per-thread stack memory, context-switch overhead)?
- What does non-blocking mode do instead (`O_NONBLOCK` — the call returns immediately with `EWOULDBLOCK` if it can't proceed)?

## select / poll / epoll

- What problem does `select`/`poll` solve — letting one thread monitor many file descriptors and act only on the ones that are ready?
- Why doesn't `select`/`poll` scale well to thousands of connections (an O(n) scan of every fd on every call, and the full fd list must be re-passed each time)?
- How does `epoll` fix this — the kernel maintains an interest list you build once (`epoll_ctl`), and `epoll_wait` returns only the ready subset, instead of re-describing everything every call?
- What's the difference between level-triggered and edge-triggered epoll, and why is edge-triggered easy to get wrong (you must drain the fd completely, or you'll never be notified again)?
- How does this connect to how high-concurrency servers (Nginx workers, Node.js's libuv, Go's netpoller) handle tens of thousands of connections with only a handful of threads?

## Socket Buffers

- What are the send and receive buffers, and why does `write()` returning success not mean the data has been received — only that it's been copied into the kernel's send buffer?
- What is backpressure at the socket level — what happens when a fast sender fills the receiver's buffer (the receive window shrinks; the sender's `write()` eventually blocks or returns `EWOULDBLOCK`)?
- Why does a bigger buffer trade memory for throughput on high-latency links (the bandwidth-delay product)?

## Linux Network Stack & NIC

- What's the high-level path of an incoming packet (NIC → DMA into a ring buffer → interrupt → kernel network stack (driver → IP → TCP) → socket receive buffer → userspace `read()`)?
- Why does a high packet rate generating one interrupt per packet become a bottleneck (an interrupt storm)?
- What is NAPI (New API) / interrupt coalescing, and how does switching to polling under load instead of interrupting on every packet reduce overhead?
- What is DMA (direct memory access), and why does it let the NIC write packet data straight into RAM without the CPU copying it byte-by-byte?
- What is RSS (receive-side scaling), and how does spreading packet processing across multiple CPU cores/queues help on multi-core machines?

## Interview / Practical Usage

- How would you explain why Nginx can handle tens of thousands of concurrent connections with only a handful of worker processes (an epoll-based event loop, not thread-per-connection)?
- A server is running out of file descriptors under load — what's happening and how do you fix it (`ulimit -n`, see `tools/process.md`)?
- Lots of connections stuck in `TIME_WAIT` — what does that tell you, and what's the trade-off of tuning it away (`SO_REUSEADDR`, a shorter `TIME_WAIT` risks confusing stray packets from a prior connection)?
