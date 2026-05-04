---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/onion-architecture/'
tags:
  - architecture
  - design
  - patterns
  - layered
---
# Onion Architecture

A layered architectural style introduced by Jeffrey Palermo that places the domain model at the core, with all dependencies pointing inward — infrastructure, UI, and external concerns depend on the domain, never the reverse.

## Problem

Traditional [[Layered Architecture]] (presentation → business logic → data access) has a fundamental flaw: the business logic layer depends on the data access layer. This creates:
- Tight coupling to database technology.
- Difficulty testing business logic in isolation (must mock/stub data access).
- The database schema often drives the domain model (database-centric design).

## Solution / Explanation

Onion Architecture inverts the dependency: the domain sits at the center, and all outer layers depend on inner layers. Infrastructure (databases, external APIs, frameworks) is pushed to the outermost layer.

```
        ┌─────────────────────────────────────┐
        │  Infrastructure (outermost)         │
        │  ┌─────────────────────────────┐    │
        │  │  Application Services       │    │
        │  │  ┌─────────────────────┐    │    │
        │  │  │  Domain Services    │    │    │
        │  │  │  ┌─────────────┐    │    │    │
        │  │  │  │  Domain     │    │    │    │
        │  │  │  │  Model      │    │    │    │
        │  │  │  │  (Core)     │    │    │    │
        │  │  │  └─────────────┘    │    │    │
        │  │  └─────────────────────┘    │    │
        │  └─────────────────────────────┘    │
        └─────────────────────────────────────┘
              Dependencies point inward →
```

### The Layers

**Domain Model (Core)**
Entities, value objects, and domain interfaces. No dependencies on any framework or external library. This is pure business logic.

**Domain Services**
Domain logic that doesn't fit in a single entity. Uses only domain model objects and interfaces defined in the domain layer.

**Application Services**
Orchestrates use cases by coordinating domain objects and services. Depends on domain interfaces (not implementations). Contains no business logic — only workflow orchestration.

**Infrastructure (Outermost)**
Implementations: database repositories, email senders, file systems, external APIs. Implements interfaces defined by inner layers. Depends on frameworks (EF Core, HTTP clients, etc.).

### Dependency Inversion

The key mechanism: inner layers define **interfaces** (e.g., `IOrderRepository`). Outer layers provide **implementations** (e.g., `SqlOrderRepository`). Dependency injection wires them together at runtime. The domain never references infrastructure code.

### Relationship to Clean and Hexagonal Architecture

All three share the same core principle — domain at center, infrastructure at the outside, dependencies point inward — but differ in layering terminology and emphasis:

| | Onion | Hexagonal (Ports & Adapters) | Clean |
|---|---|---|---|
| Origin | Jeffrey Palermo | Alistair Cockburn | Robert C. Martin |
| Emphasis | Concentric layers | Ports (interfaces) and Adapters (implementations) | Use Cases as first-class citizens |
| Layers | Domain Model → Domain Services → App Services → Infrastructure | Domain → Application → Adapters → Ports | Entities → Use Cases → Interface Adapters → Frameworks |

## When to Use

- Complex domain-rich applications where business logic must be testable in isolation.
- Long-lived applications where the domain model must survive technology changes.
- Environments where domain-driven design principles guide development.
- Teams that need clean separation to support multiple persistence stores.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Domain is fully isolated from infrastructure | More initial structure and boilerplate |
| Business logic is easily unit testable | Mapping between layers (DTOs ↔ Domain objects) adds overhead |
| Technology independence — swap databases without touching the domain | Over-engineering for simple CRUD applications |
| Aligns naturally with DDD | Requires discipline to prevent leaks through layers |

## Related

- [[Clean Architecture]] — very similar; adds explicit Use Cases layer
- [[Hexagonal Architecture]] — equivalent concept; Ports and Adapters terminology
- [[Layered Architecture]] — the traditional alternative with dependencies pointing downward
- [[Domain-Driven Design]] — Onion Architecture is designed to support DDD principles
- [[Dependency Inversion Principle]] — the principle that makes Onion Architecture work
