---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Microservices Pattern- Pattern- Event Sourcing.md'
source_date: ''
source_author: Chris Richardson / microservices.io
tags:
  - source
  - event-sourcing
  - microservices
  - atomic-updates
  - snapshot
  - saga
  - outbox
  - chris-richardson
---
# microservices.io - Event Sourcing Pattern

Chris Richardson's authoritative microservices.io entry framing Event Sourcing as the solution to the dual-write problem in microservices.

## Summary

Richardson frames Event Sourcing first and foremost as a solution to a concrete microservices problem: the dual-write problem. In a microservice architecture, an operation typically needs to update a database and publish a domain event — but doing both atomically without distributed transactions (2PC) is the fundamental challenge. ES solves this by making the event log the source of truth: writing the event IS the write operation, and the current state is derived from replaying events. There is no separate database to keep in sync.

The persistence model is aggregate-centric: each aggregate (in the DDD sense) is persisted as an ordered sequence of domain events. Reconstructing current state requires replaying all events, which introduces a performance concern addressed by snapshots — periodic state checkpoints that allow replay from a recent snapshot rather than from the beginning of time.

The article covers ES's integration with other microservices patterns. CQRS is the natural companion: ES handles the write side (event log), CQRS projections build optimized read models. Saga patterns use ES domain events as the publication mechanism for inter-service choreography. The Outbox Pattern is noted as an alternative for teams that need dual-write atomicity without adopting full Event Sourcing.

## Key Arguments

- ES solves the dual-write problem: writing an event atomically captures both the state change and the publishable notification, without requiring 2PC
- Events are the authoritative source of truth; current state is a derived projection, not the primary record
- Event replay enables temporal queries — any past state can be reconstructed by replaying events up to a point in time
- Snapshots reduce replay cost: periodic state checkpoints allow aggregates with long event histories to be loaded efficiently
- ES naturally integrates with CQRS (read model projections) and Sagas (events as inter-service notifications)
- The Outbox Pattern is an alternative for atomic dual-write without full ES adoption

## Concepts Covered

- [[Event Sourcing]] — comprehensive treatment in microservices context; primary reference
- [[CQRS]] — ES+CQRS integration; how projections build read models from the event log
- [[Saga Pattern]] — ES as the event publication mechanism for saga choreography
- [[Outbox Pattern]] — alternative to ES for dual-write atomicity
- [[Domain Event]] — ES domain events as the atomic unit of state change

## Quality Notes

Authoritative reference from Chris Richardson, the leading microservices patterns authority. microservices.io is the canonical catalog for microservices patterns. This is the primary recommended source for Event Sourcing in a microservices context.
