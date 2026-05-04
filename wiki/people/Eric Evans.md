---
type: person
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.avanscoperta.it/en/trainer/eric-evans/
  - https://martinfowler.com/bliki/DomainDrivenDesign.html
tags:
  - people
  - software-architecture
---

# Eric Evans

Software modeling expert and founder of Domain Language who authored *Domain-Driven Design* (2004), introducing the vocabulary and practices — Ubiquitous Language, Bounded Context, Aggregate, Domain Events — that define how complex business software is designed today.

## Background

Eric Evans has spent his career working on large-scale business systems across diverse industries including finance, shipping, insurance, and manufacturing automation. His experimentation with design approaches from the early 1990s onward culminated in the 2004 publication of the "Blue Book." He founded Domain Language, a consulting and training company, and continues to run workshops, coach teams, and speak at conferences such as DDD Europe, Explore DDD, and KanDDinsky. He remains an active presence in the DDD community, continuing to evolve the methodology's application to emerging challenges.

## Key Contributions

- **[[Domain-Driven Design]]** — a comprehensive approach to software design that places the domain model at the centre of the system, aligning technical implementation with business reality
- **Ubiquitous Language** — the practice of building a shared, rigorous vocabulary used by both domain experts and developers, embedded in code, tests, and conversations
- **[[Bounded Context]]** — a strategic pattern defining explicit boundaries within which a particular domain model applies, enabling large systems to be decomposed without forcing a single unified model
- **[[Aggregate]]** — a cluster of domain objects treated as a single unit for data changes, with a root entity controlling access and enforcing invariants
- **[[Domain Event]]** — an explicit representation of something significant that happened in the domain, enabling event-driven integration between bounded contexts
- **[[Value Object]]** — an immutable object defined entirely by its attributes rather than an identity, improving expressiveness and safety
- **Strategic Design** — the set of DDD patterns (context maps, bounded contexts, anti-corruption layers) for managing relationships between subsystems at the architectural level
- **Anti-Corruption Layer** — a translation layer protecting a domain model from the conceptual pollution of external or legacy systems

## Key Works

- *Domain-Driven Design: Tackling Complexity in the Heart of Software* (Addison-Wesley, 2004) — "the Blue Book"
- *DDD Reference: Definitions and Pattern Summaries* (freely available PDF) — a compact reference distilling the patterns
- Numerous conference talks refining DDD's strategic design and context mapping concepts

## Influence

Evans gave the software industry a vocabulary for something practitioners had been doing imperfectly — modelling business domains in code. Martin Fowler has noted that Evans' greatest contribution was "developing a vocabulary to talk about this approach." DDD directly inspired downstream innovations: Alberto Brandolini's EventStorming, the microservices decomposition strategy of decomposing along bounded contexts, and CQRS/Event Sourcing as patterns for persisting domain state. The Blue Book is among the most influential software architecture books ever written, and "DDD" has become a standard lens through which enterprise systems are evaluated.

## Quotes

> "The heart of software is its ability to solve domain-related problems for its users. All other features, vital as they may be, support this basic purpose."

> "If the design, or some central part of it, does not map to the domain model, that model is of little value, and the correctness of the software is suspect."

## Related

- [[Domain-Driven Design]] — the methodology Evans created
- [[Bounded Context]] — strategic pattern for large-system decomposition
- [[Aggregate]] — tactical pattern for consistency boundaries
- [[Domain Event]] — captures significant occurrences in the domain
- [[Value Object]] — immutable domain concept defined by attributes
