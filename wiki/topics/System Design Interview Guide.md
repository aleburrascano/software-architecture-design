---
type: topic
created: '2026-05-06'
updated: '2026-05-06'
tags:
  - system-design
  - interview
  - scalability
  - architecture
  - learning-path
  - distributed-systems
---
# System Design Interview Guide

A synthesis of system design as a discipline — covering the fundamental trade-offs, common patterns, and interview preparation approach. System design interviews test your ability to design large-scale distributed systems under constraints.

## What Is System Design?

System design focuses on **architecting large-scale, distributed systems** for real-world constraints: millions of users, terabytes of data, five nines availability. Unlike software architecture (focused on code structure), system design covers the full stack: databases, caching, load balancers, message queues, CDNs, and service decomposition.

## Core Trade-offs

Every system design decision involves balancing competing concerns:

| Trade-off | Description | Key Concept |
|---|---|---|
| **Consistency vs. Availability** | Strong consistency costs availability during partitions | [[CAP Theorem]] |
| **Latency vs. Consistency** | Fast reads may be stale; consistent reads require coordination | [[PACELC Theorem]] |
| **Read vs. Write optimization** | Optimize for reads = denormalize; optimize for writes = normalize | [[CQRS]] |
| **Vertical vs. Horizontal scaling** | Scale up (bigger machine) vs. scale out (more machines) | [[Quality Attributes]] |
| **SQL vs. NoSQL** | ACID and joins vs. scale and flexibility | [[CAP Theorem]] |
| **Sync vs. Async** | Immediate response vs. queue and process later | [[Event-Driven Architecture]] |

## System Design Building Blocks

### Scalability Layer

| Component | Purpose | Key Concepts |
|---|---|---|
| **Load Balancer** | Distribute traffic; enable horizontal scale | Round-robin, least connections, health checks |
| **CDN** | Cache static assets geographically | Edge caching, origin pull, cache invalidation |
| **Horizontal Scaling** | Add nodes rather than bigger nodes | Stateless services, sticky sessions |
| **Database Sharding** | Partition data across multiple DB nodes | Hash vs. range sharding, resharding |

### Data Layer

| Component | Purpose | Considerations |
|---|---|---|
| **Relational DB** | ACID transactions, complex queries | PostgreSQL, MySQL — CP systems |
| **NoSQL** | Scale and flexibility | Cassandra (AP), DynamoDB (AP/tunable) |
| **Cache** | Reduce DB load; low-latency reads | Redis, Memcached; cache aside, write-through |
| **Search** | Full-text, relevance ranking | Elasticsearch, OpenSearch |
| **Time Series DB** | Metrics, IoT, monitoring data | InfluxDB, TimescaleDB |

### Messaging & Event Layer

| Pattern | Purpose | Tool |
|---|---|---|
| **Message Queue** | Async task processing; buffer spikes | [[Apache Kafka]], RabbitMQ, SQS |
| **Pub/Sub** | Fan-out to multiple consumers | [[Publish-Subscribe Pattern]], Kafka topics |
| **Event Streaming** | Durable, replayable event log | [[Apache Kafka]], Kinesis |

### Reliability Patterns

| Pattern | Problem Solved | Wiki Concept |
|---|---|---|
| **Circuit Breaker** | Prevent cascade failures | [[Circuit Breaker Pattern]] |
| **Retry with backoff** | Transient failures | [[Resiliency Patterns]] |
| **Bulkhead** | Isolate failures to one component | [[Bulkhead Pattern]] |
| **Rate Limiting** | Protect from overload | API Gateway feature |
| **Idempotency** | Safe retries | [[Idempotency]] |

## System Design Interview Framework

A standard approach to system design interviews:

### 1. Clarify Requirements (5 min)
- **Functional:** What does the system do? (URL shortener, Twitter feed, payment system)
- **Non-functional:** Scale (users, requests/sec), availability SLA, consistency requirements, latency targets
- **Constraints:** Read-heavy or write-heavy? Global or regional?

### 2. Back-of-Envelope Estimates (5 min)
- Daily active users × requests/user = total QPS
- Storage needs per entity × entity count = total storage
- Derive read:write ratio

### 3. High-Level Design (10 min)
- Draw major components: clients, load balancer, application servers, databases, caches, message queues
- Identify data flows for key use cases

### 4. Deep Dive (15 min)
- Database schema design
- API design (key endpoints)
- Address bottlenecks (caching strategy, sharding approach)
- Handle failure scenarios

### 5. Trade-offs Discussion (5 min)
- What would you do differently with more time?
- What are the weakest points?

## Common System Design Problems

| Problem | Key Concepts |
|---|---|
| URL Shortener | Consistent hashing, KV store, redirect caching |
| Twitter Feed (Timeline) | Fan-out on write vs. read, Redis cache, Kafka |
| Rate Limiter | Token bucket, sliding window, Redis atomic ops |
| Distributed Cache | Consistent hashing, cache coherence, eviction policies |
| Notification System | Pub/sub, push/pull, priority queues |
| Search Autocomplete | Trie, top-k prefix search, distributed indexing |
| Ride-Sharing Matching | Geospatial indexing, real-time updates, WebSocket |
| Payment System | ACID transactions, idempotency, exactly-once delivery |

## Learning Resources

- **System Design Primer** (donnemartin) — comprehensive GitHub guide
- **System Design 101** (ByteByteGo) — visual explanations
- **Designing Data-Intensive Applications** (Kleppmann) — deep theory

## Related Wiki Concepts

- [[CAP Theorem]] — database selection framework
- [[Eventual Consistency]] — NoSQL consistency model
- [[CQRS]] — read/write optimization
- [[Apache Kafka]] — event streaming
- [[Circuit Breaker Pattern]] — resilience pattern
- [[API Gateway]] — entry point for microservices
- [[Microservices Architecture]] — decomposition approach
- [[Event-Driven Architecture]] — async communication
- [[Consistency Models Overview]] — choosing consistency levels
