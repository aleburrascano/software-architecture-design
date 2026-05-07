---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/articles/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/articles/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html"
tags:
  - architecture
  - architectural-pattern
  - clean-architecture
  - uncle-bob
---

# Clean Architecture

An architectural pattern proposed by Robert C. Martin ("Uncle Bob") that organizes a system into concentric layers with a strict **Dependency Rule**: source code dependencies may only point inward, toward higher-level policies — never outward toward frameworks, databases, or UI.

Uncle Bob synthesizes several prior architectural approaches — Hexagonal Architecture, Onion Architecture, Screaming Architecture, DCI, and BCE — into a unified model. All share the same core objective: **separation of concerns through layered architecture** with the domain at the center.

## Problem

Systems where business logic depends on frameworks, databases, or UI technologies become hostage to those technologies. Upgrading the database, switching web frameworks, or adding a new delivery mechanism requires touching the core of the application. Tests must spin up web servers and databases because business logic can't be exercised in isolation. The application becomes increasingly fragile as details leak into domain logic.

## Solution

Place the **domain model** (enterprise business rules) at the center. Surround it with application use cases. The outer layers — interface adapters, frameworks, databases, UI — depend on inner layers but inner layers never depend on outer ones. Infrastructure details are **plugins** to the core.

The result: a system that is independent of frameworks, testable without external components, UI-agnostic, and database-agnostic.

## Key Components / Structure

### The Concentric Layers (innermost to outermost)

1. **Entities** — The innermost layer. Enterprise-wide business rules and data structures. Independent of any application. These are the core domain objects — highest-level policies, least likely to change due to external factors.

2. **Use Cases** — Application-specific business rules. Orchestrate the flow of data to and from Entities to implement the system's use cases. Remain isolated from external concerns like databases and UI frameworks.

3. **Interface Adapters** — Converts data between the format convenient for Use Cases/Entities and the format required by external agents. Contains controllers, presenters, views, gateways, and database-specific SQL code. MVC architecture lives here.

4. **Frameworks & Drivers** — The outermost layer: web frameworks, databases, UI, external APIs. Minimal code here beyond necessary integration glue — all details live here.

```
        ┌──────────────────────────────┐
        │     Frameworks & Drivers     │
        │  ┌────────────────────────┐  │
        │  │  Interface Adapters    │  │
        │  │  ┌──────────────────┐  │  │
        │  │  │   Use Cases      │  │  │
        │  │  │  ┌────────────┐  │  │  │
        │  │  │  │  Entities  │  │  │  │
        │  │  │  └────────────┘  │  │  │
        │  │  └──────────────────┘  │  │
        │  └────────────────────────┘  │
        └──────────────────────────────┘
           Dependencies point INWARD →
```

### The Dependency Rule

> "Source code dependencies can only point inwards." — Robert C. Martin

No inner circle may reference names, functions, classes, or data structures from any outer circle. This ensures business logic remains independent of external tools and frameworks.

Consequences:
- Entities know nothing about Use Cases.
- Use Cases know nothing about Controllers or databases.
- Interfaces (ports) defined in inner layers are implemented (adapted) by outer layers — classic Dependency Inversion.

### Crossing Boundaries

When layers must interact, the **Dependency Inversion Principle** resolves the contradiction: inner layers call interfaces rather than concrete outer-layer implementations, maintaining inward-pointing dependencies despite outward control flow.

Only **simple data structures** — not database rows or framework entities — should cross layer boundaries, preventing coupling violations.

## When to Use

- Applications with significant, long-lived business logic that must be testable independently of infrastructure.
- Systems likely to undergo technology changes (database migration, new API delivery channel) over their lifetime.
- Teams practicing [[Domain-Driven Design]] who want a formal layer structure.
- Projects where testability and maintainability are first-class constraints.
- Enterprise Java, C#, and TypeScript/Node.js applications with complex domains.

## Trade-offs

**Pros:**
- Business logic is fully independent of frameworks, databases, and UI — the core can be tested without any infrastructure.
- Adding a new delivery mechanism (e.g., CLI alongside REST API) requires only a new outer-layer adapter.
- Long-term flexibility: databases, ORMs, and frameworks can be swapped without touching business logic.
- Encourages explicit use cases — each application capability is a named, testable object.
- All key system qualities follow: framework-independent, testable, UI-independent, database-independent, external-agency-independent.

**Cons / pitfalls:**
- More upfront structure than a simple [[MVC Pattern]] or [[Layered Architecture]].
- Over-engineering risk for simple CRUD applications — the full concentric model adds ceremony without benefit.
- Requires disciplined adherence to the Dependency Rule; drifts quickly without team-wide buy-in.
- More code and indirection layers can make simple operations feel verbose.

## Relationship to Other Patterns

Clean Architecture synthesizes several earlier ideas:

| Architecture | Relationship |
|-------------|-------------|
| **[[Hexagonal Architecture]]** | Direct precursor — Clean Architecture formalizes the same inside/outside dependency inversion into explicitly named concentric layers |
| **Onion Architecture** | Same inward-dependency rule, slightly different layer naming |
| **Screaming Architecture** | Emphasizes that the architecture should "scream" the application's intent — use cases are first-class citizens |
| **[[Domain-Driven Design]]** | Clean Architecture's Entities and Use Cases map closely to DDD's Domain Model and Application Services |

## Real-World Usage

- Promoted through Robert C. Martin's blog post *The Clean Architecture* (2012) and book *Clean Architecture* (2017).
- Widely adopted in enterprise Java, C#, and TypeScript/Node.js communities.
- Khalil Stemmler's work demonstrates Clean Architecture applied to Node.js DDD applications at scale.
- Key learning resource cited across the full-stack software design roadmap.

## Related

- [[Hexagonal Architecture]] — the direct precursor; ports/adapters maps to Clean Architecture's interface adapter layer
- [[Layered Architecture]] — Clean Architecture is an evolution of layered thinking with strict inward-only dependencies
- [[Domain-Driven Design]] — Clean Architecture provides the layer structure within which DDD tactical patterns operate
- [[Repository Pattern]] — repositories implement the data gateway interface defined in the Use Case / Interface Adapter layer
- [[MVC Pattern]] — MVC sits within the Interface Adapters layer in Clean Architecture
