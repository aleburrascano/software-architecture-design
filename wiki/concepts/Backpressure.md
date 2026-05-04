---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/back-pressure/'
tags:
  - distributed-systems
  - resilience
  - reactive
  - performance
---
# Backpressure

A flow-control mechanism in which a consumer signals to a producer that it cannot keep up with the current rate of data, causing the producer to slow down or pause — preventing buffer overflow, resource exhaustion, and cascading failures.

## Problem

In data pipelines and distributed systems, producers and consumers rarely process data at the same rate. Without a feedback mechanism:

- Queues grow unboundedly, exhausting memory.
- Consumers are overwhelmed, causing timeouts and errors.
- The system appears healthy until it suddenly crashes ("queue backlog disaster").
- Cascading failures propagate upstream as each layer buffers until it too fails.

## Solution / Explanation

**Backpressure** is "the resisted flow of data through software." Rather than letting producers push unlimited data into a queue, consumers signal their capacity and producers adjust their rate accordingly.

This is the same physical principle as water pressure in a pipe: downstream resistance (backpressure) propagates upstream, limiting flow.

### Handling Strategies

When backpressure is detected, a producer may:

| Strategy | Description | Trade-off |
|---|---|---|
| **Slow down (throttle)** | Reduce send rate to match consumer capacity | Adds latency; safe |
| **Buffer** | Accumulate items temporarily; flush when capacity allows | Risks memory exhaustion if buffer is unbounded |
| **Drop** | Discard excess items (with loss tracking) | Loses data; acceptable for metrics/telemetry |
| **Error / reject** | Return an error to the caller (e.g., HTTP 429) | Pushes responsibility to caller |
| **Shed load** | Prioritize high-value work; drop low-priority items | Complex to implement |

### Reactive Streams and Backpressure

The **Reactive Streams** specification (implemented in RxJava, Akka Streams, Project Reactor) formalizes backpressure as a first-class protocol: subscribers request a specific number of items from publishers, and publishers only emit that many. This prevents unconstrained push.

In practice, `Publisher → Subscriber` with demand signaling:
1. Subscriber signals `request(n)` — "I can handle n items."
2. Publisher emits at most n items.
3. Subscriber processes, then signals more demand.

### Backpressure in Message Queues

In Kafka and similar systems, consumers control their own polling rate. Backpressure is implicit — a slow consumer simply does not poll. Queue depth (consumer lag) is monitored as an early warning metric.

## When to Use

- Any system where producers and consumers operate at different speeds.
- Streaming data pipelines (Kafka consumers, Akka Streams).
- API gateways and ingestion endpoints under variable load.
- Anywhere resource exhaustion (memory, threads, connections) is a risk.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Prevents queue backlog disasters | Adds complexity to producer-consumer protocols |
| Preserves system stability under load spikes | Throttling adds end-to-end latency |
| Makes capacity limits explicit | Dropping strategies lose data |
| Enables graceful degradation | Reactive Streams have a learning curve |

## Related

- [[Resiliency Patterns]] — backpressure is a fundamental resilience mechanism
- [[Circuit Breaker Pattern]] — circuit breaker prevents overwhelmed downstreams; backpressure slows upstreams
- [[Bulkhead Pattern]] — isolates resource pools; complements backpressure
- [[Microservices Architecture]] — inter-service communication needs backpressure under load
- [[Observability]] — queue depth and consumer lag are key backpressure metrics to monitor
