---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/Software architecture 1.md
tags:
  - architecture
  - quality
  - maintenance
---

# Architecture Erosion

The gradual divergence between the intended architecture of a software system and its actual implemented structure over time.

## Problem / Why It Matters

An architecture is designed with certain structural principles — layering rules, dependency directions, service boundaries — intended to preserve [[Quality Attributes]] like maintainability, performance, and testability. Over time, implementation and maintenance decisions diverge from those principles. Each small shortcut or fix that violates an architectural rule adds up. The cumulative effect is a system that behaves nothing like its intended design, becoming progressively harder and more expensive to change.

## Origin

Perry and Wolf first described architecture erosion in 1992 alongside their foundational definition of software architecture. A canonical industry case is the **Mozilla browser rewrite**: Mozilla was a Netscape application whose codebase became harder to maintain through continuous changes that violated its original architecture. Initial poor design compounded by growing erosion eventually forced Netscape to spend two years rewriting the browser from scratch.

## Causes

- **Architectural violations** — developers take shortcuts that break intended structural rules (e.g., UI code directly calling the database in a layered architecture)
- **Technical debt accumulation** — incremental shortcuts degrade internal structure over time (see [[Technical Debt]])
- **Knowledge vaporization** — key personnel leave, taking the architectural intent with them; successors make decisions inconsistent with the original design without realizing it
- **Absent documentation** — when architectural decisions are not recorded (see [[Architectural Decision Records (ADR)]]), their rationale is unknown and therefore unenforceable

## Detection Approaches

Classified into four categories:

1. **Consistency-based** — check whether implementation matches the architectural specification (automated conformance checking)
2. **Evolution-based** — track structural changes over version history to identify drift
3. **Defect-based** — correlate defect patterns with architectural components to find structurally compromised areas
4. **Decision-based** — compare current structure against documented [[Architectural Decision Records (ADR)|architectural decisions]]

Tools include: static code analysis, dependency structure matrices, and automated architecture compliance tools.

## Consequences

- **Decreased performance** — unintended dependencies introduce overhead
- **Increased evolutionary cost** — each change becomes harder as the structural foundation weakens
- **Degraded quality** — reliability, testability, and maintainability all suffer
- **Eventually: rewrite** — in severe cases, the system becomes unmaintainable and must be rebuilt

## Prevention and Remediation

**Preventative measures:**
- Enforce architectural rules with automated checks (linters, dependency checkers)
- Regular code reviews with explicit attention to structural conformance
- Use **fitness functions** — automated tests that verify architectural properties continuously
- Document decisions in [[Architectural Decision Records (ADR)]] so rules are visible and justified

**Remedial measures:**
- Refactoring targeted at specific structural violations
- Redesign of eroded subsystems
- Documentation updates to reflect what the architecture has legitimately evolved into

## Related

- [[Technical Debt]] — debt is the primary driver of erosion
- [[Architectural Decision Records (ADR)]] — documentation prevents knowledge vaporization
- [[Quality Attributes]] — erosion degrades quality attributes over time
- [[Software Architecture Overview]] — erosion is listed as a core risk in architecture management
