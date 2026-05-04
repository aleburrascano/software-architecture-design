---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/bliki/TestDouble.html
tags:
  - testing
  - test-doubles
  - unit-testing
---

# Test Double

An umbrella term (coined by Gerard Meszaros) for any object that stands in for a real production dependency during a test — analogous to a film stunt double standing in for an actor.

## Problem

A unit under test often depends on collaborators that are slow (databases, network), non-deterministic (clocks, random number generators), hard to set up (third-party services), or destructive (payment processors). Testing with real collaborators makes tests slow, unreliable, or impossible to run in CI.

## Solution / Explanation

Meszaros defined five distinct types of test double. Fowler documented them on martinfowler.com. Each type has a specific role:

### The Five Types

| Type | Has Working Implementation? | Records Interactions? | Verifies Expectations? |
|---|---|---|---|
| Dummy | No | No | No |
| Fake | Yes (simplified) | No | No |
| Stub | No (canned answers) | No | No |
| Spy | No (canned answers) | Yes | Optional |
| Mock | No | Yes | Yes (mandatory) |

**Dummy**
Passed to satisfy a parameter signature but never called during the test. Example: passing `null` or an empty object for a logger that isn't exercised in the test path.

**Fake**
Has a real but simplified implementation that works correctly but is unsuitable for production. Classic example: an in-memory repository that replaces a SQL database — full CRUD semantics, no disk I/O. Fast, deterministic, useful for integration tests.

**Stub**
Returns hardcoded, pre-programmed answers to specific calls. Does not respond to calls outside what the test programs. Example: `priceService.getPrice("AAPL")` always returns `150.00`. Used to control inputs to the unit under test.

**Spy**
A stub that also records how it was called (which methods, with what arguments). The test can then assert post-hoc: "the email service was called once with the correct address." Less prescriptive than mocks — the test asserts after the fact rather than setting expectations up front.

**Mock**
Pre-programmed with explicit expectations about which calls it must receive. Throws an exception if an unexpected call arrives or if an expected call never happens. Verification is built in. Encourages specifying exact interaction protocols.

### Mock vs. Stub — The Key Distinction

Fowler emphasises this is the most confused pair:
- A **stub** is about **state verification**: set up the collaborator, run the unit, assert the unit's output or state.
- A **mock** is about **behaviour verification**: assert that the unit _called_ the collaborator in the right way.

Overusing mocks leads to tests that are tightly coupled to implementation details and break on every internal refactor.

## When to Use

| Scenario | Preferred Double |
|---|---|
| Satisfying unused parameter | Dummy |
| Fast in-memory replacement | Fake |
| Controlling return values / side effects | Stub |
| Asserting a call was made (post-hoc) | Spy |
| Asserting exact interaction protocol | Mock |

## Trade-offs

| Benefit | Cost |
|---|---|
| Eliminates slow/unreliable external dependencies | Over-mocking hides real integration bugs |
| Enables deterministic, fast tests | Doubles must be kept in sync with real interfaces |
| Isolates the unit under test | Mock-heavy tests couple tests to implementation |

## Related

- [[Test-Driven Development]] — test doubles are the primary tooling
- [[Test Pyramid]] — doubles enable the unit layer
- [[Integration Testing]] — fakes are useful at integration scope
- [[Consumer-Driven Contract Testing]] — stubs of providers are generated from pact files
- [[Testing Strategies Overview]]
