---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md"]
tags: [design-pattern, behavioral, topic]
---

# Behavioral Patterns Overview

The 10 Gang of Four behavioral patterns address how objects communicate, distribute responsibility, and encapsulate algorithms — the "who does what, and how do they talk" dimension of object-oriented design.

## What Are Behavioral Patterns?

Behavioral patterns are concerned with the **assignment of responsibilities between objects** and with the **algorithms** that govern their interactions. They do not focus on how objects are created (creational) or assembled (structural), but on the dynamic, runtime protocols through which objects collaborate.

The patterns range from fine-grained object-level protocols (Iterator, Visitor) to high-level communication frameworks (Mediator, Observer). Most work by introducing a level of indirection — an extra object or interface — that decouples the initiator of a behavior from the implementor.

## The Patterns at a Glance

| Pattern | Problem Solved | Key Mechanism |
|---|---|---|
| [[Observer Pattern]] | One object must keep arbitrary dependents in sync | Subject maintains subscriber list; broadcasts `notify()` |
| [[Strategy Pattern]] | Multiple algorithm variants, avoid conditional proliferation | Encapsulate each algorithm in a class; swap at runtime |
| [[Command Pattern]] | Decouple sender from receiver; enable undo/queue | Wrap request as an object with `execute()` / `undo()` |
| [[Chain of Responsibility Pattern]] | Unknown handler for a request; dynamic dispatch | Pass request down a linked handler chain |
| [[Template Method Pattern]] | Shared algorithm skeleton with variable steps | Define skeleton in base class; subclasses fill in steps |
| [[Iterator Pattern]] | Traverse collection without exposing structure | Separate traversal cursor into an iterator object |
| [[State Pattern]] | Object changes behavior as internal state changes | Delegate to interchangeable state objects |
| [[Mediator Pattern]] | Complex many-to-many coupling between components | Central hub routes all inter-component communication |
| [[Memento Pattern]] | Undo/rollback without breaking encapsulation | Originator creates opaque state snapshot; caretaker stores it |
| [[Visitor Pattern]] | Add operations to a stable hierarchy without modifying it | Double dispatch: elements `accept()` visitor; visitor `visit()` element |

## Groupings Within Behavioral Patterns

### Communication / Coupling Reducers
These three directly address how objects talk to each other and how tightly coupled they are:

- **[[Observer Pattern]]** — one-to-many broadcast; subject and observers are loosely coupled.
- **[[Mediator Pattern]]** — many-to-many communication centralised in a hub; O(n²) coupling → O(n).
- **[[Chain of Responsibility Pattern]]** — sequential, linear dispatch; sender does not know the handler.

### Algorithm Encapsulators
These patterns encapsulate an algorithm or part of one:

- **[[Strategy Pattern]]** — the whole algorithm is replaceable at runtime.
- **[[Template Method Pattern]]** — only variable steps are delegable; the skeleton is fixed.
- **[[Command Pattern]]** — a single operation (not necessarily a full algorithm) is encapsulated as an object.

### State and History
- **[[State Pattern]]** — behavior is entirely governed by current state; transitions are first-class.
- **[[Memento Pattern]]** — state history is captured for rollback.

### Traversal and Structure
- **[[Iterator Pattern]]** — uniform traversal of any collection.
- **[[Visitor Pattern]]** — operation applied uniformly across a heterogeneous object structure; leverages traversal provided by Iterator or Composite.

## Choosing Between Them

**Observer vs Mediator**
Both decouple objects. Use Observer when the relationship is naturally a one-to-many broadcast (a data source, an event emitter). Use Mediator when interactions are many-to-many and the coupling web is complex — centralising in a mediator avoids a tangled graph of direct references.

**Strategy vs Template Method**
Both vary an algorithm. Use Strategy when you need runtime algorithm swapping or want to avoid inheritance; use Template Method when the algorithm skeleton is fixed and only a few well-defined steps vary, and you are comfortable with inheritance.

**Strategy vs State**
Both delegate to an interchangeable object. The key distinction is who drives the swap: with Strategy the client chooses and replaces the strategy from outside; with State the object transitions itself based on internal conditions. States typically know about each other (for transitions); strategies are usually independent.

**Command vs Chain of Responsibility**
Both decouple sender from handler. Command wraps an action and targets a specific receiver. Chain dispatches along a sequence of potential handlers, and only one (or some) handle the request.

**Visitor vs Iterator**
Often used together: Iterator provides traversal; Visitor provides the operation applied at each element. Use Visitor when you have many distinct operations to perform on a stable hierarchy; use Iterator when you simply need to walk a collection uniformly.

**Memento vs Command (for undo)**
Use Command for undo when the operation is reversible by calling an inverse method (`undo()`). Use Memento when restoring previous state is necessary — especially when the operation is complex or the inverse is not cleanly computable.

## Related

- [[Design Patterns Overview]]
- [[Creational Patterns Overview]]
- [[Structural Patterns Overview]]
- [[Gang of Four]]
