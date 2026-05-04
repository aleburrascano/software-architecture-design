---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources: [Structural Design Patterns.md]
tags: [design-pattern, structural, topic]
---

# Structural Patterns Overview

Structural patterns describe how classes and objects are composed into larger structures, using inheritance and object composition to form flexible, efficient arrangements.

## What Are Structural Patterns?

Once you know how to create objects (creational patterns) and what behaviour they should exhibit (behavioral patterns), you face a third question: *how do you wire objects and classes together into a coherent whole?* Structural patterns answer it.

They deal with two recurring forces:
1. **Incompatibility** — you have components that don't fit together and need an intermediary
2. **Complexity** — a subsystem is too large or intricate to expose directly

The [[Gang of Four]] codified seven canonical structural patterns. Each addresses a different structural concern:

| Force | Pattern to reach for |
|---|---|
| Two incompatible interfaces need to work together | [[Adapter Pattern]] |
| Abstraction and implementation should evolve independently | [[Bridge Pattern]] |
| Clients need to treat individual objects and groups uniformly | [[Composite Pattern]] |
| Behaviour must be added to objects without subclassing | [[Decorator Pattern]] |
| A complex subsystem needs a simple entry point | [[Facade Pattern]] |
| Thousands of fine-grained objects share the same state | [[Flyweight Pattern]] |
| Access to an object must be controlled or intercepted | [[Proxy Pattern]] |

## The Patterns at a Glance

| Pattern | Problem Solved | Key Mechanism |
|---|---|---|
| [[Adapter Pattern]] | Incompatible interfaces — you have a legacy or third-party class whose interface doesn't match what your code expects | Wrapper class translates calls from the target interface to the adaptee's interface |
| [[Bridge Pattern]] | Cartesian-product class explosion when abstraction and implementation vary independently | Two separate hierarchies connected by a composition reference; neither extends the other |
| [[Composite Pattern]] | You need to model part-whole hierarchies and want to treat leaves and branches identically | All nodes implement a common `Component` interface; branches hold a list of children |
| [[Decorator Pattern]] | Add responsibilities to individual objects at runtime without affecting others of the same class | Wrapper implements the same interface as the wrapped object, forwarding calls and adding behaviour before/after |
| [[Facade Pattern]] | A subsystem of many classes is hard to use correctly; you want a single, simple entry point | New class provides a high-level interface that delegates to the subsystem's internal classes |
| [[Flyweight Pattern]] | Instantiating many objects that share most of their state would exhaust memory | Separate intrinsic state (shared, immutable) from extrinsic state (contextual, passed in); share one flyweight per unique intrinsic state |
| [[Proxy Pattern]] | Direct access to an object is undesirable (too expensive, remote, sensitive, or needs interception) | Stand-in object implements the same interface; controls and possibly enriches access to the real object |

## Choosing Between Them

### Adapter vs Bridge

**Adapter** fixes an incompatibility that already exists — you're gluing two things that weren't designed to work together. **Bridge** prevents an incompatibility from forming — you design the abstraction and implementation hierarchies to be independent from the start. If you own both sides, prefer Bridge; if you're adapting a third-party class you can't change, use Adapter.

### Adapter vs Facade

Both present a different interface to clients. **Adapter** makes one specific incompatible class usable through a standard interface. **Facade** simplifies an entire subsystem by providing a unified, higher-level interface over many classes. Adapter adapts; Facade simplifies.

### Decorator vs Proxy

Both wrap an object and implement its interface. The distinction lies in intent: **Decorator** adds new behaviour (input validation, logging, formatting) — the client chooses which decorators to stack. **Proxy** controls *access* to the same behaviour (lazy loading, caching, access control) — the proxy is transparent to the client. In practice: if the wrapper's job is to *enrich*, it's a Decorator; if it's to *gate*, it's a Proxy.

### Decorator vs Composite

Both use recursive composition. **Decorator** has exactly one child; the tree is a linear chain used to accumulate behaviour. **Composite** has any number of children; the tree represents a part-whole hierarchy. A Decorator can be seen as a degenerate Composite of one, but their intents differ.

### Facade vs Mediator

Both abstract a complex web of interactions. **Facade** is a one-directional simplification: the client calls the facade, which orchestrates subsystem classes. The subsystem classes don't know about the facade. **Mediator** is bidirectional: components communicate *through* the mediator and depend on it, while the mediator knows about all of them. Facade hides complexity; Mediator centralises it.

### When Flyweight applies

Flyweight is unique — it's not about structural relationships between classes but about *memory optimisation* when a huge number of objects share most of their data. Apply it only when profiling confirms object count is causing measurable memory pressure; the extrinsic/intrinsic split makes the code harder to read.

## Common Combinations

- **Composite + Decorator:** Decorator adds behaviour to individual nodes; Composite defines the tree structure. They compose naturally since both use the same component interface.
- **Composite + Iterator:** Iterator traverses a Composite tree uniformly without exposing the tree structure.
- **Proxy + Flyweight:** A Flyweight factory often uses the Proxy idea to return the same shared instance rather than a new one.
- **Facade + Singleton:** A Facade is often implemented as a Singleton since you only need one simplified entry point to a subsystem.
- **Adapter + Bridge:** You may apply Adapter to existing incompatible code and Bridge to new code that needs flexible variation — both can appear in the same system.
- **Decorator + Strategy:** Decorator wraps to add behaviour; Strategy swaps the core algorithm. Combine them when you need both extensible wrapping and swappable core logic.

## Structural vs Behavioral Structural Patterns

Some structural patterns have behavioral counterparts that share their shape but serve a different purpose:

| Structural | Behavioral counterpart | Difference |
|---|---|---|
| [[Facade Pattern]] | [[Mediator Pattern]] | Facade is one-directional; Mediator is bidirectional |
| [[Decorator Pattern]] | [[Chain of Responsibility Pattern]] | Decorator enriches the result; CoR passes the request along until handled |
| [[Composite Pattern]] | [[Iterator Pattern]] | Composite defines the tree; Iterator traverses it |

## Related

- [[Design Patterns Overview]]
- [[Creational Patterns Overview]]
- [[Behavioral Patterns Overview]]
- [[Gang of Four]]
