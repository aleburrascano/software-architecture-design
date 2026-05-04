---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "raw/Software design.md"
  - "https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/"
tags:
  - design-principle
  - solid
  - topic
---

# SOLID Principles

Five object-oriented design principles that together guide maintainable, extensible, and testable code.

## The Five Principles

| Acronym | Principle | Short Rule | What It Prevents |
|---------|-----------|-----------|-----------------|
| **S** | [[Single Responsibility Principle]] | One reason to change | God classes; tangled concerns; untestable modules |
| **O** | [[Open-Closed Principle]] | Open for extension, closed for modification | Regression-prone change cascades; fragile conditionals |
| **L** | [[Liskov Substitution Principle]] | Subtypes must substitute safely | Broken polymorphism; defensive `isinstance` checks |
| **I** | [[Interface Segregation Principle]] | No forced dependency on unused methods | Fat interfaces; stub-filled implementations |
| **D** | [[Dependency Inversion Principle]] | Depend on abstractions, not concretions | Untestable business logic; tightly coupled layers |

## Origins

The SOLID acronym was coined by Michael Feathers around 2000 and popularized by **Robert C. Martin** ("Uncle Bob"), who assembled and refined the five principles from earlier work in the object-oriented design community.

The individual principles predate the acronym:
- **SRP** was influenced by David Parnas's 1972 paper on *information hiding* and modular decomposition.
- **OCP** was first stated by **Bertrand Meyer** in *Object-Oriented Software Construction* (1988).
- **LSP** was formalized by **Barbara Liskov** and Jeannette Wing in their 1994 paper "A Behavioral Notion of Subtyping."
- **ISP** emerged from Martin's consulting work at Xerox in the early 1990s, where a large printer job scheduler interface was causing widespread build failures.
- **DIP** was articulated by Martin in his 1996 article in *C++ Report*.

Martin's 2000 paper "Design Principles and Design Patterns" (often called the "Uncle Bob paper") presented them as a unified framework for maintainable OO design. They were later elaborated in *Agile Software Development, Principles, Patterns, and Practices* (2002).

## How They Interact

SOLID principles are not independent — they reinforce each other as a system:

**SRP enables the others.** When each class has a single, clear responsibility, it becomes easier to apply OCP (there is only one axis of extension to manage), LSP (the behavioral contract is smaller and more precise), ISP (interfaces align with single responsibilities), and DIP (dependencies on single-purpose abstractions are well-defined).

**OCP depends on DIP.** The mechanism for "open for extension" is typically an abstraction (interface). DIP provides the design rule for how to structure those abstractions — high-level code depends on them, low-level code implements them.

**LSP validates OCP.** If new implementations (extensions) violate the behavioral contract of the base type, then the system is not truly extended safely — it is broken. LSP is the correctness criterion that OCP-based extension must satisfy.

**ISP refines DIP.** If high-level modules depend on large, fat interfaces (DIP satisfied technically), but they only use 20% of the interface, ISP is violated. Combining DIP and ISP means: depend on the *smallest* abstraction that captures exactly what you need.

**A practical chain:** 
```
SRP → classes have clear responsibilities
  → ISP → interfaces model those responsibilities narrowly
    → DIP → high-level code depends on those narrow interfaces
      → OCP → new behavior = new class implementing the interface
        → LSP → the new class must honor the interface's behavioral contract
```

## Criticisms and Nuances

**Over-application creates complexity.** Applying SOLID mechanically everywhere produces an explosion of small classes, interfaces, and indirection that can make a simple codebase harder to navigate than the violation it was meant to fix. SOLID principles are heuristics, not laws.

**SOLID works best at scale.** For a script, a simple CRUD endpoint, or an internal utility, SOLID's benefits (testability, extensibility) often do not justify its costs (abstractions, boilerplate). The principles are most valuable in large, long-lived codebases maintained by teams.

**SRP's "reason to change" is subjective.** What counts as a single responsibility depends on the level of granularity. Martin acknowledges this: SRP is about *actors* (stakeholders who can demand a change), not purely technical concerns. Two developers may legitimately disagree about whether a class has one responsibility or two.

**OCP as originally stated is impractical.** Meyer's original formulation (truly never modify) is unrealistic. Martin's restatement (don't modify in response to new extensions) is more practical but still requires judgment about which extensions are "expected."

**LSP violations are often fixable only by redesign.** By the time an LSP violation is discovered in a production system, it is often deeply embedded. The principle is most valuable as a design-time constraint, not a diagnostic.

**DIP ≠ always use a DI framework.** Many developers interpret DIP as "use Spring" or "use a container." DIP is a structural design principle; [[Dependency Injection]] is the technique, and a framework is just tooling.

> [!question] Unverified
> Some sources attribute the original SRP formulation to Tom DeMarco's concept of cohesion from structured design (1979). The exact lineage from DeMarco to Martin is not fully settled in the literature.

## When to Apply SOLID

| Context | Guidance |
|---------|----------|
| Long-lived production codebase | Apply consistently |
| Team-maintained service or library | Apply selectively — especially SRP, DIP |
| Internal script or utility | Apply KISS/YAGNI first; SOLID if it grows |
| Prototype / spike | Defer — don't over-engineer throwaway code |
| Framework/library with public API | Apply rigorously — especially ISP, OCP, LSP |

## Related

- [[Design Principles Overview]] — SOLID in context with DRY, KISS, YAGNI, and beyond
- [[Single Responsibility Principle]] — the "S"
- [[Open-Closed Principle]] — the "O"
- [[Liskov Substitution Principle]] — the "L"
- [[Interface Segregation Principle]] — the "I"
- [[Dependency Inversion Principle]] — the "D"
- [[Dependency Injection]] — the primary technique implementing DIP
- [[Composition over Inheritance]] — a complementary principle that SOLID works best alongside
