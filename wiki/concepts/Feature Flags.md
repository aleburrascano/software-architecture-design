---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources: []
tags:
  - deployment
  - devops
  - continuous-delivery
  - release-management
---

# Feature Flags

A technique for enabling or disabling functionality at runtime without deploying new code. A flag (also called a feature toggle or feature switch) wraps new code behind a conditional check, letting teams control who sees which features and when.

## Why They Matter for CI/CD

Feature flags decouple **deployment** from **release**. Code can be merged and deployed to production in a disabled state, then enabled for users independently of any deployment. This makes [[Continuous Integration and Delivery]] safer by reducing the scope of each release event.

## Types of Flags

| Type | Lifespan | Purpose |
|---|---|---|
| **Release toggle** | Days to weeks | Hide incomplete features; remove once feature ships |
| **Experiment toggle** | Days to weeks | A/B test or canary release to a subset of users |
| **Ops toggle** | Long-lived | Kill-switch for a risky feature; degrade gracefully under load |
| **Permission toggle** | Long-lived | Enable features per user tier or role |

## Trade-offs

**Benefits:**
- Enables trunk-based development with no long-lived feature branches
- Reduces deployment risk (instant rollback by disabling the flag)
- Enables gradual rollouts and A/B testing

**Costs:**
- Technical debt if flags are not cleaned up promptly
- Added cognitive overhead — conditional branches in the codebase
- Operational complexity — flag state must be managed and audited

Flags should be treated as temporary by default. Each release toggle should have a planned expiry date.

## Related Concepts

- [[Continuous Integration and Delivery]] — feature flags are a key enabler of trunk-based CI/CD
- [[Canary Release]] — gradual rollout of a feature, often gated by a flag
- [[Blue-Green Deployment]] — related deployment strategy; flags can complement it
- [[Technical Debt]] — stale flags accumulate as a form of technical debt
