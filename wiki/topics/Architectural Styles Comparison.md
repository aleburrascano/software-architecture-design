---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/What is Software Architecture A Comprehensive Guide.md"
  - "raw/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/Software architecture 1.md"
tags:
  - architecture
  - topic
  - comparison
---

# Architectural Styles Comparison

A synthesis comparing the major architectural styles and patterns covered in this wiki — their core ideas, trade-off profiles, and when to choose each.

## The Fundamental Axis: Monolith vs. Distributed

All architectures can be categorized at the highest level:

- **Monolith** — all components deployed as a single unit (Layered Architecture is the canonical example).
- **Distributed** — components deployed separately, communicating over a network (Microservices, SOA, EDA).

Each category has subcategories. A **modular monolith** is a middle ground: domain-organized code deployed as one unit.

## Style-by-Style Summary

### [[Layered Architecture]] (N-Tier)

| Dimension | Detail |
|-----------|--------|
| Decomposition axis | Horizontal (by technical concern: Presentation / Logic / Data) |
| Deployment | Single unit (monolith) or separate tiers |
| Communication | Synchronous, downward through layers |
| Scaling | Scale the whole application (or individual tiers) |
| Team model | Single team or feature teams sharing codebase |
| Complexity | Low operational complexity |
| Best for | Enterprise CRUD apps; teams starting out; clear concern separation needed |
| Avoid when | High domain complexity requiring domain-team autonomy; wildly different scaling per domain |

### [[Microservices Architecture]]

| Dimension | Detail |
|-----------|--------|
| Decomposition axis | Vertical (by business domain) |
| Deployment | Independent per service (containers / Kubernetes) |
| Communication | Synchronous REST/gRPC or async messaging |
| Scaling | Per-service, independent |
| Team model | Each service owned by one team |
| Complexity | High operational complexity (container orchestration, distributed tracing, service mesh) |
| Best for | Large orgs with distinct domains; different scaling needs per domain; many teams in parallel |
| Avoid when | Small team; not enough DevOps maturity; unclear domain boundaries |

### [[Event-Driven Architecture]]

| Dimension | Detail |
|-----------|--------|
| Decomposition axis | By event type and reaction |
| Deployment | Services/components + broker infrastructure |
| Communication | Asynchronous events via broker (Kafka, RabbitMQ) |
| Scaling | Consumers scale independently from producers |
| Team model | Any — cross-cuts other styles |
| Complexity | Moderate to high (broker ops, eventual consistency, tracing) |
| Best for | Real-time notifications; IoT pipelines; fan-out scenarios; decoupled microservices |
| Avoid when | Simple request/response patterns where async adds complexity without benefit |

### [[Service-Oriented Architecture]]

| Dimension | Detail |
|-----------|--------|
| Decomposition axis | By business function / capability |
| Deployment | Shared infrastructure with ESB |
| Communication | Standardized protocols (SOAP, REST) via ESB |
| Scaling | Centralized infrastructure |
| Team model | Central architecture team + service teams |
| Complexity | High governance overhead |
| Best for | Large enterprises integrating heterogeneous legacy systems |
| Avoid when | Greenfield projects; small-to-medium organizations; need for high agility |

## Pattern-by-Pattern Summary

### [[MVC Pattern]]

Scope: application-level (within a service or monolith). Separates Model (data/rules), View (rendering), Controller (input handling). Entry point for web application structure. Insufficient alone for complex business domains — needs supplementary patterns.

### [[CQRS]]

Scope: component or service-level. Separates command (write) and query (read) models. Applies when read/write workloads are asymmetric or when read and write models need to evolve independently. Introduces eventual consistency. Frequently paired with [[Event Sourcing]].

### [[Event Sourcing]]

Scope: persistence strategy. Stores events not state; state is derived by replay. Provides full audit trail and temporal queries. Adds complexity in schema evolution and read-path design. Naturally feeds [[CQRS]] projections.

### [[Repository Pattern]]

Scope: data access within a component. Abstracts persistence behind a collection interface. Enables testability (swap real DB for in-memory). Core building block in [[Domain-Driven Design]], [[Clean Architecture]], and [[Hexagonal Architecture]].

### [[Domain-Driven Design]]

Scope: approach to complex domain modeling. Provides Bounded Contexts (strategic), and Entities, Value Objects, Aggregates, Domain Events, Repositories (tactical). Best for domains where the business model is the hard part. Natural boundary provider for [[Microservices Architecture]].

### [[Clean Architecture]] / [[Hexagonal Architecture]]

Scope: dependency management between layers. Strict inward-only dependencies (Clean) or inside/outside with Ports and Adapters (Hexagonal). Makes the domain core independently testable. Foundational for DDD implementations.

## Decision Guide

```
Is the domain simple / CRUD-heavy?
  Yes → Layered Architecture (3-tier) + MVC
  No  → Is it a complex business domain?
          Yes → Domain-Driven Design + Layered/Clean/Hexagonal Architecture
               → Add CQRS if read/write asymmetry is significant
               → Add Event Sourcing if audit trail / replay is required

Is the system large with multiple independent teams?
  Yes → Microservices Architecture
       → Use DDD Bounded Contexts to define service boundaries
       → Use EDA for async inter-service communication

Is real-time / event-driven behavior a core requirement?
  Yes → Event-Driven Architecture (pub/sub or streaming)
       → Consider Event Sourcing if events are also the persistence model

Are you integrating large numbers of enterprise legacy systems?
  Yes → Service-Oriented Architecture (or modern API gateway + service registry)
```

## Trade-off Matrix

| Style/Pattern | Scalability | Testability | Operational Complexity | Deployment Agility | Domain Modeling |
|---------------|-------------|-------------|------------------------|--------------------|-----------------|
| Layered | Medium | Medium | Low | Low | Medium |
| Microservices | High | Medium | High | High | Medium |
| EDA | High | Low-Medium | Medium-High | High | Low |
| SOA | Medium | Low | High | Low | Low |
| CQRS | High (read) | High | Medium | Medium | High |
| Event Sourcing | Medium | High | Medium | Medium | High |
| Clean/Hexagonal | N/A (structural) | Very High | Low | Medium | High |
| DDD | N/A (approach) | High | Low | High | Very High |

## Related

- [[Software Architecture Overview]] — foundational definitions and the architect's role
- [[Layered Architecture]], [[Microservices Architecture]], [[Event-Driven Architecture]], [[Service-Oriented Architecture]] — style pages
- [[CQRS]], [[Event Sourcing]], [[Repository Pattern]], [[MVC Pattern]] — pattern pages
- [[Domain-Driven Design]], [[Clean Architecture]], [[Hexagonal Architecture]] — design approach pages
