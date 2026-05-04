---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources: [Design Patterns Tutorial.md, Creational Design Patterns.md, Structural Design Patterns.md, Behavioral Design Patterns.md]
tags: [design-pattern, gof, topic]
---

# Design Patterns Overview

Design patterns are reusable, named solutions to recurring object-oriented design problems, codified by the Gang of Four into 23 canonical patterns across three categories.

## What Are Design Patterns?

A design pattern is not finished code — it is a template: a description of the problem, the solution structure, the participants, their collaborations, and the trade-offs. Patterns provide shared vocabulary ("use an Observer here") and proven structure, but must be adapted to each context.

**Origin:** Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides — the [[Gang of Four]] — published *Design Patterns: Elements of Reusable Object-Oriented Software* in 1994. The book codified 23 patterns drawn from practice in Smalltalk and C++.

**Characteristics:**
- **Reusability** — applicable across projects and languages
- **Standardization** — shared vocabulary reduces communication overhead
- **Efficiency** — avoid re-solving solved problems
- **Flexibility** — abstract templates, not rigid implementations

## The Three GoF Categories

| Category | Concern | Entry Point |
|----------|---------|------------|
| [[Creational Patterns Overview]] | *How objects are created* — decouple construction from use | [[Factory Method Pattern]], [[Builder Pattern]] |
| [[Structural Patterns Overview]] | *How objects are composed* — simplify relationships between components | [[Adapter Pattern]], [[Facade Pattern]] |
| [[Behavioral Patterns Overview]] | *How objects communicate* — manage workflows, interactions, responsibilities | [[Observer Pattern]], [[Strategy Pattern]] |

### Creational (7 patterns)
[[Singleton Pattern]] · [[Factory Method Pattern]] · [[Abstract Factory Pattern]] · [[Builder Pattern]] · [[Prototype Pattern]] · [[Object Pool Pattern]] · [[Lazy Initialization]]

### Structural (7 patterns)
[[Adapter Pattern]] · [[Bridge Pattern]] · [[Composite Pattern]] · [[Decorator Pattern]] · [[Facade Pattern]] · [[Flyweight Pattern]] · [[Proxy Pattern]]

### Behavioral (10 patterns)
[[Observer Pattern]] · [[Strategy Pattern]] · [[Command Pattern]] · [[Chain of Responsibility Pattern]] · [[Template Method Pattern]] · [[Iterator Pattern]] · [[State Pattern]] · [[Mediator Pattern]] · [[Memento Pattern]] · [[Visitor Pattern]]

## Beyond GoF: Architectural Patterns

The GoF patterns operate at the *object level*. Larger-scale recurring solutions — architectural patterns — address system structure:

- [[MVC Pattern]], [[Repository Pattern]] — application structure
- [[CQRS]], [[Event Sourcing]], [[Event-Driven Architecture]] — data/messaging architecture
- [[Layered Architecture]], [[Hexagonal Architecture]], [[Clean Architecture]], [[Microservices Architecture]] — system decomposition

See [[Software Architecture Overview]] for the full architectural layer.

## Patterns vs. Principles

Design patterns describe *solutions*. Design principles describe *values* to optimise for. They work together:

- [[SOLID Principles]] explain *why* patterns like [[Strategy Pattern]] and [[Decorator Pattern]] are good ideas
- [[DRY Principle]] motivates patterns like [[Template Method Pattern]]
- [[Composition over Inheritance]] motivates most Structural and Behavioral patterns

See [[Design Principles Overview]].

## Learning Sequence (GoF recommended path)

1. OOP fundamentals (classes, inheritance, polymorphism)
2. [[SOLID Principles]] and [[Design Principles Overview]]
3. Creational → Structural → Behavioral patterns
4. Architectural patterns and LLD practice

## Related

- [[Gang of Four]]
- [[Creational Patterns Overview]]
- [[Structural Patterns Overview]]
- [[Behavioral Patterns Overview]]
- [[Design Principles Overview]]
- [[Software Architecture Overview]]
