---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/cloud-design-patterns/outbox-pattern/'
tags:
  - messaging
  - distributed-systems
  - reliability
  - patterns
---
# Outbox Pattern

An architectural pattern that ensures reliable at-least-once delivery of domain events by writing both the state change and the outgoing event to the same database transaction, then publishing the event asynchronously from an outbox table.

## Problem

In a microservice that must both persist state and publish an event, these two operations happen against different systems (database and message broker). Any of the following can cause the event to be lost or duplicated:

- The service crashes after writing to the database but before publishing.
- The broker is temporarily unavailable.
- A network partition occurs between the write and the publish.

Publishing first and then writing causes the same class of problem in reverse.

## Solution / Explanation

1. **Write atomically.** When handling a command, the service writes the domain state change **and** a row to an `outbox` table in the **same database transaction**.
2. **Outbox table.** Each row represents a pending event (type, payload, timestamp, delivered flag).
3. **Background publisher.** A separate process (or Change Data Capture mechanism) polls the outbox table for undelivered rows and publishes them to the message broker.
4. **Mark delivered.** Once the broker acknowledges receipt, the row is marked delivered or deleted.

Because the initial write is a single atomic transaction, either both the state change and the outbox row are committed or neither is. The event can never be silently lost.

> [!question] Unverified — At-most-once variant: some implementations use CDC (e.g., Debezium reading PostgreSQL WAL) rather than polling; this can reduce database load but adds operational complexity.

## Key Components

- **Outbox table** — a simple table alongside business tables, written in the same transaction.
- **Polling publisher** — queries for unpublished rows and forwards to the broker; requires idempotency on the consumer side since at-least-once delivery means duplicates are possible.
- **CDC publisher (alternative)** — reads the database's transaction log (WAL/binlog) via a tool like Debezium; lower latency and less database load than polling.

## When to Use

- Any service that must both persist state and publish events reliably.
- Event-driven architectures requiring strong delivery guarantees.
- Saga orchestration where reliable event publication is critical.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Guarantees at-least-once delivery | Adds an outbox table and a publisher process |
| Works with any message broker | Eventual consistency — small delay before event is published |
| No 2PC required | Consumers must handle duplicate events ([[Idempotency]]) |
| Compatible with CDC tooling | CDC adds infrastructure complexity (Debezium, Kafka Connect) |

## Related

- [[Inbox Pattern]] — the receiver-side complement; deduplicates incoming messages
- [[Saga Pattern]] — relies on outbox for reliable event publishing between saga steps
- [[Idempotency]] — consumers of outbox-published events must be idempotent
- [[Event-Driven Architecture]] — broader pattern family
- [[Eventual Consistency]] — outbox introduces a small delivery delay
- [[Domain-Driven Design]] — domain events are the typical payload written to the outbox
