---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/vaughnvernon-iddd_samples.md'
source_date: ''
source_author: Vaughn Vernon
tags:
  - source
  - ddd
  - vaughn-vernon
  - bounded-context
  - aggregate
  - iddd
  - reference-implementation
  - java
  - collaboration-context
---
# IDDD Samples - Vaughn Vernon

Official sample Bounded Contexts from Vaughn Vernon's "Implementing Domain-Driven Design," the authoritative code companion to the IDDD book.

## Summary

These samples are the canonical code reference for Vaughn Vernon's "Implementing Domain-Driven Design" (IDDD), one of the two most important books in the DDD literature. The repository contains three complete Bounded Contexts that together demonstrate how DDD patterns work in a realistic multi-context system: the Collaboration Context (forums, discussions, calendars), the Identity and Access Context (users, groups, roles, authentication), and the Agile Project Management Context (teams, sprints, product backlog, tasks).

Each context demonstrates how business capabilities should drive bounded context boundaries rather than technical layers or entity ownership. Aggregates are designed to enforce consistency within tightly coupled groups of objects while remaining loosely coupled to other aggregates. Application Services coordinate domain operations and translate between external representations and domain objects without containing business logic themselves.

The inter-context communication demonstrates domain event publishing and subscription, with the Identity & Access Context publishing events consumed by the Collaboration and Agile Contexts. This shows how Bounded Contexts integrate without sharing domain models, using published events and Anti-Corruption Layers where necessary.

## Key Arguments

- Bounded contexts should be modeled around business capabilities, not technical layers or entity clusters
- Aggregates enforce internal consistency boundaries; their boundaries should be minimal but sufficient
- Domain events are the primary integration mechanism between bounded contexts
- Application services coordinate domain operations without containing business logic
- Factories and repositories abstract aggregate creation and retrieval from domain logic
- All DDD patterns work together in a realistic system; understanding them in isolation is insufficient

## Concepts Covered

- [[Domain-Driven Design]] — IDDD book reference implementation; most authoritative code companion
- [[Bounded Context]] — multiple contexts showing realistic boundary decisions
- [[Aggregate]] — design in practice across multiple realistic domains
- [[Domain Event]] — inter-context communication implementation
- [[DDD Building Blocks]] — complete application of all tactical building blocks

## Quality Notes

Authoritative reference from the IDDD book author. Essential companion to Vernon's book for practitioners who want to see DDD patterns applied in a complete, multi-context system rather than isolated examples.
