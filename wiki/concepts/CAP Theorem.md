---
type: concept
created: '2026-05-03'
updated: '2026-05-06'
sources:
  - 'https://awesome-architecture.com/architectural-design-principles/cap/'
tags:
  - distributed-systems
  - databases
  - consistency
  - availability
---
# CAP Theorem

A theoretical result stating that a distributed data store can provide at most two of three guarantees simultaneously: **Consistency**, **Availability**, and **Partition Tolerance** — and that because network partitions are inevitable in distributed systems, real systems must choose between consistency and availability when a partition occurs.

## Problem

When designing distributed databases and data-intensive microservices, architects face fundamental trade-offs in what guarantees a system can make. The CAP theorem (proved by Eric Brewer in 2000, formally proved by Gilbert and Lynch in 2002) provides a framework for reasoning about these trade-offs.

## Solution / Explanation

### The Three Properties

**Consistency (C)**
Every read receives the most recent write or an error. All nodes see the same data at the same time. This is linearizability — the system behaves as if it has a single, authoritative copy of the data.

**Availability (A)**
Every request receives a non-error response, but without the guarantee that it contains the most recent write. The system remains operational even when some nodes are failing.

**Partition Tolerance (P)**
The system continues to operate despite network partitions — situations where messages between some nodes are lost or delayed indefinitely.

### The Fundamental Trade-off

In a distributed system, **network partitions will eventually occur** (hardware failures, network misconfiguration, data center issues). Given that P cannot be sacrificed, the real choice is:

- **CP (Consistency + Partition Tolerance)**: Refuse to answer (return an error or wait) when a partition might cause inconsistent data. Examples: HBase, Zookeeper, etcd, MongoDB (default configuration).
- **AP (Availability + Partition Tolerance)**: Return the best available answer even if it might be stale. Examples: CouchDB, Cassandra, DynamoDB, Riak.

A "CA" (Consistency + Availability) system is one that cannot tolerate partitions — only possible in a single-node system or a perfectly reliable network (i.e., not a real distributed system).

### Nuances and Criticisms

The CAP theorem is binary and simplified. In practice:
- Systems offer **spectrum** choices rather than hard binary decisions.
- **PACELC** (an extension) adds: even without a partition, there is a trade-off between latency and consistency.
- "Consistency" in CAP is strict linearizability — weaker consistency models (eventual consistency, read-your-writes) allow more nuanced guarantees.

## System Categories

| Category | Properties | Examples | Use Cases |
|---|---|---|---|
| CP | Consistent, partition-tolerant | Zookeeper, etcd, HBase | Leader election, configuration stores |
| AP | Available, partition-tolerant | Cassandra, DynamoDB, CouchDB | Shopping carts, DNS, social feeds |
| CA | Consistent, available (no partition tolerance) | Traditional RDBMS (single node) | Not truly distributed |

## When to Use as a Decision Framework

Use CAP to:
- Select the right database for a microservice's consistency requirements.
- Explain to stakeholders why certain guarantees are mutually exclusive.
- Design compensating mechanisms (e.g., conflict resolution) when choosing AP.
- Understand why distributed transactions are so costly (they enforce CP).

## Trade-offs

| CP Systems | AP Systems |
|---|---|
| Data is always correct | Data may be temporarily stale |
| System may become unavailable during partitions | System stays up during partitions |
| Suitable for financial transactions, inventory | Suitable for user sessions, caches, DNS |

## Beyond CAP: PACELC

CAP only describes behavior during a **partition**. In practice, network partitions are rare; systems spend most time in normal operation. **PACELC** extends the model:

> "If there is a **P**artition, choose between **A**vailability and **C**onsistency; **E**lse (normal operation), choose between **L**atency and **C**onsistency."

| System | Partition | Normal Ops |
|--------|-----------|------------|
| Cassandra | PA (favors availability) | EL (low latency, stale reads) |
| DynamoDB | PA | EL (configurable) |
| etcd / Zookeeper | PC (refuses writes) | EC (consistent reads at latency cost) |

For most systems the EL vs EC trade-off during normal operation is more relevant to perceived performance than the rare partition scenario. See [[PACELC Theorem]] for the full framework.

## CP Deep-Dive: etcd and Raft

**etcd** is the canonical CP distributed key-value store, backing Kubernetes cluster state. Its CP guarantee is implemented via **Raft** consensus:

- All writes go to the elected leader.
- The leader replicates to a quorum (majority) of followers before acknowledging.
- During a partition, any partition lacking a quorum cannot elect a leader and **blocks writes** until quorum is restored — choosing C over A.
- Reads can optionally be served from followers (linearizable reads add a round-trip to the leader).

See [[etcd]] and [[Raft Consensus Algorithm]] for implementation details.

## Related

- [[Eventual Consistency]] — the consistency model embraced by AP systems
- [[PACELC Theorem]] — extends CAP to cover the latency-consistency trade-off during normal operation
- [[Saga Pattern]] — avoids distributed transactions; accepts eventual consistency
- [[Distributed Transactions]] — the strong-consistency alternative to sagas
- [[Microservices Architecture]] — each service's database choice involves CAP trade-offs
- [[BASE vs ACID]] — complementary model to CAP for database guarantees
- [[etcd]] — CP key-value store; Raft-based quorum writes
- [[Raft Consensus Algorithm]] — the consensus mechanism enabling CP systems like etcd and CockroachDB
