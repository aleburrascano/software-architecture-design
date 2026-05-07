---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-06'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/bridge
  - https://www.geeksforgeeks.org/system-design/bridge-design-pattern/
tags:
  - design-pattern
  - structural
  - gof
---
# Bridge Pattern

Decouples an abstraction from its implementation so that the two can vary independently, by replacing inheritance with composition.

## Problem

When a class has two orthogonal dimensions of variation — say, *shape* and *rendering backend*, or *notification type* and *notification channel*, or *vehicle type* and *workshop operation* — extending via inheritance produces a Cartesian product of subclasses. Three shapes × four renderers = twelve subclasses. Each new shape requires four new classes; each new renderer requires three. The hierarchy explodes, and the two concerns are permanently tangled.

The deeper issue: inheritance binds abstraction to implementation at compile time. You cannot swap implementations at runtime, and changes in one dimension ripple across the other.

A concrete example: combining shapes (`Circle`, `Square`) with colours (`Red`, `Blue`) creates four classes. Adding `Triangle` requires two more classes (six total). Adding `Green` requires three more. Each new dimension multiplies the class count.

## Solution

Split the class into two separate hierarchies:

1. **Abstraction hierarchy** — the high-level control layer. It holds a reference to an *implementor* object and delegates platform-specific or dimension-specific work to it.
2. **Implementation hierarchy** — the low-level concrete implementations.

The abstraction communicates through the implementor's interface, so both hierarchies can grow independently. The field in Abstraction that holds the Implementor reference is the *bridge*.

```
interface Implementor:
    operationImpl()

class ConcreteImplementorA implements Implementor:
    operationImpl(): ...

class ConcreteImplementorB implements Implementor:
    operationImpl(): ...

class Abstraction:
    impl: Implementor           // the bridge

    operation():
        impl.operationImpl()

class RefinedAbstraction(Abstraction):
    operation():
        // may call impl differently, add pre/post logic
```

## Structure

| Participant | Role |
|---|---|
| Abstraction | High-level control layer; defines high-level interface; holds the bridge reference to Implementor |
| RefinedAbstraction | Extends Abstraction; provides variants of control logic |
| Implementor | Interface for the implementation hierarchy; does not mirror Abstraction's interface |
| ConcreteImplementor | Provides platform-specific or dimension-specific implementation |
| Client | Associates Abstraction with an Implementor and works through the Abstraction interface |

The key structural element is the field `impl: Implementor` inside Abstraction — this is the bridge.

## When to Use

- A class has two (or more) orthogonal dimensions of variation, and you want each dimension to be extensible independently.
- You want to avoid a permanent binding between abstraction and implementation at compile time (e.g. to switch implementations at runtime).
- Both abstraction and implementation should be extensible via subclassing without combinations multiplying.
- You want to hide implementation details from clients completely.
- You need to split a monolithic class that has multiple functional variants — identifying the two dimensions first.

## Trade-offs

**Pros:**
- Eliminates the Cartesian-product subclass explosion.
- Open/Closed Principle: add new abstractions or implementations without modifying the other hierarchy.
- Single Responsibility Principle: each hierarchy handles exactly one concern.
- Runtime switching of implementations is straightforward — swap the `impl` reference.
- Hides platform-specific details from higher layers; enables platform-independent client code.

**Cons / pitfalls:**
- Adds indirection and two hierarchies to understand instead of one — overkill for simple cases.
- Requires upfront design insight to identify the two orthogonal dimensions; retrofitting an existing hierarchy is harder.
- Increased initial design complexity; can be harder to understand for newcomers.
- In highly cohesive cases, the abstraction/implementation split may feel artificial.

## Code Example

````nfrom abc import ABC, abstractmethod

# Implementation hierarchy
class Workshop(ABC):
    @abstractmethod
    def work(self, item: str): ...

class Produce(Workshop):
    def work(self, item: str):
        print(f"Producing {item}", end=" ")

class Assemble(Workshop):
    def work(self, item: str):
        print(f"Assembling {item}")

# Abstraction hierarchy
class Vehicle(ABC):
    def __init__(self, workshop1: Workshop, workshop2: Workshop):
        self.workshop1 = workshop1   # the bridge
        self.workshop2 = workshop2

    @abstractmethod
    def manufacture(self): ...

class Car(Vehicle):
    def manufacture(self):
        print("Car: ", end="")
        self.workshop1.work("Car")
        self.workshop2.work("Car")

class Bike(Vehicle):
    def manufacture(self):
        print("Bike: ", end="")
        self.workshop1.work("Bike")
        self.workshop2.work("Bike")

# No subclass explosion: 2 vehicle types × 2 workshops, combined freely
Car(Produce(), Assemble()).manufacture()   # Car: Producing Car  Assembling Car
Bike(Produce(), Assemble()).manufacture()  # Bike: Producing Bike  Assembling Bike
```

Classic shapes example — decouples shape from renderer:

````nclass Renderer(ABC):
    @abstractmethod
    def render_circle(self, radius: float): ...

class VectorRenderer(Renderer):
    def render_circle(self, radius):
        print(f"Drawing circle r={radius} as vector")

class RasterRenderer(Renderer):
    def render_circle(self, radius):
        print(f"Drawing circle r={radius} as pixels")

class Circle:
    def __init__(self, renderer: Renderer, radius: float):
        self.renderer = renderer   # bridge
        self.radius = radius

    def draw(self):
        self.renderer.render_circle(self.radius)

Circle(VectorRenderer(), 5).draw()   # Drawing circle r=5 as vector
Circle(RasterRenderer(), 5).draw()   # Drawing circle r=5 as pixels
```

## Real-World Examples

- **Remote controls and devices** — a universal remote (`RemoteControl` abstraction) controls TVs, radios, or ACs (`Device` implementors) through a common interface without knowing device internals. Adding a new device type or a new remote variant is independent.
- **Java AWT / Swing** — the `Component` hierarchy (`Button`, `TextField`) is the abstraction; the platform-specific `Peer` hierarchy provides the rendering implementation (Windows, macOS, Linux). Swing components work across platforms without subclass multiplication.
- **JDBC** — the `Connection` / `Statement` abstraction hierarchy is independent of the database driver (`Driver`) implementation hierarchy. The same application code runs against MySQL, PostgreSQL, or Oracle by swapping the driver.
- **Logging frameworks (SLF4J)** — the `Logger` abstraction is decoupled from the backend `Appender`/handler implementation (Logback, Log4j, JDK logging). Applications program to the abstraction; the implementation is swapped via configuration.
- **Payment processing** — a `PaymentProcessor` abstraction (one-time charge, subscription) is decoupled from the concrete gateway implementation (Stripe, PayPal, Square).

## Related

- [[Adapter Pattern]] — also involves two interfaces, but Adapter reconciles incompatible *existing* interfaces after the fact; Bridge separates concerns *by design* from the start
- [[Abstract Factory Pattern]] — can create and configure a Bridge, selecting the implementor; encapsulates implementation relationships
- [[Strategy Pattern]] — structurally identical (object holds a reference to a swappable collaborator); Bridge focuses on structural decomposition across two independent dimensions, Strategy on behavioural substitution of algorithms
- [[State Pattern]] — similar structure to Bridge; State focuses on object behaviour changing based on internal state, Bridge focuses on separating two structural dimensions
