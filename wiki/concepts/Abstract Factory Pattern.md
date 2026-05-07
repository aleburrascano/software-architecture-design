---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/system-design/abstract-factory-pattern/", "https://refactoring.guru/design-patterns/abstract-factory"]
tags: [design-pattern, creational, gof]
---

# Abstract Factory Pattern

Provides an interface for creating families of related or dependent objects without specifying their concrete classes.

## Problem

A system must create objects from a set of related product families, and it must be possible to switch the entire family at once. Two motivating examples from the sources:

**UI Toolkit (Refactoring Guru):** A furniture shop simulator needs chairs, sofas, and coffee tables in Modern, Victorian, or Art Deco styles. Mixing a Modern chair with a Victorian sofa produces an inconsistent result. If client code calls `new ModernChair()` directly, switching to Victorian requires touching every instantiation site.

**Regional car manufacturing (GfG):** A global car manufacturer must produce vehicles with region-specific configurations (North America vs. Europe). Each region has distinct regulatory requirements; mixing products from different regional specs violates compliance rules.

If the client directly instantiates concrete classes (`new LightButton()`, `new LightCheckbox()`), swapping families requires touching every instantiation site, and there is no compile-time guarantee that all products belong to the same family.

## Solution

Define an abstract factory interface with one creation method per product type in the family (`createButton()`, `createCheckbox()`). Provide a concrete factory class for each product family (`LightThemeFactory`, `DarkThemeFactory`). The client code holds a reference to the abstract factory interface and calls its creation methods — it never uses `new` on a concrete product. To switch families, replace the factory object; the client code is unchanged.

```
interface UIFactory:
    createButton()   -> Button
    createCheckbox() -> Checkbox

class LightUIFactory(UIFactory):
    createButton()   -> LightButton
    createCheckbox() -> LightCheckbox

class DarkUIFactory(UIFactory):
    createButton()   -> DarkButton
    createCheckbox() -> DarkCheckbox

// Client:
factory = LightUIFactory()   // or DarkUIFactory — decided at startup
btn     = factory.createButton()
chk     = factory.createCheckbox()
```

Implementation steps (Refactoring Guru): (1) map product types against variants in a matrix; (2) declare abstract product interfaces; (3) create abstract factory interface with one creation method per column; (4) implement concrete factories per row; (5) initialise the appropriate factory from configuration; (6) replace all direct constructor calls with factory method calls.

## Structure

| Participant | Role |
|---|---|
| AbstractFactory | Interface declaring creation methods for each product type |
| ConcreteFactory | Implements the abstract factory; creates products belonging to one family |
| AbstractProduct | Interface for each product type (e.g. `Car`, `CarSpecification`, `Button`, `Checkbox`) |
| ConcreteProduct | A specific product in a specific family (e.g. `Sedan` for NorthAmerica, `Hatchback` for Europe) |
| Client | Uses only the AbstractFactory and AbstractProduct interfaces; never references concrete types |

The Client is decoupled from both the factory implementation and the product implementations. Switching the family at runtime requires only replacing the factory reference.

## When to Use

- The system must be independent of how its products are created, composed, and represented.
- The system needs to work with multiple families of products, and families must be used consistently (no cross-family mixing).
- You want to enforce constraints between related objects — e.g. every widget in the UI must come from the same theme.
- You need to swap an entire product family at once (e.g. switching a persistence layer, UI theme, or platform-specific implementation).
- Systems supporting multiple platforms or regions requiring distinct implementations (cloud infrastructure: AWS vs. Azure vs. GCP; database: SQL vs. NoSQL).

## Trade-offs

**Pros:**
- Ensures products from the same family are used together — the factory itself enforces consistency.
- Decouples client code from concrete product classes completely.
- Adding a new family is easy: add a new ConcreteFactory and ConcreteProduct per type; no client code changes.
- Follows Open/Closed and Dependency Inversion Principles.
- Centralises product creation (Single Responsibility Principle).

