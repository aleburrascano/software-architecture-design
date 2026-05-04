---
type: topic
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.reactivemanifesto.org/
  - https://www.reactive-streams.org/
  - https://reactivex.io/
  - https://projectreactor.io/
tags:
  - reactive
  - architecture
  - overview
  - distributed-systems
---

# Reactive Architecture Overview

A synthesis of the reactive systems landscape: the Reactive Manifesto's four properties, the Reactive Streams specification, and the major frameworks (RxJava, Project Reactor, Akka Streams) that implement reactive programming on the JVM and beyond.

## Why Reactive?

Traditional synchronous, thread-per-request architectures hit a ceiling at scale:
- **Thread exhaustion**: blocking I/O ties up OS threads; at high concurrency, the cost in memory and context-switching becomes prohibitive.
- **Cascading failures**: a slow dependency blocks all threads allocated to it, starving the rest of the system.
- **No backpressure**: a fast data producer can overwhelm a slow consumer, causing OOM or message loss.

The reactive ecosystem addresses all three.

## The Four Properties (Reactive Manifesto)

See [[Reactive Architecture]] for full treatment.

| Property | What it means | How it's achieved |
|---|---|---|
| **Responsive** | Bounded, consistent latency | Async processing; circuit breakers; timeout policies |
| **Resilient** | Responsive under failure | Isolation ([[Bulkhead Pattern]]); replication; supervision |
| **Elastic** | Responsive under load variation | Sharding; auto-scaling; backpressure to prevent overload |
| **Message-Driven** | Async, non-blocking communication | Actors; message queues; reactive streams |

The four properties are interdependent: message-passing enables isolation; isolation enables resilience and elasticity; together they produce responsiveness.

## The Reactive Streams Specification

See [[Reactive Streams]] for full treatment.

Reactive Streams (reactive-streams.org) is the interoperability layer — four minimal Java interfaces (Publisher, Subscriber, Subscription, Processor) that any reactive library must implement to participate in shared pipelines. The key innovation is **demand signalling via `request(n)`**: the consumer controls the rate at which the producer emits, eliminating the need for unbounded buffers.

Adopted into the JDK as `java.util.concurrent.Flow` (Java 9+).

The [[Backpressure]] concept pre-existed Reactive Streams but the spec gave it a standardised, interoperable implementation.

## The Programming Model: Reactive Programming

See [[Reactive Programming]] for full treatment.

Reactive Programming is the coding style — composing pipelines of operators over async data streams — that sits on top of the Reactive Streams spec.

### ReactiveX (Rx)

The most widely adopted reactive programming API. Core abstractions: Observable/Flowable (producer) and Observer/Subscriber (consumer), connected by a chain of **operators** (map, flatMap, filter, merge, retry, etc.).

Key implementations:
- **RxJava** — JVM, the first major reactive library for Java. `Flowable` is Reactive Streams-compliant.
- **RxJS** — JavaScript/TypeScript; the foundation of Angular's async model.
- **Rx.NET** — .NET.

### Project Reactor

Pivotal's fourth-generation reactive library, fully Reactive Streams-compliant, optimised for Spring:
- **`Flux<T>`** — 0 to N elements.
- **`Mono<T>`** — 0 or 1 element (a non-blocking alternative to `CompletableFuture`).

Reactor is the reactive engine of **Spring WebFlux** — Spring Boot's non-blocking web stack (replacing Spring MVC's thread-per-request model).

### Akka Streams

Part of the Akka toolkit (Lightbend). Implements Reactive Streams on top of the **Actor Model** ([[Actor Model Architecture]]). Provides graph-shaped pipelines (not just linear chains), fan-in/fan-out, and deep integration with Akka's supervision and remoting infrastructure.

### Framework Comparison

| Framework | Language | Streams Model | Backpressure | Notable Integration |
|---|---|---|---|---|
| **RxJava 2/3** | Java | Flowable/Observable | Yes (Flowable) | Android, various |
| **Project Reactor** | Java | Flux/Mono | Yes | Spring WebFlux |
| **Akka Streams** | Scala/Java | Graph DSL | Yes | Akka, Alpakka |
| **RxJS** | JS/TS | Observable | Limited | Angular |
| **Mutiny** | Java | Multi/Uni | Yes | Quarkus |

## Architectural Patterns in Reactive Systems

### Event-Driven Architecture

[[Event-Driven Architecture]] is the natural home for reactive systems. Services communicate via events on a message bus (Kafka, RabbitMQ). Reactive consumers process event streams with backpressure, filtering, and transformation operators.

### CQRS + Event Sourcing

[[CQRS]] separates command (write) and query (read) paths. In a reactive implementation:
- Commands produce events (Event Sourcing).
- The query side subscribes to the event stream and maintains materialised views reactively.
- `Mono<CommandResult>` represents asynchronous command handling; `Flux<DomainEvent>` represents the event stream.

### Service Mesh and Reactive

In [[Microservices Architecture]], a [[Service Mesh]] (Istio, Linkerd) provides circuit breaking, retries, and timeouts at the infrastructure layer — complementing application-level reactive patterns.

## When to Use Reactive Architecture

**Strong fit:**
- HTTP APIs with high concurrency and I/O-bound workloads (e.g., API gateways, streaming services).
- Real-time data pipelines (financial ticks, IoT sensor streams, analytics).
- Systems requiring zero-downtime scaling and fault isolation.
- Applications integrating multiple async data sources.

**Poor fit:**
- Simple CRUD services with modest load — synchronous code is easier to reason about.
- Teams without reactive experience — reactive code is harder to debug and test.
- CPU-bound workloads — reactive non-blocking I/O doesn't help when the bottleneck is the CPU.

## Key Trade-offs

| Benefit | Cost |
|---|---|
| Non-blocking I/O enables high throughput with fewer threads | Stack traces are deep and hard to read |
| Backpressure prevents OOM under load spikes | Debugging reactive pipelines requires specialised tools (Reactor's `checkpoint()`) |
| Composable, declarative async logic | Steep learning curve; reactive thinking differs from imperative |
| Library interoperability via Reactive Streams | Risk of accidentally blocking in a reactive pipeline, destroying throughput |

## Related Concepts

- [[Reactive Architecture]] — the Reactive Manifesto in depth
- [[Reactive Streams]] — the interoperability specification
- [[Reactive Programming]] — the programming model
- [[Backpressure]] — the flow control mechanism
- [[Observer Pattern]] — the design pattern reactive programming generalises
- [[Event-Driven Architecture]] — natural partner architecture
- [[CQRS]] — commonly combined with reactive for read/write separation
