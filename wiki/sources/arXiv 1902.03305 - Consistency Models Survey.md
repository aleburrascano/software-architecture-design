---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/papers/1902.03305.md'
source_date: ''
source_author: (arXiv preprint, 2019)
tags:
  - source
  - consistency
  - distributed-systems
  - data-centric
  - client-centric
  - cap-theorem
  - eventual-consistency
  - linearizability
  - survey
---
# arXiv 1902.03305 - Consistency Models Survey

Comprehensive survey of consistency models in distributed systems covering definitions, taxonomy, challenges, and applications with mathematical formalization.

## Summary

This survey organizes the sprawling landscape of consistency models into two principal categories: data-centric models (which specify how all nodes observe operations) and client-centric models (which specify what guarantees individual clients receive). Data-centric models form a partial order from strongest (Linearizability) to weakest (Eventual Consistency), with Sequential Consistency, Causal Consistency, and FIFO Consistency occupying intermediate positions. Client-centric models include the four session guarantees: Monotonic Read, Monotonic Write, Read-Your-Writes, and Writes-Follow-Reads.

The survey identifies violation (returning incorrect values) and staleness (returning outdated values) as the two fundamental failure modes when consistency guarantees are relaxed. It provides mathematical definitions for each model, enabling precise comparison rather than relying on informal descriptions. The formalization is valuable for understanding why certain combinations of guarantees are achievable while others are not.

The three competing system properties — availability, scalability, and consistency — frame the trade-off analysis throughout. The survey explains why stronger consistency requires coordination overhead that limits scalability and availability, while weaker models enable better performance at the cost of application-level complexity. Hybrid models that combine data-centric and client-centric guarantees are also covered.

## Key Arguments

- Consistency models form a partial order from strongest (Linearizability) to weakest (Eventual Consistency)
- Data-centric vs. client-centric models address different consistency scopes and stakeholders
- Violation (wrong values) and staleness (old values) are the fundamental failure modes under weak consistency
- Availability, scalability, and consistency are three competing system properties that cannot all be maximized simultaneously
- Hybrid models can combine data-centric and client-centric guarantees for richer application contracts
- Mathematical formalization enables precise model comparison and reasoning

## Concepts Covered

- [[Consistency Models]] — comprehensive taxonomy; primary academic reference
- [[Eventual Consistency]] — formal definition and position in the consistency lattice
- [[Session Guarantees]] — client-centric models defined and formalized
- [[CAP Theorem]] — theoretical grounding for consistency-availability trade-off
- [[Vector Clock]] — causality enforcement mechanism for causal consistency

## Quality Notes

Most comprehensive academic survey on consistency models available. Essential reference for the Consistency Models concept page. The mathematical formalization distinguishes this from informal treatments.
