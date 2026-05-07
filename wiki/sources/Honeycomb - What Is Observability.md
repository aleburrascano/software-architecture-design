---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/What Is Observability- Key Components and Best Practices.md'
source_date: ''
source_author: Honeycomb
tags:
  - source
  - observability
  - high-cardinality
  - core-analysis-loop
  - sampling
  - slo
  - incident-response
  - honeycomb
---
# Honeycomb - What Is Observability

Honeycomb's comprehensive guide to observability, emphasizing high-cardinality data and the core analysis loop as the mental model for debugging unknown failures.

## Summary

Honeycomb defines observability through the lens of high-cardinality data and the ability to ask novel questions about system behavior without predefined dashboards. The core thesis is that traditional monitoring (dashboards, predefined metrics, threshold alerts) is insufficient for modern distributed systems because failures in complex systems are often unknown-unknowns — you don't know what to look for until after something breaks. Observability requires the ability to explore data interactively, drilling into specific user IDs, request IDs, deployment versions, or any high-cardinality dimension to identify the specific instances that exhibit failure.

The core analysis loop is the mental model: observe (something is wrong), hypothesize (form a theory about cause), explore (drill into high-cardinality data to test hypothesis), confirm (validate the hypothesis). This loop distinguishes observability tooling from monitoring tooling — monitoring tells you that error rates went up, observability tells you which 47 requests out of 10,000 failed and exactly what they had in common.

Sampling strategies are covered as a cost management mechanism: capturing every event at high cardinality is expensive, but naive sampling discards rare events that may be the most important. Head-based sampling (decision at trace ingestion) vs. tail-based sampling (decision after trace completes, based on outcome) is explained with the argument that tail-based sampling preserves rare failure traces while reducing cost.

## Key Arguments

- High-cardinality data (user IDs, request IDs, session IDs, feature flags) enables drilling into specific failure instances that aggregates would hide
- Low-cardinality aggregates (average latency, p99 error rate) provide visibility into the average case but hide the specific failing cases
- Observability is about asking novel questions without predefined dashboards — the system is debuggable even for failures never seen before
- The core analysis loop (observe → hypothesize → explore → confirm) is the mental model for debugging with observability tooling
- Tail-based sampling preserves rare failure traces while reducing cost; it is preferable to head-based sampling for observability use cases
- SLO-based alerting fires on user-facing outcomes, not technical metrics — reducing alert noise and false positives

## Concepts Covered

- [[Observability]] — high-cardinality treatment; the high-cardinality argument is Honeycomb's primary contribution
- [[Observability Implementation Guide]] — implementation phases and sampling strategies
- [[Distributed Tracing]] — traces as high-cardinality events; tail-based sampling for traces

## Quality Notes

Honeycomb invented modern observability tooling and the high-cardinality data argument. This is the authoritative perspective on why high-cardinality data is essential. The core analysis loop framing is particularly useful. Vendor perspective but technically credible — Honeycomb's founders (Charity Majors et al.) are respected practitioners.
