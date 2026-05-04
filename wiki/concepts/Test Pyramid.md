---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/practical-test-pyramid.html
  - https://martinfowler.com/bliki/TestPyramid.html
tags:
  - testing
  - test-strategy
  - architecture
---

# Test Pyramid

A visual model that prescribes writing many small, fast unit tests, fewer integration tests, and very few end-to-end tests — proportional to the triangle shape of a pyramid.

## Problem

Without guidance, teams default to writing mostly UI-level or end-to-end tests because they feel "realistic." This produces slow, brittle test suites that are expensive to maintain and provide delayed feedback — the **test ice cream cone** anti-pattern, where the pyramid is inverted.

## Solution / Explanation

Mike Cohn introduced the pyramid; Martin Fowler popularised the heuristic. The model encodes two rules:

1. Write tests with different levels of granularity.
2. The higher the level, the fewer tests you should have.

The original three layers are:

| Layer | Scope | Speed | Quantity |
|---|---|---|---|
| **Unit** | Single class/function | Milliseconds | Many (70%+) |
| **Integration / Service** | Component + collaborators or external systems | Seconds | Some (~20%) |
| **UI / End-to-End** | Full stack through UI | Minutes | Few (~10%) |

Fowler's practical expansion adds named sub-categories:
- **Consumer-Driven Contract (CDC) tests** — verify service interfaces without full deployment; sit between integration and E2E in cost.
- **Narrow integration tests** — test a single integration point (e.g., the SQL mapping layer) in isolation.
- **Broad integration tests** — exercise multiple live services together.
- **Subcutaneous tests** — test just below the UI layer, avoiding browser overhead.

## Key Components / Rules

- **Unit tests**: fastest feedback; test one unit in isolation (or with real collaborators — see [[Test-Driven Development]] for the solitary/sociable distinction).
- **Integration tests**: exercise the seams where your code meets external systems — database adapters, HTTP clients, file parsers. Focus on serialisation/deserialisation boundaries.
- **Contract tests**: ensure provider and consumer agree on the interface without deploying both together. See [[Consumer-Driven Contract Testing]].
- **E2E tests**: run the entire system; kept minimal because they are slow and flaky.

## The Ice Cream Cone Anti-Pattern

An inverted pyramid: mostly manual and E2E tests, few units. Consequences:
- Slow CI pipelines (minutes to hours).
- Flaky tests tied to network/browser state.
- Poor feedback granularity — failures don't point to root cause.
- Expensive to maintain as UI changes break unrelated scenarios.

## When to Use

Apply the pyramid to any non-trivial codebase. Revisit proportions when:
- CI is too slow — likely too many broad tests.
- Bugs escape to production — likely insufficient coverage at the right level.
- Test maintenance dominates sprint capacity — likely tests are coupled to implementation details rather than behaviour.

## Trade-offs

| Benefit | Cost |
|---|---|
| Fast feedback loop | Requires discipline to resist writing only E2E tests |
| Failures localised near root cause | Unit tests can pass while integration seams are broken |
| Cheap to run in CI | Needs explicit investment in a good integration test strategy |

## Related

- [[Test-Driven Development]] — methodology that drives unit test volume
- [[Behavior-Driven Development]] — structures acceptance tests at the top of the pyramid
- [[Consumer-Driven Contract Testing]] — fills the gap between integration and E2E
- [[Integration Testing]] — deeper treatment of the middle layer
- [[Test Double]] — tooling for isolating units
- [[Testing Strategies Overview]] — synthesis page
