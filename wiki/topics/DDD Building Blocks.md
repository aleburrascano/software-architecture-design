---
type: topic
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/domain-driven-design/domain-driven-design/'
tags:
  - ddd
  - domain-driven-design
  - tactical-design
  - strategic-design
---
# DDD Building Blocks

An overview of the tactical and strategic patterns that make up Domain-Driven Design, showing how they fit together to form a coherent approach to modeling complex business domains.

## Two Levels of DDD

Domain-Driven Design operates at two levels:

- **Strategic Design** — how to decompose a large system into manageable, well-bounded parts.
- **Tactical Design** — the building blocks for implementing rich domain models within those parts.

## Strategic Design Patterns

### Bounded Context
The primary strategic pattern. See [[Bounded Context]].

Defines the boundary within which a specific domain model and ubiquitous language apply. A large system is divided into multiple bounded contexts, each with its own model.

### Context Map
Documents how bounded contexts relate: which team is upstream/downstream, how data flows, and what integration pattern (Shared Kernel, ACL, Conformist, etc.) governs each relationship.

### Ubiquitous Language
The shared vocabulary developed collaboratively between domain experts and developers. Used consistently in conversations, code, tests, and documentation within a bounded context.

### Subdomain Types
- **Core domain** — the key differentiator; deserves the most investment and DDD rigor.
- **Supporting subdomain** — necessary but not differentiating; can be built in-house with less rigor.
- **Generic subdomain** — commodity; buy or use open-source (auth, billing, email).

## Tactical Design Patterns

### Entity
An object defined by a continuous identity that persists over time. Example: `Customer`, `Order`. Two entities with the same data but different IDs are not the same entity.

### Value Object
See [[Value Object]]. An immutable object defined entirely by its attributes; no identity. Example: `Money`, `Address`, `EmailAddress`. Two value objects with identical attributes are equal.

### Aggregate
See [[Aggregate]]. A cluster of entities and value objects with a single Aggregate Root that enforces invariants. The unit of transactional consistency. Example: `Order` aggregate containing `OrderLines`.

### Domain Service
A stateless service that encapsulates domain logic that doesn't naturally belong to any single entity or value object. Example: `TransferService.Transfer(fromAccount, toAccount, amount)`.

### Domain Event
See [[Domain Event]]. An immutable record of something significant that happened. Raised by aggregate roots; used to decouple domain from application concerns and to integrate across bounded contexts.

### Repository
See [[Repository Pattern]]. An abstraction over persistence that allows the domain to load and save aggregates without knowing about the underlying database technology.

### Application Service
Orchestrates use cases by coordinating aggregates, domain services, and repositories. Contains no business logic — only workflow. Example: `OrderApplicationService.PlaceOrder(command)`.

### Factory
Encapsulates complex aggregate construction logic that does not belong in the aggregate's constructor.

## How the Building Blocks Fit Together

```
Bounded Context
└── Aggregate Root (e.g., Order)
    ├── Entities (e.g., OrderLine)
    ├── Value Objects (e.g., Money, Address)
    └── raises Domain Events (e.g., OrderPlaced)

Application Service
├── loads Aggregate from Repository
├── calls Aggregate methods (which enforce invariants)
├── collects Domain Events
└── dispatches Domain Events (→ integration events → other contexts)
```

## Discovery Techniques

| Technique | Level | Purpose |
|---|---|---|
| [[Event Storming]] | Strategic + Tactical | Discover domain events, commands, aggregates, bounded context boundaries |
| Domain Storytelling | Strategic | Discover workflows through narrative user stories |
| Bounded Context Canvas | Strategic | Define and document a bounded context |

## Related

- [[Domain-Driven Design]] — the overarching framework
- [[Bounded Context]] — strategic pattern; primary decomposition unit
- [[Aggregate]] — tactical pattern; transactional boundary
- [[Value Object]] — tactical pattern; immutable descriptors
- [[Domain Event]] — tactical pattern; cross-cutting communication
- [[Event Storming]] — discovery technique
- [[Microservices Architecture]] — bounded contexts often become service boundaries
