---
type: person
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://microservices.io/about.html
tags:
  - people
  - software-architecture
---

# Chris Richardson

Software architect, Java Champion, and serial entrepreneur who authored *Microservices Patterns* and built microservices.io — the primary reference for microservices pattern languages including the Saga pattern, CQRS, and service decomposition.

## Background

Chris Richardson is a recognised Java Champion and JavaOne rock star speaker. He founded the original CloudFoundry.com, an early Java Platform-as-a-Service, predating Pivotal's Cloud Foundry. He later founded Eventuate.IO, a platform providing infrastructure for event-sourcing and saga-based microservices. He works as a consultant and trainer, helping organisations modernise legacy systems and adopt microservices architecture without repeating the mistakes of monolithic systems in a distributed context.

## Key Contributions

- **[[Saga Pattern]]** — documented and systematised the Saga pattern for managing distributed transactions across microservices without two-phase commit; distinguished choreography-based sagas (event-driven) from orchestration-based sagas (central coordinator)
- **Microservices Pattern Language** — built the comprehensive pattern language at microservices.io, covering decomposition patterns (by subdomain, by business capability), data patterns, communication patterns, observability patterns, and deployment patterns
- **Distributed Data Management Patterns** — API Composition, CQRS (in the microservices context), and event sourcing as mechanisms for querying data that spans service boundaries
- **Service Decomposition Methodologies** — concrete guidance on how to identify service boundaries aligned with DDD bounded contexts and business capabilities
- **Microservice Chassis / Service Template** — patterns for standardising cross-cutting concerns (logging, health checks, distributed tracing) across many services
- **Eventuate** — open-source platform embodying his patterns for event sourcing and sagas in practice

## Key Works

- *POJOs in Action: Developing Enterprise Applications with Lightweight Frameworks* (Manning, 2006) — early advocacy for lightweight Java
- *Microservices Patterns: With Examples in Java* (Manning, 2018; 2nd ed. in progress)
- microservices.io — a living pattern catalogue with hundreds of patterns, articles, and anti-patterns
- Virtual bootcamp on distributed data patterns for microservices

## Influence

Richardson's *Microservices Patterns* filled a gap left by the high-level microservices enthusiasm of the mid-2010s — it provided concrete, actionable patterns for the hard problems: data consistency, distributed transactions, and inter-service queries. The Saga pattern he popularised is now a standard architectural tool in any event-driven microservices system. His warning about avoiding "a modern legacy system — a new architecture with the same old problems" has become a common framing for thoughtful microservices adoption. microservices.io is one of the most-referenced architecture sites in the industry.

## Quotes

> "The goal of using microservices is not to reduce the size of services; it's to enable the rapid, frequent, and reliable delivery of large, complex applications."

> "Don't create a modern legacy system — a new architecture with the same old problems."

## Related

- [[Saga Pattern]] — systematised and popularised by Richardson
- [[CQRS]] — documented by Richardson in the microservices context
- [[Event Sourcing]] — Richardson's Eventuate platform embodies this pattern
- [[Microservices Architecture]] — Richardson's primary domain; microservices.io is the canonical pattern reference
