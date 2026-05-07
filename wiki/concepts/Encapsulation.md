---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software design.md
tags:
  - design
  - principles
  - oop
---

# Encapsulation

The bundling of data (state) and the methods that operate on that data into a single unit (class or module), and restricting direct access to the internal state from outside that unit.

## Problem / Why It Matters

When any part of a program can directly read and modify another part's internal state, changes to that state ripple unpredictably throughout the codebase. Encapsulation establishes ownership of state — each piece of data has exactly one owner responsible for its invariants — making the system easier to reason about and safer to change.

## Explanation

Grady Booch lists encapsulation as one of the four fundamental software design principles (PHAME: principles of hierarchy, abstraction, modularization, and encapsulation).

Encapsulation has two aspects:

1. **Data hiding**: internal fields are private; external code cannot read or write them directly. Access is controlled through methods (getters/setters, or more meaningfully, domain methods).
2. **Behavioral bundling**: the logic that operates on the data lives with the data. A `BankAccount` class encapsulates its `balance` field and exposes `deposit()` and `withdraw()` methods that enforce business rules (e.g., no negative balance).

This means the class can enforce **invariants** — rules that must always hold for its state — without relying on external callers to behave correctly.

## Encapsulation vs. Information Hiding

These are closely related but not identical:

- **[[Information Hiding]]** is the design principle: hide decisions that are likely to change behind stable interfaces. It applies to modules, services, and APIs, not just classes.
- **Encapsulation** is the OOP mechanism that implements information hiding at the class level: bundle state and behavior, restrict access to state.

Encapsulation is one implementation of information hiding. A microservice that exposes only an HTTP API and hides its database schema is practicing information hiding without classical OOP encapsulation.

## Examples

**Good**: `BankAccount.withdraw(amount)` enforces the no-overdraft rule internally. Callers cannot set the balance directly.

**Bad**: `account.balance -= amount` called from every site that needs to debit an account. No invariant enforcement; rule changes require finding every call site.

## Related

- [[Information Hiding]] — the design principle; encapsulation is its OOP implementation
- [[Abstraction]] — encapsulation supports abstraction by hiding state details
- [[Coupling and Cohesion]] — encapsulation reduces coupling by making state private
- [[Single Responsibility Principle]] — tells you what to encapsulate
