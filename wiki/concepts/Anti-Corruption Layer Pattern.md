---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - >-
    https://awesome-architecture.com/cloud-design-patterns/anti-corruption-layer-pattern/
tags:
  - ddd
  - integration
  - patterns
  - domain-driven-design
---
# Anti-Corruption Layer Pattern

A translation layer that isolates a domain model from external systems or legacy systems with incompatible models, preventing the external system's design constraints and terminology from "corrupting" the internal domain.

## Problem

When integrating with external systems, legacy applications, or other bounded contexts that have different domain models, there is a risk of **model corruption**: the external system's concepts leak into your domain, forcing you to adopt its naming, structure, and constraints. Over time:

- Your codebase becomes polluted with foreign concepts.
- Business logic must accommodate the external system's quirks.
- Changes to the external system ripple through your domain model.
- Your ubiquitous language degrades into a mix of internal and external terms.

## Solution / Explanation

The **Anti-Corruption Layer (ACL)** acts as a protective wrapper between your system and the external system. It:

1. **Translates** external system concepts to internal domain concepts (and vice versa).
2. **Maps** between external data structures and internal domain objects.
3. **Isolates** the internal domain from changes in the external system — changes to the external API only affect the ACL, not the domain.
4. **Enforces** the internal ubiquitous language — the domain never sees external terms.

```
┌──────────────────────┐         ┌──────────────────────────────┐
│  Your Domain         │         │  External System / Legacy    │
│                      │ ◄──────► │  (different model, terms,    │
│  Internal concepts   │  ACL    │   API shape)                 │
│  Ubiquitous language │         │                              │
└──────────────────────┘         └──────────────────────────────┘
```

### Typical ACL Components

- **Facade** — simplifies the external system's complex interface.
- **Adapter** — converts the external interface to match the internal domain's expected interface.
- **Translator** — maps between external DTOs/models and internal domain objects.

### When to Protect with an ACL

In DDD's Context Map, an ACL is the appropriate relationship when:
- You are the **downstream** context (you depend on the upstream).
- The upstream model is significantly different from yours.
- You cannot change the upstream model (legacy system, third-party service).
- You want to be insulated from upstream changes.

Without an ACL, the alternative is **Conformist** — your model conforms to theirs, accepting corruption.

## When to Use

- Integrating with legacy systems that have outdated or poorly designed models.
- Consuming third-party APIs or external services.
- Integrating bounded contexts that have significantly different domain vocabularies.
- Protecting a clean domain model from the mess of an external system.
- Migration scenarios ([[Strangler Fig Pattern]]) where the new system must coexist with legacy.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Protects domain integrity | Additional translation layer to maintain |
| Isolates domain from external change | Translation bugs can introduce subtle errors |
| Keeps ubiquitous language clean | Performance overhead of translation |
| Enables independent evolution | Must be updated when either model changes |

## Related

- [[Domain-Driven Design]] — ACL is a strategic Context Map pattern
- [[Bounded Context]] — ACL sits at the boundary between contexts
- [[Strangler Fig Pattern]] — ACL often used during migration to isolate legacy from new system
- [[Adapter Pattern]] — ACL is often implemented using the Adapter structural pattern
- [[Facade Pattern]] — ACL may include a facade to simplify external interfaces
