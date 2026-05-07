---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Data Consistency Models in Distributed Systems.md'
  - 'Wikipedia - Consistency Model.md'
  - 'arXiv 1902.03305 - Consistency Models Survey.md'
tags:
  - distributed-systems
  - vector-clock
  - causality
  - happened-before
  - conflict-detection
  - logical-time
  - lamport-clock
---
# Vector Clock

A **Vector Clock** is a data structure that assigns a causally-ordered logical timestamp to events in a distributed system, enabling nodes to determine whether two events are causally related or concurrent — without requiring synchronized physical clocks.

## Problem

In distributed systems, physical clocks are unreliable (clock drift, NTP imprecision). Yet causality is fundamental: if event A caused event B, then B should never be applied before A. Simple Lamport clocks can establish ordering but cannot distinguish causally related events from concurrent events. Without causality tracking, distributed databases risk applying conflicting writes incorrectly.

## Solution / Explanation

A vector clock is a vector of integer counters, one per node in the system. Each node maintains its own vector `VC[n]` where n is the number of nodes.

### Rules

1. **Local event:** Node i increments its own counter: `VC[i]++`
2. **Send message:** Node i sends its current vector clock with the message
3. **Receive message:** Node i merges: `VC[j] = max(VC[j], msg.VC[j])` for all j, then increments `VC[i]++`

### Comparing Vector Clocks

Given two vector clocks VA and VB:

- **VA happened-before VB** (A → B): `VA[i] ≤ VB[i]` for all i, and `VA[j] < VB[j]` for at least one j
- **VA and VB are concurrent** (A ∥ B): neither VA ≤ VB nor VB ≤ VA — some components go each way
- **VB happened-before VA** (B → A): symmetric to above

Concurrent events detected this way can be flagged for conflict resolution (LWW, CRDT merge, or user resolution).

### Example

```
Node A: [1,0,0] → sends to B
Node B: receives, merges → [1,1,0], processes, sends to C
Node C: receives [1,1,0], merges → [1,1,1]
Node A independently: [2,0,0] — concurrent with B's [1,1,0]
```

A's `[2,0,0]` and B's `[1,1,0]` are concurrent: A[0]=2 > B[0]=1 but A[1]=0 < B[1]=1. Neither happened-before the other.

### Limitations

- **Vector size grows with N nodes:** In large clusters this becomes expensive
- **Dotted Version Vectors:** A variant that scales better for large clusters
- **CRDTs** often use vector clocks internally but expose a simpler interface

### Real-World Usage

| System | Use |
|---|---|
| **Amazon Dynamo** | Detect concurrent writes; present conflicts to application |
| **Riak** | Sibling detection for concurrent writes |
| **Git** | DAG history serves a similar purpose (merge commits = concurrent) |
| **CRDTs** | Internal causality tracking in many CRDT implementations |
| **Cassandra** | Replaced by timestamps (LWW) for simplicity — sacrifices conflict detection |

## Trade-offs

| Benefit | Cost |
|---|---|
| Precise causality tracking | Vector size = O(N) nodes |
| Detects concurrent writes correctly | Adds overhead to every message |
| Enables principled conflict resolution | Application must handle concurrent writes |
| No physical clock dependency | Complex to reason about in large systems |

## Related

- [[Consistency Models]] — vector clocks implement causal consistency
- [[Gossip Protocol]] — gossip uses vector clocks for anti-entropy
- [[Eventual Consistency]] — vector clocks detect when eventual convergence has conflicts
- [[CAP Theorem]] — AP systems need vector clocks for conflict detection
