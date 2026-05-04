---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: []
tags:
  - devops
  - infrastructure
  - cloud-native
  - iac
  - terraform
---

# Infrastructure as Code

The practice of managing and provisioning computing infrastructure through machine-readable configuration files — rather than manual processes or interactive UIs — enabling version control, repeatability, and automated deployment of infrastructure.

## Problem

Manually provisioned infrastructure is undocumented, unrepeatable, and prone to **configuration drift** — where production diverges from staging, or two supposedly identical servers have different package versions, firewall rules, or configuration. Manual processes don't scale and create operational silos.

## Solution / Explanation

Infrastructure as Code (IaC) treats infrastructure the same way as application code: declaratively described in files, committed to version control, reviewed via pull requests, and applied via automated pipelines.

### Declarative vs. Imperative IaC

**Declarative (desired-state)** — describe *what* you want; the tool figures out *how* to get there.
- Examples: Terraform (HCL), AWS CloudFormation, Pulumi (can be declarative), Kubernetes YAML.
- Idempotent by design: applying the same config twice produces the same result.

**Imperative (procedural)** — describe *how* to achieve the desired state step by step.
- Examples: Ansible playbooks (partially), shell scripts, AWS CDK (imperative mode).
- More flexible but harder to reason about idempotency.

Most modern IaC tools are declarative.

### Immutable Infrastructure

A key principle related to IaC: once a server or container is deployed, it is **never modified in place**. To update, replace it with a new, freshly provisioned instance from updated IaC code.

Benefits:
- Eliminates configuration drift entirely.
- Rollback = deploy the previous version of the IaC definition.
- Consistent, known state at all times.

Contrasts with **mutable infrastructure**, where servers are patched and updated over time (leads to drift).

### Drift Prevention

**Drift** occurs when actual infrastructure state diverges from the declared IaC state (e.g., someone manually adds a firewall rule in the console).

- `terraform plan` detects drift by comparing desired state (code) with actual state (state file + real infrastructure).
- CI/CD pipelines that enforce all changes go through IaC prevent manual drift.
- Policy-as-code tools (OPA, Sentinel) can block non-IaC changes.

### Key Tools

| Tool | Language | Scope | Model |
|------|----------|-------|-------|
| **Terraform** | HCL (HashiCorp Configuration Language) | Multi-cloud | Declarative |
| **Pulumi** | TypeScript, Python, Go, C# | Multi-cloud | Declarative + imperative |
| **AWS CDK** | TypeScript, Python, Java, C# | AWS | Imperative → CloudFormation |
| **AWS CloudFormation** | JSON / YAML | AWS | Declarative |
| **Ansible** | YAML | Configuration management | Imperative/procedural |
| **Helm** | YAML + Go templates | Kubernetes | Declarative |

### Terraform Workflow

```
terraform init      # Download providers
terraform plan      # Preview changes (drift detection)
terraform apply     # Apply changes
terraform destroy   # Tear down infrastructure
```

State is stored in a **state file** (`terraform.tfstate`) — usually in a remote backend (S3, GCS, Terraform Cloud) for team use.

### Modules and Reuse

IaC supports encapsulation through modules (Terraform modules, Pulumi components). A module packages a set of related resources (e.g., a VPC + subnets + security groups) into a reusable unit with defined inputs and outputs.

## Key Components

- **IaC definition file** — the source-of-truth description of desired infrastructure.
- **Provider** — plugin connecting the IaC tool to a cloud platform's API.
- **State file** — record of the last-known actual state of provisioned resources.
- **Plan / Diff** — preview of changes before applying.
- **Module** — reusable, parameterised infrastructure component.
- **Remote backend** — shared state storage for team environments.

## When to Use

- Any cloud environment with more than one or two resources.
- Multi-environment setups (dev, staging, prod) requiring parity (see [[Twelve-Factor App]] Factor X).
- Teams where infrastructure changes must be reviewed and audited.
- Any deployment pipeline requiring reproducible environments.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Reproducible, auditable infrastructure | Learning curve for HCL / IaC tool |
| Version control: review, rollback | State file management adds complexity |
| Drift detection and enforcement | State can get out of sync with manual changes |
| Consistent environments (dev/staging/prod) | Large IaC codebases can become complex |

## Related

- [[Twelve-Factor App]] — Factor IV (Backing Services) and Factor X (Dev/Prod Parity) rely on IaC
- [[Continuous Integration and Delivery]] — IaC changes flow through CI/CD pipelines
- [[Blue-Green Deployment]] — IaC provisions the blue and green environments
- [[Canary Release]] — IaC manages traffic routing infrastructure
- [[Microservices Architecture]] — IaC provisions the container orchestration platform
