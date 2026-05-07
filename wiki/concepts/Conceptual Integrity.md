---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software architecture 1.md
tags:
  - architecture
  - design
  - principles
---

# Conceptual Integrity

The property of a software system where its architecture represents a single, coherent overall vision — so that the system appears to have been designed by one mind, even if it was built by many.

## Problem / Why It Matters

Large systems built by teams of people tend to accumulate inconsistencies: different components make the same design decision in different ways, interfaces are inconsistent, abstractions overlap and conflict. The result is a system that is hard to understand, hard to extend, and frequently surprising. Conceptual integrity is the antidote.

## Explanation

Fred Brooks introduced the concept in *The Mythical Man-Month* (1975):

> "I will contend that conceptual integrity is the most important consideration in system design. It is better to have a system omit certain anomalous features and improvements, but to reflect one set of design ideas, than to have one that contains many good but independent and uncoordinated ideas."

A system with conceptual integrity has:
- **Consistent abstractions** — similar problems are solved in similar ways throughout the codebase
- **Coherent interfaces** — the API surface follows predictable conventions
- **Uniform style** — the system appears to have emerged from a single design philosophy
- **Clear rationale** — every part fits a recognizable architectural intent

## The Architect as "Keeper of the Vision"

Brooks argued that conceptual integrity is best achieved when one person — or a small, tightly coordinated group — holds the overall vision and guards it against inconsistent additions. The architect's role in modern terms is:

> "Keeper of the vision — making sure that additions to the system are in line with the architecture, hence preserving conceptual integrity."

This does not mean the architect makes all decisions, but that they enforce consistency in the decisions others make. [[Architectural Decision Records (ADR)]] are a tool for articulating and communicating the vision that must be preserved.

## Trade-offs

Strict conceptual integrity can slow down teams — every addition must be vetted for consistency with the whole. In fast-moving organizations this tension is real. The answer is not to abandon integrity but to make the vision explicit and accessible, so that teams can self-govern without bottleneck.

## Related

- [[Software Architecture Overview]] — conceptual integrity is listed as a core architectural characteristic
- [[Architectural Decision Records (ADR)]] — documents that communicate the vision
- [[Architecture Erosion]] — erosion is in part a loss of conceptual integrity over time
- [[Conway's Law]] — organizational structure affects how consistently a vision can be maintained
