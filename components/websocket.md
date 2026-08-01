# WebSocket (Real-Time Communication)

> Questions to know before using WebSocket (or another real-time transport) as a building block in a system design interview.

## Core Concepts

- What problem does WebSocket solve that plain HTTP request/response can't (full-duplex, low-latency server push)?
- What's the difference between polling, long polling, Server-Sent Events (SSE), and WebSocket, and when would you choose each?
- How does a WebSocket connection get established (HTTP upgrade handshake), and why does that matter for load balancers/proxies?

## Architecture

- Why are WebSocket connections stateful and long-lived, and how does that change how you scale the servers holding them compared to stateless HTTP?
- How do you route a message to a specific user who could be connected to any one of many WebSocket servers (connection registry, pub/sub fan-out via Redis/Kafka)?
- What is a heartbeat/ping-pong, and why is it needed to detect dead connections?

## Failure Modes & Scaling

- What happens when a WebSocket server restarts or crashes — how do clients reconnect, and how do you avoid losing in-flight messages?
- How do you handle a client that's offline when a message is sent (message queue/inbox, push notification fallback)?
- How do load balancers need to be configured differently for WebSocket traffic (sticky sessions, longer idle timeouts)?
- How many concurrent connections can a single server realistically hold, and what limits it (file descriptors, memory per connection)?

## Interview Usage

- How would you design the real-time delivery layer for a chat or live-notification system?
- How would you decide between WebSocket, SSE, and long polling for a given feature (chat vs live dashboard vs live comments)?
