---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/practical-test-pyramid.html
  - https://martinfowler.com/bliki/IntegrationTest.html
tags:
  - testing
  - integration-testing
  - architecture
---

# Integration Testing

Tests that verify the collaboration between your application code and external components — databases, message brokers, HTTP clients, file systems — at the boundary where serialisation, configuration, and protocol details live.

## Problem

Unit tests can all pass while the application fails to connect to a database, sends malformed HTTP requests, or maps ORM columns to the wrong fields. These failures live at the **seams** between your code and external systems, and unit tests (which replace those systems with [[Test Double|test doubles]]) cannot catch them.

## Solution / Explanation

Integration tests exercise the real adapter code against a real (or realistic) external system. Fowler's practical test pyramid places integration tests in the middle layer — more than E2E tests, fewer than unit tests.

Fowler distinguishes two scopes:

### Narrow Integration Tests
Test **one integration point** in isolation:
- Test the database adapter by starting a real database (locally or in CI via Docker), running queries, and asserting results.
- Test the HTTP client by starting a WireMock or test server and verifying your client serialises/deserialises correctly.
- All _other_ collaborators remain as test doubles.

Benefits: fast, pointed failures, no dependency on other services being available.

### Broad Integration Tests
Exercise **multiple live services together** — e.g., service A calls service B which writes to a shared database. Closer to E2E in cost and fragility. Use sparingly; prefer [[Consumer-Driven Contract Testing]] to verify cross-service contracts cheaply.

### Database Integration Tests
A common and important sub-case:
- Spin up a real database (Postgres, MySQL) in Docker using tools like Testcontainers.
- Run migrations before the test suite.
- Test repository methods, query correctness, transaction behaviour.
- Roll back or truncate between tests for isolation.

This catches ORM misconfiguration, missing indexes used in query correctness assumptions, and constraint violations that no unit test can reveal.

## Key Components / Rules

- **Test the boundary, not the business logic** — integration tests are not the place for exhaustive business rule coverage; unit tests handle that.
- **Real infrastructure where feasible** — in-memory substitutes (H2 for Postgres) mask dialect differences; prefer a real engine in a container.
- **One external system per test** — mixing multiple live systems multiplies failure modes and slows diagnosis.
- **Idempotent setup/teardown** — tests must not depend on execution order.

## When to Use

- Any code that reads from or writes to an external system (database, cache, message broker, third-party API).
- HTTP client serialisation and error handling.
- Retry/circuit-breaker logic where the behaviour depends on real responses.

## Trade-offs

| Benefit | Cost |
|---|---|
| Catches real adapter bugs missed by unit tests | Slower than unit tests (network/disk I/O) |
| Tests actual query syntax and ORM mapping | Requires infrastructure in CI (Docker, etc.) |
| Validates serialisation round-trips | Flaky if infrastructure is unreliable |
| Reasonable cost compared to full E2E | Does not validate cross-service emergent behaviour |

## Related

- [[Test Pyramid]] — integration tests occupy the middle layer
- [[Test Double]] — used for non-integration collaborators within narrow tests
- [[Consumer-Driven Contract Testing]] — preferred over broad integration tests for cross-service verification
- [[Test-Driven Development]] — TDD can drive integration test design at adapter boundaries
- [[Testing Strategies Overview]]
