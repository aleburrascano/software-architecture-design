---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.reactivemanifesto.org/
tags:
  - reactive
  - architecture
  - distributed-systems
  - resilience
---

# Reactive Architecture

A systems design approach, codified in the Reactive Manifesto (2014), that builds applications to be Responsive, Resilient, Elastic, and Message-Driven — enabling them to meet modern demands for low latency, continuous availability, and variable load.

## Problem

Contemporary applications face conditions that traditional synchronous, monolithic architectures handle poorly:

- **Millisecond response time expectations** — users abandon pages that take more than a second to load.
- **100% uptime requirements** — planned maintenance windows are no longer acceptable.
- **Unpredictable load** — traffic can spike 10–100× in seconds (flash sales, breaking news).
- **Multi-node deployment** — applications span mobile, cloud, and edge; failure is the norm, not the exception.

## Solution / Explanation

The **Reactive Manifesto** (Jonas Bonér, Dave Farley, Roland Kuhn, Martin Thompson — 2014) defines four interconnected properties that together describe a Reactive System:

### 1. Responsive

> "The system responds in a timely manner if at all possible."

Responsiveness is the primary observable quality — the foundation of usability and trust. A responsive system detects and handles problems quickly, providing consistent quality of service. This is not about being "fast" but about being **reliably bounded in latency**.

### 2. Resilient

> "The system stays responsive in the face of failure."

Resilience is achieved through:
- **Replication** — multiple copies prevent single points of failure.
- **Containment** — failures stay within their component; they do not cascade.
- **Isolation** — components can fail independently (see [[Bulkhead Pattern]]).
- **Delegation** — recovery is handled externally (supervisor strategies in actor models).

Resilience is distinct from fault tolerance: the goal is to remain **responsive under failure**, not merely to not crash.

### 3. Elastic

> "The system stays responsive under varying workload."

Elastic systems scale up and down in response to demand:
- Eliminate bottlenecks through **sharding** and **replication**.
- Support both **predictive scaling** (scheduled events) and **reactive scaling** (triggered by load metrics).
- Design so no component is a fixed bottleneck; measure and tune with real data.

### 4. Message-Driven

> "Reactive Systems rely on asynchronous message-passing to establish a boundary between components."

Message-passing is the enabling mechanism for the other three properties:
- **Loose coupling** — sender and receiver are decoupled in time and space.
- **Location transparency** — messages work the same whether the recipient is in-process, on another thread, or on another continent.
- **Failure as messages** — errors are communicated as first-class messages, not exceptions that unwind call stacks.
- **Backpressure** — queue depth signals load; consumers can signal capacity to producers. See [[Backpressure]] and [[Reactive Streams]].

### Relationship Between the Four Properties

```
Message-Driven  →  enables  →  Isolation/Replication/Elasticity
                                       ↓
                              Resilience + Elasticity  →  Responsiveness
```

Responsiveness is the goal; resilience and elasticity are the means; message-driven is the mechanism.

## Key Components / Rules

- Apply the four properties at **all scales** — from individual components to globally-distributed systems.
- Treat the **network as unreliable by default** — design for partial failure, not happy-path connectivity.
- Use [[Backpressure]] to prevent fast producers from overwhelming slow consumers.
- Model failure explicitly — prefer actor supervision trees or saga compensations over try/catch propagation.

## When to Use

- High-traffic systems requiring sustained low latency (fintech, gaming, streaming).
- Systems requiring 24/7 availability with zero-downtime deployments.
- Workloads with highly variable throughput (IoT, event processing).
- [[Event-Driven Architecture]] systems where services communicate via events.

## Trade-offs

| Benefit | Cost |
|---|---|
| High availability and responsiveness | Asynchronous reasoning is harder than synchronous |
| Scales with demand | Eventual consistency requires explicit handling |
| Failures are contained and recoverable | Distributed tracing and observability become critical |
| Message-passing enables independent evolution | Higher operational complexity |

## Related

- [[Reactive Streams]] — the JVM specification for backpressured async streams
- [[Reactive Programming]] — the programming model for composing reactive pipelines
- [[Event-Driven Architecture]] — shares message-driven decomposition philosophy
- [[Backpressure]] — the flow-control mechanism central to reactive systems
- [[CQRS]] — often combined with reactive architecture for read/write separation
- [[Reactive Architecture Overview]] — synthesis page
