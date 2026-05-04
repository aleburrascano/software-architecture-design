---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md]
tags: [design-pattern, creational, topic]
---

# Creational Patterns Overview

Creational patterns abstract the object-creation process, decoupling how objects are created, composed, and represented from the code that uses them.

## What Are Creational Patterns?

Every non-trivial program creates objects. The naive approach — sprinkling `new ConcreteClass()` throughout business logic — couples the code to specific types, makes substitution hard, and scatters creation logic everywhere. Creational patterns address this by asking a single question: *who decides what gets created, and how?*

The GoF (Gang of Four) book *Design Patterns* (Gamma, Helm, Johnson, Vlissides, 1994) identified five canonical creational patterns. The broader pattern community has added others; Object Pool and Lazy Initialization are widely recognised extensions.

Creational patterns are not interchangeable — each solves a distinct constraint:

| Constraint | Pattern to reach for |
|---|---|
| Only one instance should ever exist | [[Singleton Pattern]] |
| Subclasses decide which class to instantiate | [[Factory Method Pattern]] |
| Swap entire families of related objects at once | [[Abstract Factory Pattern]] |
| Complex multi-step construction of one object | [[Builder Pattern]] |
| Copy an existing object without knowing its class | [[Prototype Pattern]] |
| Reuse expensive objects instead of recreating them | [[Object Pool Pattern]] |
| Defer expensive creation until first actual use | [[Lazy Initialization]] |

## The Patterns at a Glance

| Pattern | Problem Solved | Key Mechanism |
|---|---|---|
| [[Singleton Pattern]] | Ensure a class has exactly one instance; provide a global access point | Private constructor + static instance field + static accessor |
| [[Factory Method Pattern]] | Decouple a creator from the concrete type it instantiates | Subclasses override a factory method to return their product type |
| [[Abstract Factory Pattern]] | Create families of related objects consistently without naming concrete classes | An interface of factory methods, one concrete implementation per product family |
| [[Builder Pattern]] | Construct complex objects step-by-step; support multiple representations | Separate builder accumulates parts; optional Director sequences the steps |
| [[Prototype Pattern]] | Clone an object without depending on its concrete class | `clone()` method on the object itself; prototype registry for named instances |
| [[Object Pool Pattern]] | Avoid repeated expensive creation/destruction of resources | Pre-allocated pool; acquire/release lifecycle; reset-before-reuse |
| [[Lazy Initialization]] | Defer creation cost until first use | Null-guarded accessor that initialises on first call and caches the result |

## Choosing Between Them

### Factory Method vs Abstract Factory

Both decouple client code from concrete types. Reach for **Factory Method** when only *one* product type varies and subclassing is acceptable. Reach for **Abstract Factory** when you need *a whole family* of related products to vary together and you want to guarantee family consistency. Abstract Factory is often implemented using Factory Methods internally.

### Abstract Factory vs Builder

**Abstract Factory** returns different products immediately — the emphasis is on *which* objects are created (by family). **Builder** returns one complex object assembled through a sequence of steps — the emphasis is on *how* a single object is constructed. If your "product" is assembled from many parts with an important ordering constraint, Builder fits better.

### Singleton vs Object Pool

**Singleton** is the degenerate case of Object Pool with a pool size of one. Use Singleton when you have exactly one resource; use Object Pool when you have a bounded set of reusable identical resources. Both typically use [[Lazy Initialization]] to defer resource allocation.

### Prototype vs Factory Method

Both can produce new objects polymorphically. Use **Prototype** when you want to copy a pre-configured object (saving configuration work) or when the number of types is large and varies at runtime. Use **Factory Method** when each creation is a fresh construction and the variation is handled cleanly through subclassing.

### When Lazy Initialization is not a standalone choice

Lazy Initialization is a *technique*, not a standalone structural pattern. It appears as the implementation strategy inside Singleton (lazy instance), Object Pool (lazy member creation), and Virtual Proxy (lazy heavyweight resource). Recognising it explicitly helps you apply it correctly — especially regarding thread safety.

## Common Combinations

- **Abstract Factory + Singleton:** Each concrete factory is usually a Singleton.
- **Factory Method + Template Method:** Factory Method is the specialisation of Template Method where the hook produces an object.
- **Builder + Composite:** A Director often builds a tree (Composite) node by node.
- **Prototype + Abstract Factory:** Abstract Factory can store and clone prototypes rather than subclassing every product type.
- **Singleton + Lazy Initialization:** The standard implementation of a lazy Singleton uses the double-checked locking or holder-class idiom.
- **Object Pool + Factory Method:** The pool uses a factory method to create new instances when the pool is empty.

## GoF vs Non-GoF

| Pattern | In GoF (1994)? |
|---|---|
| [[Singleton Pattern]] | Yes |
| [[Factory Method Pattern]] | Yes |
| [[Abstract Factory Pattern]] | Yes |
| [[Builder Pattern]] | Yes |
| [[Prototype Pattern]] | Yes |
| [[Object Pool Pattern]] | No — community addition |
| [[Lazy Initialization]] | No — implementation technique, widely documented |

## Related

- [[Design Patterns Overview]]
- [[Structural Patterns Overview]]
- [[Behavioral Patterns Overview]]
- [[Gang of Four]]
