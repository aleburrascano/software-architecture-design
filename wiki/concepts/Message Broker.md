---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.rabbitmq.com/tutorials/amqp-concepts.html
  - https://kafka.apache.org/intro
  - https://www.enterpriseintegrationpatterns.com/patterns/messaging/
tags:
  - messaging
  - infrastructure
  - middleware
  - rabbitmq
  - kafka
  - event-driven
---

# Message Broker

An intermediary middleware component that receives messages from producers, applies routing rules, and delivers them to consumers — decoupling senders from receivers in a distributed system.

## Problem

In a distributed system, services must communicate without tight temporal or logical coupling. Direct service-to-service calls create a web of dependencies, make services unavailable when downstream is down, and require every producer to know every consumer's location and protocol.

## Solution / Explanation

A message broker sits between producers and consumers. Producers send messages to the broker; the broker routes, stores, and delivers them according to declared rules (exchanges, topics, subscriptions). Neither producer nor consumer needs to know about the other.

The broker provides:
- **Routing** — directs messages to the right consumer(s) based on keys, topics, or content.
- **Buffering** — absorbs bursts and decouples processing speeds.
- **Durability** — persists messages so they survive consumer outages.
- **Delivery guarantees** — at-most-once, at-least-once, or (with application help) exactly-once.

### Brokers in Event-Driven Architecture

In an [[Event-Driven Architecture]], the broker is the backbone. Services publish domain events to the broker without caring who listens. New consumers subscribe without touching the producer. This is the core enabling infrastructure for the [[Publish-Subscribe Pattern]].

## RabbitMQ vs. Kafka

These two brokers represent fundamentally different models:

| Concern | RabbitMQ (traditional broker) | Apache Kafka (log-based broker) |
|---------|-------------------------------|--------------------------------|
| Model | Push-based; broker delivers to consumer | Pull-based; consumer fetches from offset |
| Message lifetime | Deleted after ACK | Retained by policy (time/size) |
| Routing flexibility | Rich: direct, fanout, topic, headers | Simple: topic + partition key |
| Ordering | Per queue | Per partition |
| Replay | No | Yes — consumers can rewind |
| Throughput | High (hundreds of thousands/s) | Very high (millions/s) |
| Best for | Task queues, RPC, complex routing | Event streaming, audit, replay |
| Protocols | AMQP, STOMP, MQTT | Custom Kafka protocol |

### Push vs. Pull

**Push (RabbitMQ)** — broker actively delivers messages to consumers. Fast latency; consumer may be overwhelmed if it cannot keep up. Prefetch limits help (see [[Backpressure]]).

**Pull (Kafka)** — consumer polls at its own pace. Natural back-pressure; consumer controls offset and replay. Slightly higher latency for bursty workloads.

## Key Components

- **Exchange / Topic** — the named routing point where producers publish.
- **Queue / Partition** — the durable storage unit where messages wait.
- **Binding / Subscription** — the routing rule from topic to queue.
- **Consumer Group** — set of consumers sharing partition assignment (Kafka) or competing on a queue (RabbitMQ).
- **Dead Letter Queue (DLQ)** — receives messages that could not be processed after N retries.
- **Acknowledgement (ACK)** — consumer confirmation enabling the broker to remove the message.

## When to Use

- Any scenario where producer and consumer must be temporally or logically decoupled.
- Microservices integration where services publish domain events.
- Task distribution across a worker pool.
- Reliable delivery when the consumer may be temporarily unavailable.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Decouples producers and consumers | Adds infrastructure to deploy, monitor, and operate |
| Durability and delivery guarantees | Increased end-to-end latency vs. direct calls |
| Enables fan-out without producer changes | At-least-once delivery needs idempotent consumers |
| Load levelling | Single broker is a potential bottleneck (mitigate with clustering) |

## Related

- [[Message Queue]] — point-to-point channel hosted by a broker
- [[Publish-Subscribe Pattern]] — fan-out pattern enabled by a broker
- [[Apache Kafka]] — log-based broker specialised for event streaming
- [[Enterprise Integration Patterns]] — Message Broker is a named EIP pattern
- [[Event-Driven Architecture]] — the broker is the core EDA infrastructure
- [[Circuit Breaker Pattern]] — a resilience pattern for when broker connections fail
- [[Backpressure]] — managing consumer overwhelm
- [[Idempotency]] — required for safe at-least-once redelivery
