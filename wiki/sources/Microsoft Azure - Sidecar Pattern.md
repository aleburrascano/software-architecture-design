---
type: source
created: '2026-05-03'
updated: '2026-05-03'
source_path: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/sidecar'
source_date: '2026-02-17'
source_author: Microsoft / Clayton Siemens
tags:
  - source
  - cloud-native
  - patterns
  - microsoft
---
# Microsoft Azure - Sidecar Pattern

Microsoft Azure Architecture Center documentation for the Sidecar pattern (also known as the Sidekick pattern).

## Summary

Comprehensive documentation covering the Sidecar pattern's problem statement, solution, implementation considerations, when to use / not use, and Azure Well-Architected Framework alignment. Includes practical examples (Dapr, Istio, OpenTelemetry Collector, Ambassador, protocol adapters).

## Key Arguments

- Peripheral concerns (logging, configuration, service discovery, networking) should be isolated from core business logic.
- Sidecar containers share the lifecycle of the primary application (created and destroyed together).
- Key advantages: language independence, shared resource access, low latency (same host), enhanced extensibility.
- Most common implementation: containers deployed as Kubernetes sidecar containers.
- Real-world examples: Dapr sidecar (dependency abstraction), service mesh proxies (Istio/Envoy), OpenTelemetry Collector, protocol adapters.
- Not suitable when: IPC performance is critical, application is small, component needs independent scaling, or the platform already provides the capability.

## Concepts Covered

- [[Sidecar Pattern]] — comprehensive primary coverage
- [[Service Mesh]] — Istio uses sidecar proxies as its data plane
- [[Observability]] — OpenTelemetry Collector as a sidecar
- [[Ambassador Pattern]] — a specific sidecar use case

## Quality Notes

Official Microsoft documentation; well-structured and current (updated 2026). Authoritative for practical Kubernetes/Azure deployment guidance. Includes WAF pillar alignment (Security, Operational Excellence, Performance Efficiency).
