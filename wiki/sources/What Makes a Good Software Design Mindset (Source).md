---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: raw/articles/What Makes a Good Software Design Mindset.md
source_date: 2020-03-17
source_author: Duy Pham
tags:
  - design
  - source
---

# What Makes a Good Software Design Mindset? (Duy Pham, Medium)

A practitioner-oriented overview of software design fundamentals, arguing that a design mindset — grounded in stable principles — is more valuable than mastery of any specific technology.

## Summary

Published on Medium's Better Programming publication in March 2020. Duy Pham argues that technology changes rapidly but fundamental design principles are stable. The article surveys the key principles a software engineer should internalize: coupling/cohesion, separation of concerns, SOLID, domain-driven design, composition over inheritance, and design patterns.

The central thesis: building a design mindset is more important than gaining specific technical skills, because fundamentals enable rapid adaptation to technology changes.

## Key Claims / Arguments

- "While specifics of technology change rapidly in our profession, fundamental practices and patterns are more stable." (attributed to Martin Fowler)
- High cohesion + loose coupling is the most important fundamental in software design
- Good separation of concerns = high cohesion + loose coupling
- Loose coupling enables scalability (e.g., moving modules to other tiers in multi-tier or SOA architectures)
- The SOLID principles are a set every software engineer should understand "solidly"
- Domain-centric architectures (Clean Architecture, DDD) treat the domain as the most stable part of the system
- Composition over inheritance is the better direction for highly maintainable software
- Good software must also serve user needs — elegant architecture that doesn't meet user needs or perform adequately is worthless

## Concepts Covered

- [[Coupling and Cohesion]]
- [[Separation of Concerns]]
- [[Single Responsibility Principle]]
- [[Open-Closed Principle]]
- [[Liskov Substitution Principle]]
- [[Interface Segregation Principle]]
- [[Dependency Inversion Principle]]
- [[Domain-Driven Design]]
- [[Composition over Inheritance]]
- [[Design Patterns Overview]]
- [[Law of Demeter]] (implicitly, through coupling discussion)
- [[Abstraction]]

## Quality Notes

Practitioner blog post — accessible and well-structured but not academically cited. Presents widely-accepted principles accurately, with concrete examples (especially the Penguin/LSP violation and the god object anti-pattern). The domain-centric vs. data-centric complexity graph references enterprisecraftsmanship.com rather than primary academic sources. The article is introductory in depth but reliable for its claims about established principles.
