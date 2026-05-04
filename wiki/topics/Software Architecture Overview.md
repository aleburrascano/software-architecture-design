---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/Software Architecture Guide.md"
  - "raw/Software architecture 1.md"
  - "raw/What is Software Architecture A Comprehensive Guide.md"
  - "raw/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
tags:
  - architecture
  - topic
---

# Software Architecture Overview

What software architecture is, why it matters, and the decisions it encompasses.

## Definition

Software architecture is not a single agreed-upon concept. Multiple valid perspectives exist:

**Structural view (ISO/IEC/IEEE 42010:2011):**
> The set of structures needed to reason about a software system — comprising software elements, relations among them, and properties of both elements and relations.

**Shared-understanding view (Ralph Johnson, via Martin Fowler):**
> The shared understanding that expert developers have of the system design. "Architecture is about the important stuff. Whatever that is."

**Decision-regret view (also Ralph Johnson):**
> Not "decisions made early" but "decisions you wish you could get right early" — emphasizing the cost of reversing architectural choices.

**Two fundamental laws (Fundamentals of Software Architecture, O'Reilly 2020):**
1. Everything is a trade-off.
2. "Why" is more important than "how."

**Practical synthesis:**
Software architecture is the fundamental organization of a software system — its major components, their relationships, and the constraints on them — providing a blueprint that guides development and shapes long-term quality attributes. It is the set of decisions that are **costly to change once made**.

## Core Concerns

Architecture addresses decisions that span the whole system and are hard to reverse:

- **Structural decomposition** — how the system is divided into components or services
- **Component interaction** — how components communicate (synchronous calls, events, shared data)
- **Technology stack** — languages, frameworks, databases, infrastructure
- **Data architecture** — how data is stored, accessed, and moved
- **Non-functional requirements** — performance, scalability, security, maintainability, reliability
- **Deployment model** — where components run and how they are scaled
- **Integration with external systems** — APIs, messaging, protocols

## Architectural Styles

Architectural styles define the coarsest-grained organizational structure. Each makes different trade-offs.

| Style | Core Idea | Best For |
|-------|-----------|----------|
| [[Layered Architecture]] | Horizontal layers (Presentation / Logic / Data) with downward-only dependencies | Enterprise apps with clear concern separation; monolithic deployment |
| [[Microservices Architecture]] | Independently deployable services, one per domain, own database | Large teams; high scaling and deployment autonomy needs |
| [[Event-Driven Architecture]] | Components communicate via events through a broker; producers and consumers are decoupled | Real-time systems; IoT; loosely coupled cross-domain notifications |
| [[Service-Oriented Architecture]] | Business services + Enterprise Service Bus | Large enterprise integration across heterogeneous legacy systems |
| Client-Server | Clients request; servers respond | Most web and mobile applications (foundational pattern) |
| Peer-to-Peer | Nodes share resources directly without a central server | File sharing, streaming, decentralized systems |
| Serverless | Functions run on-demand on cloud infrastructure; no server management | Event-driven workloads, bursty or unpredictable traffic, minimal ops overhead |

## Architectural Patterns

Architectural patterns explain *how* to implement a style in greater tactical detail.

| Pattern | Parent Style | Core Idea |
|---------|-------------|-----------|
| [[MVC Pattern]] | Layered/Structural | Separate Model (data), View (rendering), Controller (input handling) |
| [[CQRS]] | Any | Separate read and write models for independent optimization |
| [[Event Sourcing]] | Event-Driven | Store events (not state); derive state by replay |
| [[Repository Pattern]] | Layered/DDD | Abstract data access behind a collection-like interface |
| [[Domain-Driven Design]] | Layered | Center code on a rich domain model; use Ubiquitous Language and Bounded Contexts |
| [[Clean Architecture]] | Layered | Strict inward-only dependency rule; domain at center |
| [[Hexagonal Architecture]] | Layered | Ports and Adapters isolate domain from infrastructure |

## Quality Attributes

Architecture's primary function is to deliver the required quality attributes — the "ilities":

| Attribute | What it means |
|-----------|--------------|
| **Scalability** | Ability to handle growing load (horizontal: add nodes; vertical: upgrade hardware) |
| **Maintainability** | Ease of modifying, fixing, and extending the system over time |
| **Reliability** | System performs its function correctly over time under expected conditions |
| **Performance** | Response time, throughput, resource consumption under load |
| **Security** | Resistance to unauthorized access and malicious acts |
| **Testability** | Ease of exercising system behavior in isolation to verify correctness |
| **Deployability** | Ease of releasing changes to production |
| **Fault-tolerance** | Ability to continue operating when components fail |
| **Extensibility** | Ability to add new capabilities without major architectural changes |

Poor architecture generates **cruft** — code that impedes developer understanding — leading to slower delivery and more defects. High internal quality pays off in weeks, not months (Martin Fowler).

## Architecture vs. Design

Architecture and design exist on a continuum. The practical distinction is the **Locality Criterion**: a design decision is *architectural* (non-local) if a system satisfying it could be expanded into one that does not. Architecture addresses system-wide constraints; design addresses component-level implementation.

Architecture is design, but not all design is architectural. The architect draws the line based on what has systemic, hard-to-reverse impact.

## The Architect's Role

A software architect:
- **Matches architectural characteristics** (non-functional requirements) to business requirements.
- **Makes and documents architectural decisions** (often via Architecture Decision Records — ADRs).
- Acts as "keeper of the vision" — ensuring additions stay aligned with the architecture (conceptual integrity, Fred Brooks).
- **Balances trade-offs** — every architectural choice is a trade-off between competing quality attributes.
- Collaborates with development teams; does not operate in isolation.
- Monitors for **architecture erosion** — the gradual drift between intended and implemented architecture.

Architecture activities are iterative and ongoing throughout the SDLC:
1. **Analysis** — understand requirements (functional and non-functional)
2. **Synthesis/Design** — create the architecture
3. **Evaluation** — assess how well the design satisfies requirements (ATAM, TARA)
4. **Evolution** — maintain and adapt as requirements change

## Architecture Erosion

Software architecture erodes when the implemented structure diverges from the intended design over time. Causes: architectural violations, technical debt accumulation, knowledge vaporization. Preventative measures: automated conformance checks, code reviews, fitness functions. The Mozilla browser rewrite is a canonical case study in the cost of erosion.

## Conway's Law

Organizations which design systems are constrained to produce designs which are copies of the communication structures of those organizations. This means team structure should inform (and be informed by) architectural decisions — especially in microservices contexts where each team owns a service.

## Related

- [[Architectural Styles Comparison]] — side-by-side analysis of major styles
- [[Layered Architecture]] — foundational style
- [[Microservices Architecture]] — dominant modern distributed style
- [[Event-Driven Architecture]] — dominant messaging style
- [[Domain-Driven Design]] — dominant approach for complex domain modeling
- [[Clean Architecture]] — formal codification of layered principles
