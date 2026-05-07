---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software architecture 1.md
  - raw/articles/Software design.md
tags:
  - architecture
  - design
  - fundamentals
---

# Software Design vs Software Architecture

Software architecture and software design exist on a continuum — architecture *is* design — but they operate at different levels of abstraction and have different scopes of concern.

## The Common Confusion

The terms are sometimes used interchangeably. Both involve making decisions about how software will work. The distinction matters because architectural decisions are **costly to reverse** and have **system-wide impact**, while design decisions are more local and more easily changed.

## Software Design

**Software design** is the process of conceptualizing how a software system will work before it is implemented — and the result of that process. It spans multiple levels of abstraction:

- **High-level**: overall structure, major components, how they interact (overlaps with architecture)
- **Low-level**: individual classes, functions, algorithms, data structures

At lower levels, the design process is often informal — the only artifact may be the code itself. Donald Knuth noted that implementation constantly reveals unanticipated questions, making pure pre-implementation design futile for complex systems.

Software design activities include: requirements analysis, modeling (UML, flowcharts, pseudocode), component design, interface design, and algorithm selection.

## Software Architecture

**Software architecture** is the set of structures needed to reason about a software system — the elements, their relationships, and the properties of both. It focuses specifically on:

- Decisions that have high systemic impact
- Decisions that are hard or costly to change once made
- Quality attributes (non-functional requirements) — the "-ilities"
- System-wide structural choices (styles, patterns)

Software architecture is about the **fundamental structural choices** — choosing between microservices and a monolith, choosing an event-driven vs. request-response communication model, choosing how data is owned across services. These choices shape the entire development effort and constrain all subsequent design decisions.

## The Locality Criterion

The clearest formal distinction comes from the **Intension/Locality Hypothesis** (Eden and Kazman): a design decision is *architectural* (non-local) if a program satisfying it could be expanded into a program that does not. For example:

- Client-server is **architectural**: a system built on client-server could be expanded with peer-to-peer nodes, making it no longer purely client-server.
- Choosing a sorting algorithm is **non-architectural** (design): the choice is local to one function and doesn't constrain the system structure.

## A Continuum, Not a Binary

In practice, the architect draws the line contextually. There are no universal rules. The Wikipedia source on software architecture states:

> "Architecture is design but not all design is architectural. The architect is the one who draws the line between software architecture (architectural design) and detailed design (non-architectural design)."

The practical guideline: a decision is architectural if it:
1. Is hard to change once made
2. Affects multiple parts of the system
3. Has significant impact on quality attributes
4. Must be "right the first time" to avoid costly rework

## Application Design vs. Architecture Design

Another useful distinction from the Wikipedia software architecture article:

- **Application design** focuses on processes and data that realize required functionality (services offered by the system)
- **Architecture design** focuses on the infrastructure within which that functionality operates — ensuring quality attributes are met

Architecture is the container; application design fills it.

## Related

- [[Software Architecture Overview]] — the architecture perspective
- [[Quality Attributes]] — the primary concern of architecture (vs. functional design)
- [[Abstraction]] — architecture operates at higher abstraction levels than design
- [[Separation of Concerns]] — applied at both architectural and design levels
- [[Architectural Decision Records (ADR)]] — record architectural decisions specifically
