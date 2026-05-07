---
type: concept
created: '2026-05-03'
updated: '2026-05-06'
sources:
  - 'https://awesome-architecture.com/microservices/observability/observability/'
tags:
  - operations
  - microservices
  - distributed-systems
  - monitoring
---
# Observability

The ability to understand the internal state of a distributed system from its external outputs (logs, metrics, and traces), enabling engineers to ask arbitrary questions about system behavior and answer them without deploying new code.

## Problem

In distributed systems, failures are complex and emergent. Traditional monitoring ("Is this metric above the threshold?") breaks down when:

- The failure mode was unknown in advance — there is no pre-defined alert for it.
- Symptoms manifest in one service but the root cause is in another.
- Cascading failures make the primary signal noise.

Monitoring tells you *what* is broken; observability helps you understand *why*.

## Solution / Explanation

**Observability** (borrowed from control theory) measures how well internal states can be inferred from external outputs. In software, it is achieved through three complementary **pillars**:

### 1. Logs
Immutable, timestamped records of discrete events. Good for: debugging specific errors, audit trails, forensic analysis.

**Key practices:**
- Structured logging (JSON) so logs can be queried programmatically.
- Correlation IDs to link log lines across services for a single request.
- Avoid logging sensitive data.

### 2. Metrics
Numeric measurements aggregated over time (counters, gauges, histograms). Good for: dashboards, SLO tracking, alerting.

**Key practices:**
- USE method: Utilization, Saturation, Errors (for resources).
- RED method: Rate, Errors, Duration (for services).
- Use consistent naming conventions.

### 3. Traces (Distributed Tracing)
A trace captures the journey of a request through multiple services, represented as a tree of **spans**. Good for: understanding latency, identifying bottlenecks, debugging cross-service failures.

**Key practices:**
- Propagate trace context via standard headers (W3C Trace Context).
- Capture key attributes (service name, operation, error status) on each span.

### Observability vs. Monitoring

| | Monitoring | Observability |
|---|---|---|
| Approach | Check predefined conditions | Explore arbitrary questions |
| Failure model | Known unknowns | Unknown unknowns |
| Tool type | Dashboards, thresholds, alerts | Logs, traces, metrics together |
| Requires | Anticipating failure modes | Rich instrumentation |

Monitoring is a subset of observability. You can monitor a system without it being observable, but you cannot be truly observable without some form of monitoring.

## Key Components

- **OpenTelemetry** — the de-facto standard SDK and wire protocol for emitting and collecting all three pillars.
- **Log aggregation** — ELK (Elasticsearch, Logstash, Kibana), EFK (Fluentd/Fluent Bit), Grafana Loki.
- **Metrics platform** — Prometheus + Grafana, Datadog.
- **Tracing backend** — Jaeger, Zipkin, Grafana Tempo.
- **Correlation ID** — a unique request ID propagated through all service calls to link logs and traces.

## When to Use

Observability is a baseline requirement for any production distributed system. Invest in it proportionally to system complexity:

- Single service: structured logging + basic metrics is sufficient.
- Microservices: all three pillars are needed; distributed tracing becomes essential.
- Large-scale: invest in centralized platforms and SLOs based on metrics.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Enables debugging unknown failure modes | Instrumentation adds development cost |
| Reduces mean time to resolution (MTTR) | High cardinality metrics are expensive |
| Supports SLO-based reliability engineering | Data volume from logs/traces requires cost management |
| Tool-agnostic with OpenTelemetry | Cultural shift required — teams must instrument proactively |

## High-Cardinality Data

Traditional monitoring relies on low-cardinality aggregates (avg latency, total error count). Observability tools like Honeycomb argue that **high-cardinality data** — attributes with many unique values such as user ID, trace ID, request path, tenant, or customer tier — are the key to debugging unknown failure modes.

Where a metric asks "what is the average latency for `/checkout`?", a high-cardinality trace allows "what is the P99 latency for premium users in EU region using iOS?" — a question you could not pre-define in a dashboard.

## Core Analysis Loop

Observability enables an iterative debugging workflow:

1. **Observe** — detect an anomaly or symptom via alerts, SLO burn rate, or direct inspection.
2. **Hypothesize** — form a theory about root cause based on the available data.
3. **Test** — query logs, traces, and metrics to confirm or refute the hypothesis.
4. **Fix** — remediate and verify the resolution closes the anomaly.

This loop works without deploying new code or adding new instrumentation — a key distinction from traditional monitoring.

## Related

- [[Distributed Tracing]] — one of the three pillars; deep-dive concept
- [[Microservices Architecture]] — primary context where observability becomes critical
- [[Service Mesh]] — service meshes emit observability data automatically
- [[Sidecar Pattern]] — telemetry sidecars (OpenTelemetry Collector) collect and forward data
- [[Observability Implementation Guide]] — actionable implementation roadmap across all three pillars
- [[Self-Healing Systems]] — extends observability from passive detection to active autonomous remediation
