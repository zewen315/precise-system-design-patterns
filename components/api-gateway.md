# API Gateway

> Questions to know before using an API gateway as a building block in a system design interview.

## Core Concepts

- What problem does an API gateway solve for a microservices architecture (single entry point, cross-cutting concerns)?
- What responsibilities typically live at the gateway (authentication, rate limiting, routing, request/response transformation, TLS termination, logging)?
- What's the difference between an API gateway and a load balancer?
- What's the difference between an API gateway and a service mesh (edge/north-south traffic vs service-to-service/east-west traffic)?

## Patterns

- What is the backend-for-frontend (BFF) pattern, and why might you run multiple gateways for different clients (web/mobile)?
- How does the gateway centralize authentication/authorization (JWT validation, session lookup), and what does that simplify for backend services?
- What is path-based routing, and how does the gateway know which service owns which endpoint?
- How does the gateway implement per-client rate limiting, and where does that state live (local vs shared/Redis)?

## Trade-offs & Failure Modes

- What happens if the API gateway goes down, and how do you avoid it being a single point of failure?
- What's the risk of putting too much business logic in the gateway (it becomes a monolith / bottleneck)?
- How do you handle API versioning and backwards compatibility at the gateway?

## Interview Usage

- How do you justify adding an API gateway in a design, and what would you cut if asked to simplify?
- How does the gateway fit relative to the load balancer and CDN in the request path?
