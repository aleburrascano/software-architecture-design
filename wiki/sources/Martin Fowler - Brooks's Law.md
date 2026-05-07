---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Bliki- Mythical Man Month.md'
source_date: ''
source_author: Martin Fowler
tags:
  - source
  - brooks-law
  - team-scaling
  - communication
  - project-management
  - conways-law
---
# Martin Fowler - Brooks's Law

Martin Fowler's bliki entry on Fred Brooks' Mythical Man Month and the enduring principle that adding people to a late project makes it later.

## Summary

Fowler's entry distills the core insight from Brooks' Mythical Man Month: adding manpower to a late software project makes it later — a principle that has held up across decades of software engineering practice. The central mechanism is the communication path formula n(n-1)/2, which demonstrates that communication overhead grows quadratically with team size, not linearly.

A key cost of adding people mid-project is the training burden placed on existing team members. New hires must be onboarded, and the most experienced people — those best positioned to accelerate the project — bear the heaviest onboarding load. This creates a productivity dip precisely when the project can least afford it.

Fowler also covers the limits of task partitioning: not all work can be divided and parallelized. Tasks with sequential dependencies cannot be accelerated by adding more workers, and the overhead of coordination often exceeds any gains from parallelism. The "surgical team" model — a small, highly skilled core team supported by specialists — is presented as an alternative to large team scaling.

## Key Arguments

- Communication paths scale quadratically with team size via n(n-1)/2, making large teams structurally expensive to coordinate
- Training new members costs time from existing members, creating a productivity dip that compounds lateness
- Not all tasks can be divided; sequential dependencies impose a floor on parallelization benefits
- Small focused teams often outperform large ones on complex, interdependent work
- Brooks's Law has held up for decades and remains one of the most robust empirical observations in software engineering

## Concepts Covered

- [[Brooks's Law]] — primary treatment; the article is a direct exposition of this principle
- [[Conway's Law]] — team structure shapes system structure; both laws concern how human organization affects software
- [[Modularity]] — precondition for parallelizing work; only modular systems can be effectively partitioned across people

## Quality Notes

Canonical reference; Fowler's concise treatment of a classic software engineering insight. Useful as a starting point for any discussion of team scaling, communication overhead, or project management.
