---
type: topic
created: '2026-05-06'
updated: '2026-05-06'
tags:
  - observability
  - opentelemetry
  - monitoring
  - distributed-tracing
  - implementation
  - logs
  - metrics
  - traces
---
# Observability Implementation Guide

A synthesis of observability across four primary sources — from control theory foundations through implementation tooling — structured as an actionable guide for building observable distributed systems.

## Observability vs Monitoring

These are not the same thing:

| Aspect | Monitoring | Observability |
|---|---|---|
| **Question answered** | "Is something wrong?" | "Why is something wrong?" |
| **Known unknowns** | Yes — predefined dashboards and alerts | Yes |
| **Unknown unknowns** | No — can't alert on what you didn't predict | Yes — explore via high-cardinality data |
| **Data structure** | Pre-aggregated metrics, fixed dashboards | Raw events, flexible queries |
| **Investigation** | Follow predefined runbooks | Ad-hoc exploration |

**Control theory definition** (Kalman, 1960): A system is observable if its internal state can be inferred from its external outputs. The software analogy: can you understand what your system is doing from the signals it emits?

## The Three Pillars

### Logs

Timestamped records of events. Best for: debugging specific requests, audit trails, error details.

- **Structured logs** (JSON) are machine-queryable; prefer over unstructured text
- **Log aggregation:** ELK Stack (Elasticsearch, Logstash, Kibana), Loki, Splunk
- **Correlation IDs:** Every request gets a unique ID; propagate through all service calls

### Metrics

Numeric measurements over time. Best for: dashboards, alerting, capacity planning.

- **Types:** Counter (monotonically increasing), Gauge (current value), Histogram (distribution), Summary
- **Tools:** Prometheus (scrape-based), OpenTelemetry Collector, Datadog, CloudWatch
- **SLOs:** Define Service Level Objectives as metric thresholds (e.g., p99 latency < 200ms)

### Traces

End-to-end records of a request's journey through distributed services. Best for: understanding latency, finding where errors originated.

- **Span:** A single operation within a trace (service A handling one request)
- **Trace:** A tree of spans representing the full request path
- **Context propagation:** W3C TraceContext standard; trace ID and span ID passed in HTTP headers
- **Tools:** Jaeger (CNCF), Zipkin, Tempo, AWS X-Ray

## OpenTelemetry: The Unified Standard

OpenTelemetry (OTel) is the **merger of OpenTracing and OpenCensus** under CNCF. It provides:
- A unified SDK for emitting logs, metrics, and traces
- Auto-instrumentation for popular frameworks
- The OpenTelemetry Collector (vendor-agnostic pipeline)
- Exporters to any backend (Jaeger, Prometheus, Datadog, etc.)

```
Application (OTel SDK)
       │
       ▼
OTel Collector (filter, batch, route)
       │
       ├──► Jaeger (traces)
       ├──► Prometheus (metrics)
       └──► Loki (logs)
```

**Use OTel** rather than vendor SDKs — it avoids lock-in and provides one instrumentation for all signals.

## High-Cardinality Data

Honeycomb's key insight: **high-cardinality fields make debugging possible**.

| Low-cardinality | High-cardinality |
|---|---|
| `status_code = 500` | `user_id = "u-12345"` |
| `service = "api"` | `request_id = "req-abc"` |
| `region = "us-east"` | `customer_tier = "enterprise"` |

When you get an alert that 500 errors are up, high-cardinality data lets you ask: "Which specific users are affected? What endpoint? What customer tier?" Low-cardinality aggregates can't answer these questions.

**The cardinality trade-off:** High-cardinality data is expensive to store and index. Solutions:
- **Sampling:** Capture 1% of normal traffic, 100% of errors
- **Tail sampling:** Decide what to keep after you see the full trace

## The Core Analysis Loop

Honeycomb's observability workflow:

```
1. OBSERVE — alert fires or anomaly noticed in dashboard
2. HYPOTHESIZE — "Maybe it's the new deployment?" or "DB query slow?"
3. EXPLORE — query high-cardinality traces/logs to test hypothesis
4. CONFIRM — find the root cause in actual data
5. FIX — fix the code/config, not the symptom
6. VERIFY — confirm fix via the same observability signals
```

This loop replaces "guess and check" debugging with evidence-based investigation.

## Implementation Roadmap

### Phase 1: Foundation
- Structured logging with correlation IDs
- Basic metrics (request rate, error rate, latency — the RED method)
- Health check endpoints

### Phase 2: Distributed Tracing
- Instrument all services with OpenTelemetry
- Deploy Jaeger or Tempo for trace storage
- Propagate trace context across service boundaries

### Phase 3: SLO-Based Alerting
- Define SLOs for critical paths (availability, latency p99)
- Alert on SLO burn rate (not just absolute thresholds)
- Build error budget dashboards

### Phase 4: High-Cardinality Events
- Emit structured events with business context (user_id, order_id, tenant_id)
- Use tail-based sampling for cost control
- Enable ad-hoc exploration without predefined queries

### Phase 5: AIOps
- Anomaly detection on time-series metrics
- Automated incident correlation
- Runbook automation for known failure patterns
- Path toward [[Self-Healing Systems]]

## Related

- [[Observability]] — the core concept page
- [[Distributed Tracing]] — traces in depth
- [[Self-Healing Systems]] — observability as foundation for autonomous recovery
- [[Quality Attributes]] — observability as a quality attribute
- [[Continuous Integration and Delivery]] — observability gates in deployment pipeline
