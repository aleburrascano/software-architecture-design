---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: []
tags:
  - devops
  - ci-cd
  - deployment
  - continuous-delivery
  - pipeline
---

# Continuous Integration and Delivery

A set of practices and pipeline automation that keeps a codebase releasable at all times — by integrating code changes frequently, validating them automatically, and deploying to production with confidence and speed.

## Problem

Large batch releases are risky: many changes accumulate, testing becomes expensive, merge conflicts proliferate, and defects are hard to attribute to a specific change. Manual deployment processes are slow, error-prone, and create a bottleneck between development and production.

## Solution / Explanation

CI/CD is three related but distinct practices:

### Continuous Integration (CI)

Developers commit code to a shared main branch **frequently** (multiple times per day). Each commit triggers an automated pipeline that:
1. Builds the application.
2. Runs the full test suite (unit, integration).
3. Reports pass/fail within minutes.

The goal is to detect integration problems immediately, not days later. Requires small, frequent commits and tests that run fast.

### Continuous Delivery (CD)

Every commit that passes CI is **releasable** at any time. The pipeline automatically deploys to staging and pre-production environments, running further validation (integration tests, smoke tests). Deploying to production is a deliberate business decision, not a technical obstacle. The pipeline automates the *mechanics* of production deployment; a human approves *when* to release.

### Continuous Deployment

Goes one step further than Continuous Delivery: **every passing commit is automatically deployed to production**, with no manual approval gate. Requires high confidence in automated test coverage and strong monitoring.

---

### CI/CD Pipeline Stages

A typical pipeline:

```
Code Commit
    ↓
1. Build — compile, bundle, containerise
    ↓
2. Unit & Integration Tests — fast feedback (<10 min)
    ↓
3. Static Analysis / Linting — code quality gates
    ↓
4. Security Scanning — dependency CVE scan, SAST
    ↓
5. Build Artefact — Docker image, binary, package
    ↓
6. Deploy to Staging — automated deployment
    ↓
7. Acceptance / E2E Tests — contract tests, smoke tests
    ↓
8. Deploy to Production — manual approval (CD) or automatic (Continuous Deployment)
    ↓
9. Monitoring & Alerting — verify deployment health
```

---

### Trunk-Based Development

The branching strategy that best supports CI/CD:
- All developers commit to a single shared branch (usually `main`).
- Feature branches are short-lived (hours to 1-2 days), not weeks.
- **Feature flags** hide incomplete features in production rather than keeping them in long-lived branches.

Contrast with **GitFlow** (long-lived release branches): creates large merge conflicts, delays integration, and is incompatible with continuous deployment.

---

### Deployment Strategies

Different strategies manage the risk of deploying to production:

| Strategy | Description | Zero downtime | Rollback speed |
|----------|-------------|--------------|---------------|
| **Recreate** | Stop old, start new | No | Slow |
| **Rolling** | Replace instances incrementally | Yes | Medium |
| **[[Blue-Green Deployment\|Blue-Green]]** | Two identical envs; switch traffic atomically | Yes | Instant |
| **[[Canary Release\|Canary]]** | Gradually shift traffic to new version | Yes | Fast |
| **A/B Testing** | Route specific users to new version | Yes | Fast |

See [[Blue-Green Deployment]] and [[Canary Release]] for details.

---

### Key Metrics (DORA)

The **DORA metrics** (DevOps Research and Assessment) measure CI/CD effectiveness:
- **Deployment Frequency** — how often you deploy to production.
- **Lead Time for Changes** — time from commit to production.
- **Change Failure Rate** — percentage of deployments that cause a production failure.
- **Time to Restore Service** — how long to recover from a failure.

Elite teams deploy multiple times per day with lead times under an hour.

---

## Key Components

- **Pipeline** — the automated sequence of stages from commit to deployment.
- **Artefact** — the immutable build output (Docker image, JAR, binary) deployed at each stage.
- **Feature Flag** — a runtime toggle hiding incomplete features, enabling trunk-based development.
- **Smoke Test** — a quick post-deployment sanity check confirming the new version is alive.
- **Rollback** — the ability to return to the previous version quickly.

## When to Use

- Any software team delivering features regularly to production.
- Microservices environments where many services deploy independently.
- Teams targeting high deployment frequency and low change failure rate.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Fast feedback on broken builds | Pipeline maintenance is ongoing work |
| Reduces release risk via small batches | Requires high test coverage for confidence |
| Reproducible, auditable deployments | Feature flags add complexity |
| Enables rolling back individual changes | Automated deployment requires disciplined monitoring |

## Related

- [[Twelve-Factor App]] — Factor V (Build/Release/Run) defines the pipeline structure
- [[Blue-Green Deployment]] — zero-downtime deployment strategy
- [[Canary Release]] — gradual traffic-shifting deployment strategy
- [[Infrastructure as Code]] — provisions the environments the pipeline deploys to
- [[Microservices Architecture]] — each service has its own independent pipeline
- [[Feature Flags]] — enable trunk-based development in CI/CD
