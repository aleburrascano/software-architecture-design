---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/papers/2605.03640.md'
source_date: ''
source_author: Achilleas Michalopoulos, Dimitrios Tsitsigkos, Nikos Mamoulis
tags:
  - source
  - indexing
  - data-structures
  - in-memory
  - kd-tree
  - range-queries
  - multidimensional
---
# arXiv 2605.03640 - skd-tree In-memory Indexing

Proposes the slicing kd-tree (skd-tree) for efficient in-memory multidimensional indexing, optimized for range and nearest-neighbor queries.

## Summary

Traditional kd-trees and R-trees incur significant memory overhead for multidimensional indexing, limiting their effectiveness for in-memory workloads at scale. The skd-tree addresses this by partitioning space along a single dimension per level while using compressed splitters to reduce per-node memory footprint. This design enables better cache utilization and data parallelism during query execution.

The paper benchmarks skd-tree against R-trees, classic kd-trees, quad-trees, and learned index approaches across range query and nearest-neighbor workloads. The skd-tree outperforms competing approaches for in-memory multidimensional query workloads, particularly for dynamic datasets where learned index approaches require expensive retraining.

Single-dimension partitioning simplifies the tree structure in a way that enables vectorized query execution, making the structure well-suited to modern CPU architectures with wide SIMD units. The compressed splitter design reduces memory bandwidth requirements, a key bottleneck for in-memory analytics.

## Key Arguments

- Traditional kd-trees have high memory overhead that limits in-memory scalability
- skd-tree uses compressed splitters to reduce per-node memory footprint
- Single-dimension partitioning per level enables data parallelism and cache-friendly access
- Outperforms learned index approaches for dynamic workloads requiring frequent updates
- Applicable to spatial databases, in-memory analytics, and multidimensional query engines

## Concepts Covered

- [[Quality Attributes]] — performance optimization as a measurable architectural concern
- [[Mechanical Sympathy]] — in-memory data structure design exploiting CPU cache and SIMD

## Quality Notes

Low-level data structures research with minimal direct relevance to architectural patterns or distributed systems. Included for completeness.
