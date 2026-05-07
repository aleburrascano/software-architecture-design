---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/articles/What Makes a Good Software Design Mindset.md"
  - "raw/articles/Software design.md"
  - "https://www.geeksforgeeks.org/system-design/open-closed-principle/"
  - "https://blog.cleancoder.com/uncle-bob/2014/05/12/TheOpenClosedPrinciple.html"
  - "https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/"
tags:
  - design-principle
  - solid
---

# Open-Closed Principle

Software entities should be open for extension, but closed for modification.

## Problem

When adding new behavior requires editing existing, already-tested code, every change risks introducing regressions. Teams that must touch stable production code to accommodate new requirements bear higher testing overhead and higher defect rates. The problem compounds as a codebase grows: a single frequently-modified class becomes a perpetual source of bugs and merge conflicts.

This is related to Martin Fowler's **Shotgun Surgery** code smell: when one logical change forces modifications scattered across many existing classes, the codebase is not organized around stable extension points — it is organized around fragile internals that break under change.

## Statement

> "Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."
> — Bertrand Meyer (1988); popularized in the OOP context by Robert C. Martin

**Open for extension** means behavior can be augmented.
**Closed for modification** means the existing source code of a stable entity is not changed to achieve that augmentation.

Meyer's original formulation was about compilation units and module stability. Uncle Bob's modern restatement captures the essential idea more directly: "You should be able to extend the behavior of a system without having to modify that system." New features are written as *new code*; existing code is untouched, requires no recompilation, and can be deployed independently.

## Explanation

The canonical technique for satisfying OCP is **abstraction**: define an interface (or abstract class) for the *stable* part of a behavior, and provide *new implementations* of that interface for each new variant.

When a new requirement arrives, instead of rewriting the existing class, you:
1. Ensure the existing code depends on an abstraction (not a concrete class).
2. Write a new class that implements the abstraction.
3. Inject or configure the system to use the new class.

This is directly related to the **Strategy pattern**: the original class doesn't know or care which concrete implementation it receives — it just calls through the abstraction.

### Plugin architecture as the ultimate OCP proof

Uncle Bob points to plugin systems as the strongest evidence that OCP is achievable at scale: Eclipse, IntelliJ IDEA, Visual Studio, Vim, and Minecraft all extend their behavior through plugins without modifying the core application. The core application does not even know plugins exist — dependencies point inward (plugins depend on the core API, not vice versa). This inversion of dependencies is what enables the open/closed behavior.

### OCP at the module level

OCP also applies at the module or package level: a package that is frequently extended by other packages should export a stable API that other packages depend on, rather than having other packages reach into its internals. Adding a new feature deploys as a separate jar, dll, or gem rather than as a patch to the core package.

### A note on "closed for modification"

"Closed" does not mean the code can *never* change — it means the code should not need to change *in response to a new type of extension*. Bug fixes, refactoring, and performance improvements are expected changes that do not violate OCP. The goal is that the system grows primarily by addition, not by modification.

### Real-world analogy

A payment processor: rather than modifying the original `PaymentProcessor` class each time a new payment method is added (PayPal, Apple Pay, cryptocurrency), the processor depends on an abstract payment interface. Each new payment method is a new class implementing that interface. The original processor code is never touched.

## Example

### Violation — if/elif chain on type tags

````nclass DiscountCalculator:
    def calculate(self, order: dict) -> float:
        if order["type"] == "regular":
            return order["total"] * 0.0
        elif order["type"] == "vip":
            return order["total"] * 0.10
        elif order["type"] == "employee":
            return order["total"] * 0.30
        # Every new customer type requires modifying this class
```

Adding a "student" discount requires editing `DiscountCalculator`, re-testing the whole method, and risking breakage of existing discount logic.

### Conforming — Strategy pattern

````nfrom abc import ABC, abstractmethod

class DiscountStrategy(ABC):
    @abstractmethod
    def discount(self, total: float) -> float: ...

class NoDiscount(DiscountStrategy):
    def discount(self, total: float) -> float:
        return 0.0

class VipDiscount(DiscountStrategy):
    def discount(self, total: float) -> float:
        return total * 0.10

class EmployeeDiscount(DiscountStrategy):
    def discount(self, total: float) -> float:
        return total * 0.30

class StudentDiscount(DiscountStrategy):   # new type — no existing code touched
    def discount(self, total: float) -> float:
        return total * 0.15

class DiscountCalculator:
    def __init__(self, strategy: DiscountStrategy):
        self.strategy = strategy

    def calculate(self, order: dict) -> float:
        return self.strategy.discount(order["total"])
```

`DiscountCalculator` is closed for modification; it is open for extension via new `DiscountStrategy` implementations. Adding "student" required only writing a new class.

### Java example — payment processing

````n// Abstraction (stable)
interface PaymentProcessor {
    void process(String recipient, double amount);
}

// Original implementation — never modified when new methods are added
class CreditCardProcessor implements PaymentProcessor {
    public void process(String recipient, double amount) {
        System.out.println("Processing credit card payment to " + recipient);
    }
}

// New extension — no modification of existing code
class PayPalProcessor implements PaymentProcessor {
    public void process(String recipient, double amount) {
        System.out.println("Processing PayPal payment to " + recipient);
    }
}
```

## Common Violations

- **Long if/elif/switch chains** on type tags — every new type requires editing the same block.
- **Hardcoded conditionals** in otherwise stable library/core classes.
- **Feature flags implemented inside a class** — `if feature_enabled("new_behavior"): ...` is a modification smell; the new behavior should be a new implementation.
- **Subclassing to override behavior** when the base class has no abstraction seam, forcing modification of the parent or fragile overrides.

## Trade-offs

- **Requires upfront abstraction**: OCP works best when the right extension points are known in advance. Premature abstraction for hypothetical future requirements is a form of [[YAGNI Principle]] violation — it adds complexity for extensions that never arrive.
- **Abstraction overhead**: introducing interfaces and injection points for simple cases can be overkill. Apply OCP to parts of the code that change frequently or have known extension axes, not everywhere.
- **Discovery cost**: knowing *where* to create an extension seam requires understanding how the system will evolve, which is not always possible early in development.

## Related

- [[SOLID Principles]] — OCP is the "O" in SOLID
- [[Single Responsibility Principle]] — well-separated concerns make extension seams cleaner
- [[Dependency Inversion Principle]] — DIP provides the mechanism (depend on abstractions) that enables OCP
- [[Dependency Injection]] — the runtime technique for wiring in extensions
- [[YAGNI Principle]] — counterbalance: don't abstract prematurely for hypothetical extensions
