---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/distributed-transactions/'
tags:
  - distributed-systems
  - consistency
  - transactions
  - databases
---
# Distributed Transactions

A transaction that spans multiple independent systems (databases, services, or resource managers), coordinating them so that they all commit or all abort together — providing atomicity across distributed resources.

## Problem

Business operations often need to modify data in multiple systems atomically: debit a bank account *and* credit another, or create an order record *and* reserve inventory. If one operation succeeds and the other fails, the system is left in an inconsistent state.

In a monolith with a single database, a local ACID transaction handles this trivially. In distributed systems with the Database per Service pattern or multiple third-party integrations, no single ACID transaction boundary spans all the resources.

## Solution / Explanation

### Two-Phase Commit (2PC)

The classical protocol for distributed transactions:

**Phase 1 — Prepare**: A **coordinator** asks all **participants** (databases, services) to prepare to commit. Each participant acquires locks and writes to a prepare log, then votes Yes or No.

**Phase 2 — Commit/Abort**: If all participants vote Yes, the coordinator sends Commit to all. If any vote No, the coordinator sends Abort to all.

**Problems with 2PC:**
- **Locks are held** throughout both phases — reduces throughput significantly.
- **Blocking** if the coordinator crashes after Phase 1 — participants are stuck waiting with locks held.
- **Not available during partitions** (CP in CAP theorem).
- Most modern NoSQL databases and cloud services **do not support** the XA/2PC protocol.
- Tight coupling between all participant systems.

### Saga Pattern as the Alternative

The [[Saga Pattern]] is the modern alternative to 2PC in microservices:
- Breaks the distributed transaction into local transactions + compensating transactions.
- Achieves eventual (not immediate) consistency.
- Works without locking resources across services.
- Supported by any messaging infrastructure.

### When 2PC Is Still Appropriate

2PC remains appropriate in:
- Traditional enterprise environments with XA-compatible databases (Oracle, SQL Server).
- Systems requiring **strict consistency** (financial ledgers, inventory allocation).
- Monoliths or tightly-coupled services sharing infrastructure.

In modern microservices, the consensus is: **use Saga** rather than 2PC. As one widely-cited opinion states: "It's Time to Move on from Two Phase Commit."

### Other Approaches

**Try-Confirm/Cancel (TCC)**: An application-level protocol similar to 2PC where services implement `try`, `confirm`, and `cancel` operations. More flexible than XA but requires implementing three methods per operation.

**Outbox + Inbox**: The [[Outbox Pattern]] and [[Inbox Pattern]] together provide reliable, exactly-once-like event delivery for event-driven coordination — without a coordinator.

## Trade-offs

| Approach | Consistency | Availability | Complexity |
|---|---|---|---|
| 2PC / XA | Strong (immediate) | Low (locks, blocking) | High |
| Saga (choreography) | Eventual | High | Medium |
| Saga (orchestration) | Eventual | High | Medium-High |
| TCC | Strong (application-level) | Medium | Very High |

## Related

- [[Saga Pattern]] — the recommended alternative to 2PC in microservices
- [[CAP Theorem]] — explains why strong consistency and availability can't both be achieved
- [[Eventual Consistency]] — the consistency model sagas embrace
- [[Outbox Pattern]] — enables reliable event publication as part of distributed coordination
- [[Idempotency]] — all distributed coordination requires idempotent participants
