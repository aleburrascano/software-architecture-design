---
type: source
created: '2026-05-03'
updated: '2026-05-03'
source_path: 'https://martinfowler.com/bliki/CircuitBreaker.html'
source_date: ''
source_author: Martin Fowler
tags:
  - source
  - resilience
  - patterns
---
# Martin Fowler - Circuit Breaker

Martin Fowler's canonical bliki entry defining the Circuit Breaker pattern for distributed systems fault tolerance.

## Summary

Defines the Circuit Breaker pattern as wrapping a remote call in a proxy object that monitors failures and trips to an open state when a threshold is exceeded, preventing cascading failures. Explains the three states (Closed, Open, Half-Open) and the transition logic between them.

## Key Arguments

- Remote calls can fail or hang indefinitely, cascading resource exhaustion across the system.
- The circuit breaker proxy monitors failures against a configurable threshold.
- Three states — Closed (normal), Open (rejecting calls), Half-Open (probing) — manage the failure detection and recovery lifecycle.
- State transitions should be logged and observable; operations staff should be able to manually trip or reset breakers.
- Clients must implement fallback strategies (use cached data, queue the request, return a default) when the breaker is open.
- Different types of failures (connection refused vs. timeout) may warrant different thresholds.

## Concepts Covered

- [[Circuit Breaker Pattern]] — primary concept; comprehensive treatment
- [[Resiliency Patterns]] — circuit breaker as a member of the resilience family
- [[Observability]] — logging and monitoring state transitions

## Quality Notes

Foundational reference; concise and authoritative. Fowler popularized this pattern from Michael Nygard's *Release It!*. Still the go-to introduction to the concept.
