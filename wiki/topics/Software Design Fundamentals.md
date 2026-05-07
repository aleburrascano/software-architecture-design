---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/What Makes a Good Software Design Mindset.md
  - raw/articles/Software design.md
  - raw/articles/Software architecture 1.md
tags:
  - design
  - topic
---

# Software Design Fundamentals

The foundational concepts and principles that underpin good software design — independent of any specific language, framework, or architectural style.

## Why Fundamentals Matter

Technology changes rapidly. Frameworks are replaced. Languages evolve. Architectural fashions cycle. But the fundamental principles of good design have remained stable for decades. As Martin Fowler observed: "While specifics of technology change rapidly in our profession, fundamental practices and patterns are more stable."

Mastering fundamentals enables rapid adaptation to new technologies because every new technology must still solve the same underlying problems: managing complexity, enabling change, and making systems understandable.

## The Foundation: Coupling and Cohesion

All other design principles trace back to two measurements:

- **[[Coupling and Cohesion|Cohesion]]** — how well the elements of a module belong together (aim: high)
- **[[Coupling and Cohesion|Coupling]]** — how much modules depend on each other's internals (aim: low)

> Good design = high cohesion + loose coupling.

Every other principle in this page is either a restatement of this ideal for a specific context, or a technique for achieving it.

## Core Design Concepts

The **PHAME** principles (Grady Booch's four fundamental design concepts):

| Concept | What it means |
|---|---|
| **[[Abstraction]]** | Expose only what is relevant; hide what is not |
| **[[Encapsulation]]** | Bundle data and behavior; protect internal state |
| **[[Modularity]]** | Divide the system into independent, self-contained units |
| Hierarchy | Organize modules into layers or inheritance structures |

## Structural Principles

| Principle | Core idea |
|---|---|
| **[[Separation of Concerns]]** | Each section of code addresses exactly one concern |
| **[[Information Hiding]]** | Hide implementation decisions behind stable interfaces |
| **[[Law of Demeter]]** | Talk only to direct associates; never navigate through objects |
| **[[Composition over Inheritance]]** | Prefer composing objects over creating deep inheritance trees |

## SOLID Principles

The most widely-taught set of OOP design principles (Robert C. Martin, 2000):

| Principle | Summary |
|---|---|
| **[[Single Responsibility Principle]]** | One reason to change per class |
| **[[Open-Closed Principle]]** | Open for extension, closed for modification |
| **[[Liskov Substitution Principle]]** | Subtypes must be substitutable for their supertypes |
| **[[Interface Segregation Principle]]** | Many specific interfaces over one general-purpose interface |
| **[[Dependency Inversion Principle]]** | Depend on abstractions, not concretions |

These are covered in depth in [[SOLID Principles]].

## Design Considerations (Quality at the Design Level)

Design decisions should be evaluated against quality attributes. For detailed definitions see [[Quality Attributes]]. Davis's design principles add meta-guidance:

- Avoid tunnel vision — consider multiple approaches
- Don't reinvent the wheel — use established patterns
- Minimize intellectual distance — model the problem domain in the software structure
- Design for change — accommodate expected evolution without breaking existing behavior
- Degrade gracefully — handle unexpected inputs and failure conditions
- Assess quality during design, not after

## The Design Mindset

Good design is not a checklist but a disposition:

1. **Treat complexity as the enemy** — every abstraction, every separation, every principle exists to manage complexity
2. **Design for the reader** — code is read far more often than it is written; optimize for comprehension
3. **Prefer reversibility** — make it easy to change your mind by keeping coupling low and interfaces stable
4. **Think in trade-offs** — every design decision involves trade-offs; name them explicitly
5. **Build for users** — architectural elegance that doesn't serve user needs or meets functional requirements with poor performance is worthless

## Relationship to Architecture

Software design and software architecture exist on a continuum. For the distinction see [[Software Design vs Software Architecture]]. Design principles apply at every level — class, module, service, system — with the architectural level focusing specifically on decisions that are hard to reverse and affect [[Quality Attributes]] system-wide.

## Related

- [[SOLID Principles]] — deep dive on the SOLID set
- [[Design Patterns Overview]] — standard solutions to recurring design problems
- [[Software Design vs Software Architecture]] — the boundary between design and architecture
- [[Quality Attributes]] — what design decisions are ultimately trying to achieve
- [[Technical Debt]] — what accumulates when design principles are consistently violated
