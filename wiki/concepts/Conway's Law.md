---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software architecture 1.md
tags:
  - architecture
  - organization
  - sociotechnical
---

# Conway's Law

Organizations which design systems are constrained to produce designs which are copies of the communication structures of those organizations.

## Problem / Why It Matters

Software architecture does not emerge solely from technical requirements. The structure of the teams building a system — who talks to whom, where organizational boundaries lie — shapes the resulting software in ways that are often invisible and rarely deliberate. Ignoring this force leads to architectures that mirror organizational dysfunction rather than domain logic.

## Explanation

Melvin Conway introduced the observation in 1967 (published 1968). The mechanism is straightforward: components must be compatible, so their designers must communicate. Where communication is easy (within a team), integration is smooth; where communication is costly or absent (across team boundaries), technical seams appear. Those seams become the component boundaries in the final system.

Fred Brooks brought the idea to wide attention in *The Mythical Man-Month* (1975), coining the name "Conway's Law."

Empirical support exists: MIT and Harvard Business School researchers found "strong evidence" that loosely-coupled organizations produce more modular products than tightly-coupled ones. Microsoft and Tampere University of Technology studies produced similar results.

### The Inverse Conway Maneuver

Rather than accepting the law as a constraint, teams can use it deliberately:

> Design your team structure to match the architecture you want to produce.

If you want independently deployable microservices, organize autonomous teams that each own a service end-to-end. This is sometimes called the **Inverse Conway Maneuver** and is central to [[Microservices Architecture]] thinking.

### Stronger Formulations

Yourdon and Constantine (1979) stated it more assertively: the system structure becomes *isomorphic* to organizational structure. Coplien and Harrison (2004) argued that project success requires product architecture to align with organizational design.

## Examples

- A company with separate frontend, backend, and database teams tends to produce three-tier monoliths with sharp horizontal layers
- A company organized around business capabilities (checkout, payments, catalog) tends to produce services matching those capabilities
- A siloed enterprise with many hand-off points between teams tends to produce large integration layers and ESB-heavy architectures (see [[Service-Oriented Architecture]])

## Trade-offs

The law describes a force, not a verdict. The mirroring can be beneficial (team autonomy aligned with component autonomy) or harmful (organizational silos producing brittle integration seams). The key is to be deliberate — either design org structure to produce the desired architecture, or account for org structure when making architectural decisions.

## Related

- [[Microservices Architecture]] — inverse Conway maneuver is a primary driver
- [[Domain-Driven Design]] — Bounded Contexts often align with team boundaries
- [[Software Architecture Overview]] — Conway's Law is listed as a core characteristic
- [[Coupling and Cohesion]] — org-level coupling mirrors code-level coupling
