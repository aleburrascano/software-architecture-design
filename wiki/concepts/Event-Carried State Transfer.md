---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Architectural Patterns for Event-Driven Systems.md'
tags:
  - event-driven
  - eda
  - state-replication
  - loose-coupling
  - event-carried-state
  - temporal-decoupling
---
# Event-Carried State Transfer

An event-driven pattern where **events carry the complete relevant state** in their payload, enabling consumers to process events without making follow-up calls to the publisher — achieving full temporal decoupling.

## Problem

Event Notification (thin events) requires consumers to call back to get state, creating latency and availability coupling: if the publisher is down when the consumer processes the event, the consumer cannot proceed. How do you decouple consumers from needing the publisher to be available at processing time?

## Solution / Explanation

The publishing service includes **all state the consumer needs** in the event payload itself:

**Typical ECST payload:**
```json
{
  "type": "OrderPlaced",
  "orderId": "ord-12345",
  "timestamp": "2026-05-06T10:00:00Z",
  "customerId": "cust-789",
  "items": [
    {"productId": "prod-101", "quantity": 2, "price": 29.99},
    {"productId": "prod-202", "quantity": 1, "price": 49.99}
  ],
  "shippingAddress": { "street": "...", "city": "..." },
  "totalAmount": 109.97
}
```

The consumer has everything it needs to:
- Update its local read model / cache
- Trigger downstream business logic
- Respond to the customer

No follow-up call needed.

### Characteristics

| Aspect | Value |
|---|---|
| **Payload size** | Full state — potentially large |
| **Consumer autonomy** | Complete — no dependency on publisher at processing time |
| **Temporal decoupling** | Full — consumer processes events from queue asynchronously |
| **Data freshness** | State in event is current at time of publication; may be stale by processing time |
| **Local caching** | Consumers maintain local copies of foreign-domain state |

### Local Cache Pattern

Consumers often maintain a local projection of relevant data from the publishing domain. This eliminates **synchronous cross-service queries** entirely:

```
Order Service publishes OrderPlaced (with full product + customer state)
     ↓
Warehouse Service maintains local product catalog cache (updated via events)
Warehouse never calls Product Service synchronously
```

This is a key pattern in **CQRS read models** — projections built from event streams.

### When to Use

- Consumers are **downstream services that cache foreign-domain state** (e.g., a shipping service caching customer addresses)
- **High event throughput** — eliminating follow-up calls is critical for performance
- **Circuit breaker isolation** — consumers must work even when publishers are down
- Events are published to a **durable event stream** (Kafka) where reprocessing is possible

### Risks

**State drift:** If a consumer misses an event or processes them out of order, its local cache becomes inconsistent. Mitigation: use event sequence numbers; design for idempotency.

**Large payload size:** For events with extensive state, payload size can be a concern. Use schema registries for efficient serialization (Avro, Protobuf).

**Schema evolution:** When the publisher changes its event schema, all consumers must update. Use [[Event Upcasting]] to handle schema migration.

## Trade-offs

| Benefit | Cost |
|---|---|
| Full temporal decoupling | Large event payloads |
| No follow-up calls (low latency) | State may be stale by processing time |
| Consumers work even when publisher is down | Schema evolution affects all consumers |
| Natural CQRS read model building | Local caches can diverge from source of truth |

## Related

- [[Event-Driven Architecture]] — ECST is one of EDA's core patterns
- [[Event Notification Pattern]] — contrast: thin events requiring follow-up
- [[CQRS]] — ECST events are the feed for CQRS read model projections
- [[Event Sourcing]] — ES is a persistence model; ECST is a communication pattern
- [[Event Upcasting]] — schema evolution for ECST payloads
- [[Publish-Subscribe Pattern]] — delivery mechanism
