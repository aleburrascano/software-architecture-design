---
type: concept
created: '2026-05-03'
updated: 2026-05-03
sources:
  - 'https://awesome-architecture.com/microservices/api-gateway/api-gateway/'
  - 'https://learn.microsoft.com/en-us/azure/architecture/microservices/design/gateway'
  - 'https://microservices.io/patterns/apigateway.html'
tags:
  - microservices
  - patterns
  - api
  - cloud-native
---

# API Gateway Pattern

A microservices pattern in which a single entry point (the API Gateway) sits between external clients and backend services, routing requests and centralizing cross-cutting concerns like authentication, rate limiting, SSL termination, and response aggregation.

## Problem

In a microservices architecture, external clients (web, mobile, third-party) need to interact with multiple services. Without a gateway, clients must:

- Know the address and API of every individual service (tight coupling to service topology).
- Make multiple round trips to gather data from several services (especially costly for mobile networks).
- Handle cross-cutting concerns (auth, rate limiting, logging) themselves, or each service must re-implement them.
- Deal with service refactoring by updating every client.
- Manage protocol diversity — backend services may use non-standard protocols.
- Face an expanded attack surface from publicly exposed service endpoints.

As microservices.io states: clients face a "granularity mismatch" — microservices expose fine-grained APIs, but clients need coarser-grained functionality.

## Solution

An **API Gateway** acts as the single front door for all clients. It receives external requests, applies cross-cutting policies, and routes them to the appropriate backend services. Clients interact only with the gateway; backend services can change independently. The gateway handles requests through simple routing (reverse proxy) or request fanning (aggregating multiple service calls).

```
         ┌────────────┐
Clients ──► API Gateway├──► Order Service
         │            ├──► Product Service
         │            └──► Auth Service
         └────────────┘
```

## Core Responsibilities

Three primary design patterns within the API Gateway (Microsoft):

| Pattern | Description |
|---------|-------------|
| **Gateway Routing** | Reverse proxy: route client requests to different services using layer-7 routing. Decouples clients from service topology. |
| **Gateway Aggregation** | Fan out a single client request to multiple services; aggregate results into one response. Reduces chattiness. |
| **Gateway Offloading** | Centralize cross-cutting concerns so individual services don't implement them. |

### Offloadable concerns:
- SSL/TLS termination and mutual TLS
- Authentication and authorization (JWT, OAuth)
- Rate limiting and throttling
- IP allowlist/blocklist
- Logging, monitoring, and distributed tracing
- Response caching
- Web application firewall (WAF)
- GZIP compression
- Static content serving

### API Gateway vs. Service Mesh

| | API Gateway | Service Mesh |
|---|---|---|
| Traffic | North-south (external to internal) | East-west (service to service) |
| Concern | Client-facing API management | Internal service communication |
| Location | Edge of the cluster/system | Every service instance |
| Complementary? | Yes — they serve different layers |

### Backend for Frontend (BFF) Variant

Rather than one universal gateway, the **[[Backend for Frontend Pattern|BFF]]** approach creates separate API Gateways optimized for each client type (web, mobile, partner API). Each BFF is a thin layer that aggregates and tailors responses for its specific client. This eliminates the "one-size-fits-all" compromise.

## Popular Implementations

- **Kong** — open-source, plugin-based, high performance.
- **AWS API Gateway** — managed cloud service.
- **Azure API Management** — enterprise-grade, full lifecycle management.
- **Azure Application Gateway** — managed load balancer with layer-7 routing and WAF.
- **Ocelot** — .NET-specific, lightweight.
- **Envoy / Ambassador / Istio ingress** — cloud-native proxies with gateway capabilities.
- **Nginx / HAProxy** — open-source reverse proxies with gateway features.

## When to Use

- Microservices architectures with multiple client types.
- When cross-cutting concerns must be centralized.
- When you need protocol translation between clients and services.
- When you want to decouple client contracts from service implementations.
- When a single operation requires calls to multiple services (use aggregation).

**Not suitable when:**
- Simple, monolithic applications with a single API.
- The gateway itself becomes a development bottleneck (single team owns all routes).

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Single entry point simplifies clients | Potential single point of failure (mitigated with HA deployment) |
| Centralizes auth, rate limiting, logging | Can become an unscalable bottleneck if poorly designed |
| Clients are decoupled from service topology | Gateway team becomes a dependency for all service changes |
| Enables protocol translation | Added network hop may increase latency (typically insignificant) |
| Reduces client round trips via aggregation | Implementation requires careful scalability (reactive/event-driven approaches) |
| Reduces attack surface | Routing rules must be kept synchronized with actual service changes |

## Related

- [[Microservices Architecture]] — the architectural context
- [[Service Mesh]] — handles east-west service communication; complements the gateway
- [[Backend for Frontend Pattern]] — variant where each client type gets its own gateway
- [[Circuit Breaker Pattern]] — gateways often implement circuit breaking for backend calls
- [[Observability]] — centralized request logging and tracing at the gateway
