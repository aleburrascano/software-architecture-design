---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/richardsonMaturityModel.html
  - https://grpc.io/docs/what-is-grpc/introduction/
  - https://grpc.io/docs/what-is-grpc/core-concepts/
  - https://graphql.org/learn/
tags:
  - api-design
  - rest
  - graphql
  - grpc
  - synthesis
---

# API Design Overview

A synthesis of REST, GraphQL, and gRPC — decision guide for selecting the right API style, managing contracts, versioning, and maturity.

---

## The Three Major API Styles

Modern APIs fall into three dominant styles, each with different strengths:

| | [[REST]] | [[GraphQL]] | [[gRPC]] |
|-|----------|------------|---------|
| **Protocol** | HTTP/1.1 | HTTP/1.1 (HTTP/2 optional) | HTTP/2 |
| **Payload** | JSON / XML | JSON | Protocol Buffers (binary) |
| **Contract** | Optional (OpenAPI/Swagger) | GraphQL Schema (built-in) | .proto file (built-in) |
| **Query flexibility** | Fixed shapes per endpoint | Client-defined queries | Fixed method signatures |
| **Streaming** | Limited (SSE, WebSocket) | Subscriptions (WebSocket) | Native (4 modes) |
| **HTTP caching** | Native | Non-trivial (POST) | Not applicable |
| **Browser support** | Full | Full | Needs gRPC-Web proxy |
| **Type safety** | Optional | Built-in | Built-in |
| **Best for** | Public APIs, CRUD, broad client access | Flexible data queries, BFF | Internal RPC, low latency, streaming |

---

## Decision Guide: Which API Style to Choose?

### Choose REST when:
- Building a **public-facing API** consumed by diverse third-party clients.
- The API is **resource-oriented** (CRUD over entities: users, orders, products).
- **HTTP caching** is important (CDN, browser cache for GET responses).
- Client diversity is high (browsers, mobile, CLIs, third-party integrations).
- Team has existing REST tooling and conventions.

### Choose GraphQL when:
- **Multiple clients** (mobile, web, partners) have different data needs for the same domain.
- Eliminating **over-fetching** and **under-fetching** is a priority.
- The API aggregates data from **multiple backend services** (API gateway / BFF pattern).
- Rapid front-end iteration is required and front-end teams should own their data shape.
- **Subscription** (real-time) is needed alongside queries and mutations.

### Choose gRPC when:
- **Internal microservice-to-microservice** communication where low latency matters.
- The system is **polyglot** and needs cross-language generated clients with strict contracts.
- **Streaming** is a core requirement (server push, client upload, bidirectional).
- Bandwidth efficiency matters (binary Protobuf vs. JSON).
- The API is not directly consumed by browsers (or gRPC-Web is acceptable).

---

## Maturity and Contracts

### REST Maturity: Richardson Maturity Model

The [[Richardson Maturity Model]] describes four levels of REST adoption:

| Level | Key concept | What it adds |
|-------|-------------|-------------|
| 0 | HTTP as transport | Baseline — single endpoint |
| 1 | Resources | Distinct URIs per resource |
| 2 | HTTP Verbs + Status Codes | Correct use of GET/POST/PUT/DELETE, proper status codes |
| 3 | HATEOAS | Hypermedia links in responses for self-discovery |

**Industry reality:** Most production APIs operate at Level 2 and are correctly called "HTTP APIs." Full HATEOAS (Level 3) is rare but valuable for highly evolvable public APIs.

### GraphQL and gRPC Contracts

Both GraphQL and gRPC are **contract-first by design**:
- GraphQL schema defines all types and operations; changes are validated against it.
- Protobuf `.proto` files define all services and messages; `protoc` enforces compatibility.

REST depends on external tooling (OpenAPI/Swagger) for contract definition — best practice but not enforced by the protocol.

---

## Versioning Strategies

All three styles eventually face breaking changes. The approaches differ:

### REST Versioning
Three common patterns — see [[API Design Principles]]:
- **URI versioning**: `/api/v1/users` — most common; explicit but creates URI proliferation.
- **Header versioning**: `Accept: application/vnd.api.v2+json` — clean URIs; less visible.
- **Query param**: `/users?version=2` — simple; not cache-friendly.

### GraphQL Versioning
GraphQL's philosophy is **"versionless evolution"**:
- Add new fields/types freely (non-breaking).
- Deprecate fields with `@deprecated` directive; provide migration guidance.
- Avoid breaking changes; if unavoidable, introduce a new type alongside the old.
- This works because clients request only specific fields — adding new fields doesn't break existing queries.

### gRPC Versioning
Protocol Buffers enable **forward and backward compatibility** through field numbering:
- Adding new fields with new numbers is non-breaking.
- Never remove or renumber existing fields.
- For breaking changes: create a new service version (`v2`) alongside the old.
- Proto3 defaults unknown fields to zero values, preserving compatibility.

---

## Backward Compatibility

Common to all styles:

**Non-breaking changes (safe):**
- Adding new optional fields to a response.
- Adding new endpoints/operations.
- Adding new optional request parameters.

**Breaking changes (requires versioning):**
- Removing or renaming a field.
- Changing a field's data type.
- Making optional parameters required.
- Changing URI structure (REST) or removing a method (gRPC).

**Tolerant Reader pattern:** Consumer code should ignore unknown fields and not fail on additions. This is essential for forward compatibility.

---

## Key Cross-Cutting Concerns

- **[[Idempotency]]** — critical for POST (REST) and mutations (GraphQL); use idempotency keys for safe retries.
- **Authentication** — JWT bearer tokens (REST/GraphQL) or gRPC metadata (typically `Authorization` header).
- **Rate limiting** — `429 Too Many Requests` (REST); query complexity limits (GraphQL); server-side rate limiting (gRPC).
- **Pagination** — offset/limit or cursor-based (REST/GraphQL); server-streaming (gRPC for large datasets).
- **Error handling** — HTTP status codes (REST); `errors` array in response (GraphQL); gRPC status codes (`google.rpc.Status`).
- **Observability** — correlate requests with trace IDs; standardise logging fields.

---

## Hybrid Approaches

Many systems combine styles:
- **gRPC internally, REST externally**: microservices communicate via gRPC; a gateway exposes REST/GraphQL to clients.
- **GraphQL as a BFF layer**: a GraphQL API aggregates multiple downstream REST or gRPC services.
- **REST for resources, gRPC for streaming**: REST for CRUD endpoints, gRPC for real-time data feeds.

---

## Related

- [[REST]] — Fielding constraints, HTTP verbs, statelessness
- [[Richardson Maturity Model]] — levels of REST compliance
- [[GraphQL]] — schema-driven flexible query API
- [[gRPC]] — binary, contract-first, streaming RPC
- [[API Design Principles]] — versioning, backward compat, idempotency, HATEOAS
- [[Microservices Architecture]] — API design is central to microservices boundaries
- [[Idempotency]] — safe retries for POST/mutations
