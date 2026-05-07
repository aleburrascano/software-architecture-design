---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Consistency Model.md'
source_date: ''
source_author: Wikipedia
tags:
  - source
  - consistency-models
  - sequential-consistency
  - causal-consistency
  - release-consistency
  - memory-ordering
  - pram
  - formal-definition
---
# Wikipedia - Consistency Model

Wikipedia's formal treatment of consistency models as contracts between distributed system implementations and clients — from strict consistency through eventual.

## Summary

A consistency model is a contract between a distributed storage system and clients that use it. The contract defines what values reads may return given the history of writes that have occurred. Wikipedia's treatment covers the full spectrum of consistency models with their formal definitions, forming a lattice from strictest (strict consistency / linearizability) to weakest (eventual consistency).

The article covers the distinction between data-centric consistency models (properties of the storage system itself) and client-centric consistency models (properties from the perspective of a single client). Sequential consistency (Lamport's definition) requires that all operations appear to execute in some total order consistent with each process's program order — a weaker but more achievable guarantee than linearizability, which additionally requires operations to appear instantaneous. Causal consistency tracks the "happens-before" relation; FIFO/PRAM (Pipeline RAM) consistency only requires that writes from a single process are seen in order by all others.

Hardware memory consistency models add further complexity. CPUs do not execute memory operations in program order; cache coherence protocols (MESI, MOESI) manage consistency between cores, but store buffers and reorder buffers mean that without fence instructions, memory operations can be observed out of order. Release consistency uses acquire/release semantics to selectively enforce ordering at synchronization points — the foundation of modern lock-free programming and the C++ memory model.

## Key Arguments

- A consistency model defines what values reads return given a set of writes; it is the contract the system makes with clients
- Models form a lattice from strictest (linearizability) to weakest (eventual consistency), with each step relaxing guarantees to reduce coordination cost
- Linearizability adds real-time ordering to sequential consistency: operations appear to execute at a single instant
- PRAM/FIFO consistency only requires write order preservation from a single process — very weak but achievable with minimal coordination
- Hardware memory models (x86 TSO, ARM) are weaker than sequential consistency; fence instructions enforce ordering at synchronization points
- Release consistency uses acquire/release semantics to selectively enforce ordering, enabling efficient lock-free data structures

## Concepts Covered

- [[Consistency Models]] — formal mathematical treatment; the definitive theoretical reference
- [[Eventual Consistency]] — formal definition; weakest model in the lattice
- [[CAP Theorem]] — consistency as the C in CAP; formal grounding
- [[Vector Clock]] — causality tracking; supports causal consistency implementation

## Quality Notes

Best source for formal and mathematical grounding. More theoretical than practical — most engineers will want to read this after building intuition from practical sources like the Hazelcast article. Valuable for understanding the precise definitions and the hardware memory model implications.
