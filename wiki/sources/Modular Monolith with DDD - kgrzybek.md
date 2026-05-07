---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/kgrzybek-modular-monolith-with-ddd.md'
source_date: ''
source_author: Kamil Grzybek
tags:
  - source
  - modular-monolith
  - ddd
  - bounded-context
  - module-organization
  - intra-service-communication
  - cqrs
---
# Modular Monolith with DDD - kgrzybek

Complete working example of a Modular Monolith architecture implementing DDD bounded contexts, demonstrating that DDD modularity does not require microservices.

## Summary

This repository challenges the assumption that applying DDD strategic design requires decomposing into microservices. It provides a complete working example where each DDD bounded context maps to a module within a single deployable process. Each module owns its persistence schema, domain model, and API surface, achieving the same logical separation that microservices provide without the distributed systems overhead.

Inter-module communication uses in-process domain events rather than direct method calls across module boundaries. This maintains the decoupling that DDD prescribes — modules do not reference each other's internal types — while avoiding the latency, reliability challenges, and operational complexity of remote procedure calls or message brokers. The event-based communication pattern also makes the communication contract explicit and auditable.

The architecture demonstrates a valid evolutionary path: start with a well-structured Modular Monolith, and if specific modules need independent scaling or deployment, extract them to microservices when the operational need justifies the cost. The shared physical database with logical schema separation enables this later extraction without requiring data migration upfront.

## Key Arguments

- Modular Monolith + DDD achieves the modularity benefits of microservices without distributed systems complexity
- Bounded contexts map directly to modules; each module owns its own persistence schema and domain model
- In-process domain events maintain cross-module decoupling without the overhead of remote messaging
- Logical schema separation (shared physical DB, separate schemas per module) enables future microservice extraction
- This architecture is a valid intermediate step between an unstructured monolith and a microservices system

## Concepts Covered

- [[Modular Monolith]] — DDD integration pattern; primary reference implementation
- [[Bounded Context]] — module boundaries in a monolith context
- [[Domain-Driven Design]] — applied to monolith architecture
- [[CQRS]] — module-level read/write separation within the monolith
- [[Domain Event]] — inter-module communication mechanism

## Quality Notes

Authoritative reference for the Modular Monolith + DDD combination. Kamil Grzybek is a recognized authority on this architecture. The accompanying blog post series provides deeper rationale than the code alone.
