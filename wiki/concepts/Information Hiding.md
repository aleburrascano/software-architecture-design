---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software design.md
tags:
  - design
  - principles
  - modularity
---

# Information Hiding

The principle that modules should be specified and designed so that their internal details are inaccessible to other modules that have no need for them.

## Problem / Why It Matters

When implementation details of one module are visible to — and used by — other modules, those other modules become coupled to the implementation rather than to the interface. Any internal change breaks all callers. Information hiding protects against this by drawing a clear boundary between what a module *exposes* and what it *conceals*.

## Explanation

David Parnas formalized the principle in his influential 1972 paper "On the Criteria To Be Used in Decomposing Systems into Modules," arguing that design decisions most likely to change should be hidden behind module interfaces. The decision of *how* to store user data, *how* to sort a list, or *how* to format an output should be confined to one module — callers see only the interface and are immune to implementation changes.

This is the underlying rationale for:
- Private fields in object-oriented classes
- Package-private visibility scopes
- Internal APIs vs. public APIs
- Opaque data types

Information hiding is a design-time principle, while **encapsulation** is the programming-language mechanism that implements it. The two are often conflated but are distinct: information hiding is the *goal*, encapsulation is one *mechanism* for achieving it.

## Examples

**Good**: A `PaymentGateway` interface that exposes `charge(amount, currency)` — callers do not know whether the implementation uses Stripe, PayPal, or a bank API. Switching providers requires changing only the implementation, not the callers.

**Bad**: A `UserManager` that exposes its internal `HashMap<String, User>` as a public field — callers write code against the hash map directly, making it impossible to change the storage structure.

## Relation to Other Principles

- [[Abstraction]] is the broader concept — information hiding is a specific application of abstraction to module internals
- [[Separation of Concerns]] is achieved partly by hiding the implementation of each concern
- [[Coupling and Cohesion|Low coupling]] is the measurable result of successful information hiding
- [[Single Responsibility Principle]] motivates *what* to hide: each module hides one decision

## Related

- [[Abstraction]] — the parent concept
- [[Separation of Concerns]] — SoC is achieved through information hiding
- [[Coupling and Cohesion]] — information hiding reduces coupling
- [[Encapsulation]] — the OOP mechanism that implements information hiding
