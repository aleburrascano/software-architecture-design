---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/vertical-slice-architecture/'
tags:
  - architecture
  - patterns
  - design
  - feature-organization
---
# Vertical Slice Architecture

An architectural organizing principle that structures code around **features** (vertical slices through all technical layers) rather than **technical layers** (horizontal groupings of similar concerns like controllers, services, repositories).

## Problem

Traditional [[Layered Architecture]] organizes code by technical concern: all controllers in one folder, all services in another, all repositories in a third. This creates:

- **High coupling between unrelated features** — the service layer for `OrderManagement` sits next to the service layer for `UserManagement`, creating temptations to share/couple code.
- **Low cohesion within features** — the code for a single feature is scattered across multiple layers/folders.
- **Costly cross-layer changes** — adding a field to an API response requires touching controller, service, repository, DTO, and mapping code across four directories.
- **Friction for new developers** — finding all the code for "how orders work" requires searching multiple directories.

## Solution / Explanation

A **Vertical Slice** groups all code for a single feature — API endpoint, business logic, data access, validation, authorization — in one place. Each feature is a self-contained slice from the top (API) to the bottom (database) of the stack.

```
Layered Architecture:           Vertical Slice Architecture:
┌──────────────────┐            ┌─────────────────────────────┐
│   Controllers    │            │  Features/                  │
├──────────────────┤            │  ├── PlaceOrder/            │
│   Services       │     vs     │  │   ├── PlaceOrderRequest  │
├──────────────────┤            │  │   ├── PlaceOrderHandler  │
│   Repositories   │            │  │   └── PlaceOrderTests    │
└──────────────────┘            │  ├── CancelOrder/           │
                                │  └── GetOrderStatus/        │
                                └─────────────────────────────┘
```

### CQRS Alignment

Vertical slices pair naturally with [[CQRS]]. Each command or query is its own slice with its own handler, validation, and data access code. **No shared service layer** is needed.

### Abstraction by Need

Instead of introducing abstractions upfront (generic repositories, base services), each slice uses the simplest approach that works. Abstractions are introduced only when multiple slices genuinely need to share behavior.

> "Each slice decides for itself how to handle its request. One slice might use CQRS, another might use a transaction script, another might hit the database directly." — Jimmy Bogard

### MediatR and Vertical Slices

In .NET, vertical slices are commonly implemented with **MediatR** — each slice is a `Command` or `Query` object handled by a single `Handler` class. This makes the slice self-contained and discoverable.

## When to Use

- Feature-driven development teams that want high cohesion.
- Microservices or modular monoliths organized by business capability.
- Applications with many independent features that rarely share logic.
- Teams that find traditional layers cause too much coupling.

May not be ideal when:
- Core business logic is genuinely shared across many features (extract to a domain model).
- Strict separation of concerns for testing purposes is required at the layer level.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| High cohesion — all feature code in one place | Risk of duplicating utilities across slices |
| Low coupling between unrelated features | Requires discipline to avoid cross-slice dependencies |
| Easy to understand scope of a feature | Less structure can feel chaotic without conventions |
| Natural fit for CQRS | Shared business rules need explicit extraction |
| Easy to add/remove/modify features | Harder to enforce consistent patterns across slices |

## Related

- [[CQRS]] — commands and queries map naturally to individual slices
- [[Clean Architecture]] — an alternative organizing approach; slices can live within a clean arch structure
- [[Layered Architecture]] — the traditional alternative that vertical slices move away from
- [[Modular Monolith]] — vertical slices can organize features within modules
- [[Domain-Driven Design]] — each slice aligns with a use case in a bounded context
