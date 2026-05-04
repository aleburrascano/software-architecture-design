---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: []
tags:
  - devops
  - deployment
  - ci-cd
  - risk-reduction
  - feature-flags
---

# Canary Release

A deployment strategy where a new version is gradually rolled out to a small subset of users or servers first — monitoring for errors — before progressively expanding to the full user base, reducing the blast radius of defective releases.

## Problem

Deploying a new version to all users at once exposes the entire user base to any bugs or performance regressions in the new code. Even with thorough testing, production behaviour can differ from pre-production. [[Blue-Green Deployment]] provides instant rollback but switches 100% of traffic atomically, giving no opportunity to validate behaviour at scale before full exposure.

## Solution / Explanation

The term comes from the "canary in a coal mine" metaphor — a small, early warning signal. A small percentage of traffic is routed to the new version first. Metrics (error rate, latency, business KPIs) are monitored. If the new version behaves well, the percentage is incrementally increased. If it misbehaves, traffic is routed back and the canary is halted.

```
Users
  │
  ▼
[Load Balancer / Service Mesh]
  │
  ├──▶ [v1.0]  ←── 95% of traffic
  └──▶ [v2.0]  ←── 5% of traffic (canary)
```

Then: 5% → 10% → 25% → 50% → 100%.

### Gradual Traffic Shifting

Traffic can be shifted by:
- **Instance percentage** — deploy new version to N of M servers.
- **Load balancer weight** — e.g., AWS ALB weighted target groups, nginx `weight` directive.
- **Service mesh rules** — Istio/Linkerd virtual service traffic policies.
- **Feature flags** — route specific user cohorts to new version (overlaps with A/B testing).

### Automated Canary Analysis

Modern CD platforms (Argo Rollouts, Spinnaker, Flagger) automate canary analysis:
1. Deploy canary to N% of traffic.
2. Compare metrics (error rate, p99 latency) of canary vs. baseline.
3. If metrics are within threshold: automatically promote to next percentage tier.
4. If metrics degrade: automatically roll back to baseline.

This is called **progressive delivery**.

### Relationship to Feature Flags

Feature flags can implement canary-like behaviour at the application level (route users based on user ID, geography, cohort) without infrastructure-level traffic splitting. They are complementary:
- Infrastructure canary: routes at the network/load balancer level.
- Feature flag canary: routes at the application/user level with more granular targeting.

## Key Components

- **Canary subset** — the small fraction of traffic or users receiving the new version.
- **Baseline** — the stable existing version receiving the majority of traffic.
- **Traffic routing** — load balancer, service mesh, or feature flag mechanism.
- **Metrics comparison** — error rate, latency, business KPIs monitored during rollout.
- **Rollback threshold** — the metric degradation that triggers automatic or manual rollback.
- **Progressive delivery** — automated incremental promotion based on metric analysis.

## When to Use

- Deploying to large user bases where 100% exposure risk is high.
- Changes to performance-sensitive or business-critical paths.
- When production behaviour cannot be fully validated in pre-production.
- Teams practising continuous deployment to production multiple times per day.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Limits blast radius of defective releases | Requires traffic-splitting infrastructure |
| Real production validation before full rollout | Running two versions simultaneously adds complexity |
| Gradual rollout reduces user impact | Longer deployment window vs. blue-green |
| Automated rollback on metric degradation | Observability and alerting must be mature |

## Comparison

| | [[Blue-Green Deployment]] | Canary Release | Rolling |
|-|--------------------------|---------------|---------|
| Traffic shift | Instant (0% → 100%) | Gradual (5% → 100%) | Incremental |
| Infrastructure cost | High (2x) | Medium | Low |
| Rollback speed | Instant | Fast | Medium |
| Blast radius | Full at cutover | Limited to canary % | Increasing |
| Validation opportunity | Pre-switch only | Ongoing during rollout | Limited |

## Related

- [[Blue-Green Deployment]] — atomic traffic switch alternative
- [[Continuous Integration and Delivery]] — canary is a production deployment strategy
- [[Infrastructure as Code]] — provisions the routing infrastructure
- [[Twelve-Factor App]] — Factor V (immutable releases) supports canary rollouts
- [[Microservices Architecture]] — canary can be applied per-service independently
