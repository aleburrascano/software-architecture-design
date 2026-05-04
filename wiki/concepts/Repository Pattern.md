---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "https://www.geeksforgeeks.org/system-design/repository-design-pattern/"
tags:
  - architecture
  - design-pattern
  - data-access
  - structural
---

# Repository Pattern

A design pattern that mediates between the domain model and the data mapping layer by presenting a collection-like interface for accessing domain objects, hiding the underlying data access mechanism from business logic.

## Problem

Without a repository, business logic is directly coupled to data access technology (SQL queries, ORM calls, file I/O). This makes the domain logic hard to test in isolation, ties the codebase to a specific storage technology, and scatters data access concerns throughout the application. Testing requires a live database, and switching storage backends requires touching business code.

## Solution

Introduce a **repository** — an abstraction that looks like an in-memory collection of domain objects from the caller's perspective. The repository encapsulates all logic for querying, persisting, and retrieving objects from the underlying store. The domain and application layers depend on the repository interface, not on any concrete data access mechanism.

Think of it as a front desk at a library: the caller asks for a book by title or ISBN; the librarian handles the physical retrieval. The caller has no knowledge of shelf locations, catalog systems, or storage formats.

## Key Components / Structure

| Component | Role |
|-----------|------|
| **Repository Interface** | Abstract contract defining operations (e.g., `findById`, `save`, `delete`, `findAll`) |
| **Concrete Repository** | Implementation backed by a specific store (SQL, NoSQL, in-memory) — e.g., `InMemoryProductRepository`, `PostgresOrderRepository` |
| **Domain Entity / Aggregate** | The object type the repository manages |
| **Unit of Work** (optional) | Tracks changes across multiple repository operations and commits them atomically |

### Typical interface methods
- `findById(id)` — retrieve a single entity
- `save(entity)` — insert or update
- `delete(entity)` — remove
- `findAll()` / `findBy*(criteria)` — query collections

### Structure example

```
[Application / Domain Layer]
        |
  [Repository Interface]   ← contract defined here, owned by the domain
        |
  [Concrete Repository]    ← SQL, NoSQL, or in-memory implementation
        |
  [Database / Storage]
```

The concrete repository can be swapped (e.g., `InMemoryProductRepository` in tests, `DatabaseProductRepository` in production) without changing the application code.

## When to Use

- Any application with a non-trivial domain model where business logic must be testable without a live database.
- When storage technology may need to change (e.g., moving from SQL to NoSQL, or between database vendors).
- As a supporting pattern in [[Clean Architecture]], [[Hexagonal Architecture]], or [[Domain-Driven Design]].
- When the same domain logic must work against multiple backends (production DB vs. in-memory test fixture).
- APIs, microservices, and complex systems where maintainable data access is a priority.

**Not ideal for:** Small applications where the abstraction layer adds complexity without benefit; simple scripts or one-off data utilities.

## Trade-offs

**Pros:**
- Clean separation of domain logic from data access concerns.
- Domain logic is easily unit-tested by substituting an in-memory repository.
- Storage technology can be swapped without touching business logic.
- Centralizes query logic, reducing duplication.
- Enables data migration between storage systems without application changes.

**Cons / pitfalls:**
- Adds an abstraction layer — overhead for simple CRUD applications.
- Can encourage generic "God repositories" that expose too many query methods.
- Leaky abstractions: complex queries sometimes push ORM-specific concerns into the repository interface.
- Not a silver bullet for performance — N+1 query problems can still occur if repository usage is naive.
- Requires setup time for interfaces and concrete implementations.

## Real-World Usage

- **Online stores:** Product display fetches product entities via `ProductRepository.findById()`; the repository handles database queries without exposing SQL to the presentation layer.
- Ubiquitous in layered enterprise applications: Java Spring Data (`JpaRepository`), .NET Entity Framework (`IRepository<T>`), Rails Active Record (acts as a repository).
- Central building block in DDD: repositories are defined per Aggregate Root — one `OrderRepository`, one `CustomerRepository`, not one universal `DataRepository`.
- In [[Clean Architecture]] and [[Hexagonal Architecture]], the repository interface is defined in the inner (use case) layer and implemented by an outer-layer adapter.

## Related

- [[Domain-Driven Design]] — repositories are a core DDD tactical building block (one per aggregate root)
- [[Clean Architecture]] — repositories implement the data access boundary between domain and infrastructure layers
- [[Hexagonal Architecture]] — repository implementations are "driven (secondary) adapters" implementing an outbound port
- [[Layered Architecture]] — repository sits in the persistence/data layer
- [[CQRS]] — query handlers may use read-optimized repository variants; command handlers use write repositories
