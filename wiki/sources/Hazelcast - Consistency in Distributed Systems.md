---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Navigating Consistency in Distributed Systems- Choosing the Right Trade-Offs.md'
source_date: ''
source_author: Ed Thurman / Hazelcast
tags:
  - source
  - consistency
  - pacelc
  - cap-theorem
  - session-guarantees
  - read-your-writes
  - monotonic-reads
  - hazelcast
---
# Hazelcast - Consistency in Distributed Systems

Hazelcast article extending CAP theorem with PACELC and practical consistency model selection, including client-centric session guarantees.

## Summary

The article starts from a well-known limitation of CAP theorem: it only addresses behavior under network partitions, but production systems spend the vast majority of their time in the normal (non-partitioned) operating mode. PACELC extends CAP to cover this: if there is a Partition, the system chooses between Availability and Consistency (the CA/CP split); Else (during normal operation), the system chooses between Latency and Consistency. This addition makes PACELC a more complete framework for reasoning about distributed system trade-offs.

The consistency model hierarchy is covered in full: linearizability (the strongest — reads reflect all prior writes, all operations appear instantaneous) → sequential consistency (operations appear in a global order consistent with program order) → causal consistency (causally related operations are seen in order) → eventual consistency (all replicas will eventually converge, but with no timing guarantee). Each step down the hierarchy reduces cost and increases availability at the expense of guarantees.

Session guarantees provide a practical middle ground: client-centric consistency that is stronger than eventual but weaker than global strong consistency. The four guarantees are: Read-Your-Writes (a client always sees its own writes), Monotonic Reads (once a client reads a value, it never reads older values), Writes-Follow-Reads (a write that follows a read is ordered after the read's value), and Monotonic Writes (a client's writes are applied in order). The article includes a decision framework table matching application access patterns to appropriate consistency models.

## Key Arguments

- CAP only addresses partition scenarios; PACELC adds the normal-operation latency-consistency trade-off making it a more complete framework
- Consistency models form a hierarchy: linearizability → sequential → causal → eventual, each with different cost and guarantee profiles
- Session guarantees (RYW, Monotonic Reads, WFR, Monotonic Writes) provide client-level consistency without requiring global strong consistency
- Choosing the right model requires analyzing the application's access patterns: who reads and writes what, with what timing expectations
- Most applications do not need linearizability; causal or session-guaranteed consistency is sufficient for most user-facing features

## Concepts Covered

- [[PACELC Theorem]] — primary treatment; definition and contrast with CAP
- [[CAP Theorem]] — extended and contextualized by PACELC
- [[Consistency Models]] — full hierarchy from linearizability to eventual consistency
- [[Session Guarantees]] — Read-Your-Writes, Monotonic Reads, Writes-Follow-Reads, Monotonic Writes
- [[Eventual Consistency]] — weakest model in the hierarchy; definition and trade-offs

## Quality Notes

One of the best accessible treatments of PACELC and session guarantees available online. The decision framework table is practically useful. Vendor perspective (Hazelcast) but technically sound and well-referenced. Recommended as the primary reference for PACELC.
