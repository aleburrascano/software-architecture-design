---
type: topic
created: '2026-05-06'
updated: '2026-05-06'
tags:
  - consistency
  - distributed-systems
  - decision-framework
  - cap-theorem
  - pacelc
  - linearizability
  - eventual-consistency
---
# Consistency Models Overview

A navigation guide across the consistency model landscape — from the theoretical foundations through practical database selection. Use this topic to understand the trade-off space and navigate to specific concept pages.

## What Is a Consistency Model?

A consistency model is a contract between a distributed system and its clients that defines **what values reads can return** given a history of writes. It answers: how "up to date" and "ordered" is the data clients observe?

See [[Consistency Models]] for the comprehensive taxonomy.

## The Trade-Off Space

Two complementary frameworks describe consistency trade-offs:

### CAP Theorem
"A distributed data store can provide at most two of: Consistency, Availability, Partition Tolerance."
Since P (partition tolerance) is unavoidable in real networks, the real choice is **C vs A during partitions**.

→ [[CAP Theorem]]

### PACELC Theorem
Extends CAP: "If Partition: choose A vs C. Else (normal operation): choose Latency vs Consistency."
**PACELC is often more useful than CAP** for everyday decisions because most time is spent in normal operation.

→ [[PACELC Theorem]]

## Consistency Model Hierarchy

From strongest to weakest guarantees:

```
Linearizability
  └── Sequential Consistency
        └── Causal Consistency
              └── FIFO / PRAM Consistency
                    └── Eventual Consistency
```

Stronger = more guarantees, higher coordination cost, more latency.
Weaker = fewer guarantees, lower coordination cost, better performance/availability.

→ [[Consistency Models]] for full definitions

## Client-Centric Guarantees

Even in an eventually consistent system, individual clients can have stronger guarantees:
- **Read-Your-Writes** — you always see your own writes
- **Monotonic Reads** — reads never go backward
- **Monotonic Writes** — your writes are applied in order
- **Writes-Follow-Reads** — causality preserved across write-read sequences

These four are **Session Guarantees** and work within AP systems.

→ [[Session Guarantees]]

## Conflict Resolution

When concurrent writes occur in an AP system:
- **Last-Write-Wins (LWW):** Simple but risks losing writes
- **Vector Clocks:** Precise causality detection; enables user-level conflict resolution
- **CRDTs:** Merge-friendly data structures that always converge correctly

→ [[Vector Clock]] | [[Gossip Protocol]] (for propagation mechanism)

## Database Decision Guide

| If you need... | Choose | Example systems |
|---|---|---|
| Never return stale data | Linearizability | etcd, CockroachDB, Spanner |
| Ordered operations, some lag OK | Sequential Consistency | Single-leader replication |
| Causally correct, not fully ordered | Causal Consistency | MongoDB sessions, some Dynamo configs |
| Best performance, eventual convergence | Eventual Consistency | Cassandra, DynamoDB default |
| My own writes visible immediately | + Read-Your-Writes | Any system with session routing |
| Never see value go backward | + Monotonic Reads | Sticky sessions |

## Implementation Mechanisms

| Mechanism | Provides | Used By |
|---|---|---|
| Raft / Paxos | Linearizability | etcd, CockroachDB |
| Primary-backup replication | Sequential Consistency (usually) | MySQL, PostgreSQL |
| Vector Clocks + anti-entropy | Causal to Eventual Consistency | Riak, Dynamo-style |
| Gossip + LWW | Eventual Consistency | Cassandra |
| Multi-version concurrency control (MVCC) | Snapshot Isolation | PostgreSQL, Oracle |

## Related Concepts

- [[CAP Theorem]] — the fundamental theorem
- [[PACELC Theorem]] — extends CAP for normal operation
- [[Consistency Models]] — full taxonomy of models
- [[Session Guarantees]] — client-centric models
- [[Eventual Consistency]] — the weakest model in depth
- [[Vector Clock]] — causality tracking
- [[Gossip Protocol]] — eventual consistency propagation mechanism
- [[etcd]] — canonical linearizable system
- [[Raft Consensus Algorithm]] — mechanism for strong consistency
- [[Distributed Transactions]] — strong consistency for multi-key operations
