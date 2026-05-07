---
type: concept
created: 2026-05-03
updated: 2026-05-06
sources:
  - "raw/articles/What is Software Architecture A Comprehensive Guide.md"
  - "raw/articles/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/articles/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/articles/Software architecture 1.md"
  - "https://www.geeksforgeeks.org/system-design/event-driven-architecture-system-design/"
tags:
  - architecture
  - architectural-pattern
  - event-driven
  - messaging
---

# Event-Driven Architecture

An architectural style in which system components communicate by producing and consuming events — discrete, immutable records of significant state changes or occurrences — rather than through direct synchronous calls.

## Problem

In tightly coupled systems, components invoke each other directly. This creates temporal coupling (both caller and callee must be available simultaneously), functional coupling (the caller must know who handles its request), and fragility under load spikes (one slow component stalls the whole chain). As systems grow, the web of direct calls becomes unmanageable and any single failure propagates rapidly through the call chain.

## Solution

Decouple producers from consumers via an **event broker** (e.g., Apache Kafka, RabbitMQ, AWS EventBridge). Producers emit events without knowledge of who will react; consumers subscribe to the events they care about and act independently. The broker buffers, routes, and (in streaming systems) persists events. Components operate independently while reacting to events in real time.

## Key Components / Structure

| Component | Role |
|-----------|------|
| **Event Source** | Generates events from user interfaces, sensors, databases, or external systems |
| **Event** | Immutable record of something that happened (e.g., `OrderCreated`, `PaymentProcessed`) — contains all relevant contextual data |
| **Publisher / Producer** | The component that detects a significant occurrence and emits the event asynchronously |
| **Event Broker / Bus** | Central hub that receives, filters, routes, and (optionally) persists events; decouples producers from consumers |
| **Subscriber / Consumer** | Registers for specific event types and processes them independently |
| **Event Handler** | Contains the processing logic and business rules for a consumed event |
| **Dispatcher** | Logic within the broker that matches events to interested consumers |

### Two primary sub-patterns

**Pub/Sub (Publish-Subscribe):** Producers publish to a topic or channel; all subscribed consumers receive every event. Fire-and-forget; broker does not persist beyond delivery. Suited for fan-out notifications.

**Event Streaming:** Events are persisted in an ordered, durable log (e.g., Kafka). Consumers can replay the log from any offset. Enables audit trails, temporal decoupling, and building read models. Foundational for [[Event Sourcing]].

### Choreography vs. Orchestration

**Choreography:** Each service reacts to events and emits new events — no central coordinator. Decentralized, emergent process flow.

**Orchestration:** A central process manager emits commands and listens for outcomes — explicit, visible workflow. Better for complex multi-step processes.

## When to Use

- Real-time systems: IoT pipelines, telemetry, live analytics, anomaly detection, fraud detection.
- Loosely-coupled microservices where domains should not call each other directly.
- Fan-out scenarios: one trigger needs to notify many downstream systems simultaneously.
- Financial services transaction processing, e-commerce order management.
- Systems where workload is bursty and consumers need to process at their own pace.
- Online gaming real-time multiplayer interactions, social media feed updates.

## Trade-offs

**Pros:**
- Loose coupling — producers and consumers evolve independently.
- High scalability — consumers can be scaled independently; the broker absorbs burst traffic.
- Resilience — a slow or failed consumer does not stall the producer.
- Extensibility — new consumers can subscribe without changing producers.
- Real-time responsiveness and asynchronous processing.

**Cons / pitfalls:**
- Operational complexity — requires deploying and operating a broker.
- Hard to trace end-to-end flows; distributed tracing tooling is essential.
- Eventual consistency — downstream state may lag the event.
- Event ordering and deduplication must be managed explicitly.
- Debugging is harder; testing the full event chain is more involved than testing synchronous code.
- Latency between event occurrence and consumer response.

## Real-World Usage

- **Order management:** An `OrderCreated` event triggers payments, inventory, and shipping services independently — no single orchestrator calling each sequentially.
- **Financial services:** Fraud detection and transaction processing react to account events in real time.
- **Social media:** A "like" event is published and consumed by the notification service, analytics, and feed ranking service simultaneously.
- **IoT systems:** Sensor readings are streamed to an event bus; multiple analytics consumers process them in parallel.
- **Telecommunications:** Network monitoring systems react to infrastructure events for fault detection.
- Widely adopted via Apache Kafka (event streaming), AWS EventBridge (cloud pub/sub), RabbitMQ, and Azure Service Bus.

## Comparison

| Dimension | Event-Driven | Request/Response (REST) |
|-----------|-------------|------------------------|
| Coupling | Loose (producer unaware of consumers) | Tighter (caller knows callee) |
| Timing | Asynchronous | Synchronous |
| Scalability | High (consumers scale independently) | Moderate |
| Observability | Harder (distributed trace required) | Easier |
| Latency | Variable (eventual) | Predictable |

## EDA Patterns Taxonomy

EDA encompasses several distinct patterns that differ in what an event carries and how consumers react:

| Pattern | Event Payload | Consumer Behavior |
|---------|--------------|-------------------|
| **[[Event Notification Pattern]]** | Thin signal — "something happened" | Consumer calls back for details if needed |
| **[[Event-Carried State Transfer]]** | Full state in payload | Consumer caches local copy; no callback needed |
| **[[Event Sourcing]]** | All state changes as immutable log | Consumer replays log to reconstruct state |
| **[[Choreography vs Orchestration]]** | Topology choice | Services react to events (choreography) or follow commands (orchestration) |

Choosing the right pattern is the first EDA design decision: notification creates less data coupling but may cause thundering-herd callbacks; ECST enables full temporal decoupling but increases payload size and drift risk.

## Related

- [[CQRS]] — often combined with EDA; the command side publishes events that update the read model
- [[Event Sourcing]] — a specialization where the event log is the system of record
- [[Microservices Architecture]] — EDA further decouples services in a microservices deployment
- [[Domain-Driven Design]] — domain events are the natural currency in EDA
- [[Service-Oriented Architecture]] — async messaging can replace ESB orchestration in modern SOA variants
- [[Dapr]] — CNCF sidecar runtime providing plug-and-play pub/sub, state, and workflow building blocks for EDA
