---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/eventual-consistency/'
tags:
  - distributed-systems
  - consistency
  - databases
  - microservices
---
# Eventual Consistency

A consistency model for distributed systems that guarantees that, if no new updates are made to a given data item, eventually all reads will return the last updated value — accepting temporary inconsistency between replicas in exchange for higher availability and performance.

## Problem

Strong consistency (every read reflects the latest write) requires coordination between all replicas before a write is confirmed. In distributed systems with geographic distribution, high concurrency, or independent service databases, this coordination:

- Introduces significant latency (must wait for all nodes to acknowledge).
- Reduces availability (system is unavailable if coordination fails).
- Is technically impossible during a network partition ([[CAP Theorem]]).

## Solution / Explanation

Eventual consistency relaxes the guarantee: reads may temporarily return stale data, but given enough time without new writes, all replicas **will converge** to the same value. The system trades the *I* (Isolation) and the strictest form of *C* (Consistency) in ACID for availability and partition tolerance.

### BASE vs. ACID

| | ACID | BASE |
|---|---|---|
| Stands for | Atomicity, Consistency, Isolation, Durability | Basically Available, Soft state, Eventually consistent |
| Consistency | Strong (immediate) | Eventual |
| Availability | Lower (coordination cost) | Higher |
| Partition behavior | Refuse or serialize | Continue with stale data |
| Typical system | Traditional RDBMS | Distributed NoSQL stores |

### Consistency Spectrum

Between strong consistency and eventual consistency lies a spectrum:

- **Linearizability** — all operations appear instantaneous; the strongest guarantee.
- **Sequential consistency** — operations appear in some global order consistent with each process's order.
- **Causal consistency** — causally related operations are seen in order; concurrent operations may diverge.
- **Read-your-writes** — a client always sees its own writes immediately.
- **Monotonic read consistency** — once a client reads a value, it never sees an older value.
- **Eventual consistency** — the weakest; only guarantees convergence if updates stop.

### Conflict Resolution

When multiple replicas accept concurrent writes, conflicts must be resolved on read:
- **Last Write Wins (LWW)** — based on timestamps; simple but can lose updates.
- **Vector clocks** — track causality; allow accurate conflict detection.
- **CRDTs** (Conflict-free Replicated Data Types) — data structures that merge automatically without conflicts.

### UX Implications

Eventual consistency is a **UX concern**, not just a technical one. Users may see stale data immediately after an update. Good design strategies:
- **Optimistic UI** — show the expected result immediately before the system confirms.
- **Inform the user** — "Your changes may take a moment to appear."
- **Design workflows** that tolerate staleness (e.g., social media "likes").

## When to Use

- Systems that prioritize availability over strict correctness (AP in CAP theorem).
- Data that naturally tolerates short-lived inconsistency (user profiles, product catalogs, social feeds).
- Geographically distributed systems where synchronous replication is impractical.

Avoid when:

- Financial transactions require immediate accuracy (balances, inventory).
- Sequential business processes must see each other's results immediately.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Higher availability during partitions | Temporary stale reads |
| Lower write latency (no blocking coordination) | Conflict resolution complexity |
| Scales horizontally more easily | Application logic must tolerate inconsistency |
| Enables offline-capable systems | Harder to reason about correctness |

## Related

- [[CAP Theorem]] — theoretical foundation for the consistency/availability trade-off
- [[Saga Pattern]] — sagas embrace eventual consistency across service boundaries
- [[Outbox Pattern]] — ensures eventual delivery of domain events
- [[CQRS]] — read models are often eventually consistent with the write model
- [[Event Sourcing]] — event log provides eventual consistency via event replay
- [[Idempotency]] — required when retrying operations in eventually consistent systems
