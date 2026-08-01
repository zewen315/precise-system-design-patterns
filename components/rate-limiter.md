# Rate Limiter

> Questions to know before using a rate limiter as a building block in a system design interview.

## Core Concepts

- What problem does rate limiting solve (protect against abuse, ensure fair usage, prevent overload)?
- What's the difference between the token bucket, leaky bucket, fixed window, and sliding window algorithms, and what are the trade-offs of each (burstiness, accuracy, memory)?
- Why does a fixed window counter allow up to 2x the intended rate at window boundaries, and how does sliding window log/counter fix that?

## Placement & Design

- Where do you enforce rate limiting (client, API gateway, per-service, per-endpoint), and what are the trade-offs?
- What key do you rate limit on (user ID, IP, API key), and what happens behind a shared IP/NAT?
- What HTTP response and headers should a rate-limited request return (429, `Retry-After`, remaining quota)?
- How do you rate limit at multiple tiers simultaneously (per-user, per-IP, global)?

## Distributed Rate Limiting

- How do you implement a distributed rate limiter shared across multiple servers (centralized store like Redis)?
- How do you make a Redis-based rate limiter atomic (Lua scripts vs a racy `INCR` + `EXPIRE`)?
- How do you keep the rate limiter itself from becoming the bottleneck under extreme load (local caching, approximate counting)?

## Interview Usage

- How would you design a rate limiter for a multi-region system where state needs to be roughly consistent globally?
- How would you justify the algorithm choice for a given API's traffic pattern (bursty vs steady)?
