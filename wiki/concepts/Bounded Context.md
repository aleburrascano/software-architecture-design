---
type: concept
created: '2026-05-03'
updated: '2026-05-06'
sources:
  - 'https://awesome-architecture.com/domain-driven-design/bounded-context/'
  - 'https://awesome-architecture.com/domain-driven-design/domain-driven-design/'
tags:
  - ddd
  - domain-driven-design
  - strategic-design
  - microservices
---
# Bounded Context

A strategic Domain-Driven Design pattern that draws an explicit boundary around a domain model, within which a specific [[Domain-Driven Design|ubiquitous language]] is consistent and unambiguous.

## Problem

Large software systems model complex domains. The same word can mean different things to different teams: "Account" means one thing to the billing team and another to the authentication team. When a single unified domain model tries to satisfy everyone, it becomes bloated, ambiguous, and impossible to maintain.

## Solution / Explanation

A **Bounded Context** defines where a particular model applies. Inside the boundary:
- Every term in the ubiquitous language has exactly one meaning.
- Domain objects, rules, and behaviors are coherent and consistent.
- The model can be changed without breaking other contexts.

At the **boundary**, explicit translation (via a Context Map) mediates between the different languages of adjacent contexts.

### Bounded Context and Microservices

A Bounded Context is often (but not always) a good candidate for a single microservice or a small cluster of services. The context boundary becomes the service boundary, preventing accidental coupling across domains.

### Context Map

A **Context Map** documents how different Bounded Contexts relate and integrate. Common relationships:

| Relationship | Description |
|---|---|
| **Shared Kernel** | Two teams share a subset of the model; changes require coordination |
| **Customer-Supplier** | Downstream team (customer) depends on upstream (supplier) |
| **Conformist** | Downstream conforms to upstream's model; no translation |
| **Anti-Corruption Layer** | Downstream translates upstream's model to protect its own |
| **Open Host Service** | Upstream publishes a protocol/API that all downstreams use |
| **Published Language** | A shared, versioned interchange language |
| **Separate Ways** | No integration; contexts evolve independently |

### Bounded Context Canvas

A collaborative design tool (from the DDD Crew community) for capturing: purpose, strategic classification, inbound/outbound communication, domain roles, and ubiquitous language for a context.

## Key Components

- **Ubiquitous language** — the shared vocabulary used by developers and domain experts within the boundary.
- **Domain model** — entities, value objects, aggregates, and services that live inside the context.
- **Boundary** — explicit contract (API, events, ACL) for interaction with other contexts.

## When to Use

- Decomposing a large system into independently deployable units.
- When different sub-teams use different vocabularies for the same concepts.
- When a monolith is growing into a microservices architecture.
- When different parts of the system have different consistency, availability, or data requirements.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Reduces model complexity within a context | Requires explicit integration/translation at boundaries |
| Enables independent team ownership | Context mapping relationships must be documented and maintained |
| Aligns with Conway's Law | Drawing the right boundary is difficult and often iterative |
| Makes ubiquitous language explicit | Shared data across contexts requires careful synchronization |

## Context Mapping in Practice (DDD SLR)

A 2023 systematic literature review of 36 DDD studies found that **Bounded Context** is the most frequently applied strategic DDD pattern in practice, and **Context Mapping** is its most critical operational activity. Key practical findings:

- Context maps tend to drift from reality as teams evolve; they require active maintenance, not one-time documentation.
- The most common relationship in real systems is **Customer-Supplier** (upstream team sets the contract; downstream adapts).
- **Anti-Corruption Layers** are heavily used at legacy system boundaries to prevent domain model pollution.

### ACL Example (Laribee — Insurance Domain)

When a new insurance policy management context integrates with a legacy claims processing system, the legacy system uses the term "Contract" for what the new domain calls "Policy", and "Claimant" for "InsuredPerson". An ACL translates:

- Incoming: `LegacyContract` → `Policy` (mapping field by field, resolving semantic differences)
- Outgoing: `Policy.submit()` → legacy `Contract.finalize()` call with required field transformations

Without the ACL, the new model would gradually conform to legacy terminology (the "Conformist" anti-pattern), polluting the ubiquitous language.

## Related

- [[Domain-Driven Design]] — strategic design framework this concept belongs to
- [[Aggregate]] — the primary consistency boundary within a Bounded Context
- [[Domain Event]] — crosses context boundaries as an integration event
- [[Anti-Corruption Layer Pattern]] — translates between contexts with incompatible models
- [[Microservices Architecture]] — bounded contexts often map to microservice boundaries
- [[Event Storming]] — workshop technique for discovering bounded contexts
