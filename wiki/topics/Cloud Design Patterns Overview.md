---
type: topic
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - >-
    https://awesome-architecture.com/cloud-design-patterns/cloud-design-patterns.md
tags:
  - cloud-native
  - patterns
  - microservices
  - distributed-systems
---
# Cloud Design Patterns Overview

A synthesis of the key patterns for designing reliable, scalable, and maintainable cloud-native and microservices applications, organized by the problem they address.

## What Are Cloud Design Patterns?

Cloud design patterns are proven solutions to recurring architectural problems in distributed, cloud-native systems. Unlike traditional GoF design patterns (which operate at the class/object level), cloud patterns operate at the service and system level — addressing concerns like reliability, messaging, data management, and operational concerns.

## Patterns by Category

### Reliability & Fault Tolerance

| Pattern | Core Idea |
|---|---|
| [[Circuit Breaker Pattern]] | Trip open when a dependency fails repeatedly; prevent cascading failures |
| [[Bulkhead Pattern]] | Isolate resource pools so failures in one don't exhaust resources for others |
| [[Resiliency Patterns]] | Family: Retry, Timeout, Fallback, Hedging — layered defense in depth |
| [[Backpressure]] | Consumers signal capacity; producers throttle to prevent overload |
| [[Idempotency]] | Operations safe to retry; at-least-once delivery with no duplicate side effects |

### Messaging & Data Consistency

| Pattern | Core Idea |
|---|---|
| [[Saga Pattern]] | Distributed transactions via local transactions + compensating transactions |
| [[Outbox Pattern]] | Atomic state change + event publication in one DB transaction |
| [[Inbox Pattern]] | Deduplicate incoming messages; exactly-once handler execution |
| [[Distributed Transactions]] | 2PC and alternatives for cross-service atomicity |
| [[Eventual Consistency]] | Accept temporary staleness for higher availability |

### Service Communication & Topology

| Pattern | Core Idea |
|---|---|
| [[API Gateway Pattern]] | Single entry point for external clients; centralizes cross-cutting concerns |
| [[Backend for Frontend Pattern]] | Per-client-type gateway tailored to specific data needs |
| [[Service Mesh]] | Infrastructure layer for east-west service communication |
| [[Sidecar Pattern]] | Peripheral concerns in a co-deployed container |
| [[Ambassador Pattern]] | Outbound call management via a proxy sidecar |

### Migration & Integration

| Pattern | Core Idea |
|---|---|
| [[Strangler Fig Pattern]] | Incrementally replace legacy systems behind a façade |
| [[Anti-Corruption Layer Pattern]] | Translate between incompatible domain models at integration boundaries |

### Observability

| Pattern | Core Idea |
|---|---|
| [[Observability]] | Logs + Metrics + Traces as the three pillars |
| [[Distributed Tracing]] | Track requests end-to-end across services |

## Pattern Relationships

Many patterns work together as a system:

```
External Clients
      │
  [API Gateway] / [BFF]
      │
  [Service Mesh] ──── east-west calls ────► [Services]
      │                     │
  [Sidecar/Ambassador]  [Circuit Breaker]
                             │
                    [Saga Orchestrator]
                        │         │
              [Service A]       [Service B]
              [Outbox]          [Inbox]
                  │
          [Message Broker]
```

## Choosing Patterns

| Problem | Primary Pattern(s) |
|---|---|
| Cross-service transaction consistency | [[Saga Pattern]] + [[Outbox Pattern]] |
| Cascading failures | [[Circuit Breaker Pattern]] + [[Bulkhead Pattern]] |
| Legacy migration | [[Strangler Fig Pattern]] + [[Anti-Corruption Layer Pattern]] |
| External client API | [[API Gateway Pattern]] or [[Backend for Frontend Pattern]] |
| Service-to-service networking | [[Service Mesh]] |
| Duplicate message processing | [[Inbox Pattern]] + [[Idempotency]] |

## Related

- [[Microservices Architecture]] — the architectural context these patterns serve
- [[Event-Driven Architecture]] — many cloud patterns are event-driven
- [[Resiliency Patterns]] — detailed resilience pattern family
- [[DDD Building Blocks]] — strategic/tactical DDD aligns with cloud patterns
