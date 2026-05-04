---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://www.reactive-streams.org/
source_date: 2015-04-28
source_author: Reactive Streams Working Group (Netflix, Pivotal, Red Hat, Twitter, Typesafe)
tags:
  - reactive
  - streams
  - specification
  - source
---

# Reactive Streams Specification

The initiative and specification page for the Reactive Streams standard for asynchronous stream processing with non-blocking backpressure.

## Key Takeaways

- Goal: "provide a standard for asynchronous stream processing with non-blocking back pressure" across JVM and JavaScript environments.
- Four minimal interfaces: Publisher, Subscriber, Subscription, Processor.
- Back pressure ensures "the receiving side is not forced to buffer arbitrary amounts of data."
- Adopted into JDK 9+ as `java.util.concurrent.Flow` — semantically equivalent 1:1.
- Designed to allow interoperability: different conforming implementations can be composed in a single pipeline.
- The specification is minimal by design — libraries (Reactor, RxJava) build rich operator APIs on top.
- TCK (Technology Compatibility Kit) provided for conformance testing.

## Referenced Concepts

- [[Reactive Streams]]
- [[Backpressure]]
- [[Reactive Programming]]
- [[Reactive Architecture]]

## Confidence

High — read directly from source.
