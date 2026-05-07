---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources: []
tags:
  - resilience
  - distributed-systems
  - cloud-pattern
  - microservices
---

# Retry Pattern

A resilience pattern that automatically retries a failed operation a configurable number of times before propagating the failure to the caller. Used to handle **transient faults** — temporary failures that are likely to succeed on a subsequent attempt.

## When to Apply

- Transient network errors or timeouts
- Temporary unavailability of a downstream service
- Rate-limiting responses (e.g., HTTP 429) that will resolve after a short wait

Do **not** retry non-transient errors such as authentication failures, invalid request formats, or business logic rejections. Retrying those wastes resources without any chance of success.

## Retry Strategies

| Strategy | Description |
|---|---|
| **Fixed delay** | Wait a constant interval between each attempt |
| **Exponential backoff** | Double the wait time after each failure (e.g., 1s, 2s, 4s, 8s) |
| **Jitter** | Add randomness to the backoff delay to avoid thundering herd across many callers |
| **Immediate** | Retry instantly (only for very short transients) |

Exponential backoff with jitter is the recommended default for most distributed systems.

## Limits

Always set a maximum retry count and a total timeout. Unbounded retries can amplify load on an already-struggling downstream system. Combine with the [[Circuit Breaker Pattern]] to stop retrying once a service is confirmed to be unhealthy.

## Idempotency Requirement

Retried operations must be **idempotent** — repeating the call should produce the same result as calling it once. See [[Idempotency]].

## Related Concepts

- [[Circuit Breaker Pattern]] — stops retrying when a service is persistently unhealthy
- [[Idempotency]] — prerequisite for safe retries
- [[Bulkhead Pattern]] — isolates retry storms from affecting other workloads
- [[Saga Pattern]] — retries within long-running distributed transactions
- [[Microservices Architecture]] — retries are a fundamental concern in service-to-service communication
