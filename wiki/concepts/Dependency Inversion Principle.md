---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "https://www.geeksforgeeks.org/system-design/dependency-inversion-principle-in-software-engineering/"
  - "https://www.geeksforgeeks.org/system-design/dependecy-inversion-principle-solid/"
  - "https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/"
tags:
  - design-principle
  - solid
---

# Dependency Inversion Principle

High-level modules should not depend on low-level modules; both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

## Problem

In traditional layered architectures, high-level business logic directly imports and instantiates low-level infrastructure classes (database drivers, HTTP clients, file readers). This tight coupling means:

- Changing the persistence technology requires rewriting business logic.
- Business logic cannot be tested without a real database.
- The direction of dependencies follows the flow of control — the high-level module is at the mercy of low-level implementation choices.
- Adding a new type of infrastructure (e.g., adding a new worker type, a new storage backend) requires modifying the high-level class.

The "inversion" refers to reversing this dependency flow: instead of the business layer depending on the database layer, both depend on an interface that the database layer implements. The high-level module *owns* the abstraction; the low-level module *conforms to* it.

## Statement

> 1. High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).
> 2. Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.
> — Robert C. Martin

## Explanation

Without DIP, dependency arrows in a codebase point "downward": `OrderService → MySQLOrderRepository → MySQL driver`. The business class (`OrderService`) is tightly coupled to the concrete database class.

DIP inverts this: `OrderService → IOrderRepository ← MySQLOrderRepository`. Both high-level and low-level modules now depend on the `IOrderRepository` abstraction. The concrete `MySQLOrderRepository` depends *on* the interface, not the other way around.

This has immediate benefits:

1. **Testability**: `OrderService` can be tested by injecting a fake/mock `IOrderRepository` — no real database required.
2. **Replaceability**: switching from MySQL to PostgreSQL (or to an in-memory store for testing) requires writing a new implementation of `IOrderRepository`, with zero changes to `OrderService`.
3. **Extensibility**: new worker types, storage backends, or notification channels can be added by writing a new implementation — the high-level module does not change.

### The ownership of the abstraction

A subtle but important point: in the DIP-compliant design, the abstraction (`IOrderRepository`) is owned by the *high-level module* (the business layer), not the low-level one. The low-level module conforms to an interface defined in terms of what the business layer needs. This is what the "inversion" means: the low-level module's design is driven by the high-level module's requirements, not the other way around.

### Practical example: Manager and employees

A `Manager` class that maintains separate lists for `Developer`, `Designer`, and `Tester` instances is a DIP violation — the high-level `Manager` depends on the concrete low-level worker types. Adding a new worker type (e.g., `QA`) requires modifying `Manager`.

With DIP: introduce an abstract `Employee` base class with a `work()` method. Each worker type implements `work()`. `Manager` holds a single `List<Employee>` and calls `employee.work()` on each — it never knows the concrete type.

### DIP is closely related to but distinct from Dependency Injection

- **DIP** is the *principle*: depend on abstractions, not concretions.
- **DI** is a *technique* for supplying those abstractions at runtime (constructor injection, setter injection, etc.).
- **IoC** is the broader category: the framework controls the flow, calling user code rather than user code calling the framework.

You can satisfy DIP without a DI framework — wiring can be done manually in `main()` or a factory. DI containers (Spring, .NET DI, Guice) automate this wiring at scale.

### Inversion of Control containers

IoC frameworks implement DIP automatically:
1. Register implementations against interfaces.
2. The container resolves the full dependency graph when a root object is requested.

IoC containers are the theoretical foundation of DIP applied at enterprise scale.

### Real-world analogy (version control)

A development team depends on an abstract version control interface (`commit()`, `push()`, `pull()`) rather than on Git's internal implementation. They use these methods without knowing Git's internal mechanics. If the company switched from Git to another VCS, only the concrete adapter would change — the team's workflow (high-level module) would not.

## Example

### Violation

```python
class MySQLUserRepository:
    def find(self, user_id: int) -> dict:
        # raw SQL query against MySQL
        ...

class UserService:
    def __init__(self):
        self.repo = MySQLUserRepository()   # hard dependency on concrete class

    def get_user(self, user_id: int) -> dict:
        return self.repo.find(user_id)
```

`UserService` cannot be tested without MySQL. Switching to PostgreSQL requires editing `UserService`.

### Conforming

```python
from abc import ABC, abstractmethod

class IUserRepository(ABC):
    @abstractmethod
    def find(self, user_id: int) -> dict: ...

class MySQLUserRepository(IUserRepository):
    def find(self, user_id: int) -> dict:
        # MySQL implementation
        ...

class InMemoryUserRepository(IUserRepository):
    def __init__(self): self._store = {}
    def find(self, user_id: int) -> dict:
        return self._store.get(user_id, {})

class UserService:
    def __init__(self, repo: IUserRepository):   # depends on abstraction
        self.repo = repo

    def get_user(self, user_id: int) -> dict:
        return self.repo.find(user_id)

# Production
service = UserService(MySQLUserRepository())

# Test — no database required
service = UserService(InMemoryUserRepository())
```

`UserService` is completely decoupled from the storage technology. The abstraction (`IUserRepository`) is owned by the business layer, not the infrastructure layer.

### Java example — Manager with workers

```java
// Violation: Manager depends on concrete types
class Manager {
    private List<Developer> developers = new ArrayList<>();
    private List<Designer> designers = new ArrayList<>();
    // Must be modified every time a new worker type is added
}

// DIP-conforming: Manager depends on abstraction
abstract class Employee {
    abstract void work();
}

class Developer extends Employee {
    void work() { System.out.println("Writing code"); }
}

class Designer extends Employee {
    void work() { System.out.println("Creating designs"); }
}

class QA extends Employee {                // new type — no Manager change needed
    void work() { System.out.println("Testing"); }
}

class Manager {
    private List<Employee> employees = new ArrayList<>();

    public void addEmployee(Employee e) { employees.add(e); }

    public void manageWork() {
        for (Employee e : employees) {
            e.work();   // calls through abstraction — works for any Employee subtype
        }
    }
}
```

## Common Violations

- **`new` inside business classes**: instantiating a concrete infrastructure class inside a domain/service class is almost always a DIP violation.
- **Static calls to concrete classes**: `Logger.info(...)` or `Database.query(...)` called directly from business logic.
- **Importing concrete modules**: in Python/JS, `from my_app.db.mysql import UserRepo` inside domain code.
- **Service Locator anti-pattern**: pulling dependencies from a global registry inside the class rather than receiving them through the constructor — technically satisfies DIP but hides dependencies.

## Trade-offs

- **Indirection cost**: every abstracted dependency introduces an extra level of indirection, making it harder to trace where a call actually goes during debugging.
- **Over-abstracting trivial dependencies**: not every dependency needs an interface. `UserService` depending directly on Python's `datetime` module is not a problem worth solving.
- **Interface proliferation**: combined with [[Interface Segregation Principle|ISP]], DIP can lead to many small interface files, increasing maintenance overhead in projects that don't benefit from the flexibility.
- **Framework overhead**: using a DI container adds a framework dependency and often requires annotations/decorators that couple code to the container.

## Related

- [[SOLID Principles]] — DIP is the "D" in SOLID
- [[Dependency Injection]] — the practical technique for implementing DIP at runtime
- [[Open-Closed Principle]] — DIP enables OCP: depend on abstractions so new implementations can be added without modification
- [[Interface Segregation Principle]] — ISP shapes the abstractions that DIP depends on
- [[Single Responsibility Principle]] — when classes have clear responsibilities, their dependencies are easier to abstract
