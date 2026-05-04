---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/What Makes a Good Software Design Mindset.md
  - raw/Software design.md
tags:
  - design
  - principles
  - modularity
---

# Coupling and Cohesion

Two complementary measures of module quality: **cohesion** is the degree to which elements inside a module belong together; **coupling** is the degree of interdependence between modules. Good design seeks high cohesion and low coupling.

## Problem / Why It Matters

If functionality is spread across many modules (low cohesion), changes ripple everywhere and testing in isolation is impossible. If modules depend tightly on each other's internals (high coupling), changing one forces changes in others, creating a fragile, hard-to-test system. These two forces are the root cause of most maintainability problems.

As Duy Pham writes: "good separation of concerns = high cohesion + loose coupling."

## Cohesion

Cohesion measures how strongly the methods and data within a module relate to each other and serve a unified purpose. Types range from worst to best:

1. **Coincidental** — parts group arbitrarily with no meaningful relationship (e.g., a "Utilities" class)
2. **Logical** — parts are logically categorized together but differ in nature (e.g., all input handlers in one class)
3. **Temporal** — parts execute at the same time (e.g., all startup initialization in one place)
4. **Procedural** — parts follow a sequential execution path
5. **Communicational/Informational** — parts operate on the same data structure
6. **Sequential** — one part's output is another's input (assembly-line style)
7. **Functional** — all parts contribute to a single well-defined task (ideal)

High cohesion reduces module complexity, improves reusability, and makes the system easier to understand and change.

## Coupling

Coupling measures how much one module depends on the internals of another. Types range from highest (worst) to lowest (best):

1. **Content coupling** — one module directly uses another's internal code or data (worst)
2. **Common coupling** — multiple modules access the same global data
3. **External coupling** — modules share externally-imposed data formats or protocols
4. **Control coupling** — one module passes a flag that controls the other's behavior
5. **Stamp coupling** — modules share a data structure but only use part of it
6. **Data coupling** — modules communicate only through simple parameters (best)

Object-oriented systems add: **subclass coupling** (inheritance), **temporal coupling** (actions bundled together in time), and **semantic coupling** (implicit shared assumptions not visible in interfaces).

### Disadvantages of Tight Coupling

- Changes ripple: modifying module B forces changes in A, C, D...
- Unit testing becomes difficult or impossible in isolation
- Modules cannot be reused in other contexts without dragging dependencies along
- Deployment is all-or-nothing; modules cannot be moved or scaled independently

## The Relationship Between the Two

Cohesion and coupling are inversely correlated: high cohesion within a module naturally reduces the reasons for other modules to reach into it, which reduces coupling. Larry Constantine developed both concepts in the late 1960s as part of Structured Design methodology.

> High cohesion often correlates with loose coupling, and vice versa.

## In Architecture

These concepts scale from class-level to service-level design:

- At the class level: [[Single Responsibility Principle]] is the SRP expression of high cohesion
- At the module/package level: [[Dependency Inversion Principle]] and [[Interface Segregation Principle]] reduce coupling
- At the service level: [[Microservices Architecture]] aims for high service cohesion and loose inter-service coupling
- At the organizational level: [[Conway's Law]] connects team communication structure to coupling between system components

## Related

- [[Separation of Concerns]] — the principle that motivates pursuing high cohesion and low coupling
- [[Single Responsibility Principle]] — SRP at the class level is functional cohesion
- [[Dependency Inversion Principle]] — primary mechanism for reducing coupling
- [[Law of Demeter]] — a specific rule for reducing coupling between objects
- [[Conway's Law]] — organizational coupling mirrors code coupling
