---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "https://www.geeksforgeeks.org/hexagonal-architecture-in-java/"
tags:
  - architecture
  - architectural-pattern
  - hexagonal
  - ports-and-adapters
---

# Hexagonal Architecture

An architectural pattern — also called **Ports and Adapters** — in which the application core (domain and use cases) is isolated from external systems through abstract interfaces (ports), with concrete implementations (adapters) plugged in at the boundary.

Introduced by **Alistair Cockburn** (coined 2005, published 2006) to allow an application to be driven equally by users, automated tests, and other programs, and to be developed and tested in isolation from any runtime devices and databases.

## Problem

Applications whose business logic directly depends on HTTP frameworks, databases, or external APIs become hard to test without spinning up infrastructure, and hard to evolve when technology choices change. There is no clean way to run the application with a "test double" for the database or to add a new delivery mechanism (CLI, batch job) without touching core logic. The core is polluted with infrastructure concerns.

## Solution

Draw a clear boundary between the **inside** (application core: domain model and application services) and the **outside** (infrastructure: UI, databases, message brokers, external APIs). Define abstract **ports** at the boundary — interfaces owned by the inside. Build concrete **adapters** on the outside that implement those ports. The core depends only on ports, never on adapters.

The hexagon shape is representational: the faces of the hexagon represent different ports — each can have one or more adapters attached. It emphasizes that there is no "top" or "bottom" of the application — any external actor can interact via a port.

## Key Components / Structure

```
        [UI Adapter]  [Test Adapter]  [REST Adapter]
               ↓             ↓               ↓
          [Driving Port / Inbound Port]
                      ↓
              [Application Core]
              (Domain + Use Cases)
                      ↓
          [Driven Port / Outbound Port]
               ↓             ↓               ↓
         [DB Adapter]  [Email Adapter]  [Kafka Adapter]
```

**Three-layer internal structure:**

| Layer | Contents |
|-------|----------|
| **Domain** | Core business logic; insulated from all external implementation details |
| **Application** | Mediator between domain and framework layers; defines ports |
| **Framework** | Contains implementation specifics (adapters) for external world interaction |

| Term | Meaning |
|------|---------|
| **Port** | Interface defined by the application core — the contract for interaction |
| **Driving / Inbound Port** | Port through which external actors *drive* the application (e.g., `OrderService` interface called by REST controllers) |
| **Driven / Outbound Port** | Port the application uses to reach the outside world (e.g., `OrderRepository`, `EmailSender`) |
| **Primary (Driving) Adapter** | Implements an inbound port; initiates requests. Examples: REST controllers, CLI, web views |
| **Secondary (Driven) Adapter** | Implements an outbound port; responds to application requests. Examples: `PostgresOrderRepository`, Kafka publisher, email gateway |
| **Application Core** | Domain model + use cases — zero dependencies on any adapter |

### Practical Example (Spring Boot)

- `CakeService` (inbound port) — interface defining what the application can do
- `CakeRepository` (outbound port) — interface for data persistence
- `CakeRestController` (primary adapter) — HTTP → CakeService calls
- `CakeRepositoryImpl` (secondary adapter) — CakeRepository → JPA/database

## When to Use

- Applications requiring high testability — the core can be tested with in-memory adapters, no infrastructure needed.
- Systems expected to change delivery mechanisms (add REST alongside existing CLI; add Kafka alongside REST).
- When a clean boundary between domain and infrastructure is a design goal.
- As the structural backbone for [[Clean Architecture]] or [[Domain-Driven Design]] applications.
- When the same use case logic must be exercised by multiple external actors (users, scheduled jobs, test harnesses).

## Trade-offs

**Pros:**
- Application core is fully testable without any real infrastructure.
- Adapters can be swapped without touching business logic (e.g., replace PostgreSQL with DynamoDB — change only the secondary adapter).
- Multiple adapters can implement the same port simultaneously (REST + gRPC + CLI all driving the same use cases).
- Forces explicit, named interfaces at boundaries — improves clarity and maintainability.
- Easy to maintain: new features added to individual layers without affecting others.

**Cons / pitfalls:**
- More indirection and boilerplate than [[MVC Pattern]] or flat [[Layered Architecture]] for simple applications.
- The port/adapter distinction requires discipline to maintain.
- Risk of creating overly thin adapters that just delegate — adding complexity without value.

## Relationship to Clean Architecture

[[Clean Architecture]] is a formalization and extension of Hexagonal Architecture:

| Hexagonal | Clean Architecture |
|-----------|--------------------|
| "Inside" (application core) | Entity and Use Case layers |
| "Outside" (infrastructure) | Interface Adapters and Frameworks layers |
| Ports | Interfaces defined at the Use Case/Interface Adapter boundary |
| Adapters | Concrete implementations in outer layers |

Both enforce the same inward-pointing dependency rule.

## Real-World Usage

- Widely adopted in Java (Spring with interface-based repositories), C# (.NET with DI containers), and TypeScript/Node.js.
- The pattern underlies many enterprise application frameworks' recommended project structure.
- Used when a team needs to write comprehensive unit tests for business logic without a live database.
- Spring Boot applications naturally follow this pattern when using interface-based services and repositories.

## Related

- [[Clean Architecture]] — formalizes Hexagonal's inside/outside split into named concentric layers
- [[Repository Pattern]] — a secondary (driven) adapter implementing an outbound port (data access)
- [[Layered Architecture]] — contrasting style; Hexagonal Architecture replaces strict horizontal layers with a core-and-boundary model
- [[Domain-Driven Design]] — Hexagonal Architecture is the preferred structural pattern for DDD implementations
