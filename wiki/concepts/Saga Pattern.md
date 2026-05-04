---
type: concept
created: '2026-05-03'
updated: 2026-05-03
sources:
  - 'https://microservices.io/patterns/data/saga.html'
  - 'https://awesome-architecture.com/cloud-design-patterns/saga/'
  - 'https://learn.microsoft.com/en-us/azure/architecture/patterns/saga'
tags:
  - distributed-systems
  - microservices
  - transactions
  - patterns
---

# Saga Pattern

A sequence of local transactions that together implement a business transaction spanning multiple services, using compensating transactions to undo work when a step fails.

## Problem

In a microservices architecture each service owns its own database (the Database per Service pattern). A business operation that spans multiple services — say, placing an order that debits a customer's credit limit — cannot use a single ACID database transaction. Two-phase commit (2PC) is technically possible but creates tight coupling, reduces availability, and is not supported by many NoSQL stores and modern cloud services.

## Solution

A saga breaks the cross-service transaction into a chain of **local transactions**. Each step:
1. Completes its work atomically within its own service database.
2. Publishes a message or event to trigger the next step.

If a step fails due to a business rule violation, the saga runs a series of **compensating transactions** that undo the changes made by the preceding steps. There is no global rollback; every step either succeeds or has a compensating action.

### Key Transaction Concepts (Microsoft)

| Concept | Meaning |
|---------|---------|
| **Compensable transactions** | Steps that can be reversed by a corresponding compensating action if a later step fails |
| **Pivot transaction** | The point of no return — once it succeeds, the saga must complete all remaining steps; no more compensations relevant |
| **Retryable transactions** | Idempotent steps after the pivot; may be retried until they succeed to reach the final consistent state |

## Key Components

### Choreography-Based Saga
Services communicate purely through domain events — no central coordinator.

1. Order Service creates a pending order and emits `OrderCreated`.
2. Customer Service handles the event, reserves credit, and emits `CreditReserved` or `CreditLimitExceeded`.
3. Order Service handles the outcome event and approves or cancels the order.

**Pros:** Loose coupling, simple for short sagas, no single point of failure.  
**Cons:** Hard to follow the overall flow; risk of cyclic dependencies; integration testing requires all services.

### Orchestration-Based Saga
A dedicated **saga orchestrator** sends commands to participant services and waits for replies.

1. Order Service creates an orchestrator.
2. Orchestrator sends `ReserveCredit` to Customer Service.
3. Customer Service replies with success or failure.
4. Orchestrator approves or cancels the order.

**Pros:** Explicit flow, easier to understand and monitor, avoids cyclic dependencies, better for complex workflows.  
**Cons:** Orchestrator is a single point of failure; adds coordination logic complexity.

### Compensating Transactions
Each step that can be undone must have a corresponding compensating action (e.g., `CancelCreditReservation`). Compensating transactions must be idempotent.

## When to Use

- You have adopted the Database per Service pattern.
- A business operation spans two or more services.
- You need eventual consistency rather than strong ACID guarantees.
- You want to avoid the availability and coupling costs of 2PC.
- You need to roll back or compensate if one of the operations in the sequence fails.

**Not suitable when:**
- Transactions are tightly coupled and require strong isolation.
- Cyclic dependencies between services exist.
- Compensating transactions for earlier participants cannot be reliably designed.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Maintains consistency without distributed locking | No automatic rollback — compensating transactions must be coded manually |
| Works with any persistence store | No isolation between concurrent sagas (dirty reads possible) |
| Services remain loosely coupled | Debugging and tracing cross-service flows is harder |
| Scales well | Requires reliable messaging (at-least-once delivery + idempotency) |

### Potential Data Anomalies (Microsoft)
- **Lost updates:** One saga overwrites another's changes
- **Dirty reads:** Reading uncommitted changes from a concurrent saga
- **Fuzzy reads:** Inconsistent data seen between saga steps due to updates in between

**Mitigation strategies:** semantic locks, commutative updates, pessimistic view reordering, version files.

## Real-World Usage

- **E-commerce order placement:** Reserve inventory → charge payment → schedule delivery. If payment fails, compensate by releasing the inventory reservation.
- **Travel booking:** Book flight + hotel + car rental; if car rental fails, cancel flight and hotel via compensating transactions.
- Frameworks: Axon Framework (Java), Eventuate (Java/Node.js), MassTransit (C#), Temporal.io.

## Related

- [[CQRS]] — commonly combined with sagas for the command side
- [[Event Sourcing]] — saga state can be persisted as an event log
- [[Outbox Pattern]] — used to publish saga events reliably within the same transaction
- [[Circuit Breaker Pattern]] — protects saga steps from cascading failures
- [[Idempotency]] — saga steps must be safe to retry
- [[Eventual Consistency]] — sagas trade isolation for availability
- [[Microservices Architecture]] — the context where sagas are most commonly needed
