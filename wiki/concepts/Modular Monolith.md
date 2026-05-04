---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/modular-monolith/'
tags:
  - architecture
  - monolith
  - microservices
  - design
---
# Modular Monolith

An architectural style that combines the deployment simplicity of a monolith with the code organization discipline of microservices: a single deployable unit structured as a set of independent, loosely coupled modules with explicit boundaries and well-defined interfaces.

## Problem

Teams face a false binary: "monolith or microservices?" Traditional monoliths devolve into unmaintainable "big balls of mud" where every module knows about every other. Microservices solve modularity but introduce distributed system complexity (networking, eventual consistency, distributed tracing, operational overhead) that many teams are not ready for — and do not need.

## Solution / Explanation

A **Modular Monolith** applies the same modularity principles as microservices but within a single process:

- Each **module** owns a cohesive set of business functionality ([[Bounded Context]] in DDD terms).
- Modules communicate through **defined interfaces** (internal APIs, events, service contracts) rather than direct class-to-class calls.
- Module internals are **private** — other modules cannot access internal classes, repositories, or databases directly.
- Communication between modules is **asynchronous-first** (events or async method calls) to avoid tight coupling.

### Modular Monolith vs. Traditional Monolith

| | Traditional Monolith | Modular Monolith |
|---|---|---|
| Code organization | Layers (controllers, services, repos) | Modules (each with own layers) |
| Coupling | High (any class can call any class) | Low (enforced module boundaries) |
| Data access | Shared DB tables | Module owns its schema/tables |
| Evolution | Becomes a ball of mud | Stays maintainable |

### Modular Monolith vs. Microservices

| | Modular Monolith | Microservices |
|---|---|---|
| Deployment | Single unit | Multiple independently deployed services |
| Scaling | Whole application scales | Per-service scaling |
| Communication | In-process (fast) | Network (latency, failures) |
| Data consistency | Easier (same DB transaction possible) | Requires eventual consistency |
| Operational complexity | Low | High |
| Independence | Compile-time enforcement | Runtime enforcement |

### "MonolithFirst" Strategy

Martin Fowler's "MonolithFirst" approach: **start with a modular monolith**, understand domain boundaries, then extract microservices only when there is a genuine operational need (independent scaling, team autonomy, polyglot technology). This avoids the cost of prematurely distributed systems.

### Tools for Boundary Enforcement

Module boundaries are architectural rules that need enforcement. Approaches:
- **Access modifiers** (`internal` in C#, package-private in Java) — compiler-enforced.
- **Architecture testing libraries** (NetArchTest, ArchUnit) — test boundaries in CI.
- **Module systems** (Java JPMS, .NET Assemblies) — runtime enforcement.

## When to Use

- New projects where domain boundaries are not yet clear.
- Teams lacking distributed systems operational maturity.
- Systems with tightly integrated domains that don't need independent scaling.
- As an intermediate step before extracting specific services as true microservices.

Not suitable when:
- Different modules have genuinely different scaling requirements.
- Teams need strict deployment independence.
- Technology heterogeneity (polyglot) is a hard requirement.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Simpler deployment and operations | All modules scale together |
| In-process communication (fast, simple) | Boundary violations creep in without enforcement tooling |
| Easier local development and debugging | Single deployment unit — one bad deploy affects all |
| Clear path to microservices extraction | Shared database can still create coupling if not carefully designed |

## Related

- [[Microservices Architecture]] — the distributed alternative; extract modules when genuinely needed
- [[Bounded Context]] — each module maps to a bounded context in DDD
- [[Layered Architecture]] — replaced by feature-oriented module organization
- [[Vertical Slice Architecture]] — a complementary feature-organizing principle within modules
- [[Strangler Fig Pattern]] — used to extract modules into microservices when the time comes
