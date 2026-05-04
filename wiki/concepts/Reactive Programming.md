---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://reactivex.io/
  - https://projectreactor.io/
  - https://www.reactive-streams.org/
tags:
  - reactive
  - programming-model
  - observables
  - async
---

# Reactive Programming

A programming paradigm centred on composable asynchronous data streams, where changes propagate automatically through a pipeline of operators — combining the [[Observer Pattern]], the Iterator pattern, and functional programming.

## Problem

Traditional asynchronous code (callbacks, futures/promises) composes poorly. Nested callbacks produce "callback hell." Futures chain awkwardly. Neither provides a standard way to handle backpressure, cancellation, error propagation, or stream merging. As concurrency requirements grow, imperative async code becomes hard to reason about.

## Solution / Explanation

Reactive Programming treats **events, data, and errors as streams**. A stream is a sequence of values over time. The developer composes transformations on streams using a rich library of **operators** — map, filter, flatMap, merge, zip, retry, timeout, etc. — rather than writing imperative control flow.

### ReactiveX (Rx)

ReactiveX (reactivex.io), developed originally at Microsoft by Erik Meijer, is the most widespread family of reactive programming libraries. It is described as "a combination of the best ideas from the Observer pattern, the Iterator pattern, and functional programming."

Available as idiomatic implementations in: Java (RxJava), JavaScript (RxJS), .NET (Rx.NET), Scala, C++, Python, Clojure, Swift, Kotlin, and others.

### Core Abstractions

**Observable / Flowable**
A source of items emitted over time. Cold observables begin emitting when subscribed; hot observables emit regardless of subscriptions (e.g., UI events, sensor streams). In RxJava 2+, `Flowable` is a backpressure-aware Observable that implements [[Reactive Streams]].

**Observer / Subscriber**
Consumes items from an Observable. Provides three callbacks: `onNext(item)`, `onError(t)`, `onComplete()`.

**Operators**
First-class functions that transform streams:

| Category | Examples |
|---|---|
| **Transform** | `map`, `flatMap`, `concatMap`, `scan` |
| **Filter** | `filter`, `take`, `skip`, `distinct`, `debounce` |
| **Combine** | `merge`, `zip`, `combineLatest`, `concat` |
| **Error handling** | `retry`, `onErrorResumeNext`, `catchError` |
| **Timing** | `delay`, `timeout`, `interval`, `throttle` |
| **Backpressure** | `buffer`, `window`, `sample`, `onBackpressureDrop` |

**Schedulers**
Control which thread operators run on — `Schedulers.io()` for blocking I/O, `Schedulers.computation()` for CPU work, `AndroidSchedulers.mainThread()` for UI updates.

### Project Reactor (Spring)

Project Reactor (projectreactor.io) is a fourth-generation reactive library fully compliant with [[Reactive Streams]], designed for the JVM. It provides:
- **Flux\<T\>**: a stream of 0–N elements.
- **Mono\<T\>**: a stream of 0–1 elements (analogous to a non-blocking Future).

Reactor is the reactive foundation of **Spring WebFlux**, Spring's non-blocking web framework introduced in Spring 5.

### Reactive vs. Imperative Async

| Aspect | Imperative Async (futures/callbacks) | Reactive (Rx / Reactor) |
|---|---|---|
| Composability | Low (callback nesting) | High (operator chains) |
| Error propagation | Manual, easy to miss | Built into stream (`onError`) |
| Backpressure | None | First-class (`request(n)`) |
| Cancellation | Manual | `Disposable.dispose()` / `cancel()` |
| Thread management | Explicit | Via Schedulers |

## Key Components / Rules

- **Streams are lazy** — nothing happens until someone subscribes.
- **Immutable pipelines** — each operator returns a new stream; the original is unchanged.
- **Error channels are first-class** — errors flow through the stream and can be handled with operators like `onErrorReturn`.
- **Prefer `Flowable` over `Observable`** in RxJava 2+ for any potentially high-volume stream to enable backpressure.
- **Avoid blocking in reactive pipelines** — blocking calls inside `map` starve the event loop; use `subscribeOn(Schedulers.io())` to offload.

## When to Use

- Non-blocking I/O — HTTP servers, database drivers (R2DBC), WebSockets.
- Event-driven UI (RxJS in Angular/React).
- Real-time data pipelines (sensor streams, financial ticks).
- Systems that must compose multiple async data sources.

Not a good fit when:
- Simple request/response with no concurrency needs (a plain future or synchronous call is clearer).
- Team has no reactive experience — the learning curve is steep.

## Trade-offs

| Benefit | Cost |
|---|---|
| Composable async pipelines | High learning curve; stack traces are hard to read |
| Backpressure built in | Debugging complex operator chains is difficult |
| Consistent error handling | Blocking calls accidentally introduced destroy performance |
| Rich operator library | Operator overload — many ways to do the same thing |

## Related

- [[Observer Pattern]] — Reactive Programming generalises the Observer pattern
- [[Reactive Streams]] — the interoperability specification underlying Rx Flowable and Reactor
- [[Reactive Architecture]] — the architectural philosophy motivating reactive programming
- [[Backpressure]] — the flow control mechanism central to reactive streams
- [[Event-Driven Architecture]] — reactive programming is commonly used to implement EDA
- [[Reactive Architecture Overview]] — synthesis page
