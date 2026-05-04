---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What is Software Architecture A Comprehensive Guide.md"
  - "raw/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/Software architecture 1.md"
tags:
  - architecture
  - architectural-style
  - layered
  - n-tier
---

# Layered Architecture

An architectural style that partitions an application into horizontal layers, each with a distinct category of responsibility, where components in a higher layer call components in a lower layer — but not the reverse.

Also called: **N-Tier Architecture**, **Multi-tier Architecture**.

## Problem

Without structure, business logic, UI code, and database queries intermingle in the same codebase. Any change to one concern can inadvertently break another. Large codebases become increasingly difficult to understand, test, and maintain as concerns accumulate. A change to the database schema ripples into presentation code; UI changes force regression testing of business rules.

## Solution

Divide the application into **horizontal layers**, each responsible for one category of function. A request flows downward through layers; each layer depends only on the layer immediately below it and exposes a well-defined interface to the layer above. This is a *horizontal* separation of concerns — contrasted with the *vertical* separation of [[Microservices Architecture]].

### Common layering schemes

**Three-layer (most common for enterprise web apps):**
1. **Presentation** (Web / API) — handles HTTP requests, routes to the service layer, formats responses.
2. **Business Logic** (Application / Service) — orchestrates use cases, enforces business rules.
3. **Data / Persistence** — reads from and writes to the database; may use [[Repository Pattern]].

**Four-layer (richer domain separation):**
1. Presentation
2. Business Logic / Application Services
3. Services (external integrations, messaging)
4. Data / Persistence

**Tiers vs. Layers:** When layers are deployed as separate, independently runnable processes (e.g., web server, app server, database server), they are called **tiers**. Tiers enable independent horizontal scaling of each layer.

## Key Components / Structure

```
[Presentation Layer]       ← HTTP requests / UI
        ↓
[Business Logic Layer]     ← Use cases, domain rules
        ↓
[Service Layer] (optional) ← External APIs, messaging
        ↓
[Persistence Layer]        ← DB reads/writes
```

**Dependency rule:** Each layer may only depend on layers below it. Components within a layer may only call peers in the same or lower layers — never higher. This rule is what prevents concern bleed.

## When to Use

- Enterprise applications with clearly separable concerns (web apps, CRUD-heavy business systems).
- Systems where a single team owns the full stack and independent deployment by domain is not a priority.
- Applications with significant business logic that benefits from encapsulation away from the UI.
- Starting point for any new application before complexity warrants a more sophisticated style.
- When the team is familiar with the pattern and organizational/deployment simplicity is valued.

## Trade-offs

**Pros:**
- Clear separation of concerns — developers can focus on one layer without knowing implementation details of others.
- Improved testability — business logic can be tested independently of the UI or database.
- Higher maintainability and engineering velocity.
- Widely understood — nearly universal in enterprise development.

**Cons / pitfalls:**
- **Sinkhole anti-pattern:** Requests pass through layers without adding value — a request flows from Presentation → Business → Persistence and back with no real work being done at intermediate layers. Adds overhead without benefit.
- Does not address scaling by domain — the whole application must be scaled together.
- Business logic can "leak" into the wrong layer if architectural discipline is not maintained.
- Monolithic deployment — all layers ship together; a change in any layer requires redeploying the whole app.
- Can become a "Big Ball of Mud" as teams grow and discipline erodes.

## Real-World Usage

- **Online shopping:** Presentation layer receives REST calls; Service layer processes orders and validates stock; Persistence layer reads/writes the database — each independently modifiable.
- Java Spring applications: `@Controller` / `@Service` / `@Repository` annotations map directly to the three-layer model.
- The standard starting architecture for most enterprise web applications before domain complexity grows.
- Appropriate starting point before domain complexity warrants [[Clean Architecture]] or [[Domain-Driven Design]].

## Related

- [[Microservices Architecture]] — vertical slicing contrasted with layered horizontal slicing; microservices often implement layering internally
- [[Clean Architecture]] — extends layered thinking with strict inward-only dependency rules and domain at the center; a formal evolution of the layered style
- [[Hexagonal Architecture]] — another evolution of layered thinking, emphasizing ports and adapters over strict layers
- [[MVC Pattern]] — an application-level pattern that typically lives within the presentation and business logic layers
- [[Repository Pattern]] — the persistence layer's standard abstraction in layered architectures
- [[Domain-Driven Design]] — DDD's layered architecture places the domain model at the core
