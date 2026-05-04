---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/richardsonMaturityModel.html
tags:
  - api-design
  - rest
  - http
  - web
  - maturity-model
---

# Richardson Maturity Model

A four-level classification of REST API maturity developed by Leonard Richardson and popularised by Martin Fowler, describing the progressive adoption of web standards from bare HTTP transport to full hypermedia-driven APIs.

## Problem

"REST" is often claimed but rarely defined consistently. Teams need a practical progression to understand how deeply they are leveraging HTTP's built-in capabilities, and what incremental improvements they can make toward a truly evolvable web API.

## Solution / Explanation

Richardson identified four levels (0-3), each introducing an additional web concept. Martin Fowler described them as "a model (developed by Leonard Richardson) that breaks down the principal elements of a REST approach into three steps. These introduce resources, http verbs, and hypermedia controls." Roy Fielding confirmed that Level 3 is the pre-condition for true REST.

---

### Level 0 — HTTP as Transport (The Swamp of POX)

**Concept:** HTTP is used purely as a tunnelling protocol for Remote Procedure Calls. One endpoint handles all interactions.

- Single URI receives all requests (e.g., `POST /appointmentService`).
- Request body describes the operation and data (XML/JSON envelope).
- HTTP status code is almost always 200; errors are embedded in the response body.
- Equivalent to SOAP/XML-RPC without WS-*.

**Example:**
```
POST /appointmentService
{ "action": "getSlots", "doctorId": "mjones", "date": "2024-01-04" }
```

This is a functional baseline but exploits none of HTTP's infrastructure (caching, status codes, resource addressing).

---

### Level 1 — Resources

**Concept:** Distinct URIs identify individual resources. The single service endpoint is replaced with many resource-specific ones.

- Each resource has its own URI: `/doctors/mjones`, `/slots/1234`.
- Interactions target a specific resource, not a generic service.
- Multiple resources may still use only POST.

**Design principle:** "This is like the notion of object identity. Rather than calling some function in the ether and passing arguments, we call a method on one particular object."

**Improvement:** Reduces complexity by dividing into individual resources; enables granular access control and logging per resource.

---

### Level 2 — HTTP Verbs and Status Codes

**Concept:** HTTP verbs are used according to their defined semantics. Response codes communicate outcomes.

- **GET** for safe, idempotent reads — enables caching.
- **POST** to create; **PUT** to replace; **DELETE** to remove.
- HTTP status codes carry meaning: `201 Created`, `409 Conflict`, `404 Not Found`.
- Errors are communicated via status codes, not embedded in 200 OK bodies.

**Example:**
```
GET /doctors/mjones/slots?date=20240104&status=open
→ 200 OK  (cacheable)

POST /slots/1234/appointment
→ 201 Created  +  Location: /slots/1234/appointment
```

**Key benefit:** GET safety enables HTTP caching at every layer (CDN, proxy, browser). Standard status codes enable generic error handling.

This is the level most real-world "REST APIs" actually achieve.

---

### Level 3 — Hypermedia Controls (HATEOAS)

**Concept:** Responses include links describing the available next actions. The API becomes self-documenting and self-discoverable.

**HATEOAS** = Hypermedia As The Engine Of Application State.

- Responses embed `<link>` elements (or JSON `_links`) with relationship types and URIs.
- Clients navigate the API by following links, not by hardcoding URIs.
- Servers can change URI structure without breaking clients that follow links.

**Example response after booking:**
```json
{
  "appointmentId": "1234",
  "_links": {
    "self":   { "href": "/slots/1234/appointment" },
    "cancel": { "href": "/slots/1234/appointment", "method": "DELETE" },
    "addTest":{ "href": "/slots/1234/appointment/tests" }
  }
}
```

**Key benefit:** "It allows the server to change its URI scheme without breaking clients. As long as clients look up the 'addTest' link URI then the server team can juggle all URIs other than the initial entry points." — Fowler

**Important caveat:** Roy Fielding has stated that Level 3 is a pre-condition of REST. Most "REST APIs" in the industry operate at Level 2 and call themselves REST — technically they are **HTTP APIs**.

---

## Design Principles Behind Each Level

| Level | Design concept |
|-------|---------------|
| 1 | Divide and conquer — decompose into individual resources |
| 2 | Standardise verbs — uniform interface for consistent handling |
| 3 | Discoverability — self-documenting hypermedia protocol |

## When to Use as a Tool

- Assessing an existing API's maturity and identifying improvement areas.
- Educating a team on REST principles progressively.
- Setting API design standards (most teams target Level 2 with selective Level 3).

> [!question] Unverified
> HATEOAS (Level 3) is theoretically compelling but rarely implemented in mainstream REST APIs. Its practical adoption is limited; most API clients still rely on static documentation. The gap between the ideal and industry practice is wide.

## Trade-offs

| Higher level | Benefit | Cost |
|--------------|---------|------|
| Level 1 (Resources) | Granular targeting | More endpoints to document |
| Level 2 (Verbs + Status) | HTTP caching; standard error handling | Verb semantics require discipline |
| Level 3 (HATEOAS) | Evolvable URI structure; self-discovery | Client complexity; link parsing; sparse tooling |

## Related

- [[REST]] — the full REST architectural style
- [[API Design Principles]] — versioning, backward compatibility, idempotency
- [[GraphQL]] — an alternative that solves over/under-fetching at Level 2+
- [[gRPC]] — contract-first binary alternative
