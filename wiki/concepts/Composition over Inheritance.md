---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/articles/What Makes a Good Software Design Mindset.md"
  - "https://www.geeksforgeeks.org/java/difference-between-inheritance-and-composition-in-java/"
tags:
  - design-principle
  - oop
---

# Composition over Inheritance

Classes should achieve polymorphic behavior and code reuse by containing instances of other classes that implement the desired functionality, rather than by inheriting from a base or parent class.

## Problem

Inheritance seems to offer an elegant solution to code reuse in OOP, but its rigid coupling between parent and child classes creates brittle hierarchies that resist change:

- A change to a parent class can break all child classes silently (the **fragile base class** problem).
- A child class inherits *all* parent behavior, including behavior it does not want — and must either stub it out or throw `UnsupportedOperationException`.
- The inheritance hierarchy must be planned correctly upfront; restructuring deep hierarchies is expensive.
- It is impossible to change an object's inherited behavior at runtime.
- In Java (and most single-inheritance languages), a class can only extend one parent, limiting reuse across orthogonal dimensions.
- Inheritance cannot be used with `final` classes, which composition handles transparently.
- Testing a subclass requires the parent class to be present and functional — composition lets each component be tested independently.

Over time, inheritance hierarchies tend to grow deep and wide, producing tight coupling across the entire class graph. Maintainers find that changing one class in the middle of the hierarchy breaks seemingly unrelated classes elsewhere — a direct [[Liskov Substitution Principle|LSP]] violation risk at scale.

## Statement

> "Favor composition over inheritance."
> — Gang of Four, *Design Patterns: Elements of Reusable Object-Oriented Software* (1994)

Also known as the **Composite Reuse Principle**.

## Explanation

**Inheritance** establishes an *is-a* relationship. `Dog extends Animal` means a `Dog` is an `Animal` and inherits all animal behavior. This is appropriate when the relationship is genuinely hierarchical and stable.

**Composition** establishes a *has-a* relationship. A `Dog` *has-a* `Locomotion` behavior and *has-a* `Sound` behavior. The behaviors are implemented as separate objects injected into the `Dog`. The `Dog` delegates to them rather than inheriting from them.

### Comparison at a glance

| Aspect | Inheritance | Composition |
|--------|-------------|-------------|
| Relationship | "is-a" | "has-a" |
| Flexibility | Fixed at compile time | Swappable at runtime |
| Multiple capabilities | Limited (single inheritance in Java) | Unrestricted — compose any set |
| Testing | Requires parent class | Components testable independently |
| Final classes | Cannot extend | Can compose |
| Coupling | Tight (child depends on parent internals) | Loose (via interface only) |

### Composition advantages in detail

- **Runtime flexibility**: behaviors can be swapped at runtime (Strategy pattern).
- **Selective reuse**: a class composes only the behaviors it actually needs.
- **Loose coupling**: the containing class and its component behaviors evolve independently.
- **Testability**: each behavior can be mocked or replaced in tests.
- **Avoids fragile base class**: there is no shared base class whose changes propagate unexpectedly.
- **Works with final classes**: since composition holds a *reference* rather than extending, the component class does not need to be extendable.

### When inheritance is appropriate

Composition does not make inheritance wrong — it makes inheritance the tool of last resort rather than the first tool. Inheritance is appropriate when:
- The is-a relationship is stable and semantically correct.
- The subclass genuinely extends (adds to, not modifies) the parent's contract.
- The hierarchy is shallow (one or two levels).
- The parent is abstract with no implementation state.

The canonical "rule of thumb": if you find yourself overriding more than a couple of methods in a subclass, or adding `// do nothing` overrides, reach for composition instead.

### The Entity-Component-System (ECS) pattern

Game engines (Unity, Unreal) are the most visible real-world embodiment of composition over inheritance. Game objects are composed of components (Transform, Renderer, Physics, Collider) rather than inheriting from a `GameObject` class with all capabilities. This makes it trivial to combine behaviors in novel ways without deep inheritance trees.

## Example

### Inheritance approach — fragile (Python)

````nclass Bird:
    def move(self): self.fly()
    def fly(self): print("flap wings")

class Duck(Bird):
    def quack(self): print("quack")

class Penguin(Bird):
    def fly(self):          # forced override — LSP violation
        raise NotImplementedError("Penguins can't fly")
```

The inheritance hierarchy must be redesigned every time a new bird type is added that doesn't fit the inherited behavior.

### Composition approach — flexible (Python)

````nfrom typing import Protocol

class LocomotionBehavior(Protocol):
    def move(self): ...

class FlyBehavior:
    def move(self): print("flap wings")

class WalkBehavior:
    def move(self): print("waddle")

class SwimBehavior:
    def move(self): print("swim")

class Bird:
    def __init__(self, locomotion: LocomotionBehavior):
        self._locomotion = locomotion

    def move(self):
        self._locomotion.move()

duck = Bird(FlyBehavior())     # can fly
penguin = Bird(SwimBehavior()) # can swim
emu = Bird(WalkBehavior())     # can walk

# Runtime behavior change — no class modification needed
duck._locomotion = SwimBehavior()  # duck now swims
```

Each behavior is a separate, testable object. The `Bird` class is unmodified when new locomotion behaviors are added.

### Composition via object reference (Java)

The canonical Java example: a `Library` *has* `Book` objects rather than inheriting from them. The `Library` holds a collection and delegates book-related queries to the `Book` instances:

````nclass Book {
    public String title;
    public String author;
    Book(String t, String a) { this.title = t; this.author = a; }
}

class Library {
    private final List<Book> books = new ArrayList<>();

    public void add(Book b) { books.add(b); }

    public List<Book> getAll() { return books; }
}
```

`Library` composes `Book` via a `has-a` relationship. It can add, remove, or query books without any inheritance coupling. The `Book` class can be `final` — this doesn't matter because `Library` does not extend it.

## Common Violations

- **Deep inheritance trees** (5+ levels) for reusing implementation.
- **Subclassing to add a single field or method** — a class that inherits 90% of a parent just to add one field should compose instead.
- **`super()` call chains** that produce fragile ordering dependencies.
- **Abstract base classes with concrete implementation** that get overridden unpredictably in subclasses.
- **Multiple inheritance** used to combine behaviors — a warning sign that composition is more appropriate.
- **Extending a concrete class** (rather than an abstract one) to reuse implementation — the tightest possible coupling.

## Trade-offs

- **Verbosity**: composition can require more boilerplate than inheritance, especially for delegation (many one-line forwarding methods). Languages like Kotlin (delegation interfaces) or Go (struct embedding) reduce this cost.
- **Indirection**: tracing code through composed objects requires navigating more levels of indirection than following an inheritance call chain.
- **Conceptual overhead**: for simple domains where inheritance is genuinely an is-a hierarchy and unlikely to change, composition adds unnecessary complexity.
- **Identity and instanceof checks**: code that tests `isinstance(obj, SomeClass)` breaks with composition because the composed behavior is no longer visible via type.

## Related

- [[Liskov Substitution Principle]] — deep inheritance hierarchies are the most common source of LSP violations
- [[Open-Closed Principle]] — composition provides a clean mechanism for extending behavior without modification
- [[Dependency Injection]] — DI is the mechanism used to compose and inject behavior objects
- [[Single Responsibility Principle]] — behaviors extracted for composition naturally become single-responsibility classes
- [[SOLID Principles]] — all five SOLID principles work more naturally with composition than with deep inheritance
