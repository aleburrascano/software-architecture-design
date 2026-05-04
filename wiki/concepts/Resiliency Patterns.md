---
type: concept
created: '2026-05-03'
updated: '2026-05-03'
sources:
  - 'https://awesome-architecture.com/microservices/resiliency/resiliency/'
tags:
  - resilience
  - microservices
  - distributed-systems
  - fault-tolerance
---
# Resiliency Patterns

A family of design patterns that make distributed systems tolerant of partial failures — enabling applications to recover from transient faults, degrade gracefully, and avoid cascading failures across service boundaries.

## Problem

In distributed systems, failures are not exceptional — they are the norm. Networks are unreliable, services go down, databases become slow, third-party APIs have outages. Without explicit resilience design, a single failing dependency can cascade into a full system outage.

## Solution / Explanation

Resilience patterns work at different levels: preventing calls from reaching failing services, recovering from transient faults, isolating failures, and providing degraded-but-functional responses.

### Core Patterns

**Retry**
Automatically re-attempt a failed operation after a brief delay. Essential for transient failures (network blips, brief overloads). Key considerations:
- Use **exponential backoff** — don't hammer a struggling service.
- Add **jitter** (random delay variation) to prevent thundering herd when many clients retry simultaneously.
- Set a **maximum retry count** to avoid infinite loops.
- Only retry **idempotent** operations.

**Circuit Breaker**
See [[Circuit Breaker Pattern]] for a dedicated page. Stops retrying when a service is clearly failing, preventing resource waste and giving the service time to recover.

**Timeout**
Every remote call must have a timeout. Without one, a single slow call holds a thread indefinitely, eventually exhausting thread pools. Set timeouts aggressively — prefer a fast error over an indefinite wait.

**Bulkhead**
See [[Bulkhead Pattern]]. Isolates resource pools so that failure in one area does not exhaust resources needed by other areas.

**Fallback**
When an operation fails (and the circuit is open), return a **fallback response** — cached data, a default value, an empty result, or a user-friendly error message — rather than propagating the error. Enables graceful degradation.

**Hedging (Speculative Execution)**
Send the same request to multiple instances simultaneously; use the first response received. Reduces tail latency at the cost of higher load on the service. Appropriate only for safe, idempotent read operations.

**Backpressure**
See [[Backpressure]]. Consumers signal capacity limits to producers, preventing overload.

### Resilience in Practice (.NET — Polly)

**Polly** is the standard .NET resilience library, providing fluent configuration for all the above patterns:

```csharp
var pipeline = new ResiliencePipelineBuilder()
    .AddRetry(new RetryStrategyOptions { MaxRetryAttempts = 3, Delay = TimeSpan.FromSeconds(1) })
    .AddCircuitBreaker(new CircuitBreakerStrategyOptions())
    .AddTimeout(TimeSpan.FromSeconds(5))
    .Build();
```

**Simmy** is a companion chaos-engineering library for injecting faults to test resilience.

### Defense in Depth

No single pattern is sufficient. Effective resilience combines:
1. **Timeouts** — so calls don't block forever.
2. **Retries with backoff** — for transient failures.
3. **Circuit breakers** — when retries are likely to fail.
4. **Bulkheads** — to contain the blast radius.
5. **Fallbacks** — to serve degraded responses.
6. **Observability** — to detect and diagnose failures quickly.

## When to Use

Resilience patterns are **always** needed in production distributed systems. Apply them proportionally:
- Simple internal calls: timeout + retry is often sufficient.
- External APIs or third-party services: full circuit breaker + bulkhead.
- Critical paths (payment, checkout): comprehensive defense in depth.

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Prevents cascading failures | Complexity and configuration overhead |
| Enables graceful degradation | Wrong configuration (too aggressive retry) can worsen outages |
| Reduces MTTR for transient faults | Fallback logic adds maintenance burden |
| Protects system resources | Requires testing with chaos engineering to validate |

## Related

- [[Circuit Breaker Pattern]] — key resilience pattern; dedicated page
- [[Bulkhead Pattern]] — resource isolation; dedicated page
- [[Backpressure]] — flow control; dedicated page
- [[Idempotency]] — required for safe retries
- [[Observability]] — needed to monitor and tune resilience policies
- [[Microservices Architecture]] — the context where resilience patterns are most critical
