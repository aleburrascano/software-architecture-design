---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
source_date: 2019-09-28
source_author: "[[Khalil Stemmler]]"
tags:
  - architecture
  - source
  - roadmap
---

# Full-stack Software Design & Architecture Map (Khalil Stemmler)

Source: https://khalilstemmler.com/articles/software-design-architecture/full-stack-software-design/

## Summary

Khalil Stemmler's canonical roadmap article describing the nine stages of the full-stack software design and architecture learning journey — from clean code to enterprise patterns. Structured as a layered stack where each stage builds on the previous. One of the most comprehensive opinionated roadmaps for software architecture education available. Published 2019; widely referenced.

## Concepts Covered

- [[Software Architecture Overview]] — the goal of software and why design matters
- [[MVC Pattern]] — Stage 8, architectural pattern; limitations for complex domains
- [[Event Sourcing]] — Stage 8, functional approach to state as a log of transactions
- [[Domain-Driven Design]] — Stages 8 and 9; layered architecture + rich domain models; enterprise patterns
- [[Layered Architecture]] — Stage 7 structural style; vertical separation of concerns
- [[Clean Architecture]] — Stage 6 architectural principles; policy vs. detail, dependency rule
- [[Hexagonal Architecture]] — implied through Clean Architecture and DDD discussions
- Design Patterns (GoF) — Stage 5: creational, structural, behavioral
- SOLID and design principles — Stage 4

## Key Arguments / Claims

- **Primary goal of software:** continually produce something that satisfies user needs while minimizing the effort to do so. Software must be designed to change.
- The learning roadmap has nine stages, each building on the last: Clean Code → Programming Paradigms → OOP → Design Principles → Design Patterns → Architectural Principles → Architectural Styles → Architectural Patterns → Enterprise Patterns.
- **Architectural styles are design patterns blown up in scale to the high level.** Three categories: structural (component-based, monolithic, layered), messaging (event-driven, pub-sub), distributed (client-server, peer-to-peer).
- **Architectural patterns explain how to implement an architectural style.** Examples: DDD implements layered style; MVC implements a specific UI-layer pattern; Event Sourcing is a functional approach to state.
- **OOP** is the tool for crossing architectural boundaries via polymorphism and plugins; **functional programming** pushes data to boundaries; **structured programming** writes algorithms.
- Layered architectures cut concerns vertically; component-based architectures separate horizontally.
- MVC is useful but insufficient for complex business logic — DDD enterprise patterns are needed beyond MVC.
- **DDD enterprise patterns:** Entities (objects with identity), Value Objects (models with no identity for validation), Domain Events (subscribable significant business events).
- Recommended books: Clean Code (Martin), Refactoring (Fowler), Clean Architecture (Martin), Domain-Driven Design (Evans), Implementing DDD (Vernon), Patterns of Enterprise Application Architecture (Fowler), Enterprise Integration Patterns (Hohpe), Head First Design Patterns.

## Quality Notes

High quality and comprehensive. One of the best publicly available opinionated roadmaps for the full architecture learning journey. Written by a practitioner with deep TypeScript/Node.js DDD experience. Slightly dated (2019) but foundational content is timeless. The roadmap structure is excellent for understanding how concepts relate and build on each other. Does not go deep on distributed systems patterns (CQRS, Event Sourcing receive only brief mentions). Strong on the DDD + Clean Architecture axis.
