---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software architecture 1.md
tags:
  - architecture
  - patterns
  - styles
---

# Architectural Styles and Patterns

The distinction between **architectural styles** (coarse-grained structural templates) and **architectural patterns** (reusable solutions to recurring system-level problems) — and how they relate to software design patterns.

## Problem / Why It Matters

The terms "style," "pattern," and "architecture" are used inconsistently in the industry. Understanding the distinction helps practitioners communicate precisely and select the right level of solution for a given problem.

## Architectural Style

An **architectural style** is a high-level structural organization that defines:
- The overall system organization
- The types of components and connectors that are allowed
- The constraints on how those components interact
- Semantic models for interpreting system properties

Styles represent the most coarse-grained level of system organization. They define *what kind of system this is*.

**Examples**: [[Layered Architecture]], [[Microservices Architecture]], [[Event-Driven Architecture]], Pipe-and-Filter, Client-Server, Peer-to-Peer, [[Serverless Architecture]]

## Architectural Pattern

An **architectural pattern** is a reusable, proven solution to a recurring problem at the system level — addressing concerns related to overall structure, component interactions, and [[Quality Attributes]]. Patterns operate at a higher level of abstraction than software design patterns and solve broader, system-level challenges.

**Examples**: [[MVC Pattern]], [[CQRS]], [[Event Sourcing]], [[Repository Pattern]], Circuit Breaker, Saga

The boundary between style and pattern is blurry. Some concepts (MVC, Repository) are described as both. The distinction is primarily one of generality: a style is more general (defines the character of the whole system); a pattern is more specific (solves a particular recurring problem within or across systems).

## vs. Software Design Patterns

Software design patterns (the Gang of Four patterns — [[Strategy Pattern]], [[Observer Pattern]], [[Factory Method Pattern]], etc.) operate at the **class or object level**. They address how individual components are structured internally.

Architectural patterns and styles operate at the **system level** — how entire services, modules, or subsystems are organized and connected.

The hierarchy:
1. **Architectural style** — the overall system character (e.g., microservices)
2. **Architectural pattern** — a reusable system-level solution (e.g., CQRS within a microservices system)
3. **Design pattern** — a class-level solution (e.g., Observer within an event-driven component)

## Monolith vs. Distributed

Software architectures at the highest level fall into two categories:

- **Monolith**: a single deployable unit containing all the system's functionality. Subcategories include modular monolith, layered monolith, and the problematic "big ball of mud."
- **Distributed**: multiple independently deployable units communicating over a network. Subcategories include microservices, SOA, event-driven, and space-based architectures.

The choice between monolith and distributed is one of the most consequential architectural decisions, with profound implications for team organization, deployment complexity, data consistency, and network overhead.

> [!question] Unverified
> Specific subcategory lists for monolith and distributed architectures vary by source. The above reflects the Wikipedia software architecture article's framing.

## Related

- [[Software Architecture Overview]] — full overview of styles and patterns
- [[Architectural Styles Comparison]] — side-by-side comparison
- [[Design Patterns Overview]] — the design-pattern level
- [[Quality Attributes]] — styles and patterns exist to serve quality attributes
