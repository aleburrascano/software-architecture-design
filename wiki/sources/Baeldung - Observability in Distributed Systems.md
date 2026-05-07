---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Observability in Distributed Systems - Baeldung.md'
source_date: ''
source_author: Kumar Chandrakant / Baeldung
tags:
  - source
  - observability
  - opentracing
  - opentelemetry
  - jaeger
  - prometheus
  - elk
  - distributed-tracing
  - opentelemetry
---
# Baeldung - Observability in Distributed Systems

Technical tutorial on implementing observability in distributed systems, with tool-specific implementation depth for each of the three pillars.

## Summary

This Baeldung tutorial goes deeper than conceptual treatment into the practical implementation of observability tooling. For distributed tracing, it covers Jaeger and Zipkin implementing the OpenTracing standard, explaining how spans are created, propagated across service boundaries via HTTP headers or message metadata, and aggregated into distributed traces. The mechanics of trace context propagation — injecting and extracting trace IDs across service calls — are explained with implementation detail.

For metrics, the article covers Prometheus with its scrape-based collection model, time-series data model, PromQL for querying, and Alertmanager for threshold-based alerting. The OpenCensus library is discussed as a metrics instrumentation standard. For logs, the ELK stack (Elasticsearch, Logstash, Kibana) provides centralized log aggregation and search. The critical capability of correlation IDs linking log entries to trace spans is explained as the bridge between the pillars.

The article explains OpenTelemetry as the convergence of OpenTracing and OpenCensus into a single unified observability instrumentation standard. This merger resolved the fragmentation between the two previously competing standards and provides a vendor-neutral instrumentation API that works with any backend (Jaeger, Zipkin, Prometheus, Honeycomb, Dynatrace).

## Key Arguments

- Logs, metrics, and traces serve different observability needs and cannot substitute for each other
- Distributed tracing requires propagating trace context (trace ID + span ID) across every service boundary — missing a hop breaks the trace chain
- Jaeger and Zipkin implement OpenTracing for end-to-end request tracing in distributed systems
- Prometheus enables time-series metrics with a pull-based scrape model and PromQL for flexible querying
- OpenTelemetry is the unified standard replacing both OpenTracing and OpenCensus; it is the recommended instrumentation approach
- Correlation IDs link log entries to specific trace spans, enabling cross-pillar investigation of incidents

## Concepts Covered

- [[Observability]] — implementation depth; the three pillars in practice
- [[Distributed Tracing]] — Jaeger and Zipkin implementation; span creation and context propagation
- [[Observability Implementation Guide]] — tooling guidance; Prometheus, ELK, Jaeger, OpenTelemetry

## Quality Notes

Excellent technical depth with concrete tool guidance. Baeldung quality is consistently high and well-reviewed. This is the best source for OpenTelemetry tooling and implementation-level observability detail. Pair with Honeycomb's source for high-cardinality concepts and Dynatrace's source for the monitoring vs. observability framing.
