---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://martinfowler.com/articles/consumerDrivenContracts.html
source_date: 2006-06-12
source_author: Ian Robinson (published on martinfowler.com)
tags:
  - testing
  - contract-testing
  - microservices
  - source
---

# Martin Fowler - Consumer-Driven Contracts

Article by Ian Robinson describing the consumer-driven contract pattern for managing service evolution in SOA environments.

## Key Takeaways

- Traditional coupling: consumers validate against the provider's full schema (XSD), coupling to the entire interface even when only a subset is used.
- **Provider Contract**: the full published capability of the service.
- **Consumer Contract**: one consumer's subset of expectations — "open and incomplete" since no single consumer uses all provider functionality.
- **Consumer-Driven Contract**: the union of all active consumer expectations — "closed and complete"; this is what the provider must not break.
- Consumers should implement "just enough" validation (e.g., Schematron assertions on specific fields) rather than full-schema validation.
- Pattern excavates "hidden couplings" that broad schema validation obscures.
- Providers gain visibility into how consumers actually use their service; can safely change unused capabilities.
- Enables impact assessment: proposed changes can be evaluated against documented consumer expectations before deployment.

## Referenced Concepts

- [[Consumer-Driven Contract Testing]]
- [[Microservices Architecture]]
- [[Bounded Context]]
- [[Integration Testing]]

## Confidence

High — read directly from source.
