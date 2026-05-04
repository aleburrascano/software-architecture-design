---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/domain-driven-design/value-objects/'
tags:
  - ddd
  - domain-driven-design
  - tactical-design
---
# Value Object

An immutable domain object that is defined entirely by its attributes, has no identity of its own, and encapsulates domain concepts (with their associated validation and behavior) that would otherwise be represented as raw primitives.

## Problem

Code that uses raw primitive types (`string`, `int`, `decimal`) for domain concepts like `Money`, `EmailAddress`, or `DateRange` suffers from **primitive obsession**:

- The same primitive can represent different things interchangeably, causing bugs (e.g., passing an amount in USD where an amount in EUR is expected).
- Validation logic is scattered across the codebase rather than co-located with the concept.
- Equality comparisons must compare multiple fields manually everywhere.

## Solution / Explanation

A **Value Object** wraps one or more primitives into a named domain type that:

1. **Is immutable** — once created, it cannot be changed. Any "modification" creates a new instance.
2. **Is compared by value** — two Value Objects with the same attributes are equal, regardless of memory address.
3. **Has no identity** — there is no ID; the values *are* the identity.
4. **Encapsulates behavior** — methods like `Money.Add()`, `DateRange.Contains()` live on the object.
5. **Self-validates** — the constructor rejects invalid states; an instance in memory is always valid.

### Value Object vs. Entity

| | Entity | Value Object |
|---|---|---|
| Identity | Has a persistent ID | No ID; equal when attributes are equal |
| Mutability | Mutable (state changes over time) | Immutable |
| Lifecycle | Tracked through state changes | Replaced, not modified |
| Examples | `Order`, `Customer`, `Product` | `Money`, `Address`, `DateRange`, `EmailAddress` |

### Solving Primitive Obsession

Instead of:
```
void Transfer(decimal amount, string currency, string accountId) { ... }
```
Use:
```
void Transfer(Money amount, AccountId accountId) { ... }
```

The type system enforces domain constraints and makes illegal states unrepresentable.

## Key Components

- **Immutable state** — all fields are set in the constructor and never changed.
- **Value equality** — `Equals()` and `GetHashCode()` compare by value (not reference).
- **Factory/constructor validation** — invalid states are rejected at creation time.
- **Domain behavior** — operations that make sense for the concept (`Money.Add`, `Percentage.Of`).

## When to Use

- Any domain concept characterized by its attributes rather than its identity.
- When you find the same combination of fields being validated and used together repeatedly.
- To replace primitive types that carry domain meaning (`Currency`, `Quantity`, `Coordinates`).
- When objects are naturally replaced rather than mutated (e.g., `Address` on a `Customer`).

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Eliminates primitive obsession | More types to maintain |
| Makes invalid states impossible | ORM mapping can be more complex |
| Co-locates validation with the concept | Immutability requires new instances for changes |
| Improves readability and self-documentation | Learning curve for teams new to DDD |

## Related

- [[Domain-Driven Design]] — the framework this pattern belongs to
- [[Aggregate]] — aggregates contain value objects as attributes
- [[Domain-Driven Design|Ubiquitous Language]] — value objects express domain vocabulary
- [[Single Responsibility Principle]] — each value object has one domain concept to represent
