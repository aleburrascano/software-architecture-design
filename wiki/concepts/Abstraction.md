---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/Software design.md
  - raw/What Makes a Good Software Design Mindset.md
tags:
  - design
  - principles
  - fundamentals
---

# Abstraction

The act of reducing complexity by representing only the information relevant to a given context — hiding background details to allow reasoning at the appropriate level.

> "The essence of abstraction is preserving information that is relevant in a given context, and forgetting information that is irrelevant in that context." — John V. Guttag

## Problem / Why It Matters

Software systems are too complex to hold in their entirety in any one person's mind. Without abstraction, every change requires understanding every implementation detail of every component involved. Abstraction allows work at multiple levels — the whole system, a module, a function — without getting lost in irrelevant detail.

## Types

**Control abstraction** lets programmers write high-level operations without specifying low-level machine instructions. `a := (1 + 2) * 5` abstracts away binary representations, register operations, and memory management — the programmer reasons about arithmetic, not hardware.

**Data abstraction** separates how a data type appears to users from its internal implementation. A lookup table that stores key-value pairs may be implemented as a hash table, binary search tree, or linked list internally — callers interact through the same interface regardless. This allows implementations to be replaced without affecting any client code.

## Role in Software Design

Grady Booch lists abstraction as one of the four fundamental software design principles (alongside encapsulation, modularization, and hierarchy — the PHAME principles).

In design, abstraction manifests as:

- **Interfaces and abstract classes** — define what a component does without specifying how
- **Layered architecture** — each layer exposes an abstraction of the layer below
- **Domain models** — abstract the real-world problem domain into software concepts
- **APIs** — present a stable surface while hiding implementation

The [[Open-Closed Principle]] and [[Dependency Inversion Principle]] both rely on well-designed abstractions: you extend behavior by adding new implementations of existing abstractions, and high-level modules depend on abstractions rather than on concrete implementations.

In the [[What Makes a Good Software Design Mindset (Source)|design mindset]] literature, abstractions are described as the key enabler of extensibility at low risk: switching behavior at runtime by injecting different implementations of an interface (see [[Strategy Pattern]], [[Dependency Injection]]).

## Abstraction and Encapsulation

These are related but distinct:
- **Encapsulation** hides *state* and internal data — it is primarily about protecting access
- **Abstraction** hides *implementation* behind a behavioural contract — it is primarily about simplifying reasoning

In practice they work together: encapsulation provides the mechanism; abstraction provides the intent.

## The Leaky Abstractions Problem

Joel Spolsky observed that "all non-trivial abstractions, to some degree, are leaky" — underlying complexity occasionally surfaces through the abstraction boundary, requiring the caller to understand what is being hidden. This does not negate the value of abstraction but is a reminder that abstractions are simplifications, not perfect barriers.

## Related

- [[Separation of Concerns]] — abstraction is the mechanism that makes concerns separable
- [[Coupling and Cohesion]] — well-designed abstractions reduce coupling
- [[Dependency Inversion Principle]] — DIP is the formal principle that modules should depend on abstractions
- [[Information Hiding]] — closely related concept focusing on hiding implementation details
- [[Encapsulation]] — the implementation mechanism of data abstraction
