---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/cloud-design-patterns/bff/'
tags:
  - microservices
  - api
  - patterns
  - frontend
---
# Backend for Frontend Pattern

A variant of the [[API Gateway Pattern]] in which a separate, dedicated backend service is created for each distinct frontend client type (web, mobile, third-party), each tailored to that client's specific data requirements and interaction patterns.

## Problem

A single generic API gateway must serve all clients: the web app needs rich, nested data; the mobile app needs a lightweight subset; a partner API needs a different structure entirely. A single gateway either:

- Over-fetches (returns too much data for mobile) causing performance issues on constrained devices.
- Under-fetches (requires multiple requests for the web app) causing waterfalls.
- Becomes a complex, unwieldy service trying to satisfy contradictory requirements.

## Solution / Explanation

Instead of one universal backend, create a **separate BFF for each frontend type**. Each BFF:

- Aggregates and transforms data from multiple backend microservices.
- Returns exactly the shape and fields each client needs.
- Owns the client-specific logic (caching strategies, pagination, data shaping).
- Can be owned and deployed by the same team as the frontend.

```
              ┌────────────────────────────────────────────┐
              │  Backend Microservices                     │
              │  [Order Service] [Product Service] [User]  │
              └──────────────────────┬─────────────────────┘
                                     │
              ┌──────────────────────┼─────────────────────┐
              ▼                      ▼                      ▼
          [Web BFF]             [Mobile BFF]         [Partner BFF]
              │                      │                      │
          Web App             Mobile App             Partner API
```

### BFF and Security

BFFs are particularly valuable for web application security. Instead of exposing access tokens to the browser (SPA), the BFF:
- Runs server-side, managing OAuth flows and storing tokens in HTTP-only cookies.
- Acts as a trusted backend proxy, preventing token leakage to JavaScript.
- This is the recommended architecture for web apps that need high security posture.

### BFF vs. API Gateway

| | API Gateway | BFF |
|---|---|---|
| Number | One (or few) | One per client type |
| Ownership | Platform/API team | Frontend team |
| Tailoring | Generic | Client-specific |
| Aggregation | Limited | Heavy |

## When to Use

- Multiple client types with significantly different data needs.
- Mobile clients where bandwidth and battery efficiency matter.
- Security requirements that differ per client (browser vs. server-to-server).
- Frontend teams that need autonomy over their backend data layer.

Not suitable when:
- A single client type exists and the generic API gateway is sufficient.
- BFF proliferation leads to too many services to maintain.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Optimized responses per client type | More services to build, deploy, and maintain |
| Frontend teams own their data layer | Risk of logic duplication across BFFs |
| Improved security isolation | Common functionality must be extracted to shared libs or services |
| Enables independent frontend evolution | Coordination needed when backend services change |

## Related

- [[API Gateway Pattern]] — BFF is a specialized variant of the API gateway
- [[Microservices Architecture]] — BFFs aggregate data from multiple microservices
- [[Service Mesh]] — service mesh handles east-west traffic; BFF handles north-south per client
