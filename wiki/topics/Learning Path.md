---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources: [Design Patterns Tutorial.md, Full-stack Software Design & Architecture Map.md]
tags: [meta, learning, topic]
---

# Learning Path

A guided journey through this wiki from foundational principles to distributed system design. Each stage builds on the previous.

## How to Use This Wiki

**For queries:** Start in `wiki/topics/` — synthesis pages connect concepts and answer "how does X relate to Y?" directly. Then drill into `wiki/concepts/` for depth on a specific idea.

**For learning:** Follow the stages below. Each stage assumes the previous ones.

**For reference:** Use the [[Index]] or search by tag.

---

## Stage 1 — Foundations of Good Code
*Prerequisites: basic OOP (classes, inheritance, polymorphism, interfaces)*

These principles define what "good" code looks like at the module level. Learn these before patterns.

| Concept | Why it matters |
|---------|---------------|
| [[Separation of Concerns]] | The oldest and most universal design principle |
| [[Abstraction]] | The mechanism that makes everything else possible |
| [[Encapsulation]] | How you protect invariants and hide change |
| [[Information Hiding]] | Parnas's insight: design to the secret, not the structure |
| [[Coupling and Cohesion]] | The two metrics that measure design quality |
| [[Law of Demeter]] | Don't talk to strangers; keep coupling local |

**Topic:** [[Software Design Fundamentals]]

---

## Stage 2 — Design Principles
*When to apply: before you write any non-trivial class*

| Concept | Core idea |
|---------|-----------|
| [[Single Responsibility Principle]] | One reason to change |
| [[Open-Closed Principle]] | Extend without modifying |
| [[Liskov Substitution Principle]] | Subtypes must honour contracts |
| [[Interface Segregation Principle]] | Role interfaces, not fat interfaces |
| [[Dependency Inversion Principle]] | Depend on abstractions |
| [[DRY Principle]] | Single authoritative representation |
| [[KISS Principle]] | Simplest thing that works |
| [[YAGNI Principle]] | Don't build what you don't need yet |
| [[Composition over Inheritance]] | Favour has-a over is-a |
| [[Dependency Injection]] | Supply dependencies from outside |

**Topics:** [[SOLID Principles]], [[Design Principles Overview]]

---

## Stage 3 — GoF Design Patterns
*When to apply: when building object-oriented systems in any language*

Patterns are reusable solutions to recurring problems. Learn the GoF 23 as a vocabulary.

### Creational — *how objects are created*
[[Singleton Pattern]] · [[Factory Method Pattern]] · [[Abstract Factory Pattern]] · [[Builder Pattern]] · [[Prototype Pattern]] · [[Object Pool Pattern]] · [[Lazy Initialization]]

**Topic:** [[Creational Patterns Overview]]

### Structural — *how objects are composed*
[[Adapter Pattern]] · [[Bridge Pattern]] · [[Composite Pattern]] · [[Decorator Pattern]] · [[Facade Pattern]] · [[Flyweight Pattern]] · [[Proxy Pattern]]

**Topic:** [[Structural Patterns Overview]]

### Behavioral — *how objects communicate*
[[Observer Pattern]] · [[Strategy Pattern]] · [[Command Pattern]] · [[Chain of Responsibility Pattern]] · [[Template Method Pattern]] · [[Iterator Pattern]] · [[State Pattern]] · [[Mediator Pattern]] · [[Memento Pattern]] · [[Visitor Pattern]]

**Topic:** [[Behavioral Patterns Overview]]

**Master topic:** [[Design Patterns Overview]]

---

## Stage 4 — Application Architecture
*Scale: a single deployable application*

How to structure the internals of an application beyond individual classes.

| Concept | Core idea |
|---------|-----------|
| [[Layered Architecture]] | Horizontal concern separation (Presentation / Application / Domain / Infrastructure) |
| [[MVC Pattern]] | Separate model, view, controller responsibilities |
| [[MVVM Pattern]] | Bind views to observable ViewModels; testable UI logic |
| [[Repository Pattern]] | Abstract data access behind a collection-like interface |
| [[Domain-Driven Design]] | Model the domain explicitly; Bounded Contexts, Aggregates |
| [[Clean Architecture]] | Dependencies point inward; framework-independent core |
| [[Hexagonal Architecture]] | Ports and adapters; swap infrastructure without touching domain |
| [[Onion Architecture]] | Domain at center; infrastructure at the outer ring |
| [[Vertical Slice Architecture]] | Organise by feature, not layer |
| [[Modular Monolith]] | Strong module boundaries before splitting into services |

**Topic:** [[Architectural Styles Comparison]]

---

## Stage 5 — Distributed Systems & Microservices
*Scale: multiple services, multiple teams*

