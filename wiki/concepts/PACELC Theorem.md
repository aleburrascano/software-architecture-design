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
  - availability
  - latency
  - cap-theorem
  - pacelc
---
# PACELC Theorem

An extension of the CAP Theorem that adds the latency-consistency trade-off that occurs during **normal operation** (no partition), addressing CAP's limitation of only describing behavior during network partitions.

## Problem

The CAP Theorem only describes what happens when a network partition occurs. But distributed systems spend the **vast majority of their time in normal operation** — no partition, everything connected. Even then, a fundamental trade-off exists: a system can respond faster with potentially stale data, or respond slower with guaranteed-current data. CAP is silent on this.

## Solution / Explanation

PACELC was formalized by Daniel Abadi (2012). The theorem states:

> **If** there is a **P**artition, choose between **A**vailability and **C**onsistency;  
> **E**lse (normal operation), choose between **L**atency and **C**onsistency.

This gives four quadrants for real system classification:

| Classification | Partition behavior | Normal behavior | Examples |
|---|---|---|---|
| **PA/EL** | Prioritize Availability | Prioritize Latency | Cassandra, DynamoDB (default), Riak |
| **PC/EC** | Prioritize Consistency | Prioritize Consistency | etcd, Zookeeper, HBase, MySQL |
| **PA/EC** | Prioritize Availability | Prioritize Consistency | MongoDB (tunable), CockroachDB (some modes) |
| **PC/EL** | Prioritize Consistency | Prioritize Latency | Rare; specialized systems |

### Why This Matters More Than CAP

For most production systems, network partitions are **rare**. The more pressing daily trade-off is: should reads return immediately with potentially cached/stale data (low latency) or should they wait for linearizable confirmation (consistent but slower)?

- **PA/EL systems** (Cassandra, DynamoDB): serve reads from local replica, don't wait for quorum → fast, possibly stale
- **PC/EC systems** (etcd, Zookeeper): wait for quorum on reads and writes → slower, always current

### Real-World Examples

| System | CAP | PACELC | Why |
|---|---|---|---|
| Cassandra | AP | PA/EL | Local replica reads, eventual consistency |
| DynamoDB | AP (default) | PA/EL | Reads from single replica by default |
| etcd | CP | PC/EC | Raft quorum for all operations |
| Zookeeper | CP | PC/EC | ZAB protocol requires quorum |
| MySQL | CA* | PC/EC | Single-leader; wait for durable write |
| CockroachDB | CP | PC/EC | Raft-based; serializable isolation |

## Trade-offs

| PACELC Choice | Benefit | Cost |
|---|---|---|
| PA/EL | Low latency reads; high availability | Stale reads possible; conflict resolution needed |
| PC/EC | Always current data; no conflicts | Higher latency; lower availability during partitions |

## When to Use PACELC as a Decision Framework

- Use PACELC over CAP when choosing databases for latency-sensitive applications
- Financial systems where stale reads cause incorrect balances → PC/EC
- User-facing read-heavy systems where 50ms matters → PA/EL
- Configuration stores and coordination services → PC/EC (correctness > speed)

## Related

- [[CAP Theorem]] — PACELC extends CAP
- [[Eventual Consistency]] — the consistency model of PA/EL systems
- [[etcd]] — canonical PC/EC example
- [[Consistency Models]] — full consistency model spectrum
- [[Distributed Transactions]] — motivation for choosing PC/EC
