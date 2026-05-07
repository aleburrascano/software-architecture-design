---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Principles of Mechanical Sympathy.md'
source_date: ''
source_author: Martin Fowler / Caer Sanders
tags:
  - source
  - mechanical-sympathy
  - performance
  - hardware
  - cache-lines
  - memory-access
  - low-latency
---
# Mechanical Sympathy

Article on designing software that works with rather than against underlying hardware — the principle that understanding your machine makes you a better driver of it.

## Summary

The term "mechanical sympathy" comes from racing driver Jackie Stewart, who believed that a driver who understood the mechanics of a car would drive it better. Applied to software, it means designing systems that cooperate with the hardware they run on rather than treating hardware as an opaque abstraction layer. The article covers how CPU cache hierarchy, memory access patterns, and branch prediction behavior critically affect real-world performance.

The canonical implementation example is the LMAX Disruptor, a high-performance inter-thread messaging library that achieves exceptional throughput by co-designing with hardware: pre-allocating ring buffers for cache locality, using the single-writer principle to eliminate lock contention, and relying on natural batching to reduce CPU stalls. False sharing — where independent data on the same cache line causes unnecessary cache invalidation across cores — is highlighted as a common multi-core performance pitfall that architectural awareness can eliminate.

The article argues that sequential memory access is orders of magnitude faster than random access due to cache line prefetching, and that this simple fact should drive data structure and access pattern choices at the architectural level, not just the implementation level.

## Key Arguments

- Hardware cache hierarchy critically affects performance; ignoring it means leaving significant throughput on the table
- Sequential memory access is orders of magnitude faster than random access due to CPU prefetching behavior
- False sharing degrades multi-core performance: two threads writing to different variables on the same cache line cause constant cache invalidation
- The single-writer principle eliminates contention without locks, enabling high-throughput inter-thread communication
- Natural batching reduces CPU stalls by amortizing the cost of expensive operations across multiple work items
- LMAX Disruptor is the canonical proof-of-concept: mechanical sympathy applied to achieve 25M+ messages/second on commodity hardware

## Concepts Covered

- [[Mechanical Sympathy]] — primary treatment; the article defines and applies this principle
- [[Quality Attributes]] — performance as a first-class quality attribute requiring architectural consideration
- [[Reactive Architecture]] — hardware efficiency is a prerequisite for low-latency reactive systems

## Quality Notes

Practical performance optimization principle often overlooked by high-level architects. Particularly relevant when designing low-latency systems, high-throughput messaging infrastructure, or any system where performance is a hard requirement rather than a soft preference.
