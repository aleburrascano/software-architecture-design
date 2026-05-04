---
type: topic
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/microservices/microservices/'
  - 'https://awesome-architecture.com/microservices/resiliency/resiliency/'
tags:
  - microservices
  - distributed-systems
  - patterns
  - architecture
---
# Microservices Patterns Overview

A synthesis page mapping the common challenges in microservices architecture to the patterns that address them, providing a navigable reference for the full pattern landscape.

## Core Characteristics of Microservices

Microservices architecture decomposes an application into small, independently deployable services. Key characteristics:
- Each service owns its own database (Database per Service).
- Services communicate over the network (HTTP, gRPC, messaging).
- Services are independently deployable and scalable.
- Each service is organized around a business capability ([[Bounded Context]] in DDD).

## Challenge Map: Problems → Patterns

### Challenge: Data Consistency Across Services

Without a shared database or global transaction, keeping data consistent is the hardest microservices problem.

| Pattern | Description |
|---|---|
| [[Saga Pattern]] | Choreography or orchestration-based sequences of local transactions with compensating rollbacks |
| [[Outbox Pattern]] | Atomic state + event publication in one database transaction |
| [[Inbox Pattern]] | Deduplication of incoming events on the consumer side |
| [[Idempotency]] | Operations safe to replay without duplicate side effects |
| [[Eventual Consistency]] | Accept that replicas converge over time rather than immediately |
| [[Distributed Transactions]] | 2PC for strong consistency (with availability trade-offs) |

### Challenge: Service Reliability & Fault Tolerance

In distributed systems, failures are the norm, not the exception.

| Pattern | Description |
|---|---|
| [[Circuit Breaker Pattern]] | Trip open when a dependency fails; prevent cascading failures |
| [[Bulkhead Pattern]] | Isolate resource pools; contain blast radius |
| [[Resiliency Patterns]] | Retry, timeout, fallback, hedging — layered defense |
| [[Backpressure]] | Flow control: consumers signal capacity to producers |

### Challenge: Service Communication

Services must discover each other and communicate reliably.

| Pattern | Description |
|---|---|
| [[API Gateway Pattern]] | Single external entry point; cross-cutting concerns centralized |
| [[Backend for Frontend Pattern]] | Per-client-type API tailored to specific needs |
| [[Service Mesh]] | Infrastructure for east-west communication (mTLS, retries, tracing) |
| [[Sidecar Pattern]] | Peripheral concerns in a co-deployed proxy container |
| [[Ambassador Pattern]] | Outbound call management via a sidecar proxy |

### Challenge: Observability

With many services, debugging failures requires system-wide visibility.

| Pattern | Description |
|---|---|
| [[Observability]] | Three pillars: logs, metrics, distributed traces |
| [[Distributed Tracing]] | Track requests end-to-end; identify latency bottlenecks |

### Challenge: Migration & Evolution

Moving from monoliths to microservices (or evolving existing microservices).

| Pattern | Description |
|---|---|
| [[Strangler Fig Pattern]] | Incrementally replace legacy with a façade |
| [[Anti-Corruption Layer Pattern]] | Isolate new system from legacy model corruption |
| [[Modular Monolith]] | An intermediate architecture before splitting into services |

### Challenge: Domain Modeling

Services must be cohesively scoped around business capabilities.

| Concept | Description |
|---|---|
| [[Bounded Context]] | DDD strategic boundary; often maps to one service |
| [[Aggregate]] | Transactional unit within a service |
| [[Domain Event]] | Cross-service communication via business facts |
| [[Event Storming]] | Workshop to discover service boundaries and events |

## Service Decomposition Strategies

1. **Decompose by business capability** — one service per business function.
2. **Decompose by subdomain** — use DDD subdomains; core domain gets the most investment.
3. **[[Vertical Slice Architecture]]** — within a service, organize code by feature.

## Technology Layers

| Layer | Common Tools |
|---|---|
| Service mesh | Istio, Linkerd, Consul Connect |
| API gateway | Kong, AWS API Gateway, Ocelot, Envoy |
| Messaging | Kafka, RabbitMQ, Azure Service Bus |
| Distributed tracing | Jaeger, Zipkin, OpenTelemetry |
| Resilience (.NET) | Polly, Simmy |
| Actor frameworks | Orleans, Akka.NET, Proto.Actor |

## Related

- [[Microservices Architecture]] — the architectural style overview
- [[Cloud Design Patterns Overview]] — patterns organized by cloud concern
- [[DDD Building Blocks]] — domain modeling for microservices
- [[Event-Driven Architecture]] — many microservices communicate via events
