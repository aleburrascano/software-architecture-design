---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/cloud-design-patterns/bulkhead-pattern/'
tags:
  - resilience
  - microservices
  - patterns
  - fault-tolerance
---
# Bulkhead Pattern

A resilience pattern that isolates elements of an application into separate resource pools so that if one component fails or becomes overloaded, the others continue to function — preventing total system failure from a single point of resource exhaustion.

## Problem

When multiple services or features share the same resource pool (thread pool, connection pool, memory), an overloaded or failing component can exhaust all shared resources. This causes all other components to fail, even those that are otherwise healthy. A slow third-party service, for instance, can consume all available threads and take down the entire application.

## Solution / Explanation

The pattern is named after the watertight bulkheads used in ships: separate compartments that can be individually flooded without sinking the whole vessel.

In software, a bulkhead isolates resources so that:
- Each critical component or downstream dependency gets its own dedicated pool.
- A failure or saturation in one pool does not bleed into others.
- The rest of the application remains available even when one bulkhead is breached.

### Implementation Approaches

**Thread Pool Isolation**
Assign each dependency (Service A, Service B, external API) its own thread pool with a fixed size. If Service A hangs, only its thread pool fills up. The main thread pool and Service B's pool remain available.

```
┌─────────────────────────────────────────────┐
│ Application                                 │
│ ┌──────────────┐ ┌──────────────┐           │
│ │ Thread Pool A│ │ Thread Pool B│ ...       │
│ │ (Service A)  │ │ (Service B)  │           │
│ └──────────────┘ └──────────────┘           │
└─────────────────────────────────────────────┘
```

**Connection Pool Isolation**
Each downstream service gets its own database or HTTP connection pool. Exhaustion of connections to one database does not affect connections to others.

**Process / Container Isolation**
The strongest form: each component runs in a separate process or container, with its own CPU, memory, and network limits. Failures are fully isolated at the OS level.

**Semaphore-Based Isolation**
A lighter alternative to thread pools: a semaphore limits the maximum concurrent calls to a dependency. Faster but does not protect against slow calls blocking caller threads.

### Bulkhead + Circuit Breaker

Bulkheads and [[Circuit Breaker Pattern|circuit breakers]] are complementary:
- **Bulkhead**: limits *how many* calls can be in-flight simultaneously.
- **Circuit breaker**: limits *when* calls are allowed to proceed.

Together they provide comprehensive protection against slow or failing dependencies.

## When to Use

- Applications with multiple downstream service calls with different reliability characteristics.
- Services where one consumer or tenant must not degrade the experience for others.
- Critical components that must remain available even when non-critical ones fail.
- High-throughput services where resource pool exhaustion is a realistic failure mode.

Not suitable when:
- The overhead of separate pools outweighs the isolation benefit (very simple services).
- Resources are constrained and cannot be divided (e.g., very low-memory environments).

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Limits blast radius of failures | Resource overhead: dedicated pools use more total resources |
| Critical paths remain available | Thread pool sizing requires careful capacity planning |
| Prevents cascading resource exhaustion | More complex configuration and monitoring |
| Enables graceful degradation | Undersized pools cause premature rejection of valid requests |

## Related

- [[Circuit Breaker Pattern]] — prevents calls to failing services; complements bulkhead
- [[Resiliency Patterns]] — broader family of resilience mechanisms
- [[Backpressure]] — upstream flow control; bulkhead is downstream resource protection
- [[Microservices Architecture]] — primary context for bulkhead use
- [[Observability]] — pool saturation metrics are key signals to monitor
