---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'etcd Distributed Key-Value Store.md'
  - 'Data Consistency Models in Distributed Systems.md'
tags:
  - etcd
  - distributed-systems
  - key-value-store
  - consensus
  - raft
  - kubernetes
  - service-discovery
  - cncf
  - cp-system
  - distributed-locks
---
# etcd

A **distributed reliable key-value store** providing strong consistency via the Raft consensus algorithm. CNCF graduated project; serves as Kubernetes' primary cluster state backing store and is the canonical example of a CP distributed system.

## Problem

Distributed systems need a reliable, consistent store for critical coordination data: cluster membership, configuration, leader election, and service discovery. This data must be read by every node in the cluster, must be consistent (no split-brain), and must persist across failures. A simple replicated database is insufficient because it lacks consensus guarantees.

## Solution / Explanation

etcd provides a simple key-value API backed by the Raft consensus algorithm, guaranteeing that every read returns the most recently committed write.

### Core API Operations

| Operation | Description |
|---|---|
| `Get(key)` | Read value; always returns latest committed value |
| `Put(key, value)` | Write value; goes through Raft leader |
| `Delete(key)` | Remove key; goes through Raft leader |
| `Watch(key)` | Stream of change events; reactive service discovery |
| `Grant(TTL)` | Create a lease with TTL expiry (for distributed locks) |
| `Txn(conditions, success, failure)` | Atomic compare-and-swap transactions |

### Key Features

**Watch Operations**
Clients subscribe to changes on a key or key prefix and receive a stream of events. Used by Kubernetes controllers to react to cluster state changes (new pods, service updates, config changes) without polling.

**Leases**
A lease is a TTL-based handle attached to keys. When the lease expires, all attached keys are automatically deleted. Used for distributed locking: node acquires lock by writing to a lease-backed key; if node dies, the lease expires and the key disappears, releasing the lock automatically.

**Transactions (STM)**
Compare-and-swap across multiple keys atomically. Enables optimistic concurrency control: read version → modify → write only if version unchanged.

### Consistency Guarantee

etcd is **CP** in CAP terms:
- **Every read is linearizable** — guaranteed to return the most recent committed value
- **Writes require Raft quorum** — a 3-node cluster requires 2/3 to acknowledge before committing
- **During partition:** writes block until quorum is restored; minority partition rejects writes

In PACELC terms, etcd is **PC/EC** — consistency is prioritized both during partitions and during normal operation (reads may be slightly slower because Raft must confirm).

### Architecture

```
Client
  │
etcd gRPC API
  │
┌─────────────────────────────────┐
│  Raft State Machine             │
│  Leader ──► Follower ──► Follower │
│     │                           │
│  Write Log (append-only WAL)    │
└─────────────────────────────────┘
```

1. Client sends write to any node
2. Non-leader nodes forward to leader
3. Leader appends to Raft log
4. Leader sends AppendEntries to followers
5. After quorum ACK, leader commits
6. Leader responds to client

### Primary Use Cases

| Use Case | How etcd Is Used |
|---|---|
| **Kubernetes** | All cluster state: pods, services, deployments, namespaces |
| **Service discovery** | Services register; clients watch for changes |
| **Distributed locking** | Lease-backed keys provide auto-releasing locks |
| **Leader election** | Campaign on a key; lease ensures leader health |
| **Feature flags** | Consistent configuration updates across all nodes |
| **Configuration management** | Central config store with watch notifications |

## Trade-offs

| Benefit | Cost |
|---|---|
| Strong consistency — no stale reads | Writes require quorum (latency) |
| Automatic leader election via Raft | Cluster unavailable during partition until quorum restored |
| Watch operations enable reactive patterns | Increased latency vs. AP systems |
| Lease-based locking is safe | Not suitable for high-throughput writes |
| CNCF backed; battle-tested at Kubernetes scale | Complex to operate at very large scale |

## Related

- [[Raft Consensus Algorithm]] — the consensus mechanism powering etcd
- [[CAP Theorem]] — etcd is a canonical CP example
- [[PACELC Theorem]] — etcd is PC/EC
- [[Distributed Transactions]] — etcd transactions as lightweight alternative
- [[Service Mesh]] — etcd as service discovery backend
