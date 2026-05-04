---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "https://www.geeksforgeeks.org/system-design/dependency-injectiondi-design-pattern/"
  - "https://martinfowler.com/articles/injection.html"
tags:
  - design-pattern
  - design-principle
---

# Dependency Injection

A technique in which an object receives its dependencies from an external source rather than creating them itself.

## Problem

When a class creates its own dependencies internally (`self.repo = MySQLRepository()`), it is tightly coupled to the concrete implementation. This makes the class:

- **Hard to test**: tests must use the real dependency (database, network, file system) rather than a controlled substitute.
- **Hard to swap**: changing the implementation requires editing the consuming class's source.
- **Hard to configure**: different environments (dev, test, prod) require different implementations but get the same hardcoded one.
- **Opaque**: dependencies hidden inside a class are not visible at its API surface, making the class harder to reason about.
- **Tightly bound to a concrete implementation**: the class cannot work with alternative implementations without modification.

## Statement

Dependency Injection (DI) is the practice of providing a class's dependencies through its interface (constructor, method parameters, or property setters) rather than letting the class instantiate them.

DI is the primary *technique* used to satisfy the [[Dependency Inversion Principle]] at runtime.

## Explanation

### The four roles in a DI setup

Every DI arrangement involves four distinct roles:

1. **Client**: the class that needs a dependency. It declares *what* it needs but does not create it.
2. **Service**: the class that provides functionality (e.g., a database repository, a mailer).
3. **Interface**: the contract (abstract class or protocol) that the client depends on, and the service implements.
4. **Injector**: the code responsible for creating the service and supplying it to the client — this might be a DI container, a factory function, or simple wiring code in `main()`.

### The three canonical forms of DI

#### 1. Constructor Injection (preferred)

```python
class OrderService:
    def __init__(self, repo: IOrderRepository, mailer: IMailer):
        self.repo = repo
        self.mailer = mailer
```

Dependencies are declared in the constructor. They are immutable after construction. The class's dependencies are fully visible at its API surface. Martin Fowler recommends starting here because it creates valid objects at construction time and clearly communicates that the dependencies are required and immutable.

#### 2. Setter Injection (optional dependencies)

```python
class OrderService:
    def __init__(self): ...

    def set_notifier(self, notifier: INotifier) -> None:
        self.notifier = notifier
```

Useful when a dependency is optional or when circular dependencies prevent constructor injection. Fowler's caveat: switch to setters when constructors become unwieldy (too many parameters or complex inheritance hierarchies). Drawback: the object may be in an invalid state between construction and the setter call.

#### 3. Interface / Method Injection (parameter injection)

```python
def process_order(order: dict, repo: IOrderRepository) -> None:
    repo.save(order)
```

The dependency is passed as a function argument. Common in functional programming styles and for dependencies that vary per-call rather than per-object lifetime. Also called "parameter injection" when scoped to individual method calls.

### DI Containers / IoC Containers

In large applications, wiring together many classes manually becomes verbose. DI containers (Spring in Java, .NET's `IServiceCollection`, Python's `dependency-injector`, Guice) automate the wiring:

1. You register implementations against interfaces.
2. The container resolves the full dependency graph automatically when a root object is requested.

DI containers are useful at scale but are not required for DI itself — DI is a coding pattern, not a framework.

### DI vs. Service Locator

A **Service Locator** is a global registry: the class calls `locator.get(IMailer)` to fetch its dependency. This is DI's closest anti-pattern. The difference:

- **DI**: the class *receives* its dependencies from outside. Dependencies are declared at the API surface (constructor signature), discoverable and explicit.
- **Service Locator**: the class *asks* a global object for its dependencies. Dependencies are hidden inside the class body, not visible from the outside.

Fowler's key insight: DI is preferable for reusable components because it eliminates the component's dependency on the locator itself. Service Locator can work for application-specific configurations where its simplicity is a genuine advantage.

### DI vs. DIP vs. IoC

These three terms are often conflated:
- **Dependency Inversion Principle (DIP)**: the design principle — depend on abstractions, not concretions.
- **Inversion of Control (IoC)**: the broader principle — the framework/container controls the flow, calling your code rather than your code calling the framework.
- **Dependency Injection (DI)**: the specific technique — a form of IoC where dependencies are *injected* into an object from outside.

