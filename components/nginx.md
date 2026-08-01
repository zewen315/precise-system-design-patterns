# Nginx (Reverse Proxy / Web Server / Load Balancer)

> Questions to know before naming Nginx specifically as a building block in a system design interview.

## Core Concepts

- What is Nginx, and what three roles does it commonly play in a design (web server serving static files, reverse proxy, load balancer)?
- Why is Nginx able to handle tens of thousands of concurrent connections with just a handful of worker processes (an event-driven, non-blocking architecture built on `epoll` — see `linux/network.md`)?
- What's the difference between Nginx's architecture and a traditional thread/process-per-connection server (e.g. classic Apache `prefork`), and why does that matter under high concurrency (memory per connection, context-switch overhead)?

## Architecture

- What is the master process vs worker process model, and what does each do (master reads config, binds ports, manages workers; workers actually handle connections)?
- Why does Nginx typically run one worker process per CPU core, and how does the kernel distribute incoming connections across them?
- What's the difference between the event loop model here and how Node.js/Redis are also single-threaded-per-worker and event-driven?
- How does Nginx reload configuration without dropping connections (`nginx -s reload` spawns new workers with the new config, and gracefully lets old workers finish in-flight requests before exiting)?

## As a Reverse Proxy / Load Balancer

- What does `proxy_pass` do, and what headers should you set when proxying (`X-Forwarded-For`, `X-Forwarded-Proto`, `Host`) so the backend knows the original client/request?
- What load balancing algorithms does Nginx support (round robin, least connections, IP hash), and how does IP hash provide basic session affinity?
- How does Nginx health-check upstreams, and what's the difference between passive checks (mark a backend down after failed requests) available in open source vs active health checking (available in Nginx Plus, or via third-party modules)?
- How would you configure Nginx to terminate TLS and forward plaintext to backends (see `networking/http-tls.md`)?

## As a Cache

- How does Nginx's `proxy_cache` work, and what problem does caching at this layer solve versus caching in the application or in Redis?
- What is cache key configuration, and why does getting it wrong (e.g. ignoring query params that should vary the response) cause serving the wrong cached content to the wrong user?
- What is `proxy_cache_lock`, and how does it prevent a cache stampede on a popular cache miss (see `components/redis.md` for the same problem elsewhere)?

## Rate Limiting & Traffic Shaping

- How does Nginx's `limit_req` (leaky bucket) module implement rate limiting, and what does the `burst` parameter control?
- How does `limit_conn` differ from `limit_req` (limiting concurrent connections vs limiting request rate)?
- Why is rate limiting at the Nginx layer per-worker by default, and what does that mean for accuracy across multiple workers/instances (see `components/rate-limiter.md` for the general distributed version of this problem)?

## Configuration Model

- What's the difference between a `server` block and a `location` block, and how does Nginx pick which `location` matches a given request (longest-prefix / most-specific match, with special handling for regex vs prefix matches)?
- What is an upstream block, and how does it define a pool of backends to load balance across?
- Why is Nginx configuration declarative rather than a general-purpose scripting language, and what's the trade-off (predictable performance vs limited dynamic logic, hence modules like Lua/OpenResty for more control)?

## Interview Usage

- When would you put Nginx in a design as a reverse proxy vs reaching for a full API gateway (see `components/api-gateway.md`) — what's the line?
- Nginx vs HAProxy vs Envoy vs a cloud-managed load balancer (ALB) — how would you justify picking one over another?
- How would you explain serving static assets directly from Nginx instead of routing them through the application server, and what's the performance argument for doing so?
- What would you check first if Nginx is returning `502 Bad Gateway` (backend down, backend crashed mid-response, upstream timeout, or a Unix socket/permissions issue if proxying to a local app)?
