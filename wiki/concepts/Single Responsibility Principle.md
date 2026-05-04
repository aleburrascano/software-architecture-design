---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "raw/Software design.md"
  - "https://www.geeksforgeeks.org/system-design/single-responsibility-in-solid-design-principle/"
  - "https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html"
  - "https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/"
tags:
  - design-principle
  - solid
---

# Single Responsibility Principle

A class or module should have one, and only one, reason to change.

## Problem

When a class or module bundles multiple responsibilities together, changes to one concern ripple unexpectedly into another. A class that handles both business logic and database persistence means that tweaking the persistence layer can silently break the business rules — and vice versa. Multi-purpose classes (sometimes called "god objects") are also difficult to unit-test in isolation, hard to reuse in different contexts, and become a magnet for unrelated changes over time, growing into unmaintainable blobs.

Practical consequences of SRP violations:
- **Merge conflicts**: when two developers need to change the same class for unrelated reasons simultaneously.
- **Broad test scope**: changing one responsibility forces re-running tests for all responsibilities bundled in the class.
- **Inability to reuse**: a class that mixes persistence with business logic cannot be reused in a context without a database.
- **Unexpected breakage**: fixing a report format bug breaks payroll calculations because both live in the same class.

## Statement

> "A class should have only one reason to change."
> — Robert C. Martin

Equivalently: every module, class, or function should be responsible for a single, well-defined piece of functionality, and that responsibility should be entirely encapsulated within it.

## Explanation

### "Reason to change" means "actor"

Uncle Bob's most important clarification: a "reason to change" is not a technical concern but an *organizational* one. Changes originate from specific stakeholders or business functions. If two distinct actors — say, the CFO and the CTO — could each independently require a change to the same class, that class has two responsibilities and therefore two reasons to change.

The canonical example is an `Employee` class with three methods:

```java
public class Employee {
    public Money calculatePay();   // CFO's domain — financial calculations
    public void save();            // CTO's domain — database operations
    public String reportHours();   // COO's domain — operational auditing
}
```

When the CFO requests a change to `calculatePay`, the developer edits the `Employee` class — and risks breaking `reportHours` or `save`, which the COO and CTO depend on. The class has three reasons to change because it serves three actors.

### Cohesion framing

SRP is closely related to **high cohesion**: a cohesive module groups elements that change for the same reason and separates elements that change for different reasons. Uncle Bob's alternative formulation: *"Gather together the things that change for the same reasons. Separate those things that change for different reasons."* This is precisely the definition of cohesion applied to organizational actors.

### Granularity

SRP applies at every level:
- **Functions** should do one thing.
- **Classes** should own one concern.
- **Modules/packages** should address one domain area.

At each level, "one thing" must be calibrated to the right granularity. A function that calls three other functions may still "do one thing" at its level of abstraction if those three calls form a coherent single operation.

### Real-world analogy

A bakery: a baker who focuses exclusively on baking maintains clear responsibility. A baker who also manages inventory, serves customers, and orders supplies has multiple responsibilities — and changes to the store's customer service policy force changes to the baker's behavior. Separating roles (baker, inventory manager, customer service rep) means each role changes only when its own domain requirements change.

## Example

### Violation

```python
class UserService:
    def get_user(self, user_id: int) -> dict:
        # business logic
        ...

    def save_user(self, user: dict) -> None:
        # database persistence — different responsibility
        conn = connect("postgres://...")
        conn.execute("INSERT INTO users ...")

    def send_welcome_email(self, user: dict) -> None:
        # notification — yet another responsibility
        smtp.send(user["email"], "Welcome!")
```

`UserService` has three reasons to change: the business rules for fetching a user, the persistence strategy, and the notification mechanism. A change to the database schema forces changes here; a change to the email template also forces changes here — entirely unrelated actors drive changes to the same class.

### Conforming

```python
class UserRepository:
    def save(self, user: dict) -> None:
        conn = connect("postgres://...")
        conn.execute("INSERT INTO users ...")

class EmailNotifier:
    def send_welcome(self, user: dict) -> None:
        smtp.send(user["email"], "Welcome!")

class UserService:
    def __init__(self, repo: UserRepository, notifier: EmailNotifier):
        self.repo = repo
        self.notifier = notifier

    def register(self, user: dict) -> None:
        self.repo.save(user)
        self.notifier.send_welcome(user)
```

Each class now has exactly one reason to change. `UserRepository` changes only when the persistence strategy changes. `EmailNotifier` changes only when notification behavior changes. `UserService` changes only when the registration *business workflow* changes.

### Java example — Invoice class

```java
// Violation: one class, three responsibilities
public class Invoice {
    public void addInvoice() { /* data manipulation */ }
    public void deleteInvoice() { /* data manipulation */ }
    public void generateReport() { /* reporting */ }
    public void emailReport() { /* notification */ }
}

// Conforming: three classes, three responsibilities
public class Invoice {
    public void addInvoice() { ... }
    public void deleteInvoice() { ... }
}

public class Report {
    public void generateReport() { ... }
}

public class Email {
    public void emailReport() { ... }
}
```

## Common Violations

- **God classes** — a single class that manages data, validation, persistence, and presentation.
- **Utility/Helper classes** — catch-all `Utils` or `Helpers` files where unrelated functions accumulate.
- **Mixed layers in a single class** — business logic embedded alongside SQL queries or HTTP calls.
- **Report-generation classes** — classes that both compute data *and* format/render it.
- **"Manager" or "Handler" classes** — vague names that often signal a multi-responsibility object.

## Trade-offs

- **Over-fragmentation**: taken to an extreme, SRP can produce an explosion of tiny classes with trivial responsibilities, making the codebase harder to navigate. "One reason to change" requires judgment about the right level of granularity.
- **Increased coordination cost**: splitting responsibilities across classes means callers must coordinate more objects, which can increase boilerplate and constructor complexity. [[Dependency Injection]] helps manage this.
- **Premature splitting**: early in a project, it can be hard to predict which concerns will diverge. Splitting prematurely may create boundaries in the wrong places. Let two different actors actually diverge before splitting.

## Related

- [[SOLID Principles]] — SRP is the "S" in SOLID
- [[Open-Closed Principle]] — OCP often depends on well-separated responsibilities
- [[Dependency Injection]] — the practical mechanism for composing SRP-respecting classes
- [[DRY Principle]] — DRY encourages consolidating a concern into one place, complementing SRP
- [[Composition over Inheritance]] — composition is a natural companion when splitting responsibilities
