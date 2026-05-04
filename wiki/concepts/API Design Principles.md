---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/richardsonMaturityModel.html
  - https://restfulapi.net/
tags:
  - api-design
  - versioning
  - backward-compatibility
  - idempotency
  - hateoas
---

# API Design Principles

Cross-cutting principles for designing APIs that are robust, evolvable, and interoperable — covering versioning strategies, backward compatibility, idempotency, and HATEOAS.

## Problem

APIs are interfaces between teams and systems. A poorly designed API breaks clients on every change, requires expensive coordination, creates security risks, and is difficult to reason about under failure conditions.

## Solution / Explanation

Good API design starts with treating the API as a **product** with a contract. The contract must be stable, predictable, and evolvable without breaking existing clients.

---

### Versioning Strategies

Versioning is necessary when breaking changes must be introduced. Three common strategies:

**URI Versioning**
```
GET /api/v1/users
GET /api/v2/users
```
- Pros: Explicit, cacheable, easy to route at proxy level.
- Cons: Proliferates URIs; clients must update URLs; old versions must be maintained in parallel.

**Header Versioning**
```
Accept: application/vnd.myapi.v2+json
```
- Pros: Clean URIs; same resource can serve multiple versions.
- Cons: Less visible; harder to test with a browser; cache keys must include the header.

**Query Parameter Versioning**
```
GET /users?version=2
```
- Pros: Simple; visible.
- Cons: Caches often ignore query params; semantically pollutes the resource URI.

> [!question] Unverified
> URI versioning is the most widely adopted despite its verbosity. Semantic versioning strategies for APIs (e.g., breaking vs. non-breaking classification) are not universally standardised.

---

### Backward Compatibility

A change is **backward-compatible** (non-breaking) if existing clients continue to function:
- Adding new optional fields to a response.
- Adding new optional request parameters.
- Adding new resource endpoints.
- Expanding an enum with new values (with caution).

A change is **breaking**:
- Removing or renaming a field.
- Changing a field's type or format.
- Making an optional field required.
- Changing HTTP method or URI for an existing operation.

**Practical rule:** Treat every field in your response as a contract. Use a **Tolerant Reader** pattern on the consumer side: ignore unknown fields rather than failing.

---

### Idempotency

An operation is **idempotent** if repeating it produces the same result. This is critical for reliability: network failures cause clients to retry, and retries must not cause duplicate side effects.

| HTTP Method | Idempotent? | Safe? |
|-------------|------------|-------|
| GET | Yes | Yes |
| HEAD | Yes | Yes |
| PUT | Yes | No |
| DELETE | Yes | No |
| POST | No | No |
| PATCH | No (by default) | No |

For non-idempotent operations (POST, PATCH), use an **Idempotency Key**: a client-generated unique ID sent in a header (`Idempotency-Key`). The server stores the result keyed on this ID and returns the cached result for retries.

See [[Idempotency]] for full treatment.

---

### HATEOAS

Hypermedia As The Engine Of Application State — the Level 3 constraint in the [[Richardson Maturity Model]].

Responses include links to related resources and available actions:
```json
{
  "orderId": "123",
  "status": "pending",
  "_links": {
    "self":   { "href": "/orders/123" },
    "cancel": { "href": "/orders/123/cancel", "method": "POST" },
    "pay":    { "href": "/orders/123/payment", "method": "POST" }
  }
}
```

Benefits:
- Server can change URI structure without breaking clients.
- Clients discover available transitions from current state.
- API becomes self-documenting.

Practical reality: Full HATEOAS is rarely implemented. Most production APIs operate at [[Richardson Maturity Model|Level 2]] and rely on static documentation.

---

### Resource Naming Best Practices

- Use **nouns**, not verbs: `/orders`, not `/getOrders`.
- Use **plural** nouns for collections: `/users`, `/products`.
- Nest for clear ownership: `/users/{id}/orders` for a user's orders.
- Keep URIs stable; use versioning to introduce changes.
- Avoid deep nesting beyond 2-3 levels.

---

### Error Responses

Consistent, machine-readable error responses reduce client integration cost:
```json
{
  "status": 400,
  "error": "Bad Request",
  "message": "Field 'email' is required",
  "path": "/users"
}
```

Use standard HTTP status codes (4xx for client errors, 5xx for server errors). Consider RFC 7807 (Problem Details for HTTP APIs) for a standard error format.

---

### Rate Limiting and Throttling

Signal limits via standard headers:
- `X-RateLimit-Limit` — requests allowed per window.
- `X-RateLimit-Remaining` — requests remaining.
- `Retry-After` — seconds until the limit resets.
- Return `429 Too Many Requests` when exceeded.

## When to Use

These principles apply to any API intended for consumption by external clients or other teams — REST, GraphQL, or gRPC.

## Trade-offs

| Principle | Benefit | Cost |
|-----------|---------|------|
| URI versioning | Explicit and cacheable | URI proliferation |
| Idempotency keys | Safe retries | Server-side state to store results |
| HATEOAS | Evolvable URIs | Client complexity; sparse adoption |
| Strict backward compat | No client breakage | Accumulation of legacy fields |

## Related

- [[REST]] — where most of these principles originate
- [[Richardson Maturity Model]] — HATEOAS is Level 3
- [[GraphQL]] — avoids some versioning pain via schema evolution
- [[gRPC]] — Protobuf handles backward compat via field numbering
- [[Idempotency]] — full treatment of idempotency in distributed systems
- [[Microservices Architecture]] — API design is critical at service boundaries
