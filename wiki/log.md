---
type: log
---
# Wiki Log

Append-only record of significant wiki changes. Newest entry first.

---

## [2026-05-06] cleanup | wiki audit: raw/ reorganization, language-agnosticism pass, connectivity fixes

**raw/ reorganization:** Moved 14 files from raw/ root into subfolders (13 → raw/articles/, 1 → raw/repos/ with rename). Updated source_path frontmatter in 14 wiki/sources/ pages and sources frontmatter across all concept/topic/people pages that referenced old bare raw/ paths.

**Connectivity fixes:** Fixed 4 wikilink alias mismatches — `[[Publish-Subscribe]]` → `[[Publish-Subscribe Pattern]]` (10 files), `[[Circuit Breaker]]` → `[[Circuit Breaker Pattern]]` (4 files), `[[CI/CD]]` → `[[Continuous Integration and Delivery]]` (2 files), `[[Architecture Decision Record]]` → `[[Architectural Decision Records (ADR)]]` (1 file).

**New concept pages:** Created 4 pages — BASE vs ACID, Feature Flags, Retry Pattern, Ubiquitous Language. (Actor Model resolved to existing Actor Model Architecture page.)

**Language-agnosticism pass:** Removed all language labels from code fences (```python, ```java, ```typescript, etc.) across 36 concept files. Converted language-specific code blocks in Dapr.md (Python @workflow → pseudocode) and Mechanical Sympathy.md (C struct → prose). Genericized named framework lists in CQRS, Event Sourcing, Event Upcasting, Hexagonal Architecture, MVC Pattern, Saga Pattern, Domain-Driven Design, Vertical Slice Architecture, Repository Pattern, Chain of Responsibility Pattern, Observer Pattern, Builder Pattern, Object Pool Pattern, Domain Event (14 files total).

**Index updated:** Added 4 missing source entries (microservices.io - Saga Pattern, Microsoft Azure - Sidecar Pattern, Microsoft Azure - Strangler Fig Pattern, Reactive Streams Specification) and 4 new concept entries (BASE vs ACID, Feature Flags, Retry Pattern, Ubiquitous Language).

---

## [2026-05-06] ingest | 48 sources: 20 articles, 9 papers, 19 repos

**Created:** 48 source pages (wiki/sources/), 18 concept pages (wiki/concepts/), 4 topic pages (wiki/topics/)

New concepts: PACELC Theorem, Consistency Models, Session Guarantees, Gossip Protocol, Vector Clock, etcd, Raft Consensus Algorithm, Event Notification Pattern, Event-Carried State Transfer, Choreography vs Orchestration, Event Sourcing and CQRS Integration, Event Upcasting, Dapr, Mechanical Sympathy, Brooks's Law, Responsibility Layers, Knowledge Level Pattern, Self-Healing Systems

New topics: System Design Interview Guide, Observability Implementation Guide, DDD Advanced Patterns, Consistency Models Overview

**Enhanced (surgical additions):** Observability (high-cardinality, core analysis loop), Distributed Tracing (OpenTracing→OTel merger, AWS X-Ray), Event-Driven Architecture (EDA patterns taxonomy table, Dapr), CQRS (when-not-to-use, DLQ, schema registry, framework refs, Event Upcasting cross-ref), Event Sourcing (event versioning strategies, dual-write framing, framework refs), CAP Theorem (PACELC extension, etcd/Raft deep-dive), Domain-Driven Design (Laribee insurance case study, large-scale patterns, DDD SLR findings, legacy modernization strategies), Aggregate (Law of Demeter, behavior over data), Circuit Breaker Pattern (Netflix Hystrix details, thread pool isolation), Modular Monolith (kgrzybek pattern, DDD integration, migration path), Architectural Decision Records (MADR template, lifecycle states, tooling), Hexagonal Architecture (DDD integration, domain-driven-hexagon pattern, port types), Bounded Context (DDD SLR context mapping finding, Laribee ACL example), Saga Pattern (orchestration vs. choreography saga, compensation pattern, cross-ref)

**Created:** wiki/index.md (full page index), wiki/log.md (this file)

---

## [2026-05-03] init | initial wiki structure

Bootstrapped wiki with ~190 pages covering: GoF patterns (23 creational/structural/behavioral), SOLID principles, DDD (Bounded Context, Aggregate, Value Object, Domain Event, Event Storming), CQRS, Event Sourcing, Event-Driven Architecture, Microservices patterns, CAP Theorem, Eventual Consistency, Observability, Distributed Tracing, Testing strategies (TDD, BDD, property-based, consumer-driven contracts), Reactive Architecture, REST/GraphQL/gRPC, Twelve-Factor App, CI/CD, Messaging (Kafka, RabbitMQ, Pub/Sub, Outbox, Inbox), Actor Model, Serverless, Modular Monolith, Hexagonal Architecture, Clean Architecture, ADR, Vertical Slice, Conway's Law, and supporting design principles.
