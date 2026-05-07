---
type: topic
created: '2026-05-06'
updated: '2026-05-06'
tags:
  - ddd
  - large-scale-ddd
  - strategic-design
  - legacy-modernization
  - responsibility-layers
  - knowledge-level
  - subdomain
---
# DDD Advanced Patterns

Patterns for applying Domain-Driven Design beyond the tactical basics, covering large-scale system organization, domain distillation strategies, and legacy modernization approaches. These complement the foundational [[DDD Building Blocks]] topic.

## Prerequisites

This page assumes familiarity with:
- [[Domain-Driven Design]] — core philosophy and process
- [[DDD Building Blocks]] — tactical and strategic fundamentals (Bounded Context, Aggregate, Domain Event, etc.)

## Domain Distillation

**Domain distillation** is the strategic DDD practice of identifying which parts of your domain deserve the most investment.

### Subdomain Classification

| Subdomain Type | Description | Investment Strategy |
|---|---|---|
| **Core Domain** | What makes your business unique; competitive advantage | DDD full investment; best developers |
| **Supporting Subdomain** | Necessary for the business but not differentiating | DDD or simplified approach |
| **Generic Subdomain** | Standard functionality (auth, payments, email) | Buy off the shelf; open-source |

**Example (e-commerce):**
- Core: Recommendation engine, personalization, pricing strategy
- Supporting: Order management, inventory tracking
- Generic: Authentication, payment processing (Stripe), email (SendGrid)

### Core Domain Focus

Applying DDD everywhere is wasteful. The Evans recommendation: focus DDD investment on the **Core Domain** — that's where modeling quality makes a competitive difference. Supporting and generic subdomains can use Transaction Script or Active Record patterns.

## Large-Scale Domain Organization

### Responsibility Layers

For large domains where a single-level model becomes tangled, Evans' **Responsibility Layers** pattern organizes the domain into horizontal strata:

See [[Responsibility Layers]] for details. Summary:

```
Decision Support  (route optimization, capacity planning)
     │
Commitments       (delivery promise to customer)
     │
Policies          (delivery policy constraints)
     │
Operations        (shipment schedule, current route)
     │
Capabilities      (vehicle fleet, driver availability)
```

Each layer only depends on layers below. Business intent (commitments) is separated from mechanism (capabilities).

### Knowledge Level Pattern

The **Knowledge Level** provides a two-tier domain model where:
- **Base Level:** Concrete operational instances (specific Employee, specific Policy)
- **Knowledge Level:** Types and rules that govern base-level behavior (EmployeeType with allowed roles, PolicyType with coverage rules)

See [[Knowledge Level Pattern]] for details. Use when business rules about **how objects behave** change frequently and independently of the objects themselves.

## Context Mapping at Scale

Context Maps document the relationships between Bounded Contexts. In large systems, managing context map relationships is a strategic concern:

### Relationship Types

| Relationship | Description | Implication |
|---|---|---|
| **Shared Kernel** | Two teams share a subset of the model | High coordination cost; avoid unless necessary |
| **Customer-Supplier** | Supplier plans for customer needs | Supplier must be responsive to customer needs |
| **Conformist** | Downstream conforms to upstream model | Accept translation cost or submit to upstream |
| **Anti-Corruption Layer** | Downstream translates upstream's model | Protects domain integrity; adds complexity |
| **Open Host Service** | Publish a protocol others integrate with | Standard API; reduces integration friction |
| **Published Language** | Well-documented shared language | Enables clean integration |
| **Separate Ways** | No integration | Teams independent; no shared functionality |

**DDD SLR finding:** Context mapping is the most-used strategic DDD pattern in practice (Özkan et al., 2024) but also the most under-documented. Teams should maintain living context map diagrams.

## Legacy Modernization with DDD

Eric Evans' four strategies for applying DDD to legacy systems:

### 1. Bubble Context
Create a new, clean bounded context for new functionality. Wrap legacy with an **Anti-Corruption Layer** that translates. Legacy continues operating; new code is DDD.

### 2. Autonomous Bubble
More aggressive isolation: new bounded context communicates with legacy only through a strictly controlled ACL. New context can be developed without legacy knowledge.

### 3. Exposing Legacy as Services
Wrap legacy system with a service interface. Other contexts integrate via this service rather than directly. Enables gradual extraction.

### 4. Big Ball of Mud (Acknowledge Reality)
Sometimes the legacy system is so tangled that the pragmatic approach is to acknowledge it as a "Big Ball of Mud" and contain it — preventing it from contaminating new development while coexisting.

## DDD in Microservices: Evidence from Research

The DDD Systematic Literature Review (Özkan et al., 2024 — 36 studies) found:

- **44% of DDD studies** focus on microservices decomposition (peaked 2021)
- **Key benefit:** Bounded Contexts provide natural microservice boundaries
- **Key challenge:** Learning curve (most cited barrier) and need for domain expert involvement
- **Evaluation gap:** Only 39% of studies use empirical evaluation; most claims are anecdotal

**Pattern:** Use [[Event Storming]] to discover bounded context boundaries before committing to microservice decomposition. Premature decomposition is costly.

## Related

- [[Domain-Driven Design]] — foundational concepts
- [[DDD Building Blocks]] — tactical and strategic building blocks
- [[Bounded Context]] — primary strategic pattern
- [[Responsibility Layers]] — large-scale domain organization
- [[Knowledge Level Pattern]] — meta-level domain model
- [[Event Storming]] — discovery technique
- [[Microservices Architecture]] — DDD as decomposition guide
- [[Anti-Corruption Layer Pattern]] — legacy integration
