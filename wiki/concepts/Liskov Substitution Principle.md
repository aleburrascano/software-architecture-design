---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "https://www.geeksforgeeks.org/system-design/liskov-substitution-principle/"
  - "https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/"
tags:
  - design-principle
  - solid
---

# Liskov Substitution Principle

Objects of a subclass must be substitutable for objects of their superclass without breaking program correctness.

## Problem

Inheritance is the most commonly misused OOP mechanism. When a subclass violates the behavioral contract of its parent — throwing exceptions for operations the parent supports, narrowing preconditions, or widening postconditions — callers that work correctly with the parent break silently or crash when given the subclass. This forces defensive `isinstance` checks that couple code to concrete types, undoing the polymorphism that inheritance is supposed to provide.

These violations are often invisible at design time and surface only during maintenance when an unexpected subtype is introduced. At that point, fixing the violation may require refactoring a deep class hierarchy — a high-risk operation.

A classic symptom: a method in the subclass that throws `UnsupportedOperationException` (or a no-op `pass`) because the inherited operation makes no sense for the subtype. This is the strongest signal that the inheritance hierarchy is wrong.

## Statement

> "If S is a subtype of T, then objects of type T may be replaced with objects of type S without altering any of the desirable properties of the program."
> — Barbara Liskov (1987 keynote, formalized with Jeannette Wing in 1994)

Informally: if code works correctly with a base type, it must continue to work correctly when handed any subtype — without modification.

## Explanation

LSP is a contract-based view of subtyping. A subtype must honor the following behavioral contract of its supertype:

1. **Preconditions cannot be strengthened**: a subtype method may not demand more from its callers than the parent demanded. If the parent accepts any positive integer, the subtype cannot refuse anything below 10.
2. **Postconditions cannot be weakened**: a subtype method must deliver at least as much as the parent promised. If the parent guarantees a non-null return, the subtype cannot return null.
3. **Invariants of the supertype must be preserved**: properties that always hold for the supertype must also hold for the subtype.
4. **No new exceptions**: a subtype method may not throw exception types that the parent did not declare.
5. **History constraint**: a subtype may not change state in ways the parent disallowed (e.g., a mutable subtype of an immutable type breaks any caller that relies on immutability).

LSP is fundamentally about **abstraction design quality**. A violation usually signals that the inheritance hierarchy is wrong — the child does not truly "is-a" the parent in a *behavioral* sense, even if it is correct in a *mathematical* or *structural* sense.

### The classic Bird/Penguin problem

A `Bird` abstract class with a `fly()` method seems natural. But `Penguin` cannot fly — so it either throws an exception or does nothing. Any code iterating a list of `Bird` objects and calling `fly()` breaks when a `Penguin` is present.

The fix is not "override `fly()` with a no-op in Penguin." The fix is: remove `fly()` from `Bird` and introduce a `Flyable` interface implemented only by birds that can fly. The abstraction was wrong, not just the subclass.

### Mathematical vs. behavioral subtyping

A `Square` is mathematically a special case of `Rectangle` (all sides equal). But behaviorally, a `Square` that enforces `width == height` violates the `Rectangle` contract: any code that sets width and height independently and then asserts `area = width * height` will break when given a `Square`. LSP demands behavioral, not structural, reasoning.

## Example

### Violation — Rectangle/Square

```python
class Rectangle:
    def set_width(self, w: int): self._w = w
    def set_height(self, h: int): self._h = h
    def area(self) -> int: return self._w * self._h

class Square(Rectangle):
    def set_width(self, w: int):
        self._w = w
        self._h = w   # must stay equal — changes postcondition of Rectangle

    def set_height(self, h: int):
        self._h = h
        self._w = h   # ditto

def test_resize(r: Rectangle):
    r.set_width(5)
    r.set_height(4)
    assert r.area() == 20   # passes for Rectangle, FAILS for Square (area = 16)
```

`Square` is a mathematical subtype of `Rectangle` but not a behavioral one. Substituting a `Square` where a `Rectangle` is expected breaks the caller's invariant.

### Conforming — separate abstractions

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> int: ...

class Rectangle(Shape):
    def __init__(self, w: int, h: int): self._w, self._h = w, h
    def set_width(self, w: int): self._w = w
    def set_height(self, h: int): self._h = h
    def area(self) -> int: return self._w * self._h

class Square(Shape):
    def __init__(self, side: int): self._side = side
    def set_side(self, s: int): self._side = s
    def area(self) -> int: return self._side ** 2
```

Both share only the `area` abstraction — no inherited mutation semantics to violate. `Square` is not substitutable for `Rectangle` and doesn't pretend to be.

### Violation — Bird/Penguin

```python
class Bird:
    def fly(self): print("flap wings")

class Penguin(Bird):
    def fly(self):
        raise NotImplementedError("Penguins can't fly")  # LSP violation

def make_all_fly(birds: list[Bird]):
    for bird in birds:
        bird.fly()   # crashes when a Penguin is in the list

make_all_fly([Duck(), Eagle(), Penguin()])  # RuntimeError
```

### Conforming — refactored abstraction

```python
from abc import ABC, abstractmethod

class Bird(ABC):
    pass   # only shared bird properties (name, habitat, etc.)

class FlyingBird(Bird, ABC):
    @abstractmethod
    def fly(self): ...

class Duck(FlyingBird):
    def fly(self): print("flap wings")

class Eagle(FlyingBird):
    def fly(self): print("soar")

class Penguin(Bird):
    def swim(self): print("swim")

def make_all_fly(birds: list[FlyingBird]):
    for bird in birds:
        bird.fly()   # safe — only FlyingBirds in this list
```

The type system now encodes the behavioral contract correctly.

## Common Violations

- **Throwing `UnsupportedOperationException`** in a subclass method (Java's `Stack` extending `Vector` is a well-known example — `Stack` inherits vector methods like `add(index, element)` that break stack semantics).
- **No-op overrides**: overriding a method with an empty body to "disable" inherited behavior.
- **Checking concrete types** with `isinstance`/`type()` before calling a method — signals the abstraction hierarchy is wrong.
- **Strengthened preconditions**: a subtype that requires additional constructor arguments or rejects input the parent accepted.
- **Narrowed return types that break callers** expecting the parent's return contract (though covariant return types are generally acceptable in languages that support them).

## Trade-offs

- **Over-engineering hierarchies**: obsessively trying to satisfy LSP can lead to flat, interface-heavy designs with many small types. Sometimes a simple comment noting the behavioral difference is more pragmatic.
- **Difficulty with legacy hierarchies**: in large codebases, fixing an LSP violation often requires refactoring a deep class hierarchy, which is high-risk. Sometimes the violation must be documented and guarded rather than corrected.
- **Mathematical vs. behavioral tension**: programmers who think in mathematical terms (a square *is* a rectangle geometrically) often clash with LSP, which requires behavioral, not structural, reasoning. Both perspectives are valid; LSP applies specifically to how code uses types.

## Related

- [[SOLID Principles]] — LSP is the "L" in SOLID
- [[Interface Segregation Principle]] — ISP creates narrow, behavioral contracts that make LSP easier to satisfy
- [[Open-Closed Principle]] — LSP-compliant hierarchies enable safe extension without modification
- [[Composition over Inheritance]] — the primary escape hatch when an inheritance hierarchy violates LSP
