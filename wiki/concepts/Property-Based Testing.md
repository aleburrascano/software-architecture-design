---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://hypothesis.works/articles/what-is-property-based-testing/
  - https://martinfowler.com/articles/practical-test-pyramid.html
tags:
  - testing
  - property-based-testing
  - generative-testing
---

# Property-Based Testing

A testing technique in which the test framework automatically generates many random inputs to verify that a stated invariant (a "property") holds for all of them — finding edge cases that manually-authored examples miss.

## Problem

Example-based tests verify only the cases the developer thought to write. Humans are bad at finding edge cases: empty strings, maximum integers, null unicode characters, inputs with leading whitespace, combinations of valid-but-unusual values. Real bugs live in these corners, but no one thinks to write `test_empty_list_concatenation()`.

## Solution / Explanation

John Hughes and Koen Claessen introduced **QuickCheck** (Haskell, 1999), the seminal property-based testing library. The developer specifies a **property** — a statement that should be true for all valid inputs — and the framework generates hundreds or thousands of random inputs to try to falsify it.

When a failing case is found, modern frameworks **shrink** the input to the minimal counterexample before reporting it, making failures readable.

### Example (conceptual)

Property: reversing a list twice returns the original list.
```
for all lists L: reverse(reverse(L)) == L
```
The framework generates random lists (empty, single-element, large, with duplicates) and verifies the property holds for each.

### Key Libraries

| Library | Language |
|---|---|
| **QuickCheck** | Haskell (original) |
| **Hypothesis** | Python |
| **fast-check** | JavaScript/TypeScript |
| **ScalaCheck** | Scala |
| **jqwik** | Java |
| **FsCheck** | F# / .NET |
| **PropEr** | Erlang |

**Hypothesis** is notable for being especially ergonomic and for its shrinking algorithm, which produces very minimal failing examples.

## Key Components / Rules

- **Properties, not examples** — instead of `assert add(2, 3) == 5`, assert `for all x, y: add(x, y) == add(y, x)` (commutativity).
- **Generators** — the framework provides built-in generators for primitives; developers compose custom generators for domain types.
- **Shrinking** — automatically reduces a failing input to its simplest form.
- **Seeded randomness** — failing seeds are recorded so failures are reproducible.
- **Stateful testing** — advanced frameworks generate sequences of commands to test stateful systems (e.g., a data structure or state machine).

### Good Candidates for Properties

- Algebraic laws (commutativity, associativity, idempotency).
- Round-trips: `parse(serialise(x)) == x`.
- Inverse operations: `decode(encode(x)) == x`.
- Monotonicity: sorting a list doesn't change its length.
- Safety invariants: a bank account balance never goes negative.

## When to Use

- Pure functions with well-defined mathematical properties.
- Serialisation/deserialisation code.
- Parsers and encoders.
- Any algorithm where the specification can be expressed as an invariant.
- Security-sensitive code where edge cases are attack surfaces.

## Trade-offs

| Benefit | Cost |
|---|---|
| Finds edge cases humans miss | Properties can be hard to formulate for complex domains |
| Minimal counterexample after shrinking | Test execution is slower than a single example test |
| Increases confidence in correctness | Requires understanding of generator composition |
| Complements, not replaces, example tests | Stateful property testing is significantly more complex |

## Related

- [[Test-Driven Development]] — property-based tests complement TDD's example-based tests
- [[Test Pyramid]] — fits within the unit layer; can also apply at integration level
- [[Testing Strategies Overview]]
