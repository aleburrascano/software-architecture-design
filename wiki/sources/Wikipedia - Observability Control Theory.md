---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Observability.md'
source_date: ''
source_author: Wikipedia
tags:
  - source
  - observability
  - control-theory
  - kalman-filter
  - state-observer
  - mathematical-foundations
---
# Wikipedia - Observability Control Theory

Wikipedia's treatment of observability from its formal control theory roots, providing the mathematical foundation that inspired the term's use in distributed systems.

## Summary

Observability in control theory, formalized by Rudolf Kalman in 1960, measures how well the internal states of a dynamical system can be inferred from its external outputs over time. A system is fully observable if, given any initial state, the state can be uniquely determined from observations of the system's outputs over a finite time interval. This is captured formally through the observability matrix: if the matrix has full column rank, the system is observable.

The article covers linear and nonlinear systems separately. For linear time-invariant systems, observability is determined by the rank of the observability matrix constructed from the system's state matrix (A) and output matrix (C). For nonlinear systems, local observability is defined via the Lie derivative rank condition — a more complex criterion that reduces to the linear case for linearized systems.

State observers (also called state estimators) are the practical application: given a system model and output measurements, an observer estimates the internal state in real time. The Kalman filter is the canonical optimal state observer for linear systems with Gaussian noise. The connection to software observability is direct: if a distributed system is "observable" in the Kalman sense, then logs, metrics, and traces provide sufficient outputs to infer internal state — system behavior is not opaque.

## Key Arguments

- Observability is formally defined as the ability to infer complete internal state from external observations over time
- An observable system has a full-rank observability matrix; this is the mathematical criterion
- Kalman filters are the canonical state observers: they estimate internal state from noisy external measurements
- The software observability concept directly borrows this framing — can we infer system state from logs/metrics/traces?
- Nonlinear observability uses Lie derivative rank conditions, generalizing the linear matrix rank criterion

## Concepts Covered

- [[Observability]] — theoretical foundation; the formal control theory definition that grounds the software concept
- [[Quality Attributes]] — theoretical grounding for observability as a quality attribute with mathematical roots

## Quality Notes

Useful for the mathematical and theoretical background. Grounds software observability in formal control theory, which is valuable for understanding why the term was chosen and what it precisely means. More theoretical than practical — pair with Dynatrace, Honeycomb, or Baeldung sources for applied guidance.