**Cons / pitfalls:**
- Adding a *new product type* (a new creation method) to the abstract factory interface breaks all existing concrete factories — this is the inverse of [[Factory Method Pattern]]'s flexibility.
- Can produce many classes: for N factories × M product types you get N×M concrete product classes plus N factory classes.
- The factory object itself must be created somewhere — often with a [[Factory Method Pattern]] or a configuration mechanism.
- Introduces significant complexity through additional interfaces and classes; may be overkill for small projects.

## Variants

**Abstract Factory as interface vs abstract class:** Using an interface allows a concrete factory to implement multiple abstract factories. An abstract class can provide default implementations for some creation methods.

**Abstract Factory + Singleton:** Each concrete factory is usually a Singleton — there is rarely a reason to have more than one instance of `LightThemeFactory`.

**Abstract Factory + Prototype:** Instead of instantiating products in each factory method, the factory stores prototype instances and clones them (see [[Prototype Pattern]]). Useful when the number of product families is large and varies at runtime.

**Abstract Factory + Bridge:** The pattern pairs well with Bridge — the Director acts as the abstraction and concrete factories as implementations.

## Code Example

Python — regional car manufacturing:

````nfrom abc import ABC, abstractmethod

# Abstract Products
class Car(ABC):
    @abstractmethod
    def assemble(self):
        pass

class CarSpecification(ABC):
    @abstractmethod
    def display(self):
        pass

# Concrete Products — North America family
class Sedan(Car):
    def assemble(self):
        print("Assembling Sedan car.")

class NorthAmericaSpecification(CarSpecification):
    def display(self):
        print("North America: Safety features compliant with local regulations.")

# Concrete Products — Europe family
class Hatchback(Car):
    def assemble(self):
        print("Assembling Hatchback car.")

class EuropeSpecification(CarSpecification):
    def display(self):
        print("Europe: Fuel efficiency compliant with EU standards.")

# Abstract Factory
class CarFactory(ABC):
    @abstractmethod
    def create_car(self) -> Car: ...
    @abstractmethod
    def create_specification(self) -> CarSpecification: ...

# Concrete Factories
class NorthAmericaCarFactory(CarFactory):
    def create_car(self):
        return Sedan()
    def create_specification(self):
        return NorthAmericaSpecification()

class EuropeCarFactory(CarFactory):
    def create_car(self):
        return Hatchback()
    def create_specification(self):
        return EuropeSpecification()

# Client — only knows CarFactory and abstract products
def configure_production(factory: CarFactory):
    car = factory.create_car()
    spec = factory.create_specification()
    car.assemble()
    spec.display()

configure_production(NorthAmericaCarFactory())
configure_production(EuropeCarFactory())
```

## Real-World Examples

- **Java AWT/Swing** — `Toolkit.getDefaultToolkit()` returns a platform-specific factory for UI components (buttons, windows, scrollbars differ on Windows vs. macOS vs. Linux).
- **JDBC** — `DriverManager.getConnection()` + `Connection.createStatement()` form an abstract factory for database-specific objects (`Connection`, `Statement`, `ResultSet`).
- **Spring Framework** — `ApplicationContext` acts as an abstract factory for beans; `AnnotationConfigApplicationContext` vs. `ClassPathXmlApplicationContext` are concrete factories.
- **Cloud SDKs** — AWS SDK factory interfaces abstract over S3, DynamoDB, EC2; swapping to a mock factory enables local testing.
- **UI theming systems** — Light/Dark mode widget factories; cross-platform GUI toolkits (wxWidgets, Qt).

## Related

- [[Factory Method Pattern]] — Abstract Factory is often implemented using Factory Methods; use Factory Method when only one product type varies, Abstract Factory when a whole family varies; Factory Method often evolves into Abstract Factory as complexity grows
- [[Singleton Pattern]] — concrete factories are typically Singletons
- [[Prototype Pattern]] — can replace explicit product subclasses when families vary at runtime
- [[Builder Pattern]] — Builder constructs one complex object step-by-step; Abstract Factory returns related objects immediately
