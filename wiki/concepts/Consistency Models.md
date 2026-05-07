---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'arXiv 1902.03305 - Consistency Models Survey.md'
  - 'Wikipedia - Consistency Model.md'
  - 'Hazelcast - Consistency in Distributed Systems.md'
  - 'Data Consistency Models in Distributed Systems.md'
tags:
  - distributed-systems
  - consistency
  - linearizability
  - sequential-consistency
  - causal-consistency
  - eventual-consistency
  - session-guarantees
---
# Consistency Models

A **consistency model** is a contract between a distributed system and its clients, specifying what values reads can return given the history of writes — defining how "up to date" and "ordered" observations of shared data can be.

## Problem

In a distributed system with replicated data, different nodes may hold different versions of the same data at any given moment. The consistency model answers: if client A writes a value, when and how will client B see it? Without a defined model, application developers cannot reason about correctness.

## The Consistency Spectrum

Consistency models form a hierarchy from strongest (most guarantees, highest cost) to weakest (fewer guarantees, lower cost):

```
Linearizability (Strict Consistency)
    │ ↓ weaker
Sequential Consistency
    │ ↓ weaker
Causal Consistency
    │ ↓ weaker
FIFO / PRAM Consistency
    │ ↓ weaker
Eventual Consistency
```

### Data-Centric Models (System-Wide Guarantees)

**Linearizability (Strong Consistency)**
- Every read returns the most recent write; operations appear to execute atomically at a single point in time
- Equivalent to a single-copy system — the gold standard
- Cost: high coordination; Raft/Paxos quorum required for every operation
- Examples: etcd, Zookeeper, Google Spanner, CockroachDB

**Sequential Consistency**
- All operations appear in the same global order to all processes, but that order doesn't have to match real time
- Weaker than linearizability: write order is consistent across all readers, but reads may lag real time
- Used in: multi-core CPU memory models (within a core)

**Causal Consistency**
- Operations that are causally related appear in the same order to all processes; concurrent operations may appear in different orders
- A writes X, then reads Y=5 and writes Y=6 — B must see X before Y=6
- Examples: MongoDB causal sessions, some distributed databases with vector clocks

**FIFO / PRAM Consistency**
- Each process's writes are seen by other processes in the order they were issued (FIFO per process)
- No guarantee about ordering between different processes' writes
- Weaker than causal; doesn't require tracking causal dependencies

**Eventual Consistency**
- If no new writes occur, all replicas will eventually converge to the same value
- No guarantee about when convergence happens or what readers see in the interim
- Examples: Cassandra, DynamoDB (default), DNS, CouchDB

### Client-Centric Models (Per-Client Guarantees)

These guarantee consistency from the perspective of a **single client's session**, rather than system-wide.

| Guarantee | Description |
|---|---|
| **Read-Your-Writes (RYW)** | Client always reads its own most recent write |
| **Monotonic Reads** | Client never sees data older than what it already read |
| **Monotonic Writes** | Client's writes are applied in the order issued |
| **Writes-Follow-Reads** | A write that follows a read is applied after all values the read returned |

These four together are called **Session Guarantees** — see [[Session Guarantees]] for details.

## Conflict Resolution

When concurrent writes occur to the same key across replicas:

| Strategy | How | Trade-off |
|---|---|---|
| **Last-Write-Wins (LWW)** | Timestamp determines winner | Simple; can lose writes |
| **Vector Clocks** | Causality graph detects concurrent writes | Accurate; complex; vectors grow |
| **CRDTs** | Merge-friendly data structures (counters, sets) | Automatic; limited data types |

## Real-World Database Mapping

| Database | Consistency Model | Notes |
|---|---|---|
| Google Spanner | Linearizability | TrueTime API for global time |
| CockroachDB | Serializable (linearizable reads) | Raft-based |
| etcd | Linearizability | Raft quorum |
| Cassandra | Tunable (eventual by default) | Quorum reads/writes = strong |
| DynamoDB | Eventual by default | Strong consistency option |
| MongoDB | Causal sessions | Tunable to majority |
| Redis | Eventual (async replication) | Strong option with WAIT |

## Trade-offs

| Stronger Model | Weaker Model |
|---|---|
| More predictable for developers | Fewer coordination costs |
| Prevents anomalies (lost writes, stale reads) | Better latency and availability |
| Requires consensus protocols | Simpler replication |
| Higher operational cost | Possible data anomalies |

## Related

- [[CAP Theorem]] — consistency is CAP's C
- [[PACELC Theorem]] — latency-consistency trade-off in normal operation
- [[Eventual Consistency]] — the weakest model in detail
- [[Session Guarantees]] — client-centric model details
- [[Vector Clock]] — causality tracking mechanism
- [[Gossip Protocol]] — eventual consistency propagation
- [[Consistency Models Overview]] — navigation topic
