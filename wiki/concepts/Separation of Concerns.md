---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/What Makes a Good Software Design Mindset.md
  - raw/articles/Software architecture 1.md
tags:
  - design
  - principles
  - modularity
---

# Separation of Concerns

A design principle that organizes a program into distinct sections so that each section addresses a single, well-defined concern — making the system easier to understand, test, modify, and evolve.

## Problem / Why It Matters

When multiple concerns are tangled together in the same code — business logic mixed with data access, presentation logic mixed with validation, logging scattered everywhere — each concern becomes hard to reason about or change in isolation. A change to one concern creates unexpected side effects in others.

## Explanation

Edsger W. Dijkstra coined the term in his 1974 paper "On the Role of Scientific Thought," noting that studying one aspect of a subject in isolation — "for the sake of its own consistency" — produces clearer thinking than trying to examine multiple aspects simultaneously.

A *concern* is any piece of interest or focus in a program: what data to display, how to persist it, how to authenticate users, how to log errors. Separation of concerns says: don't let these bleed into each other.

Programs embodying SoC are called **modular programs**. Each module hides its detailed implementation behind a well-designed interface, protecting it from changes in other modules and allowing it to be developed, tested, deployed, and upgraded independently.

> In other words: good separation of concerns = high cohesion + loose coupling. — Duy Pham

### Levels of Application

**Class/module level**: Each class has one reason to change. (See [[Single Responsibility Principle]].)

**Layer level**: A [[Layered Architecture]] separates presentation, business logic, data access, and persistence. Each layer exposes only its contract interface; concrete implementation is encapsulated internally. The business layer is safe from UI or database changes.

**Aspect level**: Cross-cutting concerns (logging, security, transaction management) that span multiple modules are handled through Aspect-Oriented Programming (AOP) or middleware, rather than being scattered throughout domain code.

**Network/protocol level**: The Internet Protocol stack exemplifies SoC — SMTP handles email session details, TCP handles reliable delivery, IP handles routing — each layer ignoring the others' concerns.

**Web frontend**: HTML (content), CSS (presentation), and JavaScript (behavior) represent a classical SoC split.

## Examples

- **Good**: A `UserRepository` class that only handles database access for users; a `UserService` that only handles business rules about users.
- **Bad**: A `UserController` that queries the database directly, validates input, sends emails, and formats the response all in one method.

## Trade-offs

Strict separation sometimes creates indirection overhead — more interfaces, more classes, more files to navigate. The cost is justified when concerns change at different rates or need to be tested/deployed independently. For simple, stable code it may be unnecessary overhead.

## Related

- [[Coupling and Cohesion]] — SoC is the motivating principle; cohesion and coupling are the measurements
- [[Single Responsibility Principle]] — SoC applied at the class level
- [[Layered Architecture]] — SoC applied at the architectural level
- [[Hexagonal Architecture]] — SoC isolating domain from infrastructure
- [[Abstraction]] — enabling SoC by hiding implementation details behind interfaces
