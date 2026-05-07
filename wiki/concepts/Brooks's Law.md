---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Martin Fowler - Brooks''s Law.md'
tags:
  - brooks-law
  - team-scaling
  - communication-overhead
  - project-management
  - conways-law
  - mythical-man-month
  - software-engineering
---
# Brooks's Law

**"Adding manpower to a late software project makes it later."** — Fred Brooks, *The Mythical Man-Month* (1975)

## Problem

When a software project falls behind schedule, the intuitive management response is to add more developers. This almost always backfires, delaying the project further rather than accelerating it.

## Explanation

Fred Brooks identified three reasons why adding people to a late project makes it later:

### 1. Training Overhead

New team members don't hit the ground running. Existing members must stop productive work to bring them up to speed. The more complex the codebase, the longer the training period.

**Cost formula:** If onboarding one new person requires 20% of one senior developer's time for 4 weeks, adding 5 people requires 100% of a senior dev for 4 weeks — that's a 4-week productivity loss before new people are productive.

### 2. Communication Path Explosion

Communication overhead grows **quadratically** with team size:

```
Paths = n(n-1) / 2
```

| Team Size | Communication Paths |
|---|---|
| 2 | 1 |
| 5 | 10 |
| 10 | 45 |
| 20 | 190 |
| 50 | 1,225 |

Adding 5 people to a 10-person team increases communication paths from 45 to 105 — a 133% increase in coordination overhead.

### 3. Task Partitioning Limits

Not all software tasks can be parallelized. Writing a compiler's parser cannot be split across 10 developers in a way that's 10× faster. Some tasks have inherent sequential dependencies; others require so much coordination between parallel workers that the coordination cost exceeds the parallel benefit.

Brooks: "Nine women can't make a baby in one month."

## Corollaries

**The surgical team:** Brooks' preferred alternative to large teams. One highly skilled "surgeon" does the primary work; supporting roles (copilot, secretary, toolsmith) handle everything else. Small, high-skill teams communicate less and produce more.

**Brooks' Law applies to late projects specifically.** Adding people to a project **ahead of schedule** or **before complexity accumulates** is often fine. The law applies when: (1) project is already late, (2) codebase is already complex, (3) new people need significant onboarding.

## Relationship to Other Principles

**Conway's Law:** Team structure shapes system architecture. Large teams produce more complex systems with more interfaces — partly because communication paths require explicit coordination mechanisms (APIs, documentation, meetings).

**Modular architecture:** Well-modularized systems reduce the coordination overhead that makes Brooks' Law so painful. If new team members can be assigned a clearly bounded module, onboarding is faster and parallelization is more practical.

## When It Doesn't Apply

- Adding experienced people who already know the codebase
- Well-modularized systems where new work is genuinely independent
- Very early in project before complexity accumulates
- Projects with clear, independently-executable work items (e.g., adding unrelated features)

## Trade-offs

| If you add people | Risk |
|---|---|
| To a late, complex project | Makes it later (Brooks' Law) |
| To a modular, well-documented project | Often helps if work is parallelizable |
| Who are experts in the codebase | Usually helps |
| Who need onboarding | Temporarily slows down; recovery depends on complexity |

## Related

- [[Conway's Law]] — team structure shapes system structure (complementary principle)
- [[Modularity]] — modular systems resist Brooks' Law better
- [[Separation of Concerns]] — architectural basis for task parallelization
- [[Technical Debt]] — accumulated complexity amplifies Brooks' Law effects
