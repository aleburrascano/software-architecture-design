---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://martinfowler.com/articles/practical-test-pyramid.html
source_date: 2018-02-26
source_author: Ham Vocke (with Martin Fowler)
tags:
  - testing
  - test-pyramid
  - source
---

# Martin Fowler - The Practical Test Pyramid

Comprehensive article on Ham Vocke's martinfowler.com about structuring an automated test suite around the test pyramid.

## Key Takeaways

- The test pyramid has three layers: unit, service/integration, UI. Expands to also include contract tests and subcutaneous tests.
- Two rules: "write tests with different granularity" and "the more high-level you get the fewer tests you should have."
- **Ice cream cone anti-pattern**: inverted pyramid — mostly manual and UI tests. Results in slow CI, high maintenance cost, poor failure localisation.
- Unit tests: narrowest scope, fastest. Can be sociable (using real collaborators) or solitary (using test doubles).
- Integration tests: focus on serialisation/deserialisation boundaries; test ORM mapping, HTTP clients, message parsing against real infrastructure.
- Contract tests / CDC tests: consumers publish expectations; providers verify. Pact is the main tool.
- End-to-end tests: very few. Test that the system works end-to-end in a production-like topology.
- Subcutaneous tests: just below UI; avoid browser overhead.
- Acceptance tests: validate features from user perspective; BDD Given/When/Then.

## Referenced Concepts

- [[Test Pyramid]]
- [[Integration Testing]]
- [[Consumer-Driven Contract Testing]]
- [[Test Double]]
- [[Behavior-Driven Development]]
- [[Test-Driven Development]]

## Confidence

High — read directly from source.
