---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/articles/What is Software Architecture A Comprehensive Guide.md"
  - "raw/articles/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/articles/Software Architecture.md"
  - "raw/articles/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "https://www.geeksforgeeks.org/system-design/microservices/"
  - "https://martinfowler.com/articles/microservices.html"
tags:
  - architecture
  - architectural-style
  - microservices
  - distributed
---

# Microservices Architecture

An architectural style in which an application is built as a suite of small, independently developed, deployed, and operated services, each running in its own process, responsible for a specific functional domain, and communicating over well-defined APIs or lightweight messaging.

As Fowler and Lewis define it: "developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms."

## Problem

Monolithic applications bundle all functionality into a single deployable unit. As the system grows: deploying a small change requires redeploying the entire application; teams working on different features block each other; scaling the whole application to address bottlenecks in one component wastes resources; adopting new technologies requires consensus across the entire codebase; and a failure in one module can bring down the whole system.

## Solution

Decompose the application **vertically** by functional domain (rather than horizontally by layer). Each service owns its domain, its data store, and its deployment pipeline. Services communicate over the network — via synchronous APIs (REST, gRPC) or asynchronous messaging ([[Event-Driven Architecture]]). Teams work independently on their service, deploy independently, and scale independently.

This is a *vertical* separation of concerns — contrasted with [[Layered Architecture]]'s horizontal separation. Each microservice internally may still use a layered architecture.

## Key Characteristics (Fowler)

1. **Componentization via Services** — independently deployable units; changes to one service don't require redeploying others.
2. **Organization Around Business Capabilities** — teams structured by business function, not technical layer (Conway's Law alignment).
3. **Products Not Projects** — teams own and maintain services for their lifetime ("you build it, you run it").
4. **Smart Endpoints and Dumb Pipes** — business logic lives in services; communication channels remain simple (RESTish protocols, not orchestrating ESBs).
5. **Decentralized Governance** — technology diversity is allowed; teams choose best-fit tools.
6. **Decentralized Data Management** — polyglot persistence; each service owns its database.
7. **Infrastructure Automation** — CI/CD pipelines, automated testing, containerization (Docker/Kubernetes).
8. **Design for Failure** — services handle dependency failures explicitly; resilience patterns are essential.
9. **Evolutionary Design** — service boundaries evolve; design for replaceability.

## Key Components / Structure

| Component | Role |
|-----------|------|
| **Microservice** | Self-contained unit owning one functional domain (e.g., inventory, payments, shipments) |
| **API Gateway** | Single entry point for external clients; routes requests, handles auth, rate limiting, SSL termination. See [[API Gateway Pattern]] |
| **Service Registry / Discovery** | Tracks running service instances so other services can find them dynamically |
| **Message Broker** | Enables async communication between services (Kafka, RabbitMQ) |
| **Per-service Database** | Each service owns its own data store — no shared databases |
| **Load Balancer** | Distributes incoming traffic across service instances |
| **CI/CD Pipeline** | Independent deployment pipeline per service |
| **Container Orchestration** | Kubernetes/Docker for packaging, deploying, managing service instances |

## Design Patterns

| Pattern | Purpose |
|---------|---------|
| **[[Circuit Breaker Pattern]]** | Prevents cascading failures by stopping requests to failing services temporarily |
| **[[Saga Pattern]]** | Manages distributed transactions across services with compensating actions for failures |
| **[[Event Sourcing]]** | Stores state changes as events, enabling audit trails and recovery |
| **[[Strangler Fig Pattern]]** | Enables gradual monolith-to-microservices migration |
| **Bulkhead** | Isolates services to contain failures, preventing resource exhaustion from spreading |
| **[[CQRS]]** | Separates read and write operations within a service for optimization |
| **API Composition** | Aggregates data from multiple services into single client responses |
| **Tolerant Reader** | Services accept unexpected fields in responses, enabling independent evolution |

## When to Use

- Large teams working on a complex domain that can be cleanly partitioned.
- High-throughput systems where different domains have very different scaling requirements.
- When independent deployability across teams is a strategic priority.
- When different domains benefit from different technology stacks.
- Organizations with mature DevOps, containerization (Docker/Kubernetes), and monitoring capabilities.

**Not recommended for:**
- Small, stable applications or small teams.
- Organizations lacking infrastructure automation expertise or continuous delivery maturity.
- Early-stage products where domain boundaries are unclear.

Fowler's advice: "Consider beginning with a monolith, keeping it modular, and splitting into microservices as components become problematic."

## Trade-offs

**Pros:**
1. **Faster delivery** — teams deploy domain features independently without coordination.
2. **Efficient scaling** — scale only the services under load; allocate resources where needed most.
3. **Resilience** — a failure in one service can be isolated (especially on Kubernetes with auto-recovery).
4. **Technology flexibility** — each service can use the best-fit tech stack.
5. **Explicit module boundaries** — reduced change coupling between domains.

**Cons / pitfalls:**
- **Data consistency** — no distributed transactions; requires eventual consistency patterns (Saga, Outbox).
- **Distributed systems complexity** — network failures, latency, partial failures must be handled explicitly.
- **End-to-end testing** is harder — spinning up a full environment involves many services.
- **Operational overhead** — requires container orchestration, service mesh, distributed tracing, centralized logging.
- Remote calls are costlier than in-process calls — coarser service interfaces required.
- Refactoring across service boundaries is difficult once deployed.

## Modular Monolith (intermediate step)

A modular monolith vertically slices the code into self-contained modules (like microservices) but deploys as a single unit. It offers cleaner organization and separation of concerns with far less operational overhead — a pragmatic step before full microservices adoption.

## Real-World Usage

- **Amazon, Netflix:** Both adopted microservices to enable thousands of engineers to deploy hundreds of times per day without coordination. Netflix improved reliability and performance after 2007 outages.
- **Uber:** Switching to microservices resulted in smoother operations and increased efficiency.
- **Amazon Prime Video** (notable counter-example): moved a monitoring component *back* to a monolith, saving 90% in cost — a reminder that microservices are not always the right answer.
- Typically run on Kubernetes, with service meshes (Istio, Linkerd) for inter-service security and observability.

## Migration Strategy (from monolith)

1. Evaluate current monolith and domain boundaries.
2. Decompose into business functions.
3. Apply the [[Strangler Fig Pattern]] for incremental replacement.
4. Establish clear API contracts between services.
5. Create independent CI/CD pipelines per service.
6. Enable service discovery and centralized logging/monitoring.
7. Manage cross-cutting concerns via the API Gateway.
8. Iterate continuously.

## Related

- [[Layered Architecture]] — the contrasting horizontal-separation style; microservices are vertical slices; each service may use layers internally
- [[Event-Driven Architecture]] — commonly combined with microservices for async inter-service communication
- [[Domain-Driven Design]] — DDD's Bounded Contexts map naturally onto microservice boundaries
- [[Service-Oriented Architecture]] — SOA's predecessor pattern with heavier shared infrastructure
- [[CQRS]] — frequently applied within individual microservices to separate read and write paths
- [[API Gateway Pattern]] — the standard entry-point pattern for microservices
- [[Circuit Breaker Pattern]] — essential resilience pattern in microservices deployments
- [[Saga Pattern]] — standard approach to distributed transactions in microservices