| Concept | Core idea |
|---------|-----------|
| [[Microservices Architecture]] | Vertical domain decomposition; independent deployment |
| [[Event-Driven Architecture]] | Async communication via events; choreography vs orchestration |
| [[CQRS]] | Separate read and write models |
| [[Event Sourcing]] | Append-only event log as system of record |
| [[CAP Theorem]] | Consistency, Availability, Partition Tolerance — pick two |
| [[Eventual Consistency]] | Systems converge without locking; BASE vs ACID |
| [[Distributed Transactions]] | 2PC and why Saga is the modern answer |
| [[Saga Pattern]] | Distributed transaction via compensating transactions |
| [[Conway's Law]] | Your architecture mirrors your org structure |

**Topics:** [[Microservices Patterns Overview]], [[DDD Building Blocks]]

---

## Stage 6 — Reliability & Resilience Patterns
*When to apply: any service that calls another service*

| Concept | Core idea |
|---------|-----------|
| [[Circuit Breaker Pattern]] | Stop calling a failing service; fail fast |
| [[Bulkhead Pattern]] | Isolate resource pools to contain blast radius |
| [[Outbox Pattern]] | Atomic state + event publication |
| [[Idempotency]] | Safe-to-retry operations |
| [[Backpressure]] | Flow control when consumers can't keep up |
| [[Resiliency Patterns]] | Retry, Timeout, Fallback, Hedging |
| [[Anti-Corruption Layer Pattern]] | Protect your domain from external model corruption |

**Topic:** [[Cloud Design Patterns Overview]]

---

## Stage 7 — APIs & Communication
*Scale: service-to-service and client-to-service*

| Concept | Core idea |
|---------|-----------|
| [[REST]] | Fielding's constraints; stateless; resources |
| [[GraphQL]] | Query language; client-specified shape; schema |
| [[gRPC]] | Protocol Buffers; HTTP/2; streaming; contract-first |
| [[API Gateway Pattern]] | Single external entry point |
| [[Backend for Frontend Pattern]] | Per-client-type API Gateway variant |
| [[Service Mesh]] | Infrastructure layer for east-west traffic |

**Topic:** [[API Design Overview]]

---

## Stage 8 — Messaging & Event Streaming
| Concept | Core idea |
|---------|-----------|
| [[Message Queue]] | Point-to-point async; decoupling producers from consumers |
| [[Publish-Subscribe Pattern]] | Fan-out; topics; distributed Observer |
| [[Apache Kafka]] | Distributed log; partitions; consumer groups; retention |
| [[Message Broker]] | RabbitMQ vs Kafka trade-offs |
| [[Enterprise Integration Patterns]] | Hohpe & Woolf taxonomy: channels, routers, translators |

**Topic:** [[Messaging Patterns Overview]]

---

## Stage 9 — Testing & Quality
| Concept | Core idea |
|---------|-----------|
| [[Test Pyramid]] | Unit → Integration → E2E; balance for speed and confidence |
| [[Test-Driven Development]] | Red-Green-Refactor; design feedback loop |
| [[Consumer-Driven Contract Testing]] | Microservice integration verified without spinning up dependencies |
| [[Test Double]] | Mock, Stub, Fake, Spy, Dummy — Fowler's taxonomy |

**Topic:** [[Testing Strategies Overview]]

---

## Stage 10 — Observability & Operations
| Concept | Core idea |
|---------|-----------|
| [[Observability]] | Logs + metrics + traces; know what your system is doing |
| [[Distributed Tracing]] | Follow a request across service boundaries |
| [[Twelve-Factor App]] | Principles for cloud-native, deployable apps |
| [[Infrastructure as Code]] | Immutable infrastructure; drift prevention |
| [[Continuous Integration and Delivery]] | CI/CD pipeline; deployment strategies |
| [[Architectural Decision Records (ADR)]] | Record the why behind architecture decisions |

---

## Key People
The thinkers behind these ideas:

- [[Gang of Four]] — 23 GoF design patterns (1994)
- [[Martin Fowler]] — CQRS, Event Sourcing, Refactoring, Microservices, PoEAA
- [[Robert C. Martin]] — SOLID, Clean Architecture, Clean Code
- [[Eric Evans]] — Domain-Driven Design
- [[Alistair Cockburn]] — Hexagonal Architecture
- [[Ward Cunningham]] — Technical Debt, Wiki, XP
- [[Kent Beck]] — TDD, Extreme Programming, YAGNI
- [[Gregor Hohpe]] — Enterprise Integration Patterns
- [[Chris Richardson]] — Microservices patterns, Saga
- [[Edsger Dijkstra]] — Separation of Concerns, structured programming
- [[David Parnas]] — Information Hiding, modular decomposition

---

## Related

- [[Design Patterns Overview]]
- [[Software Architecture Overview]]
- [[Design Principles Overview]]
