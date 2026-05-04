---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.rabbitmq.com/tutorials/amqp-concepts.html
  - https://kafka.apache.org/intro
  - https://www.enterpriseintegrationpatterns.com/patterns/messaging/
tags:
  - messaging
  - async
  - integration
  - kafka
  - rabbitmq
  - synthesis
---

# Messaging Patterns Overview

A synthesis of queues, topics, and streams: when to use which, how they relate, and the Enterprise Integration Patterns taxonomy that names the recurring patterns across all of them.

---

## Three Core Messaging Primitives

### 1. Message Queue (Point-to-Point)

A [[Message Queue]] delivers each message to **exactly one consumer**. Multiple consumers on the same queue compete for messages — this is the **Competing Consumers** pattern, enabling parallel processing and load distribution.

**Characteristics:**
- One message → one consumer.
- Messages are removed after acknowledgement.
- Load-balanced across competing consumers.
- FIFO within the queue (usually).

**When to use:** Task distribution, background job processing, distributing work across a worker pool.

**Example tools:** RabbitMQ (with Direct exchange), AWS SQS, Azure Service Bus queues.

---

### 2. Publish-Subscribe (Topics)

The [[Publish-Subscribe Pattern]] delivers each message to **all subscribers**. Each subscriber maintains its own independent queue/subscription and receives every message published to the topic.

**Characteristics:**
- One message → all subscribers get a copy.
- Publisher is completely decoupled from subscribers.
- Adding a new subscriber requires no producer change.
- Fan-out amplifies load.

**When to use:** Event fan-out (domain events triggering multiple downstream services), notifications, audit logging, cache invalidation.

**Example tools:** RabbitMQ (Fanout/Topic exchange), AWS SNS, Azure Event Grid, Google Pub/Sub, Kafka (per-topic consumer group).

---

### 3. Event Streams (Distributed Log)

[[Apache Kafka]] represents a third primitive: the **persistent, replayable event log**. Unlike queues and topics, the log retains messages after consumption. Consumers maintain their own position (offset) in the stream.

**Characteristics:**
- Messages retained by policy (time or size), not by consumption.
- Multiple independent consumer groups each read the full stream independently.
- Consumers can replay from any point in history.
- Ordered within a partition; partitioned for parallelism.
- High throughput (millions of events/second).

**When to use:** Event sourcing, audit logs, stream processing, microservices integration where replay is needed, feeding multiple downstream systems from a single source of truth.

**Example tools:** Apache Kafka, AWS Kinesis, Azure Event Hubs, Confluent Platform.

---

## Queues vs. Topics vs. Streams: Decision Guide

| Question | Use |
|---------|-----|
| One worker should process each task | Queue (Point-to-Point) |
| Multiple services need the same event | Topic (Pub-Sub) |
| Need to replay past events | Stream (Kafka) |
| Multiple consumers at different speeds | Stream (Kafka, independent offsets) |
| Simple task queue, rich routing | Queue (RabbitMQ) |
| Extremely high throughput | Stream (Kafka) |
| At-least-once delivery with simple setup | Queue (RabbitMQ) |
| Event sourcing / CQRS projections | Stream (Kafka) |

---

## Message Broker vs. Event Streaming Platform

| | [[Message Broker]] (RabbitMQ) | Event Streaming Platform (Kafka) |
|-|------------------------------|--------------------------------|
| Primary model | Push delivery to consumers | Pull-based log consumers |
| Message lifetime | Deleted after ACK | Retained by policy |
| Routing | Rich (exchange types, bindings) | Simple (topic + partition key) |
| Replay | No | Yes |
| Throughput | High | Very high |
| Consumer groups | Competing consumers on a queue | Independent groups, each read full stream |
| Ordering | Per queue | Per partition |

**Rule of thumb:** Choose a message broker for task distribution and rich routing needs; choose an event streaming platform when you need replay, multiple independent consumers, or event sourcing.

---

## Enterprise Integration Patterns Taxonomy

The [[Enterprise Integration Patterns]] (Hohpe & Woolf) vocabulary applies across all messaging systems:

### Building Blocks

| Concept | EIP Name | Notes |
|---------|---------|-------|
| Transport pipe | Message Channel | Can be Point-to-Point or Pub-Sub |
| Routing logic | Message Router | Content-based, topic-based, dynamic |
| Format conversion | Message Translator | Canonical Data Model for cross-system |
| Application connection | Message Endpoint | Gateway, adapter, service activator |

### Common Patterns by Problem

**Fan-out (notify multiple services):**
→ Publish-Subscribe Channel + Recipient List

**Parallel processing (distribute load):**
→ Point-to-Point Channel + Competing Consumers

**Aggregating multiple responses:**
→ Scatter-Gather (broadcast + Aggregator)

**Complex conditional routing:**
→ Content-Based Router + Message Filter

**Large payload efficiency:**
→ Claim Check (store body externally, pass a token)

**Reliable delivery:**
→ Guaranteed Delivery + Outbox Pattern (see [[Outbox Pattern]])

**Duplicate handling:**
→ Idempotent Receiver (see [[Idempotency]])

**Error handling:**
→ Dead Letter Channel (poison messages)

---

## Delivery Semantics

All messaging systems make trade-offs on delivery guarantees:

| Semantic | Description | Requires |
|----------|-------------|---------|
| **At-most-once** | Message delivered 0 or 1 times; may be lost | Auto-ack; fastest |
| **At-least-once** | Message delivered 1+ times; may be duplicate | Explicit ack + [[Idempotency\|idempotent consumer]] |
| **Exactly-once** | Message delivered precisely once | Transactional outbox + idempotency, or Kafka transactions |

In practice, **at-least-once + idempotent consumer** is the most common pragmatic choice. True exactly-once adds significant complexity.

---

## Reliability Patterns

- **[[Outbox Pattern]]** — ensures a database write and a message publish happen atomically; prevents lost messages on process crash.
- **Dead Letter Queue (DLQ)** — messages that fail repeated processing go to a DLQ for inspection and replay.
- **[[Backpressure]]** — mechanisms for consumers to signal overload: prefetch limits (RabbitMQ), consumer lag monitoring (Kafka), reactive streams.
- **[[Circuit Breaker Pattern]]** — when a downstream consumer or broker is failing, stop sending rather than overloading.

---

## Architectural Context

Messaging is the backbone of [[Event-Driven Architecture]]. Key patterns that build on messaging:

- **[[Event Sourcing]]** — the event log (Kafka) as the system of record.
- **[[CQRS]]** — event stream feeds read-model projections.
- **[[Saga Pattern]]** — distributed transactions coordinated through messages.
- **[[Microservices Architecture]]** — services communicate via events rather than direct calls.

---

## Related

- [[Message Queue]] — point-to-point async messaging
- [[Publish-Subscribe Pattern]] — fan-out to multiple subscribers
- [[Apache Kafka]] — log-based event streaming platform
- [[Message Broker]] — infrastructure role; RabbitMQ vs. Kafka trade-offs
- [[Enterprise Integration Patterns]] — the canonical pattern vocabulary
- [[Event-Driven Architecture]] — messaging as the architectural backbone
- [[Outbox Pattern]] — reliable message publishing
- [[Idempotency]] — safe handling of duplicate messages
- [[Backpressure]] — consumer overload management
