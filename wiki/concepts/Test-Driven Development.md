---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://martinfowler.com/bliki/UnitTest.html
  - https://martinfowler.com/bliki/TestDrivenDevelopment.html
tags:
  - testing
  - tdd
  - methodology
---

# Test-Driven Development

A software development practice in which a failing automated test is written before any production code, then the minimum code needed to pass the test is written, followed by refactoring.

## Problem

Writing tests after the fact is friction-prone: code that was never designed for testability is hard to test retrospectively, coverage is incomplete because passing tests don't reveal unknown edge cases, and tests tend to mirror implementation rather than specify behaviour.

## Solution / Explanation

Kent Beck formalised TDD as the **Red-Green-Refactor** cycle:

1. **Red** — Write a small failing test that describes one piece of desired behaviour. Run it; confirm it fails (no false greens).
2. **Green** — Write the simplest code that makes the test pass. Do not over-engineer.
3. **Refactor** — Clean up the production code and test code without breaking the green state. Remove duplication, clarify names.

Repeat the cycle in very short increments — typically minutes, not hours.

### Sociable vs. Solitary Tests

Fowler identifies two schools:
- **Solitary** (mockist): every collaborator is replaced with a [[Test Double]]. Strict isolation; failures always point to the unit under test.
- **Sociable** (classic): real collaborators are used unless they are remote services or shared external state. Fowler prefers this; it tests realistic interactions without mocking everything.

TDD does not mandate either school — the practice is compatible with both.

## Key Components / Rules

- Write **one failing test at a time** — do not write multiple tests before making the first green.
- Make the green step **as small as possible** — return a hardcoded value if it passes; generalise in the refactor step.
- The refactor step is **non-negotiable** — without it, the codebase degrades.
- Tests become a **regression safety net** that enables confident change.
- Over time, TDD drives toward testable, decoupled designs because untestable code forces awkward test setup.

## When to Use

- When building new features from scratch (TDD pays immediately).
- When fixing bugs: write a failing test reproducing the bug first, then fix.
- When design intent is unclear — writing a test first forces API decisions early.

Less suitable:
- Exploratory spikes where the shape of the solution is unknown.
- UI layout and visual tests (prefer [[Behavior-Driven Development]] or screenshot testing).

## Trade-offs

| Benefit | Cost |
|---|---|
| Immediate design feedback | Upfront discipline; slower initial pace |
| Confidence to refactor | Test suite must itself be maintained |
| Living executable specification | Tests can become tightly coupled to implementation |
| Catches regressions automatically | Not a substitute for exploratory or performance testing |

**Criticism**: TDD does not guarantee good architecture. It prevents obvious mistakes but cannot replace design thinking. Also, the practice is difficult to apply to legacy codebases without first introducing seams.

## Related

- [[Test Pyramid]] — TDD produces the unit test layer
- [[Test Double]] — tooling used in the Red-Green-Refactor cycle
- [[Behavior-Driven Development]] — extends TDD with structured natural-language scenarios
- [[Integration Testing]] — TDD at integration level
- [[Testing Strategies Overview]]
