---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://martinfowler.com/articles/richardsonMaturityModel.html
source_date: 2010-03-18
source_author: Martin Fowler
tags:
  - rest
  - api-design
  - http
  - hateoas
  - richardson
---

# Martin Fowler — Richardson Maturity Model

An article by Martin Fowler describing Leonard Richardson's model for classifying REST API maturity, published at [https://martinfowler.com/articles/richardsonMaturityModel.html](https://martinfowler.com/articles/richardsonMaturityModel.html).

## Summary

The article explains Richardson's four-level model (0-3) using a medical appointment booking service as a worked example. Each level adds a new web concept: resources (Level 1), HTTP verbs and status codes (Level 2), and hypermedia controls / HATEOAS (Level 3).

## Key Quotes

- "Roy Fielding has made it clear that level 3 RMM is a pre-condition of REST."
- On HATEOAS: "It allows the server to change its URI scheme without breaking clients."
- On the model's purpose: "A tool to help us learn about the concepts and not something that should be used in some kind of assessment mechanism."

## Key Takeaways

- Level 0 (HTTP as tunnelling) is the RPC baseline; Level 2 is where most real-world APIs operate.
- Level 2 unlocks HTTP caching via safe GET methods and standardised error handling via status codes.
- Level 3 (HATEOAS) enables truly evolvable, self-documenting APIs but is rarely implemented in practice.
- The three levels correspond to: divide and conquer (resources), standardisation (verbs), discoverability (hypermedia).
- The examples use XML but the concepts apply equally to JSON APIs.

## Wiki Pages That Cite This Source

- [[Richardson Maturity Model]] — full breakdown of Level 0-3
- [[REST]] — Fielding's constraints and HTTP method semantics
- [[API Design Principles]] — HATEOAS as a versioning/evolvability tool
- [[API Design Overview]] — maturity levels in the decision guide
