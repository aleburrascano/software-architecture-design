---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.reactive-streams.org/
tags:
  - reactive
  - streams
  - backpressure
  - specification
  - jvm
---

# Reactive Streams

A JVM (and JavaScript) interoperability specification that defines a minimal set of interfaces for asynchronous stream processing with non-blocking backpressure, allowing independently-developed reactive libraries to interoperate.

## Problem

Before Reactive Streams, each reactive library (RxJava, Akka Streams, Vert.x, etc.) defined its own internal API. Pipelines could not cross library boundaries. More critically, without a shared backpressure protocol, a fast producer could overwhelm a slow consumer with no standardised way to signal "slow down" — forcing consumers to buffer unbounded data or drop messages.

## Solution / Explanation

The Reactive Streams initiative (2013–2015, led by engineers from Netflix, Pivotal, Red Hat, Twitter, Typesafe) produced a **minimal specification** of four interfaces. Any library implementing these interfaces can interoperate with any other conforming library.

In JDK 9+, the interfaces are published as `java.util.concurrent.Flow` — semantically equivalent 1:1 to the Reactive Streams interfaces. This gives the spec first-class JDK support.

### The Four Interfaces

**Publisher\<T\>**
Produces items of type T. Has one method: `subscribe(Subscriber<T> s)`. A Publisher is lazy — it does not emit anything until a Subscriber subscribes and signals demand.

**Subscriber\<T\>**
Consumes items. Four methods:
- `onSubscribe(Subscription s)` — called once when the Subscription is established; use this to call `s.request(n)` to signal initial demand.
- `onNext(T item)` — called for each item; called at most as many times as demand was signalled.
- `onError(Throwable t)` — terminal signal; stream ended with failure.
- `onComplete()` — terminal signal; stream ended successfully.

**Subscription**
The link between one Publisher and one Subscriber. Two methods:
- `request(long n)` — Subscriber signals it can accept up to n more items. This is the **backpressure mechanism**.
- `cancel()` — Subscriber signals it no longer wants items.

**Processor\<T, R\>**
Implements both Publisher\<R\> and Subscriber\<T\>. Sits in the middle of a pipeline — receives items of type T, transforms them, and emits items of type R. Examples: map, filter, buffer operators.

### Backpressure Flow

```
Publisher → emits items → Processor → emits items → Subscriber
     ↑                         ↑
 request(n)               request(n)
 (from Processor)          (from Subscriber)
```

Demand flows **upstream**; items flow **downstream**. The Subscriber controls the rate. If a Subscriber calls `request(10)`, the Publisher may emit at most 10 items before more demand is signalled. No buffering of arbitrary size is forced on the consumer.

## Key Components / Rules

- The spec is **minimal by design** — it defines only interoperability, not a full operator library. Libraries like Project Reactor and RxJava 2+ build rich APIs on top.
- `request(n)` uses **cumulative demand** — calling `request(3)` then `request(5)` signals total demand of 8.
- A Publisher must not emit more items than have been requested.
- All signals (`onNext`, `onError`, `onComplete`) must be delivered **non-blocking** and must not be called concurrently.
- The TCK (Technology Compatibility Kit) provides a conformance test suite for implementations.

## When to Use

- Any JVM system processing streams of data asynchronously where the producer can outpace the consumer.
- Choosing between reactive libraries: conforming to Reactive Streams means pipelines can bridge across libraries.
- Building framework-agnostic adapters (e.g., a library that works with both Spring WebFlux and Akka Streams).

## Trade-offs

| Benefit | Cost |
|---|---|
| Prevents unbounded buffering / OOM from fast producers | Demand management logic adds complexity |
| Library interoperability (Reactor ↔ RxJava ↔ Akka) | Four-interface model is low-level; use a library on top |
| JDK 9 `Flow` means no extra dependency | Debugging backpressure issues requires specialised tooling |

## Related

- [[Backpressure]] — the concept; Reactive Streams is the specification
- [[Reactive Programming]] — the programming model built on these interfaces
- [[Reactive Architecture]] — the architectural philosophy that motivates the spec
- [[Observer Pattern]] — Reactive Streams extends the Observer pattern with demand signalling
- [[Reactive Architecture Overview]] — synthesis page
