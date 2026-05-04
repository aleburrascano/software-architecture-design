---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/richardsonMaturityModel.html
  - https://restfulapi.net/
tags:
  - api-design
  - rest
  - http
  - web
  - architecture
---

# REST

Representational State Transfer — an architectural style for distributed hypermedia systems defined by Roy Fielding in his 2000 doctoral dissertation, describing six constraints that, when applied to HTTP-based APIs, yield scalable, cacheable, and evolvable interfaces.

## Problem

Distributed applications need a lightweight, uniform way for clients to interact with server resources over the web — without tight coupling, without proprietary protocols, and in a way that scales to the size of the internet.

## Solution / Explanation

REST is not a protocol or a standard; it is a set of architectural constraints. An API that adheres to these constraints is called **RESTful**. The web itself (HTTP + HTML + URLs) is the canonical example of a REST system.

### Fielding's Six Constraints

1. **Client-Server** — Separation of UI (client) from data storage (server) improves portability and scalability. Clients and servers evolve independently.

2. **Stateless** — Each request from client to server must contain all information needed to understand the request. No session state is stored on the server between requests. This enables load-balancing across any server instance.

3. **Cacheable** — Responses must declare themselves cacheable or non-cacheable. Caching eliminates server interactions for repeated requests, improving efficiency and scalability.

4. **Uniform Interface** — The central REST constraint. Simplifies and decouples the architecture through four sub-constraints:
   - *Resource identification in requests* — URIs identify resources.
   - *Resource manipulation through representations* — clients manipulate resources via the representations returned (JSON, XML, HTML).
   - *Self-descriptive messages* — each message includes enough information to describe how to process it (e.g., `Content-Type`).
   - *HATEOAS* — Hypermedia as the Engine of Application State; responses include links to possible next actions.

5. **Layered System** — The client cannot tell whether it is connected directly to the server or to an intermediary (load balancer, CDN, API gateway). Layers can be added transparently.

6. **Code on Demand** (optional) — Servers can extend client functionality by transferring executable code (e.g., JavaScript). The only optional constraint.

### Resources, URIs, and Representations

- A **resource** is any named concept (user, order, invoice) identified by a URI.
- A **representation** is the current or intended state of a resource in a particular format (JSON, XML, HTML).
- Clients request and manipulate representations, not resources directly.

### HTTP Methods Semantics

| Method | Meaning | Idempotent | Safe |
|--------|---------|-----------|------|
| GET | Retrieve a representation | Yes | Yes |
| POST | Create a new resource / submit data | No | No |
| PUT | Replace a resource entirely | Yes | No |
| PATCH | Partially update a resource | No | No |
| DELETE | Remove a resource | Yes | No |

### Richardson Maturity Model

The [[Richardson Maturity Model]] provides a 4-level progressive measure of REST adoption:
- **Level 0** — HTTP as tunnelling (single endpoint, RPC-style).
- **Level 1** — Individual resources with distinct URIs.
- **Level 2** — HTTP verbs and status codes used correctly.
- **Level 3** — HATEOAS: responses include hypermedia links to next actions.

Level 3 is Fielding's pre-condition for true REST.

## Key Components

- **Resource** — the conceptual entity identified by a URI.
- **URI** — the unique identifier for a resource.
- **HTTP verb** — the operation to perform on the resource.
- **Representation** — the payload format (JSON, XML).
- **Status code** — machine-readable outcome of the operation.
- **Hypermedia link** — link in a response pointing to related resources or actions (HATEOAS).

## When to Use

- Public APIs where interoperability and ubiquity of HTTP clients matter.
- CRUD-heavy interfaces over resources (users, products, orders).
- APIs consumed by many different clients (web, mobile, third-party).
- When cacheability of responses is important for scalability.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Ubiquitous tooling (HTTP clients everywhere) | Chatty: multiple round-trips for complex queries |
| Statelessness enables horizontal scaling | Over-fetching / under-fetching data |
| Cacheability via HTTP | Versioning and backward compatibility require discipline |
| Self-descriptive messages | HATEOAS is rarely fully implemented in practice |

## Comparison

| | REST | [[GraphQL]] | [[gRPC]] |
|-|------|------------|---------|
| Protocol | HTTP/1.1 | HTTP/1.1 or HTTP/2 | HTTP/2 |
| Payload | JSON / XML | JSON | Protobuf (binary) |
| Query flexibility | Fixed endpoints | Client-defined queries | Fixed service definition |
| Caching | Native HTTP caching | Requires custom cache | No native HTTP caching |
| Best for | Resource-oriented public APIs | Flexible data fetching | Low-latency internal RPC |

## Related

- [[Richardson Maturity Model]] — levels of REST compliance
- [[GraphQL]] — flexible query alternative to REST
- [[gRPC]] — binary RPC alternative
- [[API Design Principles]] — versioning, idempotency, backward compatibility
- [[Idempotency]] — required for safe PUT/DELETE operations
- [[Microservices Architecture]] — REST is a common inter-service communication style
