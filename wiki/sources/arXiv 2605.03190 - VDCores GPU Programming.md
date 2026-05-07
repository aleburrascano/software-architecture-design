---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/papers/2605.03190.md'
source_date: ''
source_author: Zijian He, Adrian Sampson, Yiying Zhang, Zhiyuan Guo
tags:
  - source
  - gpu
  - parallel-computing
  - async-execution
  - hardware
  - performance
---
# arXiv 2605.03190 - VDCores GPU Programming

Proposes VDCores (Virtual Decoupled Engines), a new GPU programming model that decouples resource orchestration from programmer responsibility.

## Summary

Modern GPUs contain specialized asynchronous hardware units (memory copy engines, tensor cores, compute engines) designed to overlap operations and improve utilization. However, existing GPU programming models require programmers to manually orchestrate these units within monolithic kernels, leading to significant underutilization in practice. VDCores addresses this by abstracting hardware units as virtual engines with dependency-connected micro-operations.

The model automatically tracks dependencies between micro-operations across virtual engines and overlaps memory and compute operations without programmer intervention. This shifts the orchestration burden from the programmer to the runtime, enabling the hardware's asynchronous capabilities to be utilized more consistently across diverse workloads.

The paper demonstrates that static orchestration strategies in conventional monolithic kernels fail to generalize across workload shapes, whereas VDCores' runtime-driven approach maintains high utilization without requiring workload-specific tuning.

## Key Arguments

- Modern GPUs have specialized asynchronous engines that are systematically underutilized by current programming models
- Static orchestration in monolithic kernels cannot generalize across diverse workload shapes
- VDCores abstracts hardware units as virtual engines with dependency-connected micro-operations
- Automatic dependency tracking enables overlap of memory and compute operations without programmer intervention
- Runtime-driven orchestration outperforms static approaches across varied workloads

## Concepts Covered

- [[Reactive Architecture]] — async execution patterns and non-blocking operation overlap
- [[Mechanical Sympathy]] — hardware-aware programming and exploiting hardware capabilities

## Quality Notes

GPU-specific systems research; low direct relevance to software architecture patterns at the application or service level. Included for completeness as a filed paper. Most tangential of the paper sources in this vault.
