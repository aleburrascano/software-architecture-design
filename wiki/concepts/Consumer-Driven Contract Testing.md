---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/consumerDrivenContracts.html
  - https://docs.pact.io/
  - https://martinfowler.com/articles/practical-test-pyramid.html
tags:
  - testing
  - contract-testing
  - microservices
  - integration
---

# Consumer-Driven Contract Testing

A testing pattern in which each service consumer publishes the specific expectations it has of a provider, creating a contract that the provider must continuously verify — enabling safe independent deployment without shared test environments.

## Problem

In [[Microservices Architecture]], services evolve independently. Traditional integration tests require deploying all services together (expensive, slow, brittle). API versioning managed by providers alone produces bloated interfaces nobody needs. Breaking changes are discovered late — in staging or production.

## Solution / Explanation

Ian Robinson described the pattern (2006); Fowler formalised it. The insight: **providers cannot know what consumers actually use**. Instead of one big provider contract that all consumers must accept, each consumer declares its minimal subset of expectations.

### Three Contract Layers (Fowler)

1. **Provider Contract** — the full published capability of the service.
2. **Consumer Contract** — one consumer's subset of expectations (fields it reads, request formats it sends).
3. **Consumer-Driven Contract** — the union of all active consumer expectations; this is what the provider must not break.

### Workflow with Pact

**Pact** is the dominant implementation:

1. The **consumer** writes a test that records interactions (request + expected response) against a mock provider.
2. Pact generates a **pact file** (JSON contract) from those interactions.
3. The pact file is published to a **Pact Broker** (shared registry).
4. The **provider** runs a verification step: it replays each request from the pact file against its real implementation and checks responses match.
5. CI enforces: the provider pipeline fails if any consumer contract is broken.

Neither side needs the other deployed. The consumer tests its own code against a mock; the provider verifies against recorded expectations.

### Connection to [[Bounded Context]]

Contract boundaries map naturally to context boundaries. When two bounded contexts communicate over HTTP or messaging, a CDC test documents the exact translation contract at the seam.

## Key Components / Rules

- **Only test what you consume** — consumers assert only the fields they actually use, allowing providers to add fields without breaking contracts.
- **Consumer owns the pact file** — the contract expresses the consumer's needs, not the provider's self-description.
- **Pact Broker** — centralises contracts and verifiability status; enables can-I-deploy queries.
- **Provider states** — mechanisms to set up required data/state on the provider side before each interaction is replayed.
- **Message contracts** — Pact supports async messaging (Kafka, SQS) in addition to HTTP.

## When to Use

- [[Microservices Architecture]] with independently-deployed services.
- Any HTTP or messaging integration where teams cannot co-deploy.
- Replacing broad integration tests that require a running environment.

Not ideal when:
- Two services are always deployed together (an integration test is simpler).
- Provider is a third-party API you don't control for verification.

## Trade-offs

| Benefit | Cost |
|---|---|
| No shared test environment needed | Both teams must adopt the Pact toolchain |
| Providers get visibility into what consumers use | Consumer test suite setup requires mock configuration |
| Breaking changes caught before deployment | Pact Broker adds infrastructure to maintain |
| Enables true independent deployment | Cannot replace E2E tests for emergent system behaviour |

## Related

- [[Integration Testing]] — CDC replaces some broad integration tests
- [[Test Pyramid]] — CDC tests sit between integration and E2E
- [[Microservices Architecture]] — primary context for CDC
- [[Bounded Context]] — contract boundaries align with context boundaries
- [[Testing Strategies Overview]]
