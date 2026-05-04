---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/system-design/prototype-design-pattern/", "https://refactoring.guru/design-patterns/prototype"]
tags: [design-pattern, creational, gof]
---

# Prototype Pattern

Lets you copy existing objects without making your code dependent on their concrete classes.

## Problem

You need to create a copy of an object, but you face two compounding difficulties:

1. **Private field access:** Private fields cannot be accessed from outside the object, so cloning from the outside requires knowledge of every field and may silently miss private state.
2. **Class coupling:** To create a copy you must know the object's concrete class. If the object is held behind an interface — as it will be in any polymorphic design — you cannot write `new ConcreteType(source)` because the concrete type is not known at the call site.

Additionally, constructing an object from scratch may be expensive (parsing a large configuration file, establishing a network connection, running a complex computation). Cloning a pre-warmed prototype is significantly cheaper.

Refactoring Guru's biological analogy: mitotic cell division — the original cell acts as a prototype, actively participating in creating an identical copy.

## Solution

Delegate the cloning responsibility to the object being cloned. Declare a `clone()` method in an interface (the Prototype interface). Each concrete class implements `clone()` by constructing a copy of itself — it has access to all its own fields, including private ones. Objects of the same class can access each other's private members, so the clone can copy private state that outside code could not touch.

The client calls `obj.clone()` without knowing the concrete type. A **Prototype Registry** (a.k.a. prototype manager) can store pre-configured prototype instances keyed by name or type, so clients can look up and clone a pre-built object rather than constructing one from scratch.

```
interface Prototype:
    clone() -> Prototype

class ConcretePrototype(Prototype):
    field1, field2

    clone():
        copy = new ConcretePrototype()
        copy.field1 = self.field1
        copy.field2 = deepCopy(self.field2)   // shallow vs deep — see below
        return copy
```

## Structure

| Participant | Role |
|---|---|
| Prototype | Interface (or abstract class) declaring `clone()` |
| ConcretePrototype | Implements `clone()`; copies its own state into a new instance |
| Client | Calls `prototype.clone()` to obtain a copy; does not reference the concrete class |
| Prototype Registry (optional) | Stores named prototypes; provides a `getPrototype(name)` lookup |

### UML relationships
Client depends only on the Prototype interface. Concrete classes implement Prototype independently. The Registry stores Prototype references and exposes `get(key)` which returns a clone.

## Shallow Copy vs Deep Copy

This is the central implementation decision:

- **Shallow copy:** Copies the object's fields by value. Reference-type fields point to the same underlying objects as the original. Fast and simple; correct when shared references are intentional (e.g. read-only objects, value objects).
- **Deep copy:** Recursively copies all referenced objects as well. Required when the clone must be fully independent (mutable nested objects). More complex to implement correctly — especially with circular references.

Many languages provide a built-in shallow copy mechanism (Python's `copy.copy`, Java's `Object.clone()`). Deep copy typically requires custom logic or serialisation-then-deserialisation. GfG warns: "deep copy implementation can be complicated" and cautions against managing references to avoid shared state.

## When to Use

- You need to copy an object but cannot depend on its concrete class (e.g. you hold it by interface).
- You want to copy subclasses polymorphically — calling `clone()` always returns the correct subtype.
- Constructing a new object from scratch is expensive or complex, and a pre-configured prototype can be cloned at lower cost.
- You need a small variation on a pre-configured object — clone a prototype and tweak the differing fields, rather than building from zero.
- You are building a prototype registry where named configurations can be looked up and duplicated on demand.
- You want to reduce the number of subclasses by using pre-configured prototypes as an alternative to a subclass hierarchy that differs only in initialisation.

## Trade-offs

**Pros:**
- Clones objects without coupling to their concrete classes.
- Reduces repeated initialisation code for objects that differ only in configuration.
- Provides an alternative to subclassing for varying behaviour — configure different prototypes at runtime rather than creating subclasses.
- Can significantly reduce instantiation cost for expensive objects.
- Objects of the same class can access each other's private members, making `clone()` implementable without exposing fields.

