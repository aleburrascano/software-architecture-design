---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Architectural Patterns for Event-Driven Systems.md'
tags:
  - event-driven
  - eda
  - pub-sub
  - loose-coupling
  - microservices
  - event-notification
---
# Event Notification Pattern

An event-driven pattern where a system **signals that something happened** by publishing a minimal event; receiving systems decide independently whether and how to react, fetching any needed data themselves.

## Problem

In microservices, when service A changes state, dependent services B and C need to know. One approach: A calls B and C directly (tight coupling — A must know about B and C, and they share the same availability). Another: A publishes an event with the full payload (Event-Carried State Transfer — but now every consumer gets all data even if they don't need it). Event Notification sits between these extremes.

## Solution / Explanation

The publishing service emits a **thin event**: just enough information to identify what happened and where to get more details.

**Typical Event Notification payload:**
```json
{
  "type": "OrderPlaced",
  "orderId": "ord-12345",
  "timestamp": "2026-05-06T10:00:00Z"
}
```

The event does **not** contain the full order details. Subscribers who need order details make a follow-up call to the Order Service API.

### Characteristics

| Aspect | Value |
|---|---|
| **Payload size** | Minimal — just identifier and event type |
| **Publisher knowledge** | Publisher doesn't know who consumes or what they need |
| **Consumer autonomy** | Consumers decide whether to react and what to fetch |
| **Data freshness** | Consumers always get current data (via follow-up call) |
| **Coupling** | Publisher ↔ Consumer: decoupled on data; Consumer ↔ Publisher API: coupled on query |

### When to Use

- Consumers have **different data needs** — sending full payload wastes bandwidth for most
- Publisher wants **maximum decoupling** — no knowledge of what consumers do with the event
- **Notification of state change** is more important than the state itself
- Events are **frequent** but only a few consumers need data for each event

### Risks

**Thundering herd:** If 100 consumers all follow up on the same event, the publishing service gets 100 API calls simultaneously. Mitigation: use Event-Carried State Transfer for high-fan-out events.

**Temporal coupling via follow-up:** The consumer still needs the publisher to be available when it makes the follow-up call. Not fully temporally decoupled.

## Trade-offs

| Benefit | Cost |
|---|---|
| Minimal event payload (bandwidth efficient) | Follow-up API call required (latency, coupling) |
| Publisher doesn't dictate consumer behavior | Thundering herd risk on high fan-out |
| Easy to add new consumers | Consumer must be online to fetch data |
| Event log is not polluted with full state | State may have changed by follow-up time |

## Contrast with Related Patterns

| Pattern | Payload | Follow-up Needed | Coupling |
|---|---|---|---|
| Event Notification | Minimal | Yes | Low (data) |
| Event-Carried State Transfer | Full state | No | None |
| Event Sourcing | State-changing command as event | N/A | Changes persistence model |

## Related

- [[Event-Driven Architecture]] — Event Notification is one of EDA's core patterns
- [[Event-Carried State Transfer]] — alternative: fat events with full payload
- [[Publish-Subscribe Pattern]] — the messaging backbone for event notification
- [[Event Sourcing]] — uses domain events (different purpose: persistence, not notification)
- [[Microservices Architecture]] — Event Notification decouples microservices
