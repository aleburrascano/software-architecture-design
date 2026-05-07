---
type: concept
created: '2026-05-03'
updated: '2026-05-06'
sources:
  - 'https://awesome-architecture.com/domain-driven-design/domain-events/'
tags:
  - ddd
  - domain-driven-design
  - tactical-design
  - messaging
---
# Domain Event

A record of something significant that happened in the business domain, expressed in the ubiquitous language of a [[Bounded Context]], used to communicate state changes within and between domain models while maintaining loose coupling.

## Problem

After a business operation completes, other parts of the system often need to react â€” sending an email confirmation, updating a read model, notifying another service. Encoding these reactions directly into domain logic creates tight coupling and violates the Single Responsibility Principle. How do you let the rest of the system know that something happened without the domain layer knowing about infrastructure or other domains?

## Solution / Explanation

A **Domain Event** is an immutable record that something noteworthy happened: `OrderPlaced`, `PaymentProcessed`, `CustomerRegistered`. The event is raised by the [[Aggregate]] root when its state changes and carries all the data downstream handlers need.

### Domain Events vs. Integration Events

| | Domain Event | Integration Event |
|---|---|---|
| Scope | Internal to a single Bounded Context | Crosses Bounded Context or service boundaries |
| Coupling | Tightly coupled to the domain model | Published as a schema contract |
| Transport | In-process (e.g., mediator library or event dispatcher) | Message broker (e.g., a distributed queue or pub/sub system) |
| Failure model | Handled in same transaction | Requires [[Outbox Pattern]] for reliability |

A Domain Event may be **translated** into an Integration Event before being published externally.

### Publishing Patterns

**Collect-and-dispatch** (preferred):
1. Domain operation runs; the aggregate root collects domain events in a list.
2. After the transaction commits successfully, the application service dispatches the collected events to handlers.
3. Avoids publishing events for transactions that ultimately rolled back.

**Avoid:** Raising events before the transaction commits â€” handlers may execute for a state that was never durably written.

### Key Design Principles

- Events are **named in past tense** (`OrderShipped`, not `ShipOrder`).
- Events should be **immutable** â€” they represent facts that cannot be changed.
- Avoid leaking internal Value Objects or aggregate internals through events â€” this creates coupling.
- Events carry enough data for handlers to act without re-querying the originating aggregate (but avoid embedding the entire aggregate state).

## Key Components

- **Event name** â€” descriptive past-tense identifier from the ubiquitous language.
- **Timestamp** â€” when the event occurred.
- **Aggregate identity** â€” identifies the source aggregate.
- **Relevant data** â€” fields needed by downstream handlers.

## When to Use

- Reacting to state changes without coupling the domain to application or infrastructure code.
- Propagating changes between aggregates within the same bounded context.
- Triggering async workflows (e.g., sending email after order placed).
- As the basis for eventual consistency between bounded contexts (translated to integration events).

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Decouples domain from application concerns | Event schema must be carefully versioned |
| Makes business rules explicit and visible | Handler ordering and idempotency must be managed |
| Enables reactive/async workflows | Debugging event-driven flows is harder |
| Foundation for event sourcing | Over-use of events can obscure domain intent |

## Related

- [[Domain-Driven Design]] â€” the framework this concept belongs to
- [[Aggregate]] â€” aggregates raise domain events
- [[Event Sourcing]] â€” stores the stream of domain events as the source of truth
- [[Event-Driven Architecture]] â€” architectural style built on events
- [[Outbox Pattern]] â€” for reliable publishing of domain events as integration events
- [[Event Storming]] â€” workshop technique that starts by discovering domain events
