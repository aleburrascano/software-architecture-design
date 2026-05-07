---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources: []
tags:
  - distributed-systems
  - consistency
  - databases
  - cap-theorem
---

# BASE vs ACID

Two contrasting sets of properties that describe how data stores handle consistency and availability. Understanding both is essential for selecting the right storage model in a given architectural context.

## ACID

Properties of traditional relational databases designed for correctness above all else.

| Property | Meaning |
|---|---|
| **Atomicity** | A transaction either fully succeeds or fully fails — no partial state |
| **Consistency** | Every transaction brings the database from one valid state to another |
| **Isolation** | Concurrent transactions behave as if executed sequentially |
| **Durability** | Committed transactions survive crashes |

ACID guarantees strong consistency but typically sacrifices availability and horizontal scalability, especially under network partitions (see [[CAP Theorem]]).

## BASE

Properties common in distributed and NoSQL databases that prioritize availability and partition tolerance over strict consistency.

| Property | Meaning |
|---|---|
| **Basically Available** | The system guarantees availability (though responses may be stale) |
| **Soft state** | State may change over time even without input, as the system converges |
| **Eventually consistent** | Given no new writes, all nodes will converge to the same value |

BASE systems trade immediate consistency for scalability and fault tolerance. See [[Eventual Consistency]].

## Choosing Between Them

| Use ACID when... | Use BASE when... |
|---|---|
| Financial transactions | Social feeds, view counts |
| Inventory that must not oversell | Product catalogs, user preferences |
| Regulatory or audit requirements | High-throughput write workloads |
| Strong consistency is non-negotiable | Temporary inconsistency is tolerable |

Many modern systems use a hybrid approach: ACID for core transactional data, BASE for read-optimized or high-volume secondary stores.

## Related Concepts

- [[CAP Theorem]] — the fundamental theorem that explains why BASE properties emerge in distributed systems
- [[PACELC Theorem]] — extends CAP to cover the latency vs. consistency trade-off during normal operation
- [[Eventual Consistency]] — the consistency model at the heart of BASE
- [[Consistency Models]] — the broader spectrum of consistency guarantees
- [[CQRS]] — often used alongside BASE stores on the read side
