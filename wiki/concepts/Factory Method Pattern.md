---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/system-design/factory-method-for-designing-pattern/", "https://refactoring.guru/design-patterns/factory-method"]
tags: [design-pattern, creational, gof]
---

# Factory Method Pattern

Defines an interface for creating an object in a superclass but lets subclasses decide which concrete class to instantiate.

## Problem

A class needs to create objects, but the exact type of those objects should be determined by subclasses or by configuration — not hardcoded in the creator. If the creator uses `new ConcreteProduct()` directly, it is tightly coupled to that product class. Adding a new product variant means modifying the creator, violating the Open/Closed Principle.

Refactoring Guru's illustration: a logistics application initially handles only trucks. When sea transport must be added, every place that creates a `Truck` must be found and modified. The creation logic is scattered, conditional branches multiply, and adding a third transport type repeats the same pain.

GfG's illustration: an application must handle vehicles — TwoWheelers, ThreeWheelers, FourWheelers — without hardcoding instantiation in client code. Without the pattern, the client violates Single Responsibility and is fragile to extension.

The pattern separates the responsibility of *deciding what to create* from the responsibility of *using what was created*.

## Solution

Declare a factory method (`createProduct()`) in the creator base class (or interface). The base class calls the factory method wherever it needs a product object, but never calls `new` directly on a concrete type. Subclasses override the factory method to return their specific product. The creator's business logic operates on the abstract product interface, so it never needs to know the concrete type.

```
abstract class Creator:
    abstract createProduct() -> Product   // factory method

    operation():
        p = createProduct()               // uses factory method
        p.doWork()

class ConcreteCreatorA(Creator):
    createProduct() -> Product:
        return new ConcreteProductA()

class ConcreteCreatorB(Creator):
    createProduct() -> Product:
        return new ConcreteProductB()
```

Concrete Creators can also return cached or pooled objects from the factory method rather than always constructing new ones.

## Structure

| Participant | Role |
|---|---|
| Product | Interface (or abstract class) that all products must implement |
| ConcreteProduct | A specific product implementation |
| Creator | Declares the factory method; may provide a default implementation; contains business logic that uses Product |
| ConcreteCreator | Overrides the factory method to return a ConcreteProduct |

The Creator class often contains other methods that call `createProduct()` internally — those methods never reference concrete product types. The factory method's return type must match the Product interface.

## When to Use

- You don't know ahead of time which class your code should instantiate, and you want subclasses to make that decision.
- You want to provide a library or framework with a way to extend its internal components without subclassing everything.
- You need to reuse existing objects rather than re-creating them — a factory method can check a cache or pool before creating a new instance (combining with [[Object Pool Pattern]]).
- Products share a common interface, but the creator should remain decoupled from any specific implementation.
- The number of object types may grow over time — each new type needs only a new ConcreteCreator, not changes to existing code.

## Trade-offs

**Pros:**
- Eliminates tight coupling between the creator and concrete product classes.
- Follows the Open/Closed Principle: add new product types by introducing a new ConcreteCreator subclass, without touching existing code.
- Follows the Single Responsibility Principle: product creation logic lives in one place.
- Supports substituting product implementations for testing (inject a test creator that returns mocks).
- Encapsulates creation logic — clients never call `new` on a concrete type.

**Cons / pitfalls:**
- Can lead to a proliferation of Creator subclasses — one per product variant.
- If the factory method is the only variation point, introducing it may add unnecessary complexity for simple cases (a plain constructor or a static factory might suffice).
- Inheritance-based: the extension point is subclassing, which can be less flexible than composition-based alternatives like [[Abstract Factory Pattern]].

## Variants

**Parameterised factory method:** A single factory method accepts a parameter (string, enum) that selects which product to create. Avoids subclass explosion but weakens type safety.

**Static factory method:** The creator is not abstract; a static method on a class acts as the factory. Common in utility classes (e.g. `List.of(...)` in Java). Not technically a GoF Factory Method (no polymorphism), but solves similar coupling problems at a lower complexity cost.

**Default implementation:** The abstract creator provides a sensible default product; subclasses override only when they need a different type.

## Code Example

Python — Vehicle factory:

```python
from abc import ABC, abstractmethod

# Product Interface
class Vehicle(ABC):
    @abstractmethod
    def printVehicle(self):
        pass

# Concrete Products
class TwoWheeler(Vehicle):
    def printVehicle(self):
        print("I am two wheeler")

class FourWheeler(Vehicle):
    def printVehicle(self):
        print("I am four wheeler")

# Factory Interface (Creator)
class VehicleFactory(ABC):
    @abstractmethod
    def createVehicle(self):
        pass

# Concrete Factories (ConcreteCreators)
class TwoWheelerFactory(VehicleFactory):
    def createVehicle(self):
        return TwoWheeler()

class FourWheelerFactory(VehicleFactory):
    def createVehicle(self):
        return FourWheeler()

# Client — works through the abstract factory interface
class Client:
    def __init__(self, factory: VehicleFactory):
        self.pVehicle = factory.createVehicle()

    def getVehicle(self):
        return self.pVehicle

# Usage
factory = TwoWheelerFactory()
client = Client(factory)
client.getVehicle().printVehicle()  # I am two wheeler
```

## Real-World Examples

- **Java `java.util.Calendar.getInstance()`** — returns a locale-appropriate Calendar subclass without the caller knowing the concrete type.
- **Java `java.util.Iterator`** — `Collection.iterator()` is a factory method; each collection subclass returns its own iterator implementation.
- **Android OS** — the Activity framework uses factories to instantiate UI components dynamically.
- **Web browsers** (Chrome, Firefox) — use factory methods for creating renderer and plugin objects.
- **Payment gateways** (Stripe, PayPal) — create payment processor objects dynamically based on region or currency.
- **JDBC `DriverManager.getConnection()`** — returns a `Connection` object whose concrete type depends on the registered driver.
- **Game engines** — enemy spawners, item generators, NPC factories all follow this pattern.

## Related

- [[Abstract Factory Pattern]] — groups multiple related factory methods; use when products come in families; Factory Method often evolves into Abstract Factory as complexity grows
- [[Singleton Pattern]] — factories are often Singletons
- [[Prototype Pattern]] — factory method can return a clone of a prototype instead of a freshly constructed object
- [[Template Method Pattern]] — Factory Method is a specialisation of Template Method where the hook creates an object
- [[Object Pool Pattern]] — a factory method can return a pooled instance rather than always creating new ones
