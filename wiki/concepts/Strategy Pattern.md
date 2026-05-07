---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/strategy-pattern-set-1/", "https://refactoring.guru/design-patterns/strategy"]
tags: [design-pattern, behavioral, gof]
---

# Strategy Pattern

A behavioral pattern that defines a family of algorithms, encapsulates each one in a separate class, and makes them interchangeable so that the algorithm can vary independently from the clients that use it.

## Problem

A class needs to support multiple variants of an algorithm (e.g., different sorting strategies, payment methods, compression formats), but hardcoding all variants with conditionals (`if`/`switch`) leads to:

- A bloated class that must be edited every time a new variant is added.
- Violation of the Open/Closed Principle.
- Difficulty in unit-testing individual algorithms in isolation.
- Code duplication when variants share partial logic.

The navigation app case illustrates this well: each time a new routing algorithm (car, walking, public transit, cycling) was added, the main navigator class doubled in size, causing maintenance difficulties, merge conflicts, and increased bug risk.

## Solution

Extract each algorithm variant into its own class behind a common **Strategy** interface. The **Context** class holds a reference to a Strategy object and delegates the work to it. At runtime the concrete strategy can be swapped freely. The context is unaware of the concrete algorithm it is using.

## Structure

| Participant | Role |
|---|---|
| **Strategy** (interface) | Declares the common `execute()` operation |
| **ConcreteStrategyA/B/…** | Implements a specific algorithm variant |
| **Context** | Holds a reference to a Strategy; calls `strategy.execute()` |
| **Client** | Selects and injects the appropriate concrete strategy into the Context |

The Context delegates the algorithm to the Strategy; the Client decides which strategy to use.

## When to Use

- Multiple related classes differ only in their behavior — strategies let you vary the behavior independently.
- You need different variants of an algorithm and want to avoid conditional branching.
- An algorithm uses data that clients should not be exposed to (encapsulate implementation details in strategies).
- A class defines many behaviors via conditionals that can be replaced by polymorphism.
- You need to swap algorithms at runtime.
- You want to isolate business logic from implementation details of individual algorithm variants.

## Trade-offs

**Pros:**
- Open/Closed: add new strategies without touching the Context.
- Algorithms are isolated, easy to understand and test in isolation.
- Eliminates conditional logic from the context.
- Composition over inheritance — strategies can be combined.
- Enables runtime algorithm swapping.

**Cons / pitfalls:**
- Clients must be aware of the different strategies to select the correct one.
- Added number of objects — each strategy is a class even if the algorithm is trivial.
- In languages with first-class functions, a simple lambda or function reference often achieves the same result without a formal class hierarchy (Strategy can be overkill for trivial cases).
- All strategies share the same interface; if algorithms need very different parameters, the interface becomes awkward.
- Unnecessary complexity for algorithms that are simple and rarely change.

## Variants

- **Functional Strategy** — in languages like Java 8+, Python, or Kotlin, the strategy interface is replaced by a functional interface or lambda, e.g., `Comparator` in Java.
- **Default Strategy** — the context provides a sensible default that clients can override.
- **Configurable Strategy** — strategy selection driven by configuration files or dependency injection at startup (common in Spring).
- **Strategy Registry** — a map from a key (string, enum) to a strategy instance, used when strategies are selected by name at runtime.

## Code Example

````n// Strategy Interface
public interface SortingStrategy {
    void sort(int[] array);
}

// Concrete Strategies
public class BubbleSortStrategy implements SortingStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("Sorting using Bubble Sort");
        // ... bubble sort implementation
    }
}

public class QuickSortStrategy implements SortingStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("Sorting using Quick Sort");
        // ... quicksort implementation
    }
}

public class MergeSortStrategy implements SortingStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("Sorting using Merge Sort");
        // ... mergesort implementation
    }
}

// Context
public class SortingContext {
    private SortingStrategy sortingStrategy;

    public SortingContext(SortingStrategy strategy) {
        this.sortingStrategy = strategy;
    }

    public void setSortingStrategy(SortingStrategy strategy) {
        this.sortingStrategy = strategy;
    }

    public void performSort(int[] array) {
        sortingStrategy.sort(array);
    }
}

// Client
SortingContext context = new SortingContext(new BubbleSortStrategy());
context.performSort(new int[]{5, 2, 9, 1});

context.setSortingStrategy(new QuickSortStrategy());
context.performSort(new int[]{8, 3, 7, 4});
```

## Real-World Examples

- **E-commerce payment systems** — credit card, PayPal, UPI, and crypto are interchangeable payment strategies. The checkout context delegates payment to the selected strategy without knowing the implementation.
- **File compression tools** — ZIP, GZIP, TAR, and BZIP2 compression are strategies selected by the user or detected from file extension. The same archiver context invokes whichever strategy is active.
- **Navigation / routing** — car navigation, walking, cycling, and transit routing are distinct strategies injected into a route planner. Switching from driving to walking just swaps the strategy.
- **Java `Comparator`** — `Collections.sort(list, comparator)` accepts any `Comparator` implementation as the sorting strategy, from anonymous classes to lambdas.
- **Spring `AuthenticationManager`** — Spring Security wires different authentication strategies (LDAP, JDBC, OAuth2) into the same manager interface.
- **Logging frameworks** — log4j / SLF4J use appender strategies (file, console, remote) that can be swapped via configuration.
- **Machine learning feature selection** — a training pipeline can swap normalization strategies (min-max, z-score, none) without changing the model training code.

## Related

- [[Template Method Pattern]] — Template Method uses inheritance to vary parts of an algorithm; Strategy uses composition to swap entire algorithms. Prefer Strategy when you want runtime flexibility; Template Method when the skeleton is fixed.
- [[State Pattern]] — structurally identical; the difference is intent. Strategy is selected by the client and rarely changes; State changes itself based on internal transitions. States often know about each other; strategies usually do not. Strategy describes different ways of doing the same thing; State allows objects to change class-like behavior as their lifecycle progresses.
- [[Command Pattern]] — commands encapsulate an action; strategies encapsulate an algorithm. Commands are about *what* to do; strategies are about *how* to do it.
- [[Decorator Pattern]] — an alternative when you need to augment an algorithm rather than replace it entirely.
