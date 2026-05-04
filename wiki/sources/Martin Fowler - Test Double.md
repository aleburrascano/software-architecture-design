---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://martinfowler.com/bliki/TestDouble.html
source_date: 2006-01-17
source_author: Martin Fowler
tags:
  - testing
  - test-doubles
  - source
---

# Martin Fowler - Test Double

Bliki entry documenting Gerard Meszaros's taxonomy of test doubles from the book *xUnit Test Patterns*.

## Key Takeaways

- "Test Double" is the umbrella term for any production object replacement used in testing (coined by Meszaros, analogous to a film stunt double).
- **Dummy**: passed but never used; satisfies a method signature.
- **Fake**: working implementation with a shortcut unsuitable for production (e.g., in-memory database).
- **Stub**: returns canned answers to calls; does not respond to calls outside what's programmed.
- **Spy**: a stub that also records how it was called; used for post-hoc verification.
- **Mock**: pre-programmed with expectations; verifies calls were made; throws on unexpected calls.
- Key distinction: stubs for state verification; mocks for behaviour verification.
- Over-mocking leads to tests that are tightly coupled to implementation and break on refactoring.

## Referenced Concepts

- [[Test Double]]
- [[Test-Driven Development]]
- [[Test Pyramid]]

## Confidence

High — read directly from source.
