---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/Software architecture 1.md
tags:
  - architecture
  - cloud
  - distributed
---

# Serverless Architecture

A cloud computing paradigm where server management is delegated entirely to the cloud provider; developers deploy individual functions that execute on demand in response to events, paying only for actual compute time consumed.

## Problem / Why It Matters

Traditional server-based deployments require provisioning, configuring, patching, and scaling servers — operational overhead that is proportionally expensive for workloads that are bursty, unpredictable, or low-volume. Serverless removes this burden and makes the cost model directly proportional to usage.

## Explanation

"Serverless" is a misnomer — servers still exist, but their management is fully abstracted away. The developer writes and deploys units of code called **Functions as a Service (FaaS)**, which are:

- **Event-triggered**: invoked in response to HTTP requests, queue messages, scheduled timers, database change events, etc.
- **Stateless**: each invocation is independent; persistent state lives in external storage
- **Ephemeral**: the runtime is created on demand and torn down after execution
- **Automatically scaled**: the cloud provider manages concurrency; zero requests = zero running instances

The event-driven approach makes serverless a specific implementation style within [[Event-Driven Architecture]].

## Cost Model

The pay-as-you-go model charges per invocation and per millisecond of execution time. For infrequently-used functions or highly variable traffic, this is significantly cheaper than maintaining always-on servers. However, as Marc Brooker notes: "you need to consider total cost of ownership not just the infra cost" — data transfer, storage, and service integration costs must be included.

## Trade-offs

| Benefit | Drawback |
|---|---|
| No server management | Cold start latency on first invocation |
| Automatic scaling to zero | Execution time limits (typically 15 minutes max) |
| Pay-per-use pricing | Harder to debug and observe than traditional services |
| Event-driven integration | Vendor lock-in on platform-specific triggers |
| Low ops overhead for bursty workloads | Statelessness requires external state management |

## Architectural Fit

Serverless is well-suited for:
- Bursty or highly variable traffic patterns
- Event-driven workflows and integrations
- Background processing, scheduled jobs, data transformation pipelines
- Lightweight APIs with unpredictable usage

It is less suited for:
- Long-running computations
- Workloads requiring persistent in-process state
- Latency-sensitive applications where cold starts are unacceptable

## Related

- [[Event-Driven Architecture]] — serverless functions are a form of event-driven computing
- [[Microservices Architecture]] — serverless and microservices are complementary but distinct; both promote independent deployment
- [[Quality Attributes]] — serverless primarily trades off latency and debuggability for scalability and cost
