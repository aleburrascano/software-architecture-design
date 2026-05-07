---
type: concept
created: 2026-05-03
updated: 2026-05-06
sources:
  - "raw/articles/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/articles/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "https://www.geeksforgeeks.org/system-design/cqrs-command-query-responsibility-segregation/"
  - "https://martinfowler.com/bliki/CQRS.html"
tags:
  - architecture
  - architectural-pattern
  - cqrs
  - data
---

# CQRS (Command Query Responsibility Segregation)

An architectural pattern that separates the model used for reading data (queries) from the model used for writing data (commands), allowing each to be optimized, scaled, and evolved independently.

CQRS extends the **Command Query Separation (CQS)** principle — which requires methods to either return a value or modify state, never both — to entire system architecture rather than individual method signatures.

## Problem

In a standard CRUD model, a single data model serves both reads and writes. Read and write workloads have very different characteristics: reads are often high-volume, latency-sensitive, and require denormalized views; writes must enforce invariants, emit domain events, and maintain consistency. A single model compromises both. As Fowler notes: "Many systems do fit a CRUD mental model, and so should be done in that style" — but for complex domains or high read/write imbalance, the unified model creates bottlenecks that cannot be resolved without separating the paths.

## Solution

Split the application into two distinct stacks:

- **Command side** — receives commands (intent to change state), validates them against business rules, executes them, and records the outcome. It works with a normalized, rule-enforcing write model (often backed by a relational database or event store).
- **Query side** — serves read requests from a separate read model (often denormalized, pre-computed, or cached in a NoSQL store) optimized purely for the query shapes the UI needs.

The two sides synchronize asynchronously: the command side emits domain events; projections consume those events and update the read store. This makes the system eventually consistent.

## Key Components / Structure

| Component | Responsibility |
|-----------|---------------|
| **Command** | Instruction to change state (e.g., `PlaceOrder`, `CancelShipment`) |
| **Command Handler** | Validates and executes a command against the write model |
| **Write Model / Domain Model** | Normalized, business-rule-enforcing representation; write storage optimized for transactional consistency |
| **Domain Event** | Record emitted when a command succeeds (consumed by projections) |
| **Projection / Event Handler** | Builds and updates the read model from incoming events |
| **Read Model** | Denormalized, query-optimized representation; often a separate DB, cache, or search index (e.g., MongoDB, Elasticsearch) |
| **Query Handler** | Serves read requests directly from the read model |

## When to Use

- High read/write asymmetry — e.g., e-commerce product catalog (many reads, few writes).
- Complex domain logic on the write side that would pollute a shared model.
- When read and write scaling requirements differ significantly.
- Systems already using [[Event Sourcing]] (the two are natural companions, though independent).
- Reporting/analytics queries that require a different shape than the transactional model.
- Event-driven workflows and complex business domains with distinct behavioral patterns.

**Fowler's caution:** Apply CQRS only to specific bounded contexts — not to entire systems. Most systems are not complex enough to justify it, and unnecessary adoption reduces productivity and increases project risk. Consider a simpler **Reporting Database** for demanding queries before committing to full CQRS.

## Trade-offs

**Pros:**
- Independent scaling of read and write paths.
- Write model can focus on enforcing business rules; read model can be tailored to exact query shapes.
- Improved read performance via denormalized projections.
- Natural fit for [[Event Sourcing]] and [[Event-Driven Architecture]].
- Performance optimization tailored to each workload independently.

**Cons / pitfalls:**
- Increased complexity — two models, two stacks, synchronization logic.
- Eventual consistency: read model may lag the write model; callers must tolerate stale reads.
- Higher operational overhead (more moving parts, more to monitor, multiple storage systems).
- Overkill for simple CRUD applications with low complexity and symmetric load.
- Careful planning required to keep projections in sync after schema changes.
- Potential write performance degradation when combined with event sourcing at scale.

## Real-World Usage

- **E-commerce:** The command side processes `AddToCart` and `PlaceOrder` with full business-rule enforcement; the query side serves product listings and order history from a denormalized read store using MongoDB or Elasticsearch.
- **Banking:** Balance inquiries are served from a pre-computed read model; transactions go through a write pipeline with full validation and relational consistency.
- Commonly implemented alongside event brokers (Kafka, RabbitMQ) and pairing with [[Event Sourcing]] for full auditability.
- Widely supported by frameworks across the JVM, .NET, and Node.js ecosystems — consult the sources section for specific libraries.

## Comparison: CQRS vs. Event Sourcing

CQRS and Event Sourcing are distinct and independent — but they are frequently combined:

| Aspect | CQRS | Event Sourcing |
|--------|------|---------------|
| Core concern | Separate read/write models | Store state as event log |
| Can be used alone? | Yes | Yes |
| Together | CQRS provides the query side that reads ES projections | ES provides the write side events that CQRS projections consume |

## When NOT to Use CQRS

Fowler's caution is worth repeating: CQRS is not a general-purpose pattern. Avoid it when:
- The domain is simple CRUD with no read/write asymmetry.
- The team is small and the overhead of two models exceeds the benefit.
- There is no significant difference in read and write scaling requirements.
- Eventual consistency between models is unacceptable to users (e.g., a user expects to immediately see their own write).

A **Reporting Database** (a read-optimized replica of the write DB) solves most query-shaping problems without the full CQRS complexity. Use full CQRS only when the domain genuinely requires separate models with different evolution paths.

## Infrastructure Concerns

**Dead Letter Queue (DLQ):** When a projection handler fails (e.g., deserialization error, downstream service unavailable), the event must not be silently dropped. Route it to a DLQ for later inspection, replay, or manual intervention.

**Schema Registry:** In CQRS+ES systems where events flow over a message broker, a schema registry (Confluent Schema Registry, AWS Glue) enforces event schema compatibility and enables consumers to decode events independently — see [[Event Upcasting]] for handling schema evolution.

## Framework References

This pattern is implemented by frameworks across multiple languages and ecosystems. Consult the sources section for specific library examples.

## Related

- [[Event Sourcing]] — CQRS's write side is often an event store; ES and CQRS are frequently co-deployed
- [[Event Sourcing and CQRS Integration]] — deep-dive on the combined pattern
- [[Event-Driven Architecture]] — CQRS events flow through an event bus to update projections
- [[Domain-Driven Design]] — CQRS aligns well with DDD's aggregates and domain events; Fowler recommends CQRS mainly for DDD-style complex domains
- [[Repository Pattern]] — query handlers often use a repository abstraction to access the read model
- [[Event Upcasting]] — handling schema evolution for events consumed by CQRS projections
- [[Choreography vs Orchestration]] — projections can be updated via choreography (event-reactive) or orchestration
