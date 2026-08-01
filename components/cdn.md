# CDN (Content Delivery Network)

> Questions to know before using a CDN as a building block in a system design interview.

## Core Concepts

- What problem does a CDN solve (latency via geographic proximity, offloading origin traffic, absorbing spikes)?
- What's the difference between a push CDN and a pull CDN, and when would you use each?
- How does an edge node decide whether to serve from cache or forward to origin (cache hit/miss)?
- How does a CDN route a user to the nearest edge node (anycast, GeoDNS)?

## Caching Strategy

- What is a cache key, and how do query parameters/headers affect what gets cached?
- What's the trade-off between a long TTL (staleness) and a short TTL (more origin load)?
- How do you invalidate/purge a CDN cache when content changes, and why is that expensive/slow at global scale?
- What is origin shielding, and what problem does it solve (protecting the origin from a stampede across many edge nodes)?
- What's the difference between caching static assets and caching dynamic/API content — can a CDN cache API responses?

## Access Control

- What is a signed URL / signed cookie, and how does a CDN handle access-controlled content?

## Failure Modes & Interview Usage

- What happens on a cache miss storm (e.g., viral content), and how does the CDN protect the origin?
- When does a CDN not help (highly personalized, real-time, or write-heavy traffic)?
- How would you use a CDN to serve user-generated content (images/video), and what are the cache-invalidation implications?
- How would you explain the request path difference between "CDN cache hit" and "CDN cache miss → origin → cache fill" in an interview?
