---
type: concept
created: '2026-05-03'
updated: 2026-05-03
sources:
  - 'https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig'
  - 'https://martinfowler.com/bliki/StranglerFigApplication.html'
tags:
  - migration
  - modernization
  - patterns
  - legacy-systems
---

# Strangler Fig Pattern

A migration approach that incrementally replaces a legacy system by routing requests through a facade that directs traffic to either the legacy system or new replacement components, until the legacy system is fully decommissioned.

Named after the strangler fig tree, which germinates in a host tree's canopy, grows roots down to the soil, and gradually surrounds and replaces the host — all while the host continues to function. Observed and named by **Martin Fowler** in Queensland, Australia (2001).

## Problem

Replacing an entire legacy system in a single "big bang" rewrite is extremely risky and expensive. Legacy systems accumulate decades of business logic that is poorly documented, and complete rewrites typically fail because:
- They take too long; users need new features during transition.
- Undocumented behaviors are hard to replicate.
- Organizations face a modernization impasse between endless maintenance of brittle systems and costly failed rewrites.

Running two versions in parallel forces clients to track which version has which features, adding coordination overhead with every migration step.

## Solution

1. **Introduce a façade (proxy)** between clients and the legacy system. Initially it routes all requests to legacy.
2. **Build new components** alongside the legacy system. Each iteration implements more functionality in the new system.
3. **Shift traffic** incrementally — the façade routes specific features to new components as they are ready.
4. **Decommission the legacy** once all functionality has migrated. The façade routes everything to the new system.
5. **Remove the façade** (or retain it as an adapter for legacy clients while the core evolves for newer clients).

This makes progress visible, reduces risk at each step, and allows the team to learn from small replacements before tackling harder ones.

### The Four High-Level Activities (Fowler)

1. **Clarify outcomes** — establish alignment on desired business goals across stakeholder groups.
2. **Decompose the system** — identify "seams" (separation points) to break the legacy system into manageable replacement pieces.
3. **Deliver incrementally** — replace small components individually, reducing risk and enabling earlier value realization.
4. **Enable organizational change** — shift development practices and structure to prevent repeating past mistakes.

The pattern requires building **transitional architecture** — temporary integration code allowing old and new systems to operate together during the migration period.

## Key Components

- **Façade / proxy** — intercepts all requests and routes based on which features have been migrated. Must not become a bottleneck or single point of failure.
- **New system components** — built incrementally; can use a modern technology stack.
- **Routing logic** — updated as migration progresses; routing rules must stay synchronized with actual migration state.

## When to Use

- Gradually migrating a complex back-end to a new architecture.
- The original system can operate in parallel for an extended period.
- You want to reduce migration risk by delivering value incrementally.
- Users cannot wait for complete system replacement.
- System behavior documentation is incomplete or unclear.

This pattern is **not** suitable when:
- Requests to the back-end cannot be intercepted (no seam to insert a façade).
- The legacy system is small enough to replace entirely.
- You need the legacy system decommissioned quickly.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Reduces big-bang migration risk | Two systems must operate concurrently, increasing cost |
| Delivers value incrementally | Façade can become a bottleneck or new legacy |
| Clients experience no disruption | Data synchronization between old and new systems is complex |
| Reversible — can roll back individual features | Requires organizational discipline to complete the migration |
| Team learns from small replacements before tackling harder ones | Transitional architecture adds maintenance burden |

## Real-World Usage

- Mainframe system modernization: introduce seams to decompose monolithic processing into replaceable components.
- Extracting individual microservices from a monolith while the monolith serves remaining traffic.
- E-commerce platforms migrating from legacy platforms (COBOL, Oracle Forms) to modern stacks.

## Related

- [[Anti-Corruption Layer Pattern]] — often used alongside the façade to translate between old and new domain models
- [[Microservices Architecture]] — common target architecture for strangler fig migrations
- [[Modular Monolith]] — an intermediate destination before full microservices
- [[Hexagonal Architecture]] — new system components typically follow ports-and-adapters design