**Cons / pitfalls:**
- Deep copying objects with complex object graphs (circular references, shared sub-trees) is hard to implement correctly.
- Each ConcretePrototype class must implement `clone()` — in Java this means implementing `Cloneable` and dealing with its quirks (`clone()` is `protected` on `Object` and must be overridden; `CloneNotSupportedException` must be handled).
- Private fields of parent classes may be inaccessible if `clone()` is implemented in a subclass without the parent providing its own `clone()` support.
- Cloning can create unexpected aliasing bugs if the developer assumes a deep copy but only gets a shallow one.

## Variants

**Prototype Registry:** A centralised map from string keys (or enum values) to pre-configured prototype instances. Clients call `registry.get("large-red-circle")` and get a clone. This moves object creation knowledge out of client code into the registry.

**Virtual copy constructor (C++):** A virtual `clone()` method on a base class serves the same purpose as the pattern, and is idiomatic C++ for polymorphic copying.

**Serialise-then-deserialise deep copy:** Serialize the object to bytes (JSON, pickle, Java serialisation) and deserialise it into a fresh instance. Guaranteed deep copy with no custom per-field logic, at the cost of serialisation overhead.

## Code Example

Python — shape cloning with prototype registry:

```python
import copy
from abc import ABC, abstractmethod

class Shape(ABC):
    def __init__(self, color: str):
        self.color = color

    @abstractmethod
    def clone(self):
        pass

    @abstractmethod
    def draw(self):
        pass

class Circle(Shape):
    def __init__(self, color: str, radius: float):
        super().__init__(color)
        self.radius = radius

    def clone(self):
        return copy.deepcopy(self)

    def draw(self):
        print(f"Drawing a {self.color} circle with radius {self.radius}.")

class Rectangle(Shape):
    def __init__(self, color: str, width: float, height: float):
        super().__init__(color)
        self.width = width
        self.height = height

    def clone(self):
        return copy.deepcopy(self)

    def draw(self):
        print(f"Drawing a {self.color} rectangle {self.width}x{self.height}.")

# Prototype Registry
registry: dict[str, Shape] = {
    "small-red-circle":   Circle("red", 5.0),
    "large-blue-circle":  Circle("blue", 50.0),
    "wide-green-rect":    Rectangle("green", 100.0, 20.0),
}

# Client — clones without knowing the class
c1 = registry["small-red-circle"].clone()
c1.color = "orange"  # mutate only the clone

registry["small-red-circle"].draw()  # Drawing a red circle with radius 5.0.
c1.draw()                            # Drawing a orange circle with radius 5.0.
```

Java — explicit clone implementation:

```java
public interface Shape {
    Shape clone();
    void draw();
}

public class Circle implements Shape {
    private String color;

    public Circle(String color) { this.color = color; }

    @Override
    public Shape clone() {
        return new Circle(this.color);
    }

    @Override
    public void draw() {
        System.out.println("Drawing a " + color + " circle.");
    }
}
```

## Real-World Examples

- **Java `Object.clone()`** — every Java class inherits a shallow `clone()` method; implementing `Cloneable` opts the class into the protocol.
- **Python `copy` module** — `copy.copy()` (shallow) and `copy.deepcopy()` (deep) are the language's built-in prototype mechanism.
- **Document Management Systems** — word processors clone template documents to create new documents without rebuilding from scratch.
- **Game Engines** — complex characters and environment objects are cloned at spawn time instead of being re-initialised; particle systems clone a single particle prototype.
- **Spring Framework** — prototype-scoped beans (`@Scope("prototype")`) return a new clone on each `getBean()` call.
- **GUI frameworks** — toolbar buttons and palette items are frequently cloned from a master template.

## Related

- [[Factory Method Pattern]] — factory method can return a clone of a stored prototype rather than a freshly constructed object
- [[Abstract Factory Pattern]] — Abstract Factory can use prototypes internally to avoid subclassing every product
- [[Builder Pattern]] — Builder assembles a new object step-by-step; Prototype copies an existing one
- [[Singleton Pattern]] — a prototype registry entry is effectively a shared, reusable template (distinct from Singleton, which restricts instantiation)
- Memento — Prototype is simpler than Memento for straightforward state storage/copying needs
