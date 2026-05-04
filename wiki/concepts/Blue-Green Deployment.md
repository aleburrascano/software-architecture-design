---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: []
tags:
  - devops
  - deployment
  - zero-downtime
  - ci-cd
---

# Blue-Green Deployment

A deployment strategy that maintains two identical production environments — blue (current live) and green (new version) — and switches traffic atomically between them, achieving zero-downtime deployments and instant rollback.

## Problem

Deploying a new version of an application in-place takes the service down (or degrades it) during the update window. If the new version has a critical bug, rolling back requires redeploying the old version — slow and risky. Traditional deployment is the highest-risk moment in the delivery lifecycle.

## Solution / Explanation

Two environments (blue and green) are maintained simultaneously. Only one is live at a time:

```
Users
  │
  ▼
[Load Balancer / Router]
  │
  ├──▶ [Blue: v1.0]  ◀── currently live
  └──▶ [Green: v2.0] ◀── new version being deployed and tested
```

**Deployment process:**
1. Green environment runs the previous version (or is idle).
2. Deploy the new version to Green.
3. Run smoke tests and validation against Green (not yet receiving user traffic).
4. Switch the router/load balancer to point all traffic to Green.
5. Blue is now idle — the previous version is still running.
6. If Green has issues: switch traffic back to Blue instantly (rollback).
7. Once Green is confirmed stable: decommission or repurpose Blue for the next deployment.

### Traffic Switching Mechanisms

- **DNS cutover** — update DNS records to point to the Green IP. Simple but DNS TTL creates a delay.
- **Load balancer rule** — update routing weights (0%/100%). Instant; no DNS propagation.
- **Feature flag / proxy** — route at the application level (e.g., nginx `upstream` swap, AWS ALB target group swap).

### Database Considerations

The trickiest part of blue-green is database schema changes. If the migration is backward-incompatible (e.g., dropping a column), you cannot run Blue and Green simultaneously against the same database.

The **expand-contract pattern**:
1. **Expand** — deploy a schema migration that is backward compatible (add a new column; keep the old one).
2. **Run Blue and Green** — both versions work against the new schema.
3. **Contract** — after Blue is decommissioned, drop the old column.

> [!question] Unverified
> Expand-contract is widely recommended but requires careful coordination in teams where schema migrations and code deployments are managed separately.

## Key Components

- **Blue environment** — the currently live production environment.
- **Green environment** — the newly deployed version, tested before receiving traffic.
- **Router / Load Balancer** — the traffic-switching component.
- **Smoke tests** — validation run against Green before the traffic switch.
- **Database migration strategy** — expand-contract or versioned schemas for backward compatibility.

## When to Use

- Applications requiring zero-downtime deployments.
- Systems where instant rollback is a business requirement.
- Any [[Continuous Integration and Delivery]] pipeline targeting high deployment confidence.
- Services with infrequent but high-risk releases.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Zero downtime | Double the infrastructure cost (two environments running) |
| Instant rollback (seconds) | Database migration complexity |
| Full production validation before go-live | Requires robust routing/load balancer infrastructure |
| Clear, auditable deployment steps | More complex than simple rolling deployments |

## Comparison with Other Strategies

| Strategy | Downtime | Rollback speed | Infrastructure cost |
|----------|----------|---------------|-------------------|
| Recreate | Yes | Slow (redeploy) | Lowest |
| Rolling | No | Medium | Low |
| **Blue-Green** | **No** | **Instant** | **High (2x)** |
| [[Canary Release\|Canary]] | No | Fast | Medium |

## Related

- [[Canary Release]] — gradual traffic shifting; lower cost but slower rollback
- [[Continuous Integration and Delivery]] — blue-green is a pipeline deployment strategy
- [[Twelve-Factor App]] — Factor IX (Disposability) and Factor V (immutable releases) support blue-green
- [[Infrastructure as Code]] — provisions and manages both environments
