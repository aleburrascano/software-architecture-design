---
type: person
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
tags:
  - people
  - design-pattern
---

# Gang of Four

## Who They Are

**Erich Gamma**, **Richard Helm**, **Ralph Johnson**, and **John Vlissides** — four software engineers who co-authored the most influential book in software design patterns.

## Key Work

*Design Patterns: Elements of Reusable Object-Oriented Software* (1994), Addison-Wesley.

Informally referred to as the **"GoF book"** or **"Gang of Four book"** — the nickname "Gang of Four" (GoF) refers to the four authors.

## Contribution

The GoF book created the canonical classification of 23 design patterns into three categories:

| Category | Purpose | Examples |
|----------|---------|---------|
| **Creational** | Control how objects are created | Singleton, Factory Method, Abstract Factory, Builder, Prototype |
| **Structural** | Simplify relationships between components | Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy |
| **Behavioral** | Facilitate communication between objects | Observer, Strategy, Command, Template Method, Iterator, Mediator, State |

The book gave developers a shared vocabulary for discussing recurring design solutions. Before the GoF book, these solutions existed in practice but lacked names or systematic documentation.

Key contributions per category (as described in Khalil Stemmler's architecture map):
- **Creational:** Singleton (single instance guarantee), Abstract Factory (family-of-classes creation), Prototype (clone-from-existing).
- **Structural:** Adapter (enable incompatible interfaces to cooperate), Bridge (split implementation hierarchies), Decorator (add responsibilities dynamically).
- **Behavioral:** Template Method (defer algorithm steps to subclasses), Mediator (define allowed communication channels), Observer (subscribe to changes).

## Significance

The GoF book is required reading at Stage 5 of the full-stack software design learning roadmap. Design patterns provide the vocabulary and building blocks from which architectural patterns at the higher level are constructed — architectural patterns are, in a sense, design patterns "blown up in scale."

> [!question] Unverified
> Ralph Johnson (one of the GoF) is the same Ralph Johnson cited by Martin Fowler in the "Architecture is about the important stuff" discussion. If confirmed, this connects the GoF directly to the architectural philosophy captured in Fowler's architecture guide.

## Related

- [[Software Architecture Overview]] — architectural patterns build on top of design patterns
- [[Architectural Styles Comparison]] — architectural styles are design patterns at the macro level
- [[Domain-Driven Design]] — DDD's tactical patterns (Entities, Value Objects, Repositories) build on GoF foundations
