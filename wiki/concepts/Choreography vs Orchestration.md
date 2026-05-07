---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Architectural Patterns for Event-Driven Systems.md'
  - 'arXiv 2605.03143 - Pact Choreographic Language.md'
  - 'Python CQRS Framework - vadikko2.md'
tags:
  - choreography
  - orchestration
  - event-driven
  - saga
  - microservices
  - distributed-workflow
  - coupling
---
# Choreography vs Orchestration

Two fundamentally different approaches to coordinating multi-step workflows across distributed services: **orchestration** uses a central coordinator that directs each service, while **choreography** has services react to events and emit new events with no central controller.

## Problem

When a business process spans multiple services (e.g., an order placement that triggers inventory reservation, payment processing, and shipping scheduling), how should the coordination work? Who drives the process?

## Solution / Explanation

### Orchestration

A dedicated **orchestrator** (a service or process manager) explicitly controls the flow:

```
Orchestrator ──► calls Inventory Service ──► success
             ──► calls Payment Service ──► success
             ──► calls Shipping Service ──► success
             ──► updates Order status
```

The orchestrator knows the entire workflow; it tells each service what to do and handles failures centrally.

**Characteristics:**
- Flow is explicit and centralized — easy to understand and debug
- Orchestrator is a single logical view of the process state
- Services are called, not reactive — they don't need to know about the workflow
- Central process manager is a potential single point of failure

**Tools:** AWS Step Functions, Temporal, Cadence, Dapr Workflow, Conductor

### Choreography

Services react to domain events and emit new events; no central coordinator:

```
OrderPlaced event published
     ↓
Inventory Service reacts ──► publishes InventoryReserved
     ↓
Payment Service reacts ──► publishes PaymentProcessed
     ↓
Shipping Service reacts ──► publishes ShipmentScheduled
```

The overall flow emerges from the individual reactions; no service knows the "whole picture."

**Characteristics:**
- Maximum service autonomy and decoupling
- Flow is implicit — emerges from event reactions
- Debugging requires following event chains across services
- No single point of failure; but failures are harder to detect and handle
- New services can join the workflow by subscribing to events

**Tools:** Apache Kafka, RabbitMQ, AWS SNS/SQS, Watermill

### Saga Pattern

The **Saga Pattern** applies to both:
- **Orchestration Saga:** The orchestrator calls services and handles compensation on failure
- **Choreography Saga:** Services listen for failure events and compensate themselves

## Decision Framework

| Factor | Use Orchestration | Use Choreography |
|---|---|---|
| **Flow visibility** | Need to see the whole workflow | Emergent flow is acceptable |
| **Centralized error handling** | Yes — compensations need coordination | No — services self-compensate |
| **Service coupling** | Services can be called | Services must be autonomous |
| **Debugging** | Easy — one log to follow | Hard — distributed event tracing needed |
| **Adding steps** | Requires orchestrator change | Just add a new subscriber |
| **Long-running workflows** | Orchestration (state management built in) | Choreography (but needs careful design) |
| **Agent systems** | Orchestration (predictable AI tool calls) | Choreography (reactive agent protocols) |

## Trade-offs

| | Orchestration | Choreography |
|---|---|---|
| **Coupling** | Services decoupled from each other; all coupled to orchestrator | Services fully decoupled |
| **Visibility** | High — central log | Low — distributed tracing required |
| **Failure handling** | Centralized compensations | Distributed compensations |
| **Scalability** | Orchestrator can bottleneck | Scales with event bus |
| **Testability** | Orchestrator testable in isolation | Integration tests required |
| **Complexity** | Simpler for complex flows | Complex for error handling |

## Related

- [[Saga Pattern]] — implements distributed transactions with either approach
- [[Event-Driven Architecture]] — choreography is native to EDA
- [[Publish-Subscribe Pattern]] — event bus for choreography
- [[Dapr]] — provides both workflow (orchestration) and pub/sub (choreography)
- [[Microservices Architecture]] — coordination challenge in microservices
