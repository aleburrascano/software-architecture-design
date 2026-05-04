---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.rabbitmq.com/tutorials/amqp-concepts.html
  - https://www.enterpriseintegrationpatterns.com/patterns/messaging/
tags:
  - messaging
  - async
  - integration
  - pattern
  - event-driven
---

# Publish-Subscribe Pattern

A messaging pattern where a publisher broadcasts events to a **topic** or channel and multiple independent subscribers each receive a copy, without the publisher knowing who (or how many) subscribers exist.

## Problem

Multiple services need to react to the same event (e.g., an order placed triggers inventory update, email notification, and analytics pipeline). Directly calling each service couples the event source to every downstream consumer and requires changes every time a subscriber is added or removed.

## Solution / Explanation

The publisher sends messages to a **topic** (or fanout exchange). The messaging infrastructure routes a copy of each message to every registered subscriber's queue. Publishers and subscribers are fully decoupled: neither knows the other's identity or count.

### Topics vs. Queues

| | Queue (Point-to-Point) | Topic (Publish-Subscribe) |
|-|-----------------------|--------------------------|
| Receivers | One consumer per message | All subscribers get a copy |
| Competing consumers | Yes — load-balanced | No — each subscriber is independent |
| Use case | Task distribution | Event fan-out |

### In AMQP (RabbitMQ)

A **fanout exchange** broadcasts every message to all bound queues. Each subscriber declares its own queue and binds it to the exchange. A **topic exchange** adds selective subscription via routing-key patterns (e.g., `order.*.created` matches any region).

### In Kafka

Kafka models pub-sub through **consumer groups**. Multiple consumer groups can read the same topic independently, each maintaining their own offset. Within a consumer group, partitions are distributed among members for parallel processing. See [[Apache Kafka]] for detail.

## Difference from Observer Pattern

The **Observer** pattern (GoF) operates **in-process**: subject and observers share the same runtime, and notification is synchronous. Pub-sub is **distributed**: publisher and subscribers run in separate processes (often separate services), communication is asynchronous, and the broker guarantees delivery even if a subscriber is temporarily offline (durable subscribers).

| | Observer | Publish-Subscribe |
|-|----------|------------------|
| Scope | In-process | Distributed / cross-service |
| Coupling | Subject knows observer interface | No direct coupling |
| Delivery | Synchronous | Asynchronous |
| Durability | Lost if observer is unavailable | Durable when queue is persistent |

## Key Components

- **Publisher** — produces events without knowledge of consumers.
- **Topic / Exchange** — the named channel events are published to.
- **Subscription / Binding** — the link between a topic and a subscriber's queue.
- **Subscriber** — receives events and reacts independently.
- **Durable Subscriber** — retains messages while offline (EIP pattern).

## When to Use

- Multiple independent services must react to the same domain event (order created, user registered).
- Fan-out notifications (push to mobile devices, webhooks).
- Decoupling an event source from a variable number of downstream consumers.
- Audit logs, analytics pipelines, and caches that shadow domain events.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Publisher/subscriber fully decoupled | No delivery confirmation back to publisher |
| Easy to add new subscribers | Message duplication possible; consumers must be [[Idempotency\|idempotent]] |
| Each subscriber processes independently | Fan-out amplifies load on the broker |
| Supports slow/fast subscribers independently | Ordering only guaranteed per-partition/queue |

## Related

- [[Message Queue]] — point-to-point counterpart
- [[Message Broker]] — infrastructure hosting topic routing
- [[Apache Kafka]] — durable, replayable pub-sub via distributed log
- [[Enterprise Integration Patterns]] — formalises Publish-Subscribe Channel pattern
- [[Event-Driven Architecture]] — architectural style where pub-sub is the communication backbone
- [[Observer Pattern]] — in-process equivalent
- [[Backpressure]] — managing subscriber overwhelm
