---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Mia-Platform - Event Sourcing and CQRS.md'
  - 'Mastering CQRS and Event Sourcing.md'
  - 'CQRS and Event Sourcing Implementation Guide.md'
  - 'microservices.io - Event Sourcing Pattern.md'
tags:
  - event-sourcing
  - cqrs
  - projections
  - kafka
  - audit-trail
  - ddd
  - architectural-pattern
---
# Event Sourcing and CQRS Integration

The architectural pattern that combines **Event Sourcing** (ES) as the write-side persistence model with **CQRS** (Command Query Responsibility Segregation) read-side projections — each pattern resolving the other's primary weakness.

## Problem

**Event Sourcing alone:** Storing state as an event log is powerful, but rebuilding current state by replaying all events is expensive. Complex read queries (reporting, filtering, search) against an event store are impractical.

**CQRS alone:** Separating read and write models is useful, but what populates the write model's event log? CQRS without Event Sourcing typically still uses a traditional database for writes, losing the audit trail.

**Combined:** ES provides the authoritative write-side log; CQRS projections provide optimized read models built from that log.

## Solution / Explanation

### The Combined Flow

```
Command
   │
   ▼
Command Handler
   │
   ▼
Aggregate (ES write model)
   │ validates command
   │ applies domain event
   ▼
Event Store (append-only log)
   │
   ├──► Event Bus (Kafka / RabbitMQ)
   │
   ▼ (async)
Projection Engine / Event Handler
   │
   ├──► Read Model A (search optimized)
   ├──► Read Model B (reporting)
   └──► Read Model C (mobile view)
         │
         ▼
    Query Handler responds to read requests
```

### Event Sourcing as the Write Side

- Aggregates process commands, validate business rules, and emit domain events
- Domain events are appended to the event store (immutable, append-only)
- Current aggregate state is reconstructed by replaying events (or from a snapshot)
- The event log is the **single source of truth** for the write side

### CQRS Projections as the Read Side

- Event handlers consume domain events from the event bus
- Each projection builds a denormalized read model optimized for a specific query
- Read models are **disposable and rebuildable** — just replay all events into a fresh projection
- Multiple projections can serve different consumers' needs without affecting the write side

### Kafka-Based Pattern

In high-scale systems, Apache Kafka typically serves as the event bus:

```
Write Service ──► Event Store ──► Kafka Topic ──► Projection Service ──► Read DB
                                              └──► Analytics Service ──► Analytics DB
                                              └──► Search Service ──► Elasticsearch
```

Kafka's durable, replayable log means:
- New projections can be bootstrapped by replaying historical events
- Multiple consumers each maintain their own read model without coordination
- **Single Views** (denormalized documents representing a complete entity) are a common pattern

### Benefits of Combining

| Benefit | How ES+CQRS Achieves It |
|---|---|
| **Full audit trail** | Every state change is an event in the store |
| **Optimized reads** | Each projection tailored to its consumers |
| **Change tracking** | Event history shows who changed what, when |
| **Audit compliance** | Immutable event log satisfies regulatory requirements |
| **Temporal queries** | Replay up to a timestamp to see historical state |
| **Read model rebuild** | Drop and recreate projections by replaying events |
| **Reduced contention** | Write side and read side scale independently |

### Schema Evolution

Event schemas evolve over time. Use **[[Event Upcasting]]** to transform old events to new schema at read time, or version events explicitly.

## When to Use

Use when you need:
- Regulatory audit requirements
- Complex reporting with multiple read models
- High read/write asymmetry
- Temporal queries (historical state)
- Multiple consumers needing different data shapes

Avoid when:
- Simple CRUD with no audit requirements
- Small team / limited operational capacity
- No meaningful read/write asymmetry

## Trade-offs

| Benefit | Cost |
|---|---|
| Full audit trail | Significant architectural complexity |
| Rebuildable read models | Eventual consistency on read side |
| Independent read/write scaling | Event schema evolution management |
| Multiple tailored read models | Operational overhead (event bus, projections) |

## Related

- [[CQRS]] — the read-side pattern
- [[Event Sourcing]] — the write-side pattern
- [[Event-Driven Architecture]] — the broader architectural style
- [[Apache Kafka]] — common event transport
- [[Event Upcasting]] — schema evolution for event log
- [[Domain Event]] — the domain-level events that flow through this architecture
- [[Saga Pattern]] — ES+CQRS in distributed transaction context
