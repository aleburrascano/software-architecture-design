---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Best Practice - an Introduction to Domain-Driven Design.md'
source_date: '2009-02'
source_author: David Laribee / MSDN Magazine
tags:
  - source
  - ddd
  - aggregate
  - bounded-context
  - value-object
  - domain-service
  - repository
  - anti-corruption-layer
  - laribee
---
# Best Practice Introduction to DDD

MSDN Magazine article by David Laribee introducing DDD through an insurance policy management case study, using the Platonic Forms metaphor to frame the domain modelling goal.

## Summary

Laribee introduces DDD through a concrete running example — an insurance policy management system — rather than through abstract definitions. The framing device is the Platonic Forms metaphor: the domain model is the ideal form (the platonic ideal of what a policy, claim, or premium truly is), while the implementation is an imperfect reflection of that ideal. The goal of DDD is to make the code as close as possible to the ideal domain model, with the domain model itself driven by business language rather than technical or database concerns.

The article covers the full set of DDD tactical patterns with insurance-domain examples. Entities have identity that persists through state changes (a policy has a policy number). Value Objects are immutable and identity-less — they are defined entirely by their attribute values (a monetary amount, a date range). Aggregate Roots enforce invariants and serve as the only entry point to a cluster of related objects (a Policy aggregate root controls access to its Clauses and Premiums). Domain Services express operations that cut across multiple entities and don't naturally belong to any single entity. Application Services orchestrate use cases without containing domain logic. Repositories abstract persistence, allowing the domain model to be written without database awareness.

The Anti-Corruption Layer (ACL) is illustrated in the context of integrating with a legacy policy administration system: the legacy system uses a different terminology and data model, and the ACL translates at the boundary so the new domain model is not polluted by legacy concepts.

## Key Arguments

- The Platonic Forms metaphor: code is an imperfect reflection of the ideal domain model; DDD minimizes the imperfection
- Domain models should be driven by business language, not technical or database concerns
- Aggregate Roots enforce invariants and are the only entry points; the Law of Demeter applies — external code should not reach inside an aggregate
- Value Objects are immutable: equality is by value, not identity; immutability eliminates entire classes of bugs
- Domain Services express operations that don't naturally belong to any entity and cut across aggregate boundaries
- Repositories abstract persistence: the domain model is written as if persistence is free, and repositories handle the mapping
- Anti-Corruption Layers translate between legacy terminology and domain language, protecting the new model from contamination

## Concepts Covered

- [[Domain-Driven Design]] — foundational treatment with concrete insurance policy case study
- [[Aggregate]] — Law of Demeter and invariant enforcement; insurance policy as aggregate root example
- [[Bounded Context]] — Anti-Corruption Layer as a bounded context integration pattern
- [[Value Object]] — immutability and value-based equality

## Quality Notes

Classic MSDN Magazine article (2009) that remains highly relevant. The insurance policy case study makes abstract DDD concepts tangible in a way that many theoretical treatments miss. Good as a first practical DDD read alongside the Evans DDD Reference. Laribee was an influential DDD practitioner in the .NET community.
