---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/domain-driven-design/aggregation/'
  - 'https://awesome-architecture.com/domain-driven-design/domain-driven-design/'
tags:
  - ddd
  - domain-driven-design
  - tactical-design
---
# Aggregate

A cluster of domain objects (entities and value objects) that are treated as a single unit for the purpose of data changes, with one object designated as the **Aggregate Root** that controls all access to and modification of the cluster.

## Problem

Domain objects rarely exist in isolation. An `Order` contains `OrderLines`; a `Customer` has `Addresses`. Without a principled way to group them:

- Invariants (business rules) spanning related objects are hard to enforce.
- Concurrent modifications to related objects can corrupt data.
- Determining transaction boundaries becomes arbitrary.

## Solution / Explanation

An **Aggregate** groups a cluster of entities and value objects into a single consistency boundary. The **Aggregate Root** is the single entry point:

- External objects may only hold references to the Aggregate Root, never to internal entities directly.
- All state changes must go through the Aggregate Root, allowing it to enforce invariants.
- A single database transaction should span at most one aggregate; cross-aggregate consistency uses eventual consistency and [[Domain Event|domain events]].

### Aggregate Root

The root entity that:
1. Has a global identity (ID) that persists outside the aggregate.
2. Enforces all invariants (business rules) for the cluster.
3. Controls the lifecycle of all child entities.
4. Raises domain events when state changes occur.

### Invariants

Business rules that must always hold true within the aggregate boundary. Example: "An order's total cannot exceed the customer's credit limit." The aggregate root checks this rule on every modification.

### Design Heuristics

- **Keep aggregates small.** Large aggregates cause lock contention and load performance issues. Prefer many small aggregates over one large one.
- **Reference other aggregates by ID, not by object.** Cross-aggregate navigation happens outside the aggregate, via repository lookups.
- **Design around true invariants.** Ask: "What must be consistent immediately?" Only put objects that share an invariant into the same aggregate.
- **Use eventual consistency between aggregates.** A command on Aggregate A publishes a domain event; Aggregate B handles it asynchronously.

## Key Components

- **Aggregate Root** — the single point of access and the entity with global identity.
- **Internal Entities** — entities that only make sense within this aggregate; referenced by local ID.
- **Value Objects** — immutable descriptors used within the aggregate (e.g., `Money`, `Address`).
- **Invariants** — rules the root enforces on every state change.
- **Domain Events** — raised by the root to signal state changes to the outside world.

## When to Use

Apply the Aggregate pattern in any domain that has:
- Groups of objects with shared business rules.
- Concurrency concerns around related data.
- Clear transactional boundaries that align with business operations.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Enforces invariants at the correct granularity | Aggregate boundaries are hard to get right initially |
| Clear transactional scope | Large aggregates cause lock contention |
| Protects internal consistency | Cross-aggregate operations require eventual consistency |
| Reduces coupling between unrelated objects | Requires rethinking relational DB schemas |

## Related

- [[Domain-Driven Design]] — the framework this pattern belongs to
- [[Bounded Context]] — aggregates live inside bounded contexts
- [[Value Object]] — immutable components used inside aggregates
- [[Domain Event]] — raised by aggregate roots to communicate changes
- [[Repository Pattern]] — the mechanism for loading and saving aggregates
- [[Event Sourcing]] — aggregate state can be rebuilt from its event history
