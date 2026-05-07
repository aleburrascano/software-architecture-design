---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/donnemartin-system-design-primer.md'
source_date: ''
source_author: Donne Martin
tags:
  - source
  - system-design
  - scalability
  - caching
  - load-balancing
  - databases
  - microservices
  - interview
  - availability
  - reliability
---
# System Design Primer - donnemartin

One of the most popular GitHub repositories for system design knowledge, covering scalability fundamentals through distributed system patterns.

## Summary

The System Design Primer covers the breadth of distributed systems knowledge needed for system design interviews and practical architecture work. The structure moves from foundational trade-offs (performance vs. scalability, latency vs. throughput, availability vs. consistency) through infrastructure components (DNS, CDN, load balancers, reverse proxies) to application architecture concerns (microservices, databases, caching, asynchronism) and communication patterns.

The database section provides actionable guidance on SQL vs. NoSQL selection, explaining that the choice should be driven by access patterns, consistency requirements, and scale needs rather than trend-following. The CAP theorem section is one of the clearest practical explanations available, translating the formal theorem into concrete system design implications. The caching section covers client-side, CDN, server-side, database query, and object caching strategies with their respective trade-offs.

The asynchronism section explains how message queues and task processing decouple producers from consumers, improve throughput, and provide back-pressure mechanisms. The communication section covers synchronous (HTTP, REST, RPC) versus asynchronous (message queue) patterns and when each is appropriate. Throughout, the emphasis is on trade-off reasoning rather than prescriptive answers.

## Key Arguments

- System design requires explicit reasoning about competing concerns: availability vs. consistency, performance vs. scalability
- Caching is consistently the highest-impact scalability intervention across system types
- Asynchronism via message queues improves throughput and decouples producers from consumers
- Horizontal scaling requires stateless service design; state must live in shared external stores
- NoSQL databases trade consistency guarantees for scale and flexibility; choice should be access-pattern driven
- CDNs solve geographic latency; load balancers solve traffic distribution and enable horizontal scaling

## Concepts Covered

- [[CAP Theorem]] — practical guide with concrete system design implications
- [[Eventual Consistency]] — NoSQL trade-off and practical implications
- [[Microservices Architecture]] — system design context and decomposition patterns
- [[Apache Kafka]] — async messaging and task queue patterns
- [[API Gateway]] — reverse proxy and entry point patterns
- [[Publish-Subscribe Pattern]] — asynchronism patterns and message queue design
- [[System Design Interview Guide]] — primary source

## Quality Notes

Most-starred system design resource on GitHub. Comprehensive, well-organized, and regularly updated. Essential reference for both interview preparation and practical architecture decision-making.
