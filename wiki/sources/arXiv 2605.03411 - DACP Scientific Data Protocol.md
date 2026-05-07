---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/papers/2605.03411.md'
source_date: ''
source_author: Zhihong Shen, Xiaojie Zhu, Zhenjing Cheng, Hao Ren, Zhaoji Liang, Changfa Lu
tags:
  - source
  - data-protocol
  - scientific-computing
  - streaming
  - collaboration
  - ai4science
  - protocol-design
---
# arXiv 2605.03411 - DACP Scientific Data Protocol

Proposes DACP (Data Access and Collaboration Protocol), a network protocol for scientific computing that addresses data silos and cross-domain collaboration.

## Summary

Scientific research increasingly depends on large distributed datasets hosted across independent data centers, but the lack of unified protocols creates data silos that impede AI4Science collaboration. DACP addresses this by introducing Streaming Data Frames (SDF) as a core data model with unified resource identification, enabling cross-domain data access without requiring data migration.

The protocol supports in-situ computation, where analytics and AI workloads execute near the data rather than copying large datasets over the network. A reverse supply mechanism enables result streaming back to requesting clients, making the protocol efficient for the read-heavy, large-volume access patterns typical of scientific workloads.

DACP positions itself between general-purpose protocols like HTTP (too generic, not optimized for scientific data shapes) and domain-specific systems (too narrow for cross-domain collaboration). The columnar stream framing aligns with scientific data's tendency to be accessed in bulk column-oriented queries rather than record-by-record.

## Key Arguments

- Scientific data silos impede AI4Science collaboration and require unified protocol infrastructure
- Streaming Data Frames (SDF) provide a columnar data model suited to scientific workload access patterns
- Unified resource identification enables cross-domain data access without data migration
- In-situ computation reduces network transfer costs for large scientific datasets
- DACP fills the gap between overly generic protocols (HTTP) and overly narrow domain systems

## Concepts Covered

- [[REST]] — protocol design contrast; DACP designed where REST falls short for scientific workloads
- [[gRPC]] — streaming protocol comparison; DACP provides similar streaming but with scientific data model
- [[Microservices Architecture]] — data collaboration patterns across distributed services

## Quality Notes

Domain-specific to scientific computing and AI4Science. Low direct relevance to general software architecture. Included for completeness as a filed paper.
