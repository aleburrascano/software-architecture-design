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
  - rabbitmq
  - amqp
---

# Message Queue

A durable, ordered buffer that holds messages sent by producers until they are consumed by a single consumer, enabling point-to-point asynchronous communication.

## Problem

Services that communicate synchronously are tightly coupled: if the downstream service is slow or unavailable, the caller blocks or fails. High-throughput workloads can overwhelm a downstream service with no way to absorb bursts.

## Solution / Explanation

A message queue acts as an intermediary buffer between a **producer** (sender) and a **consumer** (receiver). The producer places messages on the queue and returns immediately; the consumer pulls (or is pushed) messages at its own pace. The queue absorbs load spikes and survives transient consumer unavailability.

In **AMQP 0-9-1** (the protocol used by RabbitMQ), the flow is:

```
Publisher → Exchange → (Binding) → Queue → Consumer
```

- **Exchange** receives published messages and routes copies to one or more queues based on routing rules.
- **Binding** is the rule linking an exchange to a queue (optionally filtered by a routing key).
- **Queue** stores messages and delivers them to subscribed consumers.

### Exchange Types (AMQP)

| Type | Routing behaviour |
|------|------------------|
| Direct | Exact routing-key match |
| Fanout | Broadcast to all bound queues (ignores key) |
| Topic | Wildcard pattern matching on routing key |
| Headers | Routes on message header attributes |
| Default | Pre-declared direct exchange; binds queue by name |

### Delivery Guarantees

- **At-most-once** — auto-acknowledgement; fastest but may lose messages.
- **At-least-once** — explicit `basic.ack`; messages re-queued on failure; requires [[Idempotency]] on the consumer.
- **Exactly-once** — requires additional coordination (e.g., [[Outbox Pattern]] + idempotency).

### Message Durability

Durability requires three layers to all be set to durable/persistent:
1. The exchange must be declared durable.
2. The queue must be declared durable.
3. The message delivery mode must be set to persistent.

Omitting any one layer means messages can be lost on broker restart.

### Channels and Connections

AMQP multiplexes many **channels** over a single long-lived TCP **connection**, keeping network overhead low. Virtual hosts (**vhosts**) provide isolation within a single broker.

## Key Components

- **Producer / Publisher** — creates and sends messages.
- **Consumer** — receives and processes messages.
- **Broker** — the middleware (e.g., RabbitMQ) that hosts exchanges, queues, and bindings.
- **Acknowledgement** — confirmation signal from consumer that a message was processed successfully.
- **Dead Letter Channel** — where undeliverable or expired messages are routed.
- **Prefetch count** — limits how many unacknowledged messages a channel delivers to a consumer at once.

## When to Use

- Decoupling services that have different processing speeds.
- Background job processing (email sending, image resizing, report generation).
- Distributing work across multiple competing consumers.
- Absorbing traffic spikes without back-pressure propagating upstream.
- Ensuring delivery even when the consumer is temporarily unavailable.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Producer/consumer decoupling | Added operational complexity (broker to deploy and monitor) |
| Load levelling and burst absorption | Message ordering only guaranteed within a single queue |
| At-least-once delivery via acks | Consumers must handle duplicate messages ([[Idempotency]]) |
| Durability survives restarts | Disk I/O overhead for persistent messages |

## Comparison

| | Message Queue (point-to-point) | [[Publish-Subscribe Pattern]] (topic) |
|-|-------------------------------|--------------------------------------|
| Receivers | Single consumer per message | Multiple subscribers each get a copy |
| Message retention | Gone after ACK | Depends on broker (Kafka retains indefinitely) |
| Use case | Task distribution | Event fan-out, notifications |

## Related

- [[Publish-Subscribe Pattern]] — broadcast variant of messaging
- [[Message Broker]] — the infrastructure role that hosts queues and routing
- [[Enterprise Integration Patterns]] — taxonomy that names queue and channel patterns
- [[Apache Kafka]] — log-based alternative to traditional queues
- [[Event-Driven Architecture]] — architectural style built on messaging
- [[Idempotency]] — required for safe at-least-once processing
- [[Outbox Pattern]] — reliably publishing messages from within a DB transaction
- [[Backpressure]] — mechanism for consumer to signal overload to producer
