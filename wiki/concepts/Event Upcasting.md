---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'CQRSKit - TypeScript CQRS ES Framework.md'
  - 'CQRS and Event Sourcing Implementation Guide.md'
tags:
  - event-sourcing
  - schema-evolution
  - event-versioning
  - backward-compatibility
  - event-upcasting
  - cqrs
---
# Event Upcasting

A technique for **migrating event schemas over time** in Event Sourcing systems by transforming old event payloads to the current schema at read time — without modifying the stored events.

## Problem

In an event-sourced system, events are **immutable once written**. But business requirements evolve: fields get renamed, types change, new required fields are added, events get split. When you replay old events to rebuild aggregate state, the old events no longer match your current domain model.

Options that don't work well:
- **Rewrite history:** Immutability is violated; corrupts the audit trail
- **Keep old handlers:** Code complexity grows indefinitely as every version must be supported forever
- **Ignore old events:** Breaks rebuild correctness

## Solution / Explanation

**Upcasting** applies a chain of transformers to old events *before* they reach the aggregate. Each transformer handles one version transition:

```
Raw event (v1) from store
       │
       ▼
Upcaster v1→v2: renames "userName" to "name"
       │
       ▼
Upcaster v2→v3: adds default "locale": "en-US"
       │
       ▼
Current event schema (v3) reaches aggregate
```

The event store remains unchanged. The transformation happens in memory at read time.

### When to Apply Upcasting

| Change Type | Upcaster Approach |
|---|---|
| **Field renamed** | `event.name = event.userName; delete event.userName` |
| **Field added (with default)** | `event.locale = event.locale \|\| "en-US"` |
| **Field removed** | Simply don't include removed field in output |
| **Field type changed** | Transform value at upcast time |
| **Event split** | One event becomes two — return an array from upcaster |
| **Event renamed** | Change `event.type` field |

### Upcast Chain

Multiple upcasters are chained in sequence. Each handles a single version step:

```
EventStore.load(aggregateId)
    → raw events
    → UpcasterChain.upcast(events)  // applies all transformers in order
    → current-schema events
    → Aggregate.apply(events)
```

### Alternative: Event Versioning

Instead of upcasting, use explicit version fields:

```json
{ "type": "UserCreated", "version": 2, "name": "Alice" }
{ "type": "UserCreated", "version": 1, "userName": "Bob" }
```

Handle each version with a dedicated handler:

```
on UserCreated v1: use event.userName
on UserCreated v2: use event.name
```

**Trade-off:** Versioning keeps handlers explicit but multiplies handler count. Upcasting keeps handlers simple but hides transformations.

### Framework Support

Upcasting support is available in event sourcing frameworks across multiple languages and ecosystems. Consult the sources section for specific library examples.

## Trade-offs

| Benefit | Cost |
|---|---|
| Event store remains immutable (audit intact) | Upcast chain must be maintained |
| Aggregate code only handles current schema | Hidden complexity in transformer chain |
| Smooth schema evolution | Performance cost for old events (all transformed at read) |
| Multiple versions supported simultaneously | Must ensure upcasters are idempotent |

## Related

- [[Event Sourcing]] — upcasting is the schema evolution technique for ES
- [[Event Sourcing and CQRS Integration]] — schema evolution in ES+CQRS systems
- [[Event-Carried State Transfer]] — ECST payloads also need schema evolution
- [[Domain Event]] — the events being upcasted
- [[Aggregate]] — the consumer of upcasted events
