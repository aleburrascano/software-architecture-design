---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/cloud-design-patterns/inbox-pattern/'
tags:
  - messaging
  - distributed-systems
  - reliability
  - patterns
---
# Inbox Pattern

A messaging pattern that stores incoming messages in a local inbox table before processing them, enabling duplicate detection and ensuring exactly-once business logic execution even when the message broker delivers messages more than once.

## Problem

Message brokers typically guarantee **at-least-once delivery**: a message may be delivered multiple times due to network retries, consumer crashes during processing, or re-delivery after a timeout. If a consumer processes a message and then crashes before acknowledging it, the broker re-delivers it. Without deduplication, this causes duplicate side effects (e.g., double-charging a customer, sending two confirmation emails).

## Solution / Explanation

The **Inbox Pattern** is the consumer-side complement to the [[Outbox Pattern]]. It ensures that even if the same message arrives multiple times, the business logic executes exactly once.

### How It Works

1. **Receive and persist**: When a message arrives, the consumer writes it to a local `inbox` table — along with a unique message ID — within a database transaction, *before* processing.
2. **Deduplication check**: Before processing, the consumer checks whether this message ID already exists in the inbox.
3. **Process or skip**: If new, process and mark as handled (in the same transaction). If already seen, skip processing (return success to the broker to stop re-delivery).
4. **Acknowledge**: The broker is acknowledged after the transaction commits.

```
Message Broker → Consumer Service → [Inbox Table Check] → [Business Logic] → DB Commit → Broker ACK
                                         ↑
                              (duplicate? skip and ACK)
```

### Message ID as the Deduplication Key

The inbox table stores:
- `message_id` — the broker-assigned unique ID (or an idempotency key).
- `processed_at` — timestamp.
- `status` — pending / processed.

Duplicate messages share the same `message_id`, so the unique constraint on this field rejects re-insertion.

### Inbox vs. Idempotent Handler

An alternative is designing handlers to be naturally idempotent (calling them twice causes no harm). The Inbox Pattern is needed when:
- Natural idempotency is not achievable (e.g., "send email once").
- You need an audit trail of received messages.
- You need exactly-once guarantees across a transactional boundary.

## When to Use

- Event consumers in systems with at-least-once delivery guarantees.
- Any handler where duplicate execution would cause unacceptable side effects.
- Systems that need an audit log of received messages.
- Combined with the [[Outbox Pattern]] for full end-to-end delivery guarantees.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Prevents duplicate business logic execution | Adds an inbox table and deduplication lookup |
| Works with any at-least-once broker | Inbox table grows over time; needs TTL/cleanup |
| Provides an audit trail of received messages | Unique constraint requires careful concurrency handling |
| Complements the Outbox Pattern for end-to-end reliability | Adds a DB round-trip to each message receive |

## Related

- [[Outbox Pattern]] — the publisher-side complement
- [[Idempotency]] — the property the Inbox Pattern enforces for message consumers
- [[Saga Pattern]] — saga participants use inbox/idempotency to handle command retries
- [[Event-Driven Architecture]] — the broader architectural context
- [[Eventual Consistency]] — inbox enables reliable eventual consistency
