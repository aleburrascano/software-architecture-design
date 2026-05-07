---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/template-method-design-pattern/", "https://refactoring.guru/design-patterns/template-method"]
tags: [design-pattern, behavioral, gof]
---

# Template Method Pattern

A behavioral pattern that defines the skeleton of an algorithm in a base class, deferring specific steps to subclasses, which can override those steps without altering the overall algorithm structure.

## Problem

Multiple classes share the same high-level algorithm but differ in certain steps. Duplicating the overall structure in each subclass violates DRY and means the algorithm's skeleton must be updated in many places if it changes. A brute-force approach of using a common utility class leads to loss of polymorphism.

The data mining application example illustrates this well: processing PDF, DOC, and CSV documents required format-specific parsing, but "the code for data processing and analysis is almost identical." Without the pattern, you end up with either: (a) code duplication in each format handler, or (b) an ugly conditional in a single class that mixes all three implementations. Similarly, making beverages like tea and coffee follows the same high-level steps (boil water → steep/brew → pour → add extras) but differs in individual steps.

## Solution

Place the invariant algorithm skeleton in a **template method** on an abstract base class. Individual steps that vary are declared as abstract (or overridable) methods. Subclasses provide concrete implementations for those steps, but the sequencing logic remains in the base class.

## Structure

| Participant | Role |
|---|---|
| **AbstractClass** | Declares the `templateMethod()` (final or non-overridable) and abstract primitive operations |
| **primitiveOperation1/2()** | Abstract steps that subclasses must override |
| **hook()** | Optional overridable method with a default (empty or no-op) implementation |
| **ConcreteClass** | Implements the primitive operations; does not alter the skeleton |

The template method calls the primitive operations in a fixed order. Hooks give subclasses optional extension points without requiring override.

Three step categories exist:
- **Abstract steps** — must be implemented by subclasses (no default).
- **Optional steps** — have a default implementation but can be overridden.
- **Hooks** — empty methods providing extension points before or after critical steps.

## When to Use

- Several classes implement variations of the same algorithm; you want to factor out the shared structure once.
- You want to control which parts of an algorithm subclasses can extend (abstract methods = required; hooks = optional).
- You need to apply the Hollywood Principle ("don't call us, we'll call you") — the framework calls the subclass, not the other way around.
- JUnit test lifecycle (`setUp`, `test`, `tearDown`) is a classic example.
- "Let clients extend only particular steps of an algorithm, but not the whole algorithm or its structure."

## Trade-offs

**Pros:**
- Eliminates code duplication in the algorithm skeleton.
- Enforces the invariant structure; subclasses cannot reorder steps.
- Clear extension points via hooks.
- The Hollywood Principle — framework controls the sequence.
- Duplicate code migrates to the base class.

**Cons / pitfalls:**
- Inheritance coupling — subclasses are tightly bound to the base class.
- Violates Liskov Substitution if subclasses override methods in unexpected ways (suppressing steps).
- Algorithm skeleton and variations live in different classes, making the whole picture harder to see in one place.
- Can lead to deep inheritance hierarchies if not controlled.
- Not suitable if the algorithm skeleton itself needs to vary at runtime (use [[Strategy Pattern]] instead).
- Changes to the template method affect all subclasses simultaneously.

## Variants

- **Hooks** — optional extension points with no-op defaults; subclasses override only if needed. Provides more flexibility than pure abstract methods.
- **Factory Method as primitive** — a primitive operation is a Factory Method, making Template Method and Factory Method work together naturally.
- **Callback / lambda variant** — in functional languages, the "template" is a higher-order function; steps are passed as function arguments rather than overriding methods (eliminates the inheritance requirement entirely).

## Code Example

````nabstract class BeverageMaker {
    // Template Method — final prevents subclasses from reordering steps
    public final void makeBeverage() {
        boilWater();
        brew();
        pourInCup();
        if (customerWantsCondiments()) {  // hook
            addCondiments();
        }
    }

    // Abstract steps — subclasses must implement
    abstract void brew();
    abstract void addCondiments();

    // Concrete steps — shared by all subclasses
    void boilWater() { System.out.println("Boiling water"); }
    void pourInCup() { System.out.println("Pouring into cup"); }

    // Hook — optional override (default: yes)
    boolean customerWantsCondiments() { return true; }
}

class TeaMaker extends BeverageMaker {
    @Override void brew()           { System.out.println("Steeping the tea"); }
    @Override void addCondiments()  { System.out.println("Adding lemon"); }
}

class CoffeeMaker extends BeverageMaker {
    @Override void brew()           { System.out.println("Dripping coffee through filter"); }
    @Override void addCondiments()  { System.out.println("Adding sugar and milk"); }
    @Override boolean customerWantsCondiments() { return false; }  // overrides hook
}

// Usage
new TeaMaker().makeBeverage();
// Boiling water → Steeping the tea → Pouring into cup → Adding lemon

new CoffeeMaker().makeBeverage();
// Boiling water → Dripping coffee through filter → Pouring into cup
```

## Real-World Examples

- **JUnit test lifecycle** — `@BeforeEach`, `@Test`, `@AfterEach` are the hook/primitive methods; JUnit's test runner is the template method that calls them in order.
- **Java `AbstractList` / `AbstractMap`** — define the algorithm for standard `List`/`Map` operations using a few abstract primitives (`get(index)`, `size()`). Subclasses only implement those primitives and inherit all other operations.
- **Data export pipelines** — a report generator defines: fetch data → transform → format → output. Each subclass provides different format/output implementations (CSV, PDF, HTML) while sharing fetch and transform logic.
- **Beverage machines** — coffee machines and tea makers follow the same brew sequence; individual steps differ.
- **Document processing frameworks** — parsing PDF, DOC, and CSV shares the same pipeline: open → extract text → process → close. Only the extraction step varies by format.
- **Game AI turns** — a turn-based game defines `takeTurn()` as a template: gather input → compute move → execute move → end turn. Human and AI players override only the "compute move" step.
- **Spring `JdbcTemplate`** — defines the skeleton of JDBC operations (get connection → prepare statement → execute → close) and lets callers inject only the SQL and parameter binding logic.

## Related

- [[Strategy Pattern]] — Template Method varies steps via inheritance (compile-time); Strategy varies the whole algorithm via composition (runtime). Prefer Strategy for runtime flexibility, Template Method for a fixed skeleton with well-defined extension points.
- [[Factory Method Pattern]] — Factory Method is often used as a primitive operation inside a Template Method.
- [[Command Pattern]] — commands may use Template Method to define setup/execute/cleanup lifecycles.
