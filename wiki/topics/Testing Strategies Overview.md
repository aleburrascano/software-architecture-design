---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/practical-test-pyramid.html
  - https://martinfowler.com/bliki/TestDouble.html
  - https://martinfowler.com/bliki/UnitTest.html
  - https://martinfowler.com/articles/consumerDrivenContracts.html
  - https://docs.pact.io/
tags:
  - testing
  - strategy
  - architecture
  - overview
---

# Testing Strategies Overview

A synthesis of testing approaches from an architectural perspective: how to compose unit, integration, contract, and E2E tests into a coherent strategy that provides fast feedback, catches real bugs, and remains maintainable at scale.

## The Central Framework: The Test Pyramid

Start with [[Test Pyramid]]. The model encodes two rules: write tests at different granularities, and the higher the level, the fewer tests you should have. In practice, a healthy portfolio looks like:

```
         ▲  E2E / UI
        ▲▲▲  Contract
      ▲▲▲▲▲  Integration
    ▲▲▲▲▲▲▲  Unit
```

The **ice cream cone anti-pattern** (inverted) — mostly manual and E2E tests — is the single most common testing dysfunction in mature systems. It produces slow pipelines, fragile tests, and poor root-cause localisation.

## Layer-by-Layer Guide

### Unit Tests (Most)

See [[Test-Driven Development]] and [[Test Double]].

Unit tests are fast, isolated, and numerous. They verify that a single unit (class, function, module) behaves correctly given controlled inputs. Use [[Test Double|test doubles]] (stubs, fakes, mocks) to isolate from slow or non-deterministic collaborators.

**Choosing sociable vs. solitary**: Fowler favours sociable unit tests — use real collaborators unless they touch external systems or make tests non-deterministic. Reserve mocks for interaction verification, not to eliminate all real dependencies.

**Augment with [[Property-Based Testing]]** for any algorithmic, serialisation, or stateful logic. Property-based tests find the edge cases example tests miss.

### Integration Tests (Some)

See [[Integration Testing]].

Integration tests verify the seams: how your ORM maps to the database, how your HTTP client serialises requests, how your message consumer parses events. Use real infrastructure (Docker + Testcontainers) for database tests; use WireMock or test servers for HTTP clients.

Prefer **narrow integration tests** (one seam at a time) over broad ones (multiple live services). Broad integration tests are expensive to maintain and are better replaced by contract tests.

### Contract Tests (For Distributed Systems)

See [[Consumer-Driven Contract Testing]].

In [[Microservices Architecture]], contract tests replace the broad integration test layer for cross-service verification. Each consumer publishes its expectations (a pact); each provider verifies against all pacts. Neither needs the other deployed. The **Pact Broker** acts as the contract registry.

Contract tests are especially valuable at [[Bounded Context]] boundaries, where independent teams own each side of the interface.

### Behaviour / Acceptance Tests

See [[Behavior-Driven Development]].

BDD scenarios (Given/When/Then) are the right tool when business stakeholders need to read and validate test coverage. They sit above unit tests and exercise the application from the outside, either through the UI (subcutaneous) or the API.

Keep scenario count small — they are expensive to maintain. Focus on critical user journeys, not exhaustive edge cases.

### End-to-End Tests (Few)

E2E tests run the full system — browser, API, database, and dependencies. They catch emergent integration bugs that no other layer catches, but they are slow, flaky, and expensive to debug.

Rule of thumb: if a bug can be caught by a unit or integration test, do not write an E2E test for it. Reserve E2E for "does the happy path work in production topology?"

## Choosing What to Test and How

| Scenario | Recommended approach |
|---|---|
| Business logic, algorithms | Unit tests; augment with property-based tests |
| Database adapter / query | Narrow integration test with real DB in Docker |
| HTTP client serialisation | Narrow integration test with WireMock |
| Cross-service API contract | [[Consumer-Driven Contract Testing]] with Pact |
| Critical user journey | BDD scenario (subcutaneous or E2E) |
| Edge cases humans miss | [[Property-Based Testing]] |
| Bug fix | Reproduce with failing test first, then fix |

## Test Design Principles

- **Test behaviour, not implementation** — tests that know about private methods or internal state break on every refactor.
- **One reason to fail** — a test that asserts many unrelated things hides root cause.
- **Deterministic setup** — tests must not share mutable state; order-dependent tests are a symptom of missing isolation.
- **Fast feedback** — unit tests should run in seconds; full CI in minutes, not hours.
- **Tests as documentation** — a well-named test describes the system's expected behaviour more precisely than a comment.

## Common Dysfunctions

| Dysfunction | Symptom | Fix |
|---|---|---|
| Ice cream cone | CI takes 45+ minutes | Identify E2E tests that unit/integration tests can replace |
| Mock abuse | Tests pass but bugs escape | Replace mocks with real collaborators or fakes where feasible |
| No integration tests | Deploys break on DB changes | Add Testcontainers integration tests for all repository code |
| No contract tests | Cross-service breakage in staging | Introduce Pact between consumer/provider pairs |
| Flaky tests | CI passes 80% of the time | Isolate and fix non-determinism; do not retry |

## Related Concepts

- [[Test Pyramid]] — the foundational model
- [[Test-Driven Development]] — practice that drives unit test volume
- [[Behavior-Driven Development]] — structures acceptance tests
- [[Consumer-Driven Contract Testing]] — cross-service verification
- [[Test Double]] — isolation tooling
- [[Integration Testing]] — middle layer
- [[Property-Based Testing]] — generative edge-case finding
- [[Microservices Architecture]] — context where contract testing is critical
