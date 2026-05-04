---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar'
tags:
  - cloud-native
  - microservices
  - patterns
  - deployment
---
# Sidecar Pattern

A deployment pattern that attaches a secondary process or container (the sidecar) alongside the primary application to handle cross-cutting concerns — such as logging, configuration, service discovery, and networking — without modifying the primary application's code.

## Problem

Applications require peripheral capabilities beyond their core functionality: monitoring, logging, configuration management, service discovery, TLS termination, retry logic, and so on. Embedding these capabilities directly into each service has two problems:

1. **Tight coupling** — the peripheral code is mixed with business logic, making both harder to evolve independently.
2. **Language/framework duplication** — if services are polyglot, each language needs its own implementation of the same concerns. Maintaining identical behavior across languages is error-prone.

Decomposing into fully separate services solves the duplication problem but adds network latency and operational overhead.

## Solution / Explanation

Deploy the peripheral functionality in a **separate process or container** that runs on the same host as the primary application, shares its lifecycle (created and destroyed together), and communicates via localhost or an interprocess mechanism.

Like a motorcycle sidecar, it is attached to the primary vehicle but is its own distinct unit. The pattern is also known as the *Sidekick pattern*.

```
┌────────────────────────────────┐
│  Host (VM or Pod)              │
│  ┌──────────────┐  ┌────────┐  │
│  │  Primary App │  │Sidecar │  │
│  │  (business   │◄─► (logs, │  │
│  │   logic)     │  │  TLS,  │  │
│  └──────────────┘  │  proxy)│  │
│                    └────────┘  │
└────────────────────────────────┘
```

Key advantages:
- **Language independence** — one sidecar implementation works with any language.
- **Low latency** — same-host communication avoids network hops.
- **Shared resource access** — sidecar can observe the same OS metrics as the primary.
- **Independent updates** — sidecar can be updated without redeploying the primary app.

## Common Use Cases

- **Service mesh data plane** — Envoy/Linkerd proxies injected as sidecars to handle mTLS, retries, circuit breaking, and telemetry (e.g., Istio).
- **Dependency abstraction** — Dapr sidecar provides state management, pub/sub, service invocation via a language-agnostic HTTP/gRPC API.
- **Ambassador sidecar** — routes and enriches outbound calls (logging, circuit breaking).
- **Protocol adaptation** — translates between protocols (e.g., legacy TCP to HTTP/2).
- **Telemetry enrichment** — OpenTelemetry Collector running as a sidecar.

## When to Use

- Primary application uses diverse languages/frameworks.
- A cross-cutting concern (security, observability, networking) is owned by a different team.
- The component must share the application's lifecycle but be updated independently.
- You need language-agnostic platform access.

Not suitable when:

- Very frequent inter-process communication makes the sidecar's latency overhead unacceptable.
- The application is small enough that sidecar overhead outweighs the benefits.
- The component needs to scale independently of the application.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Decouples cross-cutting concerns from business logic | Adds per-instance resource cost |
| Language-agnostic | Adds inter-process communication complexity |
| Independent deployability of the sidecar | Not suitable when IPC is performance-critical |
| Enables service mesh patterns | Increases number of running processes |

## Related

- [[Service Mesh]] — service meshes rely on sidecar proxies as their data plane
- [[Observability]] — telemetry sidecars (OpenTelemetry Collector) are a common pattern
- [[Ambassador Pattern]] — a specific sidecar variant for outbound call management
- [[Microservices Architecture]] — typical deployment context
- [[Circuit Breaker Pattern]] — often implemented in the sidecar proxy layer
