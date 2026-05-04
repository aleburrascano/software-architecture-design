---
type: source
created: '2026-05-03'
updated: '2026-05-03'
source_path: 'https://microservices.io/patterns/data/saga.html'
source_date: ''
source_author: Chris Richardson
tags:
  - source
  - microservices
  - patterns
  - transactions
---
# microservices.io - Saga Pattern

Chris Richardson's microservices.io pattern catalog entry for the Saga pattern.

## Summary

Canonical pattern catalog entry defining the Saga pattern as the solution for maintaining data consistency across services in a microservices architecture that uses the Database per Service pattern. Defines both choreography-based and orchestration-based coordination approaches with worked examples.

## Key Arguments

- A saga is a sequence of local transactions; each local transaction publishes a message/event triggering the next.
- When a step fails, the saga executes compensating transactions to undo preceding changes.
- **Choreography**: services communicate through domain events; no central coordinator; simpler for short sagas.
- **Orchestration**: a central orchestrator sends commands and receives replies; explicit flow; better for complex sagas.
- Key drawbacks: no automatic rollback (compensating transactions must be hand-coded); no isolation (concurrent sagas can read each other's intermediate state — "dirty reads").
- Services must atomically update their database and publish events (addressed by [[Outbox Pattern]]).
- Clients need asynchronous mechanisms to determine saga outcomes.

## Concepts Covered

- [[Saga Pattern]] — definitive primary treatment
- [[Eventual Consistency]] — sagas achieve eventual rather than immediate consistency
- [[Outbox Pattern]] — recommended for reliable event publishing within saga steps
- [[Idempotency]] — saga steps must be safe to retry
- [[Distributed Transactions]] — the problem sagas solve

## Quality Notes

Authoritative; Chris Richardson is the original cataloger of microservices patterns. Includes UML sequence diagrams and concrete examples (order/customer credit). This is the reference that essentially defined the microservices community's vocabulary for this pattern.
