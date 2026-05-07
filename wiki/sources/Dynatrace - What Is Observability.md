---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/What Is Observability- Not Just Logs, Metrics and Traces.md'
source_date: ''
source_author: Dynatrace
tags:
  - source
  - observability
  - monitoring
  - logs
  - metrics
  - traces
  - aiops
  - slo
  - unknown-unknowns
  - dynatrace
---
# Dynatrace - What Is Observability

Dynatrace's treatment of observability going beyond the three pillars — addressing unknown unknowns, AIOps integration, and user experience as observability dimensions.

## Summary

The article defines observability as the ability to measure the internal state of a system from its external outputs — a framing derived from control theory. The key distinction from monitoring is that monitoring answers "is something wrong?" (known knowns: predefined metrics and thresholds), while observability answers "why is something wrong?" (unknown unknowns: ad hoc exploration of data without predefined queries). This distinction matters because modern distributed systems fail in ways that weren't anticipated when dashboards were built.

While logs, metrics, and traces are necessary pillars of observability, Dynatrace argues they are not sufficient. AIOps integration adds automated anomaly detection that can surface unknown-unknown failures without requiring engineers to manually explore all possible failure dimensions. SLO-based observability shifts alerting from technical metrics (CPU usage, error rates) to user-facing service level objectives — aligning operations with business outcomes. User experience is treated as a first-class observability dimension: if the system is technically healthy but users are experiencing slowness, observability has failed.

The "single source of truth" goal is the aspirational state: all telemetry unified into a coherent model that allows any question about system behavior to be answered without context-switching between tools.

## Key Arguments

- Monitoring answers "is something wrong?"; observability answers "why is something wrong?" — the difference is unknown unknowns
- The three pillars (logs, metrics, traces) are necessary but not sufficient for true observability
- AIOps automates anomaly detection, enabling discovery of unknown-unknown failures without predefined queries
- SLO-based observability aligns operations with user-facing outcomes rather than technical metrics
- User experience must be included as an observability dimension — technical health ≠ user experience health
- Observability enables proactive operations (catch problems before users report them) vs. reactive operations (respond to incidents)

## Concepts Covered

- [[Observability]] — comprehensive treatment; monitoring vs. observability distinction is a key contribution
- [[Distributed Tracing]] — traces as the third pillar; context propagation across services
- [[Quality Attributes]] — reliability and performance as quality attributes that observability supports
- [[Self-Healing Systems]] — AIOps connection; automated detection as a step toward self-healing

## Quality Notes

Vendor perspective (Dynatrace) but reasonably balanced. Best used for the monitoring vs. observability conceptual distinction and the argument that three pillars are necessary but not sufficient. Cross-reference with Honeycomb's treatment for the high-cardinality angle.
