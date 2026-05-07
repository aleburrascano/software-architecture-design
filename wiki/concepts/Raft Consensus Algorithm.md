---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'etcd Distributed Key-Value Store.md'
  - 'Data Consistency Models in Distributed Systems.md'
tags:
  - raft
  - consensus
  - distributed-systems
  - leader-election
  - log-replication
  - paxos
  - distributed-consensus
  - etcd
---
# Raft Consensus Algorithm

A **consensus algorithm** designed for understandability that achieves the same safety guarantees as Paxos by decomposing consensus into three independent sub-problems: leader election, log replication, and safety. Used by etcd, CockroachDB, TiKV, and Consul.

## Problem

Distributed systems need to agree on shared state (e.g., a key-value store's contents, a database transaction log) even when some nodes fail. The classic algorithm, Paxos, is famously hard to understand and implement correctly — leading to implementation bugs and subtle consistency violations in production systems. How can you build a consensus algorithm that is both provably correct and implementable by ordinary engineers?

## Solution / Explanation

Diego Ongaro and John Ousterhout designed Raft (2014) specifically for understandability. Raft decomposes consensus into three clearly separated sub-problems:

### 1. Leader Election

Every node starts as a **follower**. If a follower receives no heartbeat from a leader within a timeout period, it becomes a **candidate** and requests votes from other nodes.

**Election rules:**
- Each node votes for at most one candidate per term (monotonically increasing ID)
- A candidate only wins if it has the most up-to-date log (prevents electing a node missing committed entries)
- Winning candidate becomes leader and starts sending heartbeats

**Split vote:** If no candidate wins majority, a new election term starts after a random timeout (prevents perpetual split votes).

```
Follower ──timeout──► Candidate ──majority votes──► Leader
   ▲                       │                          │
   └────────────────────────┘◄─────── heartbeat ──────┘
         discover leader
```

### 2. Log Replication

All writes go through the leader. The leader appends the entry to its log and sends `AppendEntries` RPCs to all followers.

**Commit protocol:**
1. Leader appends entry with current term to its log
2. Sends `AppendEntries` to all followers (contains log entry + preceding entry reference)
3. Waits for **majority** of nodes (including itself) to acknowledge
4. Once majority ACK → leader marks entry **committed**
5. Leader applies entry to state machine and responds to client
6. Next heartbeat/AppendEntries tells followers about the commit; they apply it too

### 3. Safety Guarantees

Raft ensures:
- **Election Safety:** At most one leader per term
- **Log Matching:** If two logs have identical entries at an index and term, all preceding entries are identical
- **Leader Completeness:** A leader has all committed entries from previous terms
- **State Machine Safety:** Once a node applies an entry at index i, no other node will apply a different entry at index i

### Cluster Membership Changes

Changing the cluster topology (adding/removing nodes) uses a two-phase **joint consensus** protocol to avoid split-brain during the transition.

### Real-World Implementations

| System | Use of Raft |
|---|---|
| **etcd** | All key-value operations; Kubernetes cluster state |
| **CockroachDB** | Per-range consensus for distributed SQL |
| **TiKV** | Distributed key-value store (TiDB's storage layer) |
| **Consul** | Cluster membership and KV store |
| **RethinkDB** | Table shards |
| **InfluxDB IOx** | Write-ahead log replication |

## Raft vs Paxos

| Aspect | Raft | Paxos |
|---|---|---|
| Understandability | Designed for it | Notoriously difficult |
| Safety guarantees | Equivalent | Equivalent |
| Leader | Always has all committed logs | Leader may need to catch up |
| Log structure | Explicit log as first-class concept | Log is implicit |
| Membership changes | Joint consensus | Complex multi-round protocol |
| Industrial adoption | etcd, CockroachDB, Consul | Chubby, Zookeeper (ZAB is Paxos-like) |

## Trade-offs

| Benefit | Cost |
|---|---|
| Understandable; fewer implementation bugs | Requires stable leader; leader is a bottleneck |
| Strong consistency guarantees | Quorum required; minority partition becomes unavailable for writes |
| Clean leader/follower mental model | Leader election causes brief unavailability |
| Handles arbitrary node failures | Not designed for Byzantine failures |

## Related

- [[etcd]] — canonical Raft implementation
- [[CAP Theorem]] — Raft systems are CP
- [[PACELC Theorem]] — Raft systems are PC/EC
- [[Distributed Transactions]] — Raft enables distributed consensus for transactions
- [[Eventual Consistency]] — contrast: Raft provides linearizability, not eventual consistency