DI implements DIP. IoC is the broader category that includes DI as well as other patterns (Template Method, event-driven inversion, etc.).

Fowler's clarification: "inversion of control" is not unique to DI containers — it is a general characteristic of frameworks. What DI inverts specifically is *how dependencies are looked up*: rather than components instantiating implementations directly, an assembler module injects them, breaking the hard coupling.

### The overarching principle

More important than the specific DI form chosen: **separate the configuration of services from their use within an application**. This enables flexible deployment (different implementations for different environments) without conditional logic scattered throughout the codebase.

## Example

### Without DI — tightly coupled

```python
class NotificationService:
    def __init__(self):
        self.sender = SMTPEmailSender()   # hardcoded — impossible to swap in tests

    def notify(self, user: dict, msg: str) -> None:
        self.sender.send(user["email"], msg)
```

### With DI — loosely coupled

```python
from abc import ABC, abstractmethod

class IMessageSender(ABC):
    @abstractmethod
    def send(self, address: str, message: str) -> None: ...

class SMTPEmailSender(IMessageSender):
    def send(self, address: str, message: str) -> None:
        # real SMTP logic
        ...

class SMSSender(IMessageSender):
    def send(self, address: str, message: str) -> None:
        # real SMS logic
        ...

class FakeSender(IMessageSender):
    def __init__(self): self.sent: list = []
    def send(self, address: str, message: str) -> None:
        self.sent.append((address, message))  # records calls for assertions

class NotificationService:
    def __init__(self, sender: IMessageSender):   # dependency injected
        self.sender = sender

    def notify(self, user: dict, msg: str) -> None:
        self.sender.send(user["email"], msg)

# Production — inject real implementation
service = NotificationService(SMTPEmailSender())

# Test — inject fake, no network required
fake = FakeSender()
service = NotificationService(fake)
service.notify({"email": "a@b.com"}, "hello")
assert fake.sent == [("a@b.com", "hello")]
```

The `NotificationService` is completely decoupled from the sending mechanism. Switching to SMS requires only creating a new injector call — `NotificationService(SMSSender())` — with zero changes to `NotificationService`.

## Common Violations

- **`new` inside constructors or methods**: `self.thing = ConcreteClass()` is a DI smell — the class controls its own dependency creation.
- **Service Locator**: a global registry that the class calls to fetch its dependencies. This is a DI anti-pattern — dependencies are hidden rather than declared.
- **Static method calls** on concrete infrastructure classes from within business logic (e.g., `Database.query(...)` called directly from a service).
- **Hard-coded configuration values** inside classes rather than injected through the constructor.
- **Constructor injection with too many parameters**: a constructor with 8+ injected dependencies is a signal that the class has too many responsibilities ([[Single Responsibility Principle]] violation), not a DI problem per se.

## Trade-offs

- **Boilerplate**: constructor injection with many dependencies produces verbose constructors. This often signals a SRP violation (too many dependencies = too many responsibilities) rather than a DI problem.
- **Indirection**: a dependency graph resolved by a container can be opaque — it is not obvious which concrete class is injected without inspecting container configuration.
- **Runtime errors**: misconfigured dependencies may not surface until runtime, unlike hardcoded dependencies which fail at compile time.
- **Framework coupling**: when using a DI container, annotations and registration boilerplate can couple domain code to the framework.
- **Over-injection of trivials**: not every dependency needs to be injected. `datetime.now()` or `uuid.uuid4()` are sometimes injected for testability but this can be overkill — consider whether the test actually needs control over those values.

## Related

- [[Dependency Inversion Principle]] — DIP is the principle; DI is its primary implementation technique
- [[Open-Closed Principle]] — DI enables OCP by making it easy to swap implementations
- [[Single Responsibility Principle]] — a class with too many injected dependencies likely has too many responsibilities
- [[Composition over Inheritance]] — DI is the mechanism for composing objects at runtime
- [[Interface Segregation Principle]] — ISP shapes the interfaces that are injected
