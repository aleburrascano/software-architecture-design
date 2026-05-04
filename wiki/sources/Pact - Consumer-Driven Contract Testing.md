---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://docs.pact.io/
source_date: 2026-05-03
source_author: Pact Foundation
tags:
  - testing
  - contract-testing
  - pact
  - source
---

# Pact - Consumer-Driven Contract Testing Documentation

Official documentation site for Pact, the leading open-source consumer-driven contract testing tool.

## Key Takeaways

- Pact is a code-first testing tool for validating HTTP and message-based integrations through contract tests.
- "Contract tests assert that inter-application messages conform to a shared understanding that is documented in a contract."
- **Consumer creates the contract**: the consuming application writes tests against a mock provider, and Pact generates a pact file (JSON) from recorded interactions.
- **Provider verifies the contract**: the provider replays each recorded request against its real implementation and asserts responses match.
- Contracts are "enforced by executing test cases, each describing a single concrete request/response pair" — not static schema validation.
- Only actively-used communication paths are tested; unused provider behaviours may evolve freely.
- **Pact Broker**: central repository for sharing and managing contracts across distributed teams; enables `can-i-deploy` queries.
- Supports both HTTP/REST and async messaging (Kafka, SQS, etc.).
- Analogy: "testing a smoke alarm by pressing its button rather than setting your house on fire" — no full integration environment needed.

## Referenced Concepts

- [[Consumer-Driven Contract Testing]]
- [[Integration Testing]]
- [[Microservices Architecture]]

## Confidence

High — read directly from source.
