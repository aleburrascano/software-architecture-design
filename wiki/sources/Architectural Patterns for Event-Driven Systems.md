---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Architectural Patterns for Event-Driven Systems.md'
source_date: ''
source_author: Gravitee Team
tags:
  - source
  - event-driven
  - eda
  - choreography
  - orchestration
  - event-notification
  - event-carried-state-transfer
  - pub-sub
  - cqrs
  - kafka
---
# Architectural Patterns for Event-Driven Systems

Comprehensive catalog of EDA patterns with decision framework for when to use each, including Kafka as an API Gateway vehicle for event-driven microservices.

## Summary

The article provides a structured taxonomy of the main event-driven architecture patterns: Event Notification, Event-Carried State Transfer (ECST), Event Sourcing, CQRS, Publish-Subscribe, and the Choreography vs. Orchestration decision axis. These patterns are not interchangeable; each serves a different decoupling and communication need, and choosing incorrectly leads to unnecessary complexity or insufficient capability.

Event Notification and ECST differ in intent and payload: Event Notification signals that something happened (minimal payload, receiver fetches state if needed) while ECST carries the full state delta in the event itself, eliminating the need for downstream services to call back. Event Sourcing goes further, changing the persistence model entirely — state is derived from the event log rather than stored as current state. CQRS complements ES by providing separate optimized read models built from event projections.

The choreography vs. orchestration decision is framed as a trade-off between autonomy and visibility: choreography keeps services decoupled (each reacts to events independently) at the cost of difficult-to-trace business processes; orchestration provides explicit process visibility via a central coordinator at the cost of tighter coupling. The article includes Kafka-specific patterns for implementing async microservice communication, treating Kafka as an API gateway alternative for event-driven architectures.

## Key Arguments

- EDA patterns are not interchangeable; Event Notification and ECST serve fundamentally different decoupling needs
- Event Notification: minimal payload, receiver fetches state; ECST: full state in event, no callback needed
- Event Sourcing changes the persistence model entirely; it is not just a messaging pattern
- CQRS and Event Sourcing complement each other naturally: ES provides the write model, CQRS projections build read models
- Choreography favors service autonomy; orchestration favors business process visibility — the choice depends on observability requirements
- Kafka patterns enable async microservice communication as an alternative to synchronous REST-based API gateways

## Concepts Covered

- [[Event-Driven Architecture]] — comprehensive taxonomy of EDA patterns
- [[Event Notification Pattern]] — primary treatment; definition and use cases
- [[Event-Carried State Transfer]] — primary treatment; definition and contrast with Event Notification
- [[CQRS]] — complements EDA; read model separation from event streams
- [[Event Sourcing]] — ES in EDA context; persistence model change
- [[Choreography vs Orchestration]] — decision framework; trade-offs and when to use each
- [[Publish-Subscribe Pattern]] — messaging backbone for EDA
- [[Apache Kafka]] — implementation vehicle for EDA patterns

## Quality Notes

One of the best single-article treatments of EDA patterns. The decision framework distinguishing when to use each pattern is particularly strong. Recommended as the primary reference for EDA pattern taxonomy.
