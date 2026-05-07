---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'Dapr - Distributed Application Runtime.md'
tags:
  - dapr
  - distributed-systems
  - microservices
  - pub-sub
  - state-management
  - actors
  - workflow
  - cncf
  - sidecar
  - cloud-agnostic
---
# Dapr

**Distributed Application Runtime** — a CNCF project that provides a set of portable, event-driven building blocks for distributed applications via a sidecar architecture, decoupling application code from infrastructure choices.

## Problem

Building distributed microservices requires solving the same problems repeatedly: reliable pub/sub messaging, distributed state management, service invocation with retries, secret management, workflow orchestration. Each cloud provider has their own services for these, creating vendor lock-in. How do you build portable distributed applications without rewriting the infrastructure layer for each environment?

## Solution / Explanation

Dapr runs as a **sidecar process** alongside each application instance. Applications communicate with Dapr via a local HTTP or gRPC API, and Dapr handles the interaction with the underlying infrastructure components (which are pluggable).

```
Application ──HTTP/gRPC──► Dapr Sidecar ──► Redis (state)
                                       ──► Kafka (pub/sub)
                                       ──► Vault (secrets)
                                       ──► Azure Service Bus (pub/sub)
```

The application only knows the Dapr API. Swap Redis for DynamoDB by changing Dapr configuration — zero code changes.

### Building Blocks

| Building Block | Purpose | Example Use |
|---|---|---|
| **Pub/Sub** | Reliable async messaging | Order events → multiple consumers |
| **State Management** | Distributed key-value state | Session state, shopping cart |
| **Service Invocation** | Service-to-service calls with retries + tracing | Synchronous API calls |
| **Bindings (Input)** | Trigger app from external event | S3 upload triggers processing |
| **Bindings (Output)** | Send events to external systems | Send email on order completion |
| **Secrets** | Access secrets from any store | DB passwords, API keys |
| **Actors** | Virtual actor model | Per-user or per-entity state machines |
| **Workflow** | Durable multi-step orchestration | Order fulfillment process |
| **Configuration** | Read/watch app configuration | Feature flags, dynamic settings |
| **Cryptography** | Encrypt/decrypt/sign data | PII protection |

### Workflow Building Block

Dapr Workflow provides durable, fault-tolerant workflow orchestration comparable to Azure Durable Functions or Temporal. Workflows are defined as code, survive process restarts, and have built-in retry and compensation:

```
workflow OrderWorkflow(orderId):
    inventory ← call ReserveInventory(orderId)
    if inventory.success:
        payment ← call ProcessPayment(orderId)
        call ScheduleShipping(orderId)
```

### Actor Model Integration

Dapr's Actor building block implements the **virtual actor pattern**: actors are created on demand, activated on a specific host node, and deactivated when idle. State is automatically persisted to the configured state store.

### Pluggable Components

All Dapr building blocks use a **component model** — swap infrastructure without code changes:

| Building Block | Component Options |
|---|---|
| State | Redis, Azure CosmosDB, DynamoDB, Cassandra, PostgreSQL |
| Pub/Sub | Kafka, RabbitMQ, Azure Service Bus, AWS SNS/SQS, Google Pub/Sub |
| Secrets | Vault, Azure Key Vault, AWS SSM, Kubernetes secrets |

## When to Use

Strong fit:
- Cloud-agnostic microservices (need to run on multiple clouds or on-prem)
- Teams want to avoid vendor lock-in for messaging/state
- Event-driven architecture without direct dependency on specific broker
- Workflow orchestration across microservices

Weaker fit:
- Single-cloud, fully managed infrastructure (AWS-native stack already solves these problems)
- Simple applications where the sidecar overhead isn't justified
- Extreme performance requirements where sidecar RPC adds too much latency

## Trade-offs

| Benefit | Cost |
|---|---|
| Cloud-agnostic portability | Sidecar adds latency and operational complexity |
| Infrastructure swap without code changes | Dapr is another system to operate and upgrade |
| Unified building blocks reduce custom code | Dapr API is a new abstraction layer to learn |
| CNCF backed; active ecosystem | Still relatively young; breaking changes possible |
| Built-in observability (OpenTelemetry integration) | Debugging requires understanding sidecar interactions |

## Related

- [[Sidecar Pattern]] — Dapr's architectural pattern
- [[Event-Driven Architecture]] — pub/sub building block
- [[Actor Model Architecture]] — virtual actor building block
- [[Publish-Subscribe Pattern]] — Dapr pub/sub abstraction
- [[Choreography vs Orchestration]] — workflow = orchestration; pub/sub = choreography
- [[Microservices Architecture]] — Dapr's primary target
