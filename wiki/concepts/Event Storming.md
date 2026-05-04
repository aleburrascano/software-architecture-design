---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/modeling/event-storming/'
tags:
  - ddd
  - modeling
  - workshop
  - domain-driven-design
  - discovery
---
# Event Storming

A collaborative, workshop-based modeling technique that brings together domain experts and developers to rapidly explore and visualize a complex business domain using sticky notes on a large surface, with domain events as the primary building block.

## Problem

Designing software for complex business domains requires deep domain knowledge that is typically spread across many people: product owners, subject-matter experts, developers, UX designers. Traditional methods (requirements documents, UML diagrams) fail to facilitate real-time knowledge sharing and leave large gaps between what experts know and what developers build.

## Solution / Explanation

Event Storming was created by Alberto Brandolini. Participants collaborate in a room (or remote equivalent) with a very long horizontal surface (or virtual canvas). They use **orange sticky notes** to represent **Domain Events** — things that happened — and build a shared timeline of the domain from left (past) to right (future).

Additional sticky note colors represent other building blocks:

| Color | Building Block | Description |
|---|---|---|
| Orange | **Domain Event** | Something that happened (`OrderPlaced`, `PaymentFailed`) |
| Blue | **Command** | What triggered the event (`PlaceOrder`, `ProcessPayment`) |
| Yellow | **Aggregate** | Domain object that handles the command and raises the event |
| Purple | **Policy/Reaction** | "Whenever X, then Y" — automated reactions to events |
| Pink | **External System** | Third-party systems or bounded contexts |
| Green | **Read Model / View** | Information needed to support a decision |
| Red | **Hot Spot** | Areas of uncertainty, conflict, or questions to resolve |

### Three Workshop Formats

**1. Big Picture Event Storming**
Maps the entire business domain at high level. Goal: shared understanding of the end-to-end flow, discover bounded contexts, identify pain points, and align stakeholders. Typically involves the whole team/domain.

**2. Process Level Event Storming**
Focuses on a specific workflow within a bounded context. Adds commands, policies, and read models. Goal: design the interaction model for a specific process.

**3. Design Level Event Storming**
Technical deep-dive for architects and developers. Adds aggregates, services, and UI screens. Goal: produce a design that can be directly translated into code.

### Typical Big Picture Session Flow

1. Participants independently add domain events (orange stickies) to the timeline.
2. Duplicates are merged; conflicts become **Hot Spots** (red stickies).
3. Events are sorted into a causal timeline.
4. Commands (what causes events) and policies (what events trigger) are added.
5. Boundaries emerge around clusters of related events — candidate **Bounded Contexts**.

## When to Use

- Kicking off design of a new system.
- Decomposing a monolith into microservices (events reveal natural service boundaries).
- Understanding a complex legacy system.
- Aligning cross-functional teams around a shared business understanding.
- Remote teams needing structured knowledge sharing.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Breaks down silos between business and tech | Requires facilitation skill to run well |
| Rapidly surfaces domain complexity | Needs physical or well-configured virtual space |
| Produces bounded context candidates directly | Can devolve into endless discussion without clear goals |
| Identifies unknowns (hot spots) explicitly | Output (stickies) must be digitized and preserved |

## Related

- [[Domain-Driven Design]] — the strategic design framework Event Storming serves
- [[Bounded Context]] — discovered through clusters of events in the workshop
- [[Domain Event]] — the primary building block of the workshop
- [[Aggregate]] — identified during design-level Event Storming
- [[Microservices Architecture]] — event storming reveals natural service boundaries
- [[Event Sourcing]] — close conceptual relationship; events as the source of truth
