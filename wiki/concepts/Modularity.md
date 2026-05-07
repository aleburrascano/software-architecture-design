---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software design.md
  - raw/articles/What Makes a Good Software Design Mindset.md
tags:
  - design
  - principles
  - architecture
---

# Modularity

The degree to which a system is composed of well-defined, independent components (modules) that can be developed, tested, deployed, and reasoned about in isolation.

## Problem / Why It Matters

Without modularity, all parts of a system are interdependent. One change can break anything and everything. Teams cannot work independently. Testing requires the entire system. Deployment is all-or-nothing. Modularity is the structural prerequisite for nearly every other software quality attribute: maintainability, testability, scalability, reusability.

## Explanation

A module is a self-contained unit with:
- A clearly defined **interface** that other modules interact with
- **Hidden implementation** details that other modules cannot depend on (see [[Information Hiding]])
- **Cohesive responsibility** — it addresses one concern or capability (see [[Coupling and Cohesion]])

Grady Booch lists modularization as one of the four fundamental software design principles (PHAME: principles of hierarchy, abstraction, modularization, and encapsulation).

### Modularity in Design

At the **class level**, a well-designed class is a module. The [[Single Responsibility Principle]] and [[Interface Segregation Principle]] define what makes a class a good module.

At the **package/component level**, packages group related classes into larger modules. [[Separation of Concerns]] guides how packages are divided.

At the **service level**, [[Microservices Architecture]] takes modularity to its logical extreme — each service is an independently deployable module with its own database, its own team, and its own deployment lifecycle.

### Design Benefits

- **Independent development**: teams work on separate modules without constant coordination
- **Independent testing**: modules can be tested in isolation with mocked interfaces
- **Independent deployment**: in distributed systems, modules can be released separately
- **Reuse**: a well-defined, low-coupling module can be reused in other systems
- **Fault isolation**: failures in one module are less likely to cascade into others

## Structural Partitioning

The Wikipedia source on software design describes two partitioning strategies:

- **Horizontal partitioning**: separate branches of the module hierarchy per major program function (e.g., layers in [[Layered Architecture]])
- **Vertical partitioning**: control and work distributed top-down in the hierarchy (e.g., domain verticals in [[Domain-Driven Design]])

## Trade-offs

More modules mean more interface overhead and more integration complexity. The right granularity depends on the system's size, team structure (see [[Conway's Law]]), deployment model, and expected change patterns. Very fine-grained modularity (nano-services) can create network overhead and operational complexity that outweighs the isolation benefits.

## Related

- [[Coupling and Cohesion]] — the measurements of module quality
- [[Separation of Concerns]] — the guiding principle for how to split modules
- [[Information Hiding]] — modules hide their implementations
- [[Microservices Architecture]] — service-level modularity
- [[Conway's Law]] — team structure determines module boundaries in practice
