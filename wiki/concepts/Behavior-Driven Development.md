---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/articles/practical-test-pyramid.html
tags:
  - testing
  - bdd
  - methodology
  - collaboration
---

# Behavior-Driven Development

A collaborative software development practice that extends TDD by expressing tests as plain-language scenarios using a Given/When/Then structure, bridging the communication gap between technical and non-technical stakeholders.

## Problem

Unit tests written by developers are opaque to product owners and business analysts. Acceptance criteria live in tickets or wikis, disconnected from the tests that verify them. The result: misaligned implementations, expensive late-stage corrections, and tests that verify the wrong thing.

## Solution / Explanation

Dan North introduced BDD (around 2003) as a refinement of TDD focused on behaviour rather than implementation. The key contribution is the **Given/When/Then** (GWT) template — derived from user stories — that both humans and tools can parse:

```gherkin
Feature: Account withdrawal
  Scenario: Withdraw within balance
    Given the account has a balance of 100
    When the customer withdraws 20
    Then the account balance is 80
    And no error is returned
```

This format, codified in **Gherkin** syntax, is processed by tools like **Cucumber** (Java, Ruby, JavaScript), **SpecFlow** (.NET), and **Behave** (Python) to generate executable test stubs.

### BDD as a Collaboration Tool

BDD is primarily a **conversation technique**, not just a test format. The "Three Amigos" practice brings together:
- A **developer** (can we build it?)
- A **tester** (how do we break it?)
- A **product owner / BA** (what does the business need?)

Together they refine scenarios before a line of code is written, catching misunderstandings early. The Gherkin file is the output of that conversation, not the starting point.

## Key Components / Rules

- **Feature files** — plain-text `.feature` files written in Gherkin; serve as living documentation.
- **Step definitions** — code that maps GWT phrases to executable test logic.
- **Scenarios as acceptance criteria** — each scenario is one verifiable behaviour; scenarios replace or complement traditional acceptance tests.
- **Ubiquitous Language** — GWT scenarios should use the domain language of the [[Bounded Context]], not technical jargon.

## When to Use

- Features with clearly-defined business rules that benefit from stakeholder-readable tests.
- Regression suites for critical user journeys.
- Organisations practising agile where POs participate in story refinement.

Less suitable:
- Low-level algorithmic logic (unit tests are clearer).
- Teams where no non-technical stakeholder will read the scenarios (BDD degrades into verbose TDD).

## Trade-offs

| Benefit | Cost |
|---|---|
| Scenarios double as living documentation | GWT syntax adds ceremony for simple cases |
| Shared language aligns teams | Step definition maintenance grows with scenario count |
| Catches requirements gaps before development | Scenarios can become brittle if tied to UI details |
| Reduces rework from misunderstood requirements | Requires cultural buy-in from non-technical participants |

## Related

- [[Test-Driven Development]] — BDD extends TDD's Red-Green-Refactor with business-readable syntax
- [[Test Pyramid]] — BDD scenarios sit near the top (acceptance layer)
- [[Bounded Context]] — domain language used in Gherkin scenarios
- [[Testing Strategies Overview]]
