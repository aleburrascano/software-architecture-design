---
type: topic
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/'
  - 'https://awesome-architecture.com/modular-monolith/'
  - 'https://awesome-architecture.com/vertical-slice-architecture/'
  - 'https://awesome-architecture.com/onion-architecture/'
tags:
  - architecture
  - architectural-styles
  - reference
  - comparison
---
# Architectural Styles Reference

A reference page mapping all architectural styles covered in this wiki, with brief descriptions and guidance on when to choose each.

## Layering & Structuring Styles

| Style | Core Idea | When to Use |
|---|---|---|
| [[Layered Architecture]] | Technical layers: Presentation → Business Logic → Data Access | CRUD apps, simple domains, small teams |
| [[Onion Architecture]] | Domain at center; all dependencies inward; infrastructure at the edge | Domain-rich apps; want to isolate domain from persistence |
| [[Clean Architecture]] | Adds explicit Use Cases to Onion; uncle Bob's variant | Long-lived apps; maximal testability; strong dependency rules |
| [[Hexagonal Architecture]] | Ports (interfaces) and Adapters (implementations); domain in center | Multiple integration points; need to swap storage/messaging easily |
| [[Vertical Slice Architecture]] | Organize by feature, not layer; each feature is a vertical cut | Feature-driven teams; CQRS-heavy; want high cohesion per feature |

## Deployment & Distribution Styles

| Style | Core Idea | When to Use |
|---|---|---|
| [[Modular Monolith]] | Single deployable; strict internal module boundaries | New projects; smaller teams; microservices not yet justified |
| [[Microservices Architecture]] | Independent services per business capability; own DB each | Scale, team autonomy, polyglot tech needs justified the cost |
| [[Service-Oriented Architecture]] | Services share infrastructure (ESB); coarser-grained than microservices | Enterprise integration; legacy modernization |

## Communication & Event Styles

| Style | Core Idea | When to Use |
|---|---|---|
| [[Event-Driven Architecture]] | Services communicate via events; loose temporal coupling | Audit logs, async processing, integration between systems |
| [[CQRS]] | Separate read model from write model | High-scale reads; complex query requirements; event sourcing |
| [[Event Sourcing]] | State = log of events; never update, only append | Audit trail required; temporal queries; complex domain history |
| [[Actor Model Architecture]] | State in actors; communicate via messages; supervision hierarchies | High concurrency; distributed stateful entities; IoT |

## Domain-Centric Approaches

| Style | Core Idea | When to Use |
|---|---|---|
| [[Domain-Driven Design]] | Model domain expertly; bounded contexts; ubiquitous language | Complex business domains; need tight alignment with domain experts |
| [[Vertical Slice Architecture]] | Features as slices; aligns with DDD use cases | Feature-oriented teams; good CQRS/MediatR fit |

## Architecture Selection Guide

### Choosing Your Deployment Style

```
New project?
├── Small/medium domain + small team → [[Modular Monolith]]
│   └── When genuinely outgrown → [[Microservices Architecture]]
└── Already operating microservices → [[Microservices Architecture]]
    └── Legacy migrating → [[Strangler Fig Pattern]]
```

### Choosing Your Internal Code Organization

```
What's most important?
├── Testability + DDD richness → [[Clean Architecture]] or [[Hexagonal Architecture]]
├── Domain isolation + DDD → [[Onion Architecture]]
├── Feature cohesion + speed → [[Vertical Slice Architecture]]
└── Simplest that works → [[Layered Architecture]]
```

### Choosing Your Consistency Model

```
Need distributed transactions?
├── Strong consistency required → [[Distributed Transactions]] (2PC)
└── Can tolerate eventual consistency → [[Saga Pattern]] + [[Outbox Pattern]]
    └── Understand → [[CAP Theorem]] + [[Eventual Consistency]]
```

## Related

- [[Architectural Styles Comparison]] — existing comparison page
- [[Design Principles Overview]] — principles that guide architectural decisions
- [[Cloud Design Patterns Overview]] — operational patterns for distributed systems
- [[Microservices Patterns Overview]] — patterns for microservices specifically
