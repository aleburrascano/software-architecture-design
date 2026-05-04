---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What Makes a Good Software Design Mindset.md"
  - "raw/Software design.md"
  - "raw/Software architecture 1.md"
tags:
  - design-principle
  - topic
---

# Design Principles Overview

A synthesis of the core software design principles — SOLID, DRY, KISS, YAGNI, Composition over Inheritance — and how they form a coherent design philosophy.

## The Principles at a Glance

| Principle | One-Line Rule | Primary Benefit | Primary Risk if Violated |
|-----------|--------------|-----------------|--------------------------|
| [[Single Responsibility Principle\|SRP]] | One reason to change | Focused, testable classes | God objects, tangled concerns |
| [[Open-Closed Principle\|OCP]] | Extend without modifying | Safe evolution | Change cascades, regressions |
| [[Liskov Substitution Principle\|LSP]] | Subtypes substitute safely | Sound polymorphism | Broken subtype behavior |
| [[Interface Segregation Principle\|ISP]] | No forced unused dependencies | Cohesive interfaces | Fat interfaces, stub implementations |
| [[Dependency Inversion Principle\|DIP]] | Depend on abstractions | Testability, replaceability | Tightly coupled layers |
| [[DRY Principle\|DRY]] | Single source of truth | Consistent changes | Shotgun surgery, divergence |
| [[KISS Principle\|KISS]] | Simplest solution that works | Readability, maintainability | Accidental complexity |
| [[YAGNI Principle\|YAGNI]] | Only build what's needed now | Lean codebase, speed | Dead code, wrong abstractions |
| [[Composition over Inheritance\|Composition]] | Has-a over is-a | Flexible, loosely coupled | Fragile inheritance, LSP violations |
| [[Dependency Injection\|DI]] | Inject, don't create | Testability, configurability | Hardcoded infrastructure coupling |

## The Underlying Goals

All design principles ultimately serve the same three goals:

1. **Maintainability**: the ability to change the system safely and efficiently over time.
2. **Testability**: the ability to verify behavior in isolation without expensive setup.
3. **Evolvability**: the ability to add new features or replace components with low risk.

These goals are related to **coupling** and **cohesion** — the two fundamental metrics that predate all of these named principles:
- **High cohesion**: elements that belong together are kept together. (SRP, DRY, ISP)
- **Low coupling**: elements that do not need to know about each other remain independent. (DIP, DI, Composition, OCP)

As Duy Pham summarizes in *What Makes a Good Software Design Mindset*: "good separation of concerns = high cohesion + loose coupling."

## How the Principles Relate

### SOLID as a cluster

[[SOLID Principles]] is itself a cluster of five mutually-reinforcing principles. See that page for the internal relationships. In summary: SRP creates clean boundaries; OCP and DIP together define how extensions are managed; ISP narrows the abstractions DIP depends on; LSP ensures that extensions don't break existing behavior.

### KISS and YAGNI as a counterweight

KISS and YAGNI are in productive tension with SOLID, OCP, and DRY:

- SOLID encourages abstractions (interfaces, strategy objects, repositories). KISS warns against introducing abstractions unless they are earning their cost.
- OCP encourages extension points. YAGNI says extension points only belong in code once you have two concrete extensions, not speculatively.
- DRY encourages consolidation. The "Rule of Three" (extracted from XP) synthesizes DRY and YAGNI: wait until you see a third instance of duplication before abstracting.

**Resolution**: abstractions and consolidation should be driven by *demonstrated* need, not anticipated future need. The order: make it work, make it clear, then (when the pattern is confirmed) make it right.

### Composition and DI as the mechanism layer

[[Composition over Inheritance]] and [[Dependency Injection]] are less "design heuristics" and more *techniques* that enable the other principles:

- Composition makes it possible to combine single-responsibility classes (SRP) without coupling them via inheritance (Composition).
- DI wires those composed objects together at runtime without hardcoding dependencies (DIP, OCP).

They are the implementation layer that makes the principle layer practical.

### DRY and SRP: the same insight at different scales

DRY says: don't duplicate *knowledge*. SRP says: each unit should *own* one piece of knowledge. They converge on the same thing: every business concept has exactly one place in the codebase where it lives. DRY catches violations after the fact; SRP prevents them by design.

## A Mental Model for Design Decisions

When evaluating a design decision, three questions cover most of the principles:

1. **"What changes independently?"** (SRP, ISP, DRY) — things that change for different reasons should live in different places; knowledge should have a single authoritative home.
2. **"How does new behavior get added?"** (OCP, DIP, Composition) — new behavior should be additive; existing code should not need to change; dependencies should point toward abstractions.
3. **"Is this earning its complexity?"** (KISS, YAGNI) — does this abstraction, layer, or feature exist because of real, demonstrated need, or speculative future need?

## Principles vs. Patterns

Design principles (this page) are guidelines — they describe qualities a good design should have. **Design patterns** (Strategy, Observer, Factory, etc.) are solutions — they describe proven structural arrangements that tend to satisfy those qualities. Patterns often *embody* principles:

| Pattern | Principles it embodies |
|---------|----------------------|
| Strategy | OCP, DIP, Composition |
| Repository | DIP, SRP |
| Factory Method | OCP, DIP |
| Decorator | OCP, SRP, Composition |
| Observer | OCP, SRP, DIP |

Understanding principles first makes patterns more meaningful and easier to apply correctly.

## Priority and Pragmatism

No codebase applies all principles perfectly everywhere. The pragmatic hierarchy:

1. **Make it correct first** — a clean design is worthless if the code is wrong.
2. **Make it readable** (KISS) — the highest-leverage investment in long-lived code.
3. **Eliminate real duplication** (DRY) — once there is actual duplication.
4. **Separate concerns** (SRP, ISP) — as complexity grows.
5. **Decouple for extensibility** (OCP, DIP, Composition, DI) — when change patterns are understood.
6. **Don't over-engineer** (YAGNI) — always.

> [!question] Unverified
> Several sources attribute a synthesis of these principles to Robert C. Martin as a unified "Clean Code" philosophy, but the exact canonical ordering or priority across principles is not standardized in any single authoritative source.

## Related

- [[SOLID Principles]] — detailed coverage of the five OO principles
- [[Single Responsibility Principle]]
- [[Open-Closed Principle]]
- [[Liskov Substitution Principle]]
- [[Interface Segregation Principle]]
- [[Dependency Inversion Principle]]
- [[DRY Principle]]
- [[KISS Principle]]
- [[YAGNI Principle]]
- [[Composition over Inheritance]]
- [[Dependency Injection]]
