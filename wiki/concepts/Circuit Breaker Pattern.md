---
type: concept
created: '2026-05-03'
updated: 2026-05-03
sources:
  - 'https://martinfowler.com/bliki/CircuitBreaker.html'
  - 'https://awesome-architecture.com/cloud-design-patterns/circuit-breaker/'
  - 'https://www.geeksforgeeks.org/system-design/microservices/'
tags:
  - resilience
  - microservices
  - patterns
  - fault-tolerance
---

# Circuit Breaker Pattern

A stability pattern that wraps a remote call in a proxy object that monitors failures and, after a threshold is exceeded, trips to an "open" state and immediately returns errors instead of invoking the failing resource — preventing cascading failures across distributed systems.

## Problem

Remote calls across networks can fail or hang for an extended period. When many callers continue to invoke an unresponsive service, threads pile up waiting for timeouts, resource pools exhaust, and the failure cascades through the system. Simply adding retry logic can worsen the situation by amplifying load on an already struggling service. Multiple unresponsive callers trigger cascading failures that can bring down far more than the original failing component.

## Solution

Wrap the protected call in a **circuit breaker object** that tracks failures. The breaker moves through three states:

### Closed (Normal)
All calls pass through. The breaker counts failures (and optionally successes). As long as the failure rate stays below a configurable threshold the breaker stays closed.

### Open (Tripped)
After the failure count reaches the threshold the breaker trips open. Calls are **rejected immediately** — the underlying function is not invoked. This gives the failing service time to recover and prevents further resource waste on callers.

### Half-Open (Probe)
After a configurable reset timeout the breaker moves to half-open and allows a small number of trial calls through. If they succeed, the breaker closes and normal operation resumes. If they fail, the breaker opens again and the reset timeout restarts.

```
  [Closed] --failure threshold exceeded--> [Open]
     ^                                        |
     |--trial call succeeds--[Half-Open] <----| (after reset timeout)
                                |
                    --trial call fails--> [Open]
```

## Key Components

- **Invocation timeout** — how long to wait before counting a call as a failure.
- **Failure threshold** — number (or rate) of failures that triggers the trip. Thread pool exhaustion can also trigger the breaker.
- **Reset timeout** — how long the breaker stays open before trying half-open.
- **Fallback strategy** — what callers do when the breaker is open (return cached data, queue the request, degrade gracefully).

## When to Use

- Calls to external or downstream services that may be slow or unavailable.
- Any integration point where failures could cascade through the system.
- Services with different reliability SLAs that should not take each other down.
- Asynchronous communications using queues that may back up.
- Essential in [[Microservices Architecture]] where services form dependency chains (Fowler and Lewis explicitly list it as a key pattern).

## Trade-offs

| Benefit | Drawback |
|---------|---------|
| Stops cascading failures | Adds latency in monitoring overhead |
| Gives failing services recovery time | Fallback logic must be explicitly coded |
| Reduces resource waste | Threshold tuning requires operational knowledge |
| State changes are observable / alertable | Does not distinguish all error types equally |

**Additional considerations (Fowler):**
- Callers must handle breaker-open failures gracefully — show cached data, queue requests, or fail cleanly.
- Sophisticated implementations distinguish error types: connection failures vs. timeouts vs. application errors may warrant different thresholds.
- State change events (breaker opens/closes) should trigger monitoring alerts — they signal systemic degradation.

## Real-World Usage

- Netflix Hystrix popularized the Circuit Breaker for microservices. **Resilience4j** (Java) and **Polly** (.NET) are modern successors.
- AWS SDK clients embed configurable circuit breaker policies.
- Netflix, Amazon, and Uber all employ it to prevent cascading failures in their microservices deployments.
- Combine with **Bulkhead** (isolate resource pools) and **Timeout** patterns for comprehensive resilience.

## Related

- [[Bulkhead Pattern]] — complements circuit breaker; isolates resource pools
- [[Retry Pattern]] — should be combined with circuit breaker; retry when closed
- [[Saga Pattern]] — saga steps need circuit-breaker protection on service calls
- [[Observability]] — circuit breaker state should be exposed as metrics
- [[Microservices Architecture]] — primary context for circuit breaker use
- [[Resiliency Patterns]] — broader family this pattern belongs to
