---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Hazelcast - Consistency in Distributed Systems.md'
  - 'arXiv 1902.03305 - Consistency Models Survey.md'
tags:
  - distributed-systems
  - consistency
  - session-guarantees
  - read-your-writes
  - monotonic-reads
  - client-centric
---
# Session Guarantees

**Session Guarantees** are four client-centric consistency properties that ensure a single client's session has a coherent view of the data, even in a distributed system that uses eventual consistency globally.

## Problem

Global consistency (linearizability) is expensive — it requires coordination across all replicas. But users have a minimum expectation: I should be able to read what I just wrote. I shouldn't see a value disappear after I've seen it. Eventual consistency alone violates both of these. Session guarantees provide a practical middle ground.

## The Four Guarantees

### 1. Read-Your-Writes (RYW)

> A client always sees the effect of its own previous writes.

If Alice writes `username = "alice"`, her next read of that key must return `"alice"` — even if other clients still see the old value. This is the most fundamental session guarantee.

**Implementation:** Server tracks a write token per client session; reads are routed to a replica with at least that version.

### 2. Monotonic Reads

> Once a client reads a value `v`, all subsequent reads return `v` or a newer value. Reads never go backward.

If Alice reads `counter = 5`, she will never subsequently read `counter = 3`. Each read is at least as recent as the last.

**Implementation:** Client tracks a read timestamp; subsequent reads only served from replicas with at least that timestamp. Often implemented via sticky sessions to the same replica.

### 3. Monotonic Writes

> A client's writes are applied in the order they were issued.

If Alice writes `A` then writes `B`, all replicas that see `B` must have previously applied `A`. No write can "skip ahead" of an earlier write from the same client.

**Implementation:** Per-client write sequence numbers; replicas apply writes in sequence order.

### 4. Writes-Follow-Reads (Session Causality)

> A write that follows a read will be applied after all the values that read returned.

If Alice reads the current shopping cart (sees items X, Y), then adds item Z, the write for Z is causally dependent on the state Alice read. All replicas must see X and Y before they see Z added.

**Implementation:** Client piggybacks the version of the last-read state onto the write; replicas apply the write only after reaching that version.

## Combined Guarantees

All four together provide **session consistency** — a client's session has a coherent, monotonically progressing view of the system, even without global linearizability. This is the "shopping cart" level of consistency: you see your own changes, and things don't go backward.

## Trade-offs

| Benefit | Cost |
|---|---|
| User-facing correctness ("my change stuck") | Requires session state tracking |
| Lower coordination cost than linearizability | Sticky sessions limit load distribution |
| Works well for user-facing applications | Cross-session consistency still not guaranteed |
| Per-session performance is predictable | Version vectors add bookkeeping overhead |

## When to Use

- **E-commerce, social media, content platforms:** users must see their own actions (RYW critical)
- **Collaborative editing:** monotonic reads prevent confusing regressions
- **Order management:** writes-follow-reads ensures causal correctness
- **Not needed:** background batch jobs that don't need user-visible consistency

## Related

- [[Consistency Models]] — session guarantees are client-centric models
- [[Eventual Consistency]] — global model these guarantees augment
- [[CAP Theorem]] — session guarantees work within AP systems
- [[PACELC Theorem]] — PA/EL systems can still offer session guarantees
