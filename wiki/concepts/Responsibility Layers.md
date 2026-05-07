---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'SoftwareMill - Advanced Large-Scale DDD.md'
tags:
  - ddd
  - responsibility-layers
  - large-scale-ddd
  - domain-stratification
  - knowledge-level
  - evans
---
# Responsibility Layers

An advanced DDD pattern for **organizing large domains into horizontal layers of increasing abstraction**, where each layer expresses business intent at a different level — from concrete operational capabilities at the bottom to strategic decision support at the top.

## Problem

In large, complex domains (logistics, financial systems, supply chain), a flat domain model becomes tangled: entities at all levels of abstraction (vehicles, shipments, delivery policies, contracts, optimization strategies) live side by side, making it hard to understand which rules live where and why. Conway's Law suggests this leads to equally tangled teams and codebases.

## Solution / Explanation

Responsibility Layers organizes the domain into horizontal strata where **each layer only knows about the layers below it**. Higher layers express intent; lower layers express mechanism.

### Evans' Five Layers

Eric Evans originally identified these five layers for the logistics domain, but the principle generalizes:

| Layer | Purpose | Example (Logistics) | Example (E-commerce) |
|---|---|---|---|
| **Decision Support** | Analytics, optimization, recommendations | Route optimization AI | Pricing strategy engine |
| **Commitments** | Contractual obligations and promises | Delivery commitment to customer | Order confirmation SLA |
| **Policies** | Business rules and constraints | Delivery policy constraints | Return policy rules |
| **Operations** | Active business processes | Current shipment schedule | Active order fulfillment |
| **Capabilities** | Available resources and skills | Vehicle fleet, driver roster | Warehouse inventory, staff |

### Layer Rules

1. Each layer **depends only on layers below it** — no upward dependencies
2. Higher layers **express intent** ("we've committed to deliver this by Tuesday")
3. Lower layers **express mechanism** ("here are the available trucks and routes")
4. **Domain events** flow upward: a capability change (driver unavailable) propagates through operations (reschedule), policies (check SLA), commitments (update customer), and decision support (reoptimize)

### Contrast with Technical Layered Architecture

This is **not** the same as the technical Layered Architecture pattern (Presentation → Application → Domain → Infrastructure).

| Aspect | Technical Layers | Responsibility Layers |
|---|---|---|
| **Purpose** | Separate technical concerns | Separate domain abstraction levels |
| **Content** | Code/framework categories | Business concepts at different granularity |
| **Direction** | Top-down call flow | Bottom-up event flow; top-down queries |
| **Examples** | Controller → Service → Repository | Commitments → Policies → Operations |

Both can coexist: within each Responsibility Layer, you still use the technical layered architecture.

### When to Apply

**Apply when:**
- Your domain has multiple levels of abstraction (commitments vs. operations)
- Business rules at one level need to be insulated from implementation details of lower levels
- Domain is large enough that finding "where does this rule live?" is a daily question

**Skip if:**
- Domain is small/simple — this pattern adds complexity that must be earned
- Layers are not clearly distinguishable in your domain

## Trade-offs

| Benefit | Cost |
|---|---|
| Clear home for every business rule | Requires deep domain understanding to define layers correctly |
| Higher-level layers are stable; lower layers change more | Initial investment to identify correct layer boundaries |
| Reduces tangling of abstraction levels | Adds architectural ceremony |
| Enables independent evolution of commitment logic from capability logic | Teams must coordinate on layer boundaries |

## Related

- [[Domain-Driven Design]] — this is an advanced DDD pattern
- [[Knowledge Level Pattern]] — companion pattern for configurable behavior
- [[Bounded Context]] — Responsibility Layers organize within/across bounded contexts
- [[DDD Advanced Patterns]] — topic page that contextualizes this pattern
- [[Layered Architecture]] — technical layering; complementary but distinct
