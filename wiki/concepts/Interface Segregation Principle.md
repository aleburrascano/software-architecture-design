---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "https://www.geeksforgeeks.org/system-design/interface-segregation-principle/"
  - "https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/"
tags:
  - design-principle
  - solid
---

# Interface Segregation Principle

Clients should not be forced to depend on interfaces they do not use.

## Problem

When an interface groups too many operations — operations that are unrelated or that only a subset of clients need — every implementor must provide all of them. This creates two specific pains:

1. **Implementors** must write stubs or throw `UnsupportedOperationException` for methods they don't support, polluting the class with dead code and signaling a broken abstraction.
2. **Clients** are coupled to parts of the interface they never call. A change to a method that client A uses forces a recompile (or re-test) of client B, even though client B doesn't use that method — a needless coupling.

A "fat interface" forces irrelevant dependencies on clients and signals that the abstraction is not cohesive. Large interfaces also make mocking harder in tests — a test that only needs one method must still provide implementations for the entire interface.

## Statement

> "Clients should not be forced to implement interfaces they don't use."
> — Robert C. Martin

Equivalently: prefer many narrow, client-specific interfaces over one large, general-purpose interface.

## Explanation

ISP is the interface-level analogue of the [[Single Responsibility Principle]]. Just as SRP says a class should have one reason to change, ISP says an interface should describe a cohesive role — one that any given client either needs in full or not at all.

The remedy is **interface splitting**: take the fat interface and decompose it into role interfaces, each representing a coherent capability. Clients declare dependencies on only the role(s) they need. Implementors implement only the role(s) their concrete type actually supports.

This makes the system easier to test: instead of mocking a giant interface, tests depend on a tiny one with only the one method they care about.

### Role interfaces vs. header interfaces

Martin Fowler distinguishes **role interfaces** (small, client-specific) from **header interfaces** (one fat interface per class, mirroring its entire public surface). ISP argues for role interfaces: each interface is shaped by what a *client* needs, not by what an *implementor* happens to offer.

### ISP is language-agnostic

In Python, "interfaces" are protocols or abstract base classes; in Go, they are implicit structural interfaces; in Java/C#, they are explicit interface types. The principle applies equally in all cases.

### Real-world analogy

A restaurant menu: a vegetarian customer who receives a complete menu (vegetarian, non-vegetarian, drinks, desserts) must filter through irrelevant options every time. Splitting into separate menus — vegetarian, non-vegetarian, drinks — means each customer sees only what is relevant to them. Changes to the non-vegetarian menu do not affect the vegetarian menu. The vegetarian customer has no dependency on meat dishes.

## Example

### Violation — fat Worker interface

```python
from abc import ABC, abstractmethod

class Worker(ABC):
    @abstractmethod
    def work(self): ...

    @abstractmethod
    def eat(self): ...    # not all workers eat (e.g., robots)

    @abstractmethod
    def sleep(self): ...  # not all workers sleep


class HumanWorker(Worker):
    def work(self): print("working")
    def eat(self): print("eating")
    def sleep(self): print("sleeping")

class RobotWorker(Worker):
    def work(self): print("working")
    def eat(self): raise NotImplementedError  # forced stub — broken abstraction
    def sleep(self): raise NotImplementedError
```

`RobotWorker` is forced to provide stubs for methods that make no sense for it. Any client depending on `Worker` could accidentally call `robot.eat()` and crash at runtime.

### Conforming — segregated role interfaces

```python
from abc import ABC, abstractmethod

class Workable(ABC):
    @abstractmethod
    def work(self): ...

class Feedable(ABC):
    @abstractmethod
    def eat(self): ...

class Restable(ABC):
    @abstractmethod
    def sleep(self): ...


class HumanWorker(Workable, Feedable, Restable):
    def work(self): print("working")
    def eat(self): print("eating")
    def sleep(self): print("sleeping")

class RobotWorker(Workable):
    def work(self): print("working")
    # No stubs needed — robots simply don't implement Feedable or Restable
```

Clients that only care about `Workable` depend on only that interface, regardless of whether the concrete object behind it is human or robot.

### Java example — read/write repository split

A common application of ISP in repository patterns: not all consumers need both read and write access.

```java
// Fat interface — violation
interface UserRepository {
    User findById(int id);
    List<User> findAll();
    void save(User user);
    void delete(int id);
}

// Clients that only read are forced to depend on save/delete

// ISP-conforming: split by role
interface UserReader {
    User findById(int id);
    List<User> findAll();
}

interface UserWriter {
    void save(User user);
    void delete(int id);
}

// Read-only consumers depend only on UserReader
class UserReportService {
    private final UserReader reader;
    public UserReportService(UserReader reader) { this.reader = reader; }
    // ...
}

// Full repository implements both for persistence layer
class MySQLUserRepository implements UserReader, UserWriter {
    // ...
}
```

The `UserReportService` has no compile-time or test-time dependency on `save` or `delete` — a change to write operations cannot affect it.

## Common Violations

- **One interface per module with dozens of unrelated methods** — common in service layer interfaces that aggregate every operation a class exposes.
- **"Do-it-all" repository interfaces** that mix read and write operations when some clients are read-only consumers.
- **Callback/listener interfaces** with many event methods where most handlers only care about one event (e.g., a 15-method `MouseListener` when only `mouseClicked` is needed).
- **Implementing with stubs/no-ops**: when a class implements an interface and leaves several methods empty or throwing `NotImplementedError`, ISP is almost certainly violated.

## Trade-offs

- **Interface proliferation**: splitting aggressively can produce many tiny interfaces, each with one method, which adds cognitive overhead when a developer needs to understand all capabilities of a type.
- **Implementation verbosity**: in languages without multiple interface inheritance (or where it is awkward), a class implementing many role interfaces becomes syntactically noisy.
- **Over-splitting stable interfaces**: if clients always use a group of methods together, splitting the interface adds no real value and only increases the number of files to manage.
- **Increased coordination**: when an implementor changes (e.g., a new method is added), if many role interfaces exist, deciding which one it belongs in requires more deliberate design work.

## Related

- [[SOLID Principles]] — ISP is the "I" in SOLID
- [[Single Responsibility Principle]] — ISP applies SRP thinking to interface design
- [[Liskov Substitution Principle]] — ISP-compliant narrow interfaces make LSP easier to satisfy (less contract to violate)
- [[Dependency Inversion Principle]] — ISP and DIP together define the structure of good dependency graphs
