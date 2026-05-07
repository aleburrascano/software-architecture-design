---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/bytebytegohq-system-design-101.md'
source_date: ''
source_author: ByteByteGo
tags:
  - source
  - system-design
  - visual-learning
  - scalability
  - distributed-systems
  - interview
  - communication-protocols
  - caching
---
# System Design 101 - ByteByteGo

ByteByteGo's visual explanation repository making complex system design concepts accessible through diagrams and simple terminology.

## Summary

System Design 101 uses visual diagrams as the primary explanation medium, making distributed systems concepts accessible to practitioners who learn better from visual representations than dense prose. The repository covers communication protocols with direct comparisons — REST vs. GraphQL vs. gRPC vs. WebSocket vs. Webhook — explaining when each is appropriate and what trade-offs each makes. The visual format makes protocol differences concrete in a way that text descriptions often fail to achieve.

Database coverage includes the SQL vs. NoSQL decision framework, practical CAP theorem implications, indexing strategies, replication patterns, and sharding approaches. The caching section addresses multiple caching layers (client-side, CDN, application, database query) with cache invalidation strategies and the consistency trade-offs each creates. Redis is covered as the canonical application cache.

The microservices section covers the patterns that make microservices operationally viable: API Gateway (single entry point, cross-cutting concerns), service discovery (client-side vs. server-side), circuit breaker (preventing cascade failures), and distributed tracing. The CI/CD section covers pipeline patterns from source to production.

## Key Arguments

- Visual explanations make distributed systems concepts accessible and comparable across options
- System design requires understanding trade-offs at every layer; no single choice is universally correct
- Communication protocol choice (REST, gRPC, WebSocket) drives coupling, performance, and operational complexity
- Caching strategy determines the scalability ceiling; cache invalidation is the hardest related problem
- Database choice should be driven by access patterns, not familiarity or trend
- Microservices patterns (API Gateway, service discovery, circuit breaker) are not optional at scale

## Concepts Covered

- [[CAP Theorem]] — visual explanation with practical system design implications
- [[REST]] — protocol overview with comparison to alternatives
- [[gRPC]] — comparison with REST; when to prefer binary over text protocols
- [[API Gateway]] — microservices entry point and cross-cutting concerns
- [[Circuit Breaker Pattern]] — resilience pattern in microservices context
- [[System Design Interview Guide]] — primary source for interview preparation
- [[Continuous Integration and Delivery]] — pipeline patterns from source to production

## Quality Notes

100k+ GitHub stars. Most accessible visual system design reference available. Excellent for interview preparation and for building intuition about distributed systems trade-offs before reading more technical treatments.
