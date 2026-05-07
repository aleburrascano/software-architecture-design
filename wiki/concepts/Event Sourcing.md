---
type: concept
created: 2026-05-03
updated: 2026-05-06
sources:
  - "raw/articles/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/articles/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "https://www.geeksforgeeks.org/system-design/event-sourcing-pattern/"
  - "https://martinfowler.com/eaaDev/EventSourcing.html"
tags:
  - architecture
  - architectural-pattern
  - event-sourcing
  - data
---

# Event Sourcing

An architectural pattern in which the state of an application is derived entirely from a persisted, append-only log of domain events — rather than from a current-state snapshot stored in a traditional database.

As Fowler defines it: "Capture all changes to an application state as a sequence of events."

## Problem

Conventional databases store only the current state of an entity. When a record changes, the previous state is overwritten and lost. This means:
- No audit trail of how and why the state reached its current form.
- Debugging production issues requires reconstructing history from logs, which are often incomplete.
- Rebuilding projections or integrating with downstream systems requires re-engineering.
- Temporal queries ("what was the account balance on date X?") are expensive or impossible.
- New application features cannot easily retroactively process historical facts.

## Solution

Instead of storing *state*, store every *event* that caused state to change. The event store is an immutable, append-only log. Current state is computed by **replaying** all events from the beginning (or from a snapshot checkpoint). Events are facts in the past tense: `AccountOpened`, `FundsDeposited`, `OrderShipped`, `UserRegistered`, `RegistrationCancelled`.

Fowler's analogy: it is like "keeping a detailed diary" — every state change is recorded as a separate entry rather than overwriting what came before.

### Snapshots

Replaying the full event log on every read becomes expensive for long-lived aggregates. A **snapshot** is a point-in-time capture of the current state saved periodically. On read, the system loads the latest snapshot and replays only events that occurred after it.

### What Becomes Possible (Fowler)

1. **Complete Rebuild** — Discard current application state and rebuild it entirely from the event log on an empty system.
2. **Temporal Query** — Reconstruct application state at any historical point by replaying events up to that moment.
3. **Event Replay** — Correct past errors by reversing incorrect events and replaying corrected versions forward.

## Key Components / Structure

| Component | Role |
|-----------|------|
| **Domain Event** | Immutable record of a state-changing fact (past-tense, includes timestamp and all data needed to rebuild state) |
| **Event Store** | Append-only, durable, sequential log of all events; the single source of truth |
| **Command** | Client request that aggregates validate and process, generating events if approved |
| **Aggregate** | The entity (or cluster of entities) whose state is rebuilt by replaying its event history |
| **Projection / Event Handler** | Processes events to build read-optimized views |
| **Snapshot** | Periodic captured state of an aggregate to avoid full replay |
| **Event Bus** | Messaging infrastructure enabling async communication between components subscribing to event types |

## When to Use

- Systems requiring a **complete audit trail** (financial services, healthcare, compliance-heavy domains).
- When the ability to **replay history** to debug, test, or backfill is valuable.
- Complex domains where understanding *why* state changed matters as much as *what* it is.
- Systems that need to derive multiple different read models from the same facts (often paired with [[CQRS]]).
- When temporal queries ("state as of date X") are a product requirement.
- Microservices architectures needing loose coupling via event streaming.
- Supply chain tracking, e-commerce order lifecycle, gaming state management.

**When NOT to use:**
- Systems requiring only current state with no historical needs.
- High-frequency update scenarios where event storage becomes prohibitive.
- Applications demanding strong consistency across distributed events.
- Performance-critical systems where event replay impacts latency unacceptably.
- Simple CRUD domains — as Fowler cautions, "it's not a natural choice."

## Trade-offs

**Pros:**
- Full audit log and temporal query capability built in by design.
- Events are the natural communication mechanism for downstream systems ([[Event-Driven Architecture]]).
- Easy to add new projections retroactively — just replay the log through a new projector.
- Debugging becomes more tractable: reproduce exact production state by replaying events.
- Natural fit with functional programming: `state = fold(events)`.
- Serializable events enable easy integration with new applications via event stream taps.

**Cons / pitfalls:**
- Querying current state requires either a projection or a replay — there is no direct "SELECT * FROM orders".
- **Schema evolution** is hard: old events must remain readable even as domain models change.
- Event store storage grows unboundedly (mitigated by snapshots and archiving strategies).
- **External system integration is complex**: outbound calls (notifications, payments) must be suppressed during replay; inbound query results must be cached per-event for consistent replay.
- Conceptual leap for teams unfamiliar with the pattern — higher onboarding cost.
- Eventual consistency: projections may lag behind the event store.

## Handling External Systems (Fowler)

A subtlety often overlooked:

- **Outbound calls** (e.g., sending an email): Wrap external systems in Gateways that detect replay mode and suppress notifications during non-live processing.
- **Inbound queries** (e.g., fetching a stock price during an event): Log all query responses tied to specific events; replay those cached responses when reprocessing to ensure consistent results.

## Real-World Usage

- **Financial systems:** Every debit, credit, and transfer is recorded as an event. Account balance = sum of all transactions. Enables instant audit trail and regulatory compliance.
- **Healthcare:** Documenting patient interactions, treatments, and medical procedures in EHR systems — immutable patient history.
- **E-commerce:** `OrderPlaced`, `PaymentReceived`, `ItemShipped`, `OrderCancelled` — full lifecycle traceable; replay for new analytics projections.
- **Event registration systems:** `UserRegistered`, `RegistrationCancelled` — subscribers handle notifications, ensuring timely user communication without tight coupling.
- Event sourcing frameworks and libraries exist across multiple languages and ecosystems — consult the sources section for specific examples.

## Comparison: Event Sourcing vs. CQRS

| Aspect | Event Sourcing | CQRS |
|--------|---------------|------|
| Core concern | *How* state is stored (as events) | *How* reads and writes are separated |
| Independent? | Yes — can use ES without CQRS | Yes — can use CQRS without ES |
| Together | ES provides the write-side event log; CQRS query side reads projections built from that log |

## Event Versioning

As the domain model evolves, old events in the store must remain readable. Three strategies:

- **Explicit version field:** Each event carries a `version` or `schemaVersion` field; the aggregate has a handler per version.
- **Upcasting chain:** Transform old event payloads to the current schema on read, before they reach the aggregate — see [[Event Upcasting]]. Keeps aggregate code clean; history is never modified.
- **Lazy migration:** Keep old event types as-is; handle them alongside new types until the event log is fully migrated.

## Dual-Write Solution

A frequently overlooked benefit (microservices.io framing): Event Sourcing eliminates the **dual-write problem**. Without ES, a service must atomically update its database *and* publish a message to a broker — these cannot be in the same ACID transaction, risking lost events. With ES, the event store is the single write target; the event is simultaneously the state change and the publishable message, which the [[Outbox Pattern]] or event store streaming can forward reliably.

## Framework References

This pattern is implemented by frameworks across multiple languages and ecosystems. Consult the sources section for specific library examples.

## Related

- [[CQRS]] — Event Sourcing's write model naturally feeds CQRS projections; the two are frequently paired
- [[Event Sourcing and CQRS Integration]] — deep-dive on the combined ES + CQRS pattern
- [[Event-Driven Architecture]] — events emitted by the event store can be published to an event bus for downstream consumers
- [[Domain-Driven Design]] — domain events are a core DDD concept; ES operationalizes them as the persistence mechanism
- [[Event Upcasting]] — handling schema evolution for events in the event store
- [[Outbox Pattern]] — reliable event publishing from the write side
