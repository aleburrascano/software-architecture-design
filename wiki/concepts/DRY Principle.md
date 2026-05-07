---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/articles/What Makes a Good Software Design Mindset.md"
  - "raw/articles/Software design.md"
  - "https://www.geeksforgeeks.org/software-engineering/dont-repeat-yourselfdry-in-software-development/"
tags:
  - design-principle
---

# DRY Principle

Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

## Problem

When the same logic, data, or knowledge is duplicated across a codebase, changes must be made in every copy. Missing even one copy produces inconsistency and bugs. Duplication also inflates codebase size, increases cognitive load (a reader must recognize that two pieces of code are *meant* to be identical), and makes refactoring harder. Duplication is often a symptom of low cohesion: the same concept is scattered rather than owned by one place.

Real costs of duplication:
- **Inconsistency**: a bug fix applied to one copy but missed in another leaves the system in a split-brained state.
- **Change amplification**: every requirement change requires hunting down all copies.
- **Testing overhead**: tests for duplicated logic must themselves be duplicated or they provide incomplete coverage.
- **Onboarding confusion**: new developers cannot tell whether two similar-looking pieces of code are intentionally identical or accidentally similar.

## Statement

> "Don't Repeat Yourself."
> — Andy Hunt and Dave Thomas, *The Pragmatic Programmer* (1999)

The full formulation: "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system." The word *knowledge* is important — DRY is not just about not copy-pasting code, but about not duplicating the *meaning* behind code.

## Explanation

DRY has a specific scope: it targets duplicated *knowledge* (business rules, logic, algorithms, configuration), not merely syntactic repetition. Two functions that happen to have similar structure but represent entirely different concepts are *not* a DRY violation — prematurely merging them would be [[YAGNI Principle|YAGNI]] waste or introduce harmful coupling.

### WET code

The opposite of DRY is sometimes called **WET**: "Write Everything Twice" or "We Enjoy Typing." WET code has the same calculation appearing in three service classes, the same validation rule in both the frontend and backend, or the same SQL query copy-pasted across five repositories.

### Resolving duplication — four techniques

1. **Extract a function or method**: identify repeated logic and encapsulate it in a single callable unit.
2. **Extract a constant**: a hardcoded value repeated in multiple locations becomes a named constant in one place.
3. **Use classes and inheritance (or composition)**: for more complex repeated behaviour, a shared class owns it.
4. **Modularize**: break the system into cohesive modules each owning a specific piece of knowledge; other modules call through the module's API rather than reimplementing its logic.

### DRY in non-code artifacts

DRY applies beyond source code:
- **Configuration**: a database URL defined in three config files rather than one.
- **Documentation**: a process documented in the wiki, the README, and a Confluence page — now three places to update when the process changes.
- **Database schema and application models**: ORMs can violate DRY if a business rule is encoded both in a database constraint and separately in application code.
- **Validation logic**: the same email format check implemented in the frontend, the API controller, and the domain service layer.

### DRY and SRP together

The [[Single Responsibility Principle]] and DRY are complementary. SRP ensures a class or module has only one reason to change; DRY ensures that reason to change exists in only one place in the codebase. Together they reduce ripple effects when requirements change and make refactoring safer.

### When duplication is acceptable

Not all repetition is a DRY violation. Duplicating code across **test** and **production** contexts, or duplicating across **bounded contexts** in a microservices system, is often intentional — coupling them would create worse problems than the duplication. The DRY principle applies within a context of shared knowledge, not across independently evolving systems.

## Example

### Violation — duplicated business rule

````nclass InvoiceService:
    def get_total(self, items: list) -> float:
        subtotal = sum(i["price"] * i["qty"] for i in items)
        return subtotal * 1.20   # 20% VAT hardcoded

class QuoteService:
    def get_total(self, items: list) -> float:
        subtotal = sum(i["price"] * i["qty"] for i in items)
        return subtotal * 1.20   # same VAT logic — duplicated knowledge
```

If the VAT rate changes to 22%, it must be changed in both places. If a developer misses one copy, the system produces inconsistent totals.

### Conforming — single authoritative source

````nVAT_RATE = 1.20   # single authoritative source

def calculate_total(items: list) -> float:
    subtotal = sum(i["price"] * i["qty"] for i in items)
    return subtotal * VAT_RATE

class InvoiceService:
    def get_total(self, items: list) -> float:
        return calculate_total(items)

class QuoteService:
    def get_total(self, items: list) -> float:
        return calculate_total(items)
```

The VAT rate and total calculation live in exactly one place. A single edit propagates everywhere.

### Violation — duplicated validation logic

````n# In the API controller
def create_user(data):
    if "@" not in data["email"] or "." not in data["email"]:
        raise ValueError("Invalid email")
    ...

# In the domain service
def register_user(email, password):
    if "@" not in email or "." not in email:
        raise ValueError("Invalid email")
    ...
```

### Conforming

````ndef is_valid_email(email: str) -> bool:
    return "@" in email and "." in email

# Both call the single shared function
def create_user(data):
    if not is_valid_email(data["email"]):
        raise ValueError("Invalid email")
    ...

def register_user(email, password):
    if not is_valid_email(email):
        raise ValueError("Invalid email")
    ...
```

## Common Violations

- **Copy-paste programming**: duplicating logic instead of extracting a shared function. The most visible form — two functions with identical or near-identical bodies.
- **Magic numbers and strings**: the same constant (a tax rate, a URL, a timeout) hard-coded in multiple locations.
- **Parallel class hierarchies**: two inheritance trees that mirror each other because behavior was duplicated rather than shared.
- **Repeated validation logic**: the same email format check in the model, the controller, and the API client.
- **Business rule repetition across layers**: the same business rule duplicated in controller, service, and DAO layers, causing inconsistency and tight coupling.
- **Hard-coded configuration in multiple files**: the same environment value or connection string set independently in several config files.
- **Database constraints re-implemented in application code** without a shared definition.

## Trade-offs

- **Wrong abstraction**: merging two pieces of code that *look* the same but represent different concepts produces a "wrong abstraction" — a function with too many parameters and awkward conditionals. Sandi Metz's observation: "duplication is far cheaper than the wrong abstraction."
- **Coupling**: DRY consolidation creates coupling. If two previously independent modules now share a function, a change to that function affects both. Sometimes the duplication was providing a useful independence boundary.
- **Premature abstraction**: extracting a shared utility from two similar-but-not-identical pieces of code too early can lead to a brittle shared component that needs to be split later.
- **Cross-service duplication in distributed systems**: in microservices, each service should own its data and logic. Sharing a library to avoid DRY violations can create tight deployment coupling between services.
- **Over-engineering risk**: applying DRY blindly to all syntactically similar code — especially in small or experimental codebases — can produce unnecessary abstractions that add complexity without benefit.

## Related

- [[KISS Principle]] — simplicity and DRY are complementary; duplicated code is rarely simple
- [[Single Responsibility Principle]] — SRP naturally reduces duplication by assigning each concern to one owner
- [[YAGNI Principle]] — the tension: don't abstract for hypothetical reuse (YAGNI), but do consolidate actual duplication (DRY)
- [[Composition over Inheritance]] — composition over inheritance is one technique for sharing behavior without repetition
