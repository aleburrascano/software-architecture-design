---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/sairyss-domain-driven-hexagon.md'
source_date: ''
source_author: Sairyss (GitHub)
tags:
  - source
  - ddd
  - hexagonal-architecture
  - ports-adapters
  - domain-model
  - typescript
  - clean-architecture
---
# Domain-Driven Hexagon - Sairyss

GitHub repository providing a complete DDD implementation using Hexagonal Architecture (Ports & Adapters) with TypeScript code examples.

## Summary

This repository demonstrates how DDD tactical patterns integrate with Hexagonal Architecture by providing a working TypeScript codebase. The central domain model contains Aggregates, Value Objects, Domain Events, and Repositories defined as interfaces. Driving ports (HTTP controllers, CLI handlers) initiate domain interactions, while driven ports (database adapters, messaging adapters) implement the infrastructure interfaces defined by the domain.

The code organization makes the dependency rule concrete: all dependencies point inward toward the domain. Framework code, ORM code, and messaging code exist exclusively in adapters, never in the domain model. This enables domain logic to be tested in complete isolation by substituting in-memory adapters without any framework startup overhead.

The repository covers the full lifecycle of a domain operation: command validation at the application service layer, domain logic execution in aggregates, domain event publication, and persistence through repository interfaces. It provides a practical template that teams can adapt for their own DDD + Hexagonal implementations.

## Key Arguments

- DDD tactical patterns fit naturally into hexagonal architecture's layering model
- The domain model should have zero framework dependencies to preserve portability and testability
- Ports define the domain's needs expressed as interfaces; adapters implement those needs against specific infrastructure
- Adapters can be swapped (e.g., Postgres to MongoDB) without touching any domain logic
- Domain logic can be tested in isolation by substituting lightweight in-memory adapters for production adapters

## Concepts Covered

- [[Hexagonal Architecture]] — DDD integration; primary practical reference implementation
- [[Domain-Driven Design]] — hexagonal implementation of tactical patterns
- [[Bounded Context]] — module boundaries aligned with hexagonal layers
- [[Clean Architecture]] — layering principles shared with hexagonal approach

## Quality Notes

Excellent practical reference with TypeScript code. Best available source for DDD+Hexagonal integration patterns in a modern language. Well-maintained and widely referenced.
