---
type: concept
created: '2026-05-03'
updated: '2026-05-06'
sources:
  - >-
    https://awesome-architecture.com/microservices/observability/distributed-tracing/
tags:
  - observability
  - microservices
  - distributed-systems
  - monitoring
---
# Distributed Tracing

An [[Observability]] technique that tracks a request as it traverses multiple services in a distributed system, recording the timing and metadata of each operation as **spans** within a unified **trace**, enabling end-to-end visibility into request flows.

## Problem

In a microservices architecture, a single user request may touch ten or more services. When a request is slow or fails:
- Logs from each service are separate; correlating them manually is tedious.
- Metrics show that latency increased but not *where* in the chain.
- It is impossible to visualize the causal chain of calls without purpose-built tooling.

## Solution / Explanation

Distributed tracing assigns a unique **Trace ID** to each request at its origin. Every service that handles the request creates a **Span** — a named, timed operation — and attaches the Trace ID to it. Spans are sent to a tracing backend that assembles them into a tree showing the full request lifecycle.

### Core Concepts

**Trace**
The entire journey of a request through the system. Represented as a tree of spans. Has a globally unique Trace ID.

**Span**
A single unit of work within the trace (e.g., "handle HTTP request", "query database", "call payment service"). Each span records:
- Operation name
- Service name
- Start time and duration
- Status (OK / error)
- Key-value attributes
- Parent Span ID (to build the tree)

**Context Propagation**
The mechanism for passing the Trace ID (and Span ID) across service boundaries. The W3C **Trace Context** standard defines `traceparent` and `tracestate` HTTP headers for this purpose.

**Baggage**
Key-value pairs that travel alongside trace context across all services (e.g., tenant ID, user ID). Useful for correlating traces with business context.

### Example Trace Tree

```
Trace: a3b2c1d4
├── [0ms - 250ms] API Gateway: POST /checkout
│   ├── [5ms - 80ms]  Order Service: CreateOrder
│   │   └── [10ms - 40ms] DB: INSERT orders
│   ├── [85ms - 180ms] Inventory Service: ReserveItems
│   └── [185ms - 245ms] Payment Service: ProcessPayment
│       └── [190ms - 240ms] External: Stripe API
```

### OpenTelemetry

The de-facto standard for distributed tracing instrumentation, formed by merging **OpenTracing** and **OpenCensus** into a single CNCF project. Provides:
- **API** — vendor-neutral interfaces for creating spans.
- **SDK** — implementation with samplers, exporters, processors.
- **OTLP** — wire protocol for exporting telemetry to backends.
- **Instrumentation libraries** — automatic tracing for popular frameworks (HTTP, gRPC, DB drivers).
- **Collector** — a standalone agent/gateway for receiving, processing, and exporting telemetry.

Common backends: **Jaeger** (CNCF-graduated, OpenTracing-compatible), **Zipkin**, **Grafana Tempo**, **AWS X-Ray**, **.NET Aspire Dashboard**.

## When to Use

- Any microservices or distributed system where latency or failures are hard to trace manually.
- Systems with SLO requirements where bottleneck identification matters.
- Post-incident analysis of cross-service failures.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| End-to-end visibility across services | Requires instrumentation investment |
| Pinpoints latency bottlenecks precisely | High cardinality (many unique trace IDs) → storage cost |
| Correlates logs, metrics, and traces | Context propagation must be implemented in every service |
| Standard (OpenTelemetry) reduces vendor lock-in | Sampling decisions affect trace completeness |

## Related

- [[Observability]] — distributed tracing is one of the three observability pillars
- [[Microservices Architecture]] — the primary context where distributed tracing is essential
- [[Service Mesh]] — service mesh proxies emit trace spans automatically
- [[Sidecar Pattern]] — OpenTelemetry Collector deployed as a sidecar
- [[Observability Implementation Guide]] — implementation roadmap covering OpenTelemetry stack setup and tool selection
