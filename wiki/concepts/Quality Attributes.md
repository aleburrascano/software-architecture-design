---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/Software architecture 1.md
  - raw/articles/Software design.md
tags:
  - architecture
  - quality
  - non-functional-requirements
---

# Quality Attributes

The measurable, testable properties of a system that indicate how well it satisfies the needs of its stakeholders beyond basic functional correctness — also called **non-functional requirements (NFRs)**, **architectural characteristics**, or informally the **"-ilities"**.

## Problem / Why It Matters

A system that functions correctly but responds in 30 seconds, goes down for hours a week, or cannot be safely modified is not a good system. Quality attributes capture what "good" means beyond "does it work." Because they often pull in opposite directions — improving security typically degrades performance; improving availability typically increases cost — they are the primary driver of architectural trade-offs.

> "Everything is a trade-off" — the first law of software architecture. Quality attributes are what you are trading off.

## Categories

ISO/IEC 25010:2011 divides quality attributes into:

- **Execution qualities** — observable at runtime: performance, reliability, availability, security, usability, fault-tolerance
- **Evolution qualities** — embedded in system structure: maintainability, testability, extensibility, portability, scalability

### Key Attributes

| Attribute | Definition |
|---|---|
| **Scalability** | Ability to handle growing load — horizontally (add nodes) or vertically (upgrade hardware). Marc Brooker: "a system is scalable in the range where marginal cost of additional workload is nearly constant." |
| **Availability** | The proportion of time the system is operational and accessible. Often expressed as "nines" (99.9%, 99.99%). Requires redundancy, failover, and health monitoring. |
| **Reliability** | The system performs its required function correctly over time under stated conditions. Related to but distinct from availability — a system can be available but unreliable (returns wrong results). |
| **Performance** | Response time, throughput, and resource consumption under expected and peak load. |
| **Maintainability** | How easily the system can be modified — bug fixes, functional changes, new features. High maintainability depends on modularity, readability, and low coupling. |
| **Testability** | The degree to which software supports testing in isolation. High testability requires [[Coupling and Cohesion|loose coupling]], clear interfaces, and [[Dependency Injection]]. |
| **Security** | Resistance to unauthorized access, data breaches, and malicious acts. Encompasses confidentiality, integrity, and availability (CIA). |
| **Extensibility** | New capabilities can be added without major changes to the existing architecture. Enabled by the [[Open-Closed Principle]] and well-designed abstractions. |
| **Fault-tolerance** | Ability to continue operating correctly when components fail. Requires redundancy, circuit breakers, graceful degradation. |
| **Deployability** | Ease of releasing changes to production. Affected by modularity, testing coverage, and CI/CD pipeline design. |
| **Portability** | The system can operate across different environments and platforms. |
| **Interoperability** | The system can work with other systems through standard interfaces or protocols. |

## Role in Architecture

Non-functional requirements are detailed in the system *architecture*, whereas functional requirements are detailed in system *design*. This is the key distinction: NFRs represent architecturally significant requirements that determine system structure.

The architect's primary responsibility is to match architectural characteristics to business requirements:

- High customer satisfaction → availability, fault-tolerance, security, testability, performance
- Mergers and acquisitions → extensibility, scalability, adaptability, interoperability
- Tight budget/time → feasibility, simplicity
- Fast time-to-market → maintainability, testability, deployability

Quality attributes are also used in architecture evaluation methods such as **ATAM** (Architecture Tradeoff Analysis Method), which analyzes how architectural decisions affect quality attributes.

## Fitness Functions

As architectures grow complex, quality attributes can be monitored with **fitness functions** — automated checks (performance benchmarks, coupling metrics, security scans) that continuously verify the architecture maintains desired quality levels. This enables proactive detection of [[Architecture Erosion]].

## Related

- [[Software Architecture Overview]] — quality attributes are the primary driver of architectural decisions
- [[Architectural Decision Records (ADR)]] — each ADR documents which quality attributes a decision serves
- [[Technical Debt]] — debt is the erosion of quality attributes over time
- [[Architecture Erosion]] — what happens when quality attributes are left unmonitored
- [[Coupling and Cohesion]] — fundamental contributors to maintainability and testability
