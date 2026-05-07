---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: raw/articles/Software architecture 1.md
source_date: 2002-04-09
source_author: "[[Wikipedia]]"
tags:
  - architecture
  - source
---

# Software Architecture (Wikipedia)

Source: https://en.wikipedia.org/wiki/Software_architecture

## Summary

Wikipedia's main article on software architecture. Comprehensive reference covering the definition, scope, the style vs. pattern distinction, anti-patterns, key characteristics, historical development, architecture activities, design strategies, agile/architecture tensions, architecture erosion, and related fields. Well-cited with academic and industry references.

## Concepts Covered

- [[Software Architecture Overview]] — full definition, scope, characteristics, history
- [[Layered Architecture]] — cited as a canonical architectural style
- [[Microservices Architecture]] — cited as a canonical architectural style
- [[Event-Driven Architecture]] — cited as a canonical architectural style
- [[Clean Architecture]] — implied via architectural patterns discussion
- Serverless architecture — brief treatment as a related architecture type
- Enterprise architecture — TOGAF, Zachman Framework, layers
- Architecture activities: analysis, synthesis, evaluation, evolution
- Architecture evaluation methods: ATAM, TARA
- Architecture documentation: 4+1 view model (Kruchten)
- Architecture erosion, recovery, and reverse engineering

## Key Arguments / Claims

- **Two fundamental laws of software architecture:** (1) Everything is a trade-off; (2) "Why" is more important than "how."
- Architecture is the set of structures needed to reason about a software system — not just models but the decisions and rationale behind them.
- Architectural decisions are those that are **costly to change once implemented** — defining what is architectural.
- **Architecture style vs. pattern distinction:** Style = high-level structural organization (Layered, Microservices, EDA); Pattern = reusable solution to a recurring system-level problem (Circuit Breaker). The line can be blurry.
- Architecture exhibits: multitude of stakeholders, separation of concerns, quality-driven design (non-functional requirements), recurring styles, conceptual integrity (Fred Brooks), and cognitive constraints (Conway's Law).
- Architecture benefits: basis for behavior analysis before building, reuse of elements and decisions, early design decisions that shape lifecycle, stakeholder communication, risk management, cost reduction.
- **Four core architecture activities:** analysis, synthesis/design, evaluation, evolution — iterative throughout the SDLC.
- **Architecture erosion:** gap between intended and implemented architecture; causes include architectural violations, technical debt accumulation, knowledge vaporization. Mozilla browser is cited as a famous erosion case.
- Serverless: abstracts infrastructure management; FaaS (Function as a Service); pay-as-you-go; cold start challenges.
- Enterprise architecture: TOGAF and Zachman Framework; business layer, application layer, technology layer.
- Conceptual integrity (Fred Brooks, *The Mythical Man-Month*): architecture represents an overall vision — the architect is "keeper of the vision."
- Conway's Law: organizations produce designs that copy their communication structures.

## Quality Notes

High quality, encyclopedic depth. Excellent for definitional grounding and historical context. Citations are rigorous (IEEE standards, academic papers, O'Reilly books). The style-vs-pattern distinction is well-explained. The history section traces the field from Dijkstra (1968) and Parnas (1970s) through IEEE 1471 (2000) to ISO/IEC/IEEE 42010 (2011). Does not go deep on individual patterns — references them but doesn't explain implementation.
