---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://kafka.apache.org/intro
tags:
  - messaging
  - kafka
  - event-streaming
  - distributed-systems
  - stream-processing
---

# Apache Kafka

A distributed, durable event-streaming platform built around a partitioned, replicated commit log — where events are retained by time or size rather than deleted on consumption.

## Problem

Traditional message queues delete messages after they are consumed, making it impossible to replay past events, audit history, or have multiple independent consumers read the same stream at different speeds. High-throughput data pipelines (IoT, financial transactions, microservices integration) require a system that can ingest millions of events per second, store them durably, and serve them to heterogeneous consumers.

## Solution / Explanation

Kafka models data as an ordered, immutable **log**. Producers append events to **topics**; consumers read from any offset in the log. The log is retained for a configurable window (time- or size-based), independent of consumption. This makes Kafka both a message bus and a persistent event store.

### Core Abstractions

**Events**
The unit of data. Each event has: key, value, timestamp, and optional headers. Events are immutable once written.

**Topics**
Named, durable log streams. A topic is analogous to a filesystem folder for events. Topics support multiple simultaneous producers and consumers. Events are not deleted on consumption.

**Partitions**
Each topic is split into one or more **partitions**, each an ordered, append-only log hosted on a broker. Partitions enable parallelism:
- Events with the same key always route to the same partition (ordered delivery for that key).
- Different partitions may be on different brokers, distributing I/O.

**Offsets**
A sequential integer identifying a message's position within a partition. Consumers track their own offset, giving them full control over replay vs. live consumption.

**Brokers**
Kafka servers forming a cluster. Each partition has one **leader** broker and zero or more **follower** replicas for fault tolerance.

**Replication**
Topics are typically replicated with a factor of 3 across brokers and datacenters. If the leader fails, a follower is elected automatically.

**Retention**
Kafka retains events based on configured time (e.g., 7 days) or total size, regardless of whether they have been consumed. This enables:
- Replay from any point in time.
- Multiple independent consumers at different positions.
- Event sourcing replay to rebuild state.

### Consumer Groups

A **consumer group** is a set of consumer instances that collectively read a topic. Kafka assigns each partition to exactly one member of the group at a time, distributing load. Multiple independent consumer groups can read the same topic simultaneously, each at their own pace and offset — enabling both parallel processing and pub-sub semantics in one model.

```
Topic Partition 0 → Consumer Group A: Instance 1
Topic Partition 1 → Consumer Group A: Instance 2
Topic Partition 0 → Consumer Group B: Instance 1  (independent read)
```

### Kafka APIs

| API | Purpose |
|-----|---------|
| Producer API | Publish events to topics |
| Consumer API | Subscribe and read events |
| Kafka Streams | Client library for stream processing (filter, join, aggregate) |
| Kafka Connect | Connectors to import/export data from external systems |
| Admin API | Manage topics, partitions, configurations |

## Key Components

- **ZooKeeper / KRaft** — cluster coordination (KRaft replaces ZooKeeper from Kafka 3.x).
- **Topic** — named log stream.
- **Partition** — ordered sub-log enabling parallelism.
- **Consumer Group** — set of consumers sharing partition assignment.
- **Offset** — consumer position within a partition.
- **Retention Policy** — time- or size-based log expiry.

## When to Use

- Real-time event streaming and data pipelines (high throughput).
- Event sourcing — Kafka as the system of record for event history (see [[Event Sourcing]]).
- CQRS read-model projection — consuming the event log to build query stores (see [[CQRS]]).
- Microservices integration — services publish domain events, others consume asynchronously.
- Activity tracking (page views, clicks, IoT sensor data).
- Log aggregation from many services into a central stream.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Extremely high throughput (millions of events/s) | Operationally complex cluster to manage |
| Durable, replayable log | Higher latency than in-memory queues for very low-volume workloads |
| Multiple independent consumer groups | Ordering only within a partition; global ordering not guaranteed |
| Decouples producers from consumers | Consumers must manage offset state |
| Native stream processing (Kafka Streams) | Schema evolution requires discipline (use a Schema Registry) |

## Comparison: Kafka vs. RabbitMQ

| | Apache Kafka | RabbitMQ |
|-|-------------|---------|
| Model | Distributed log (pull-based) | Message broker (push-based) |
| Retention | Time/size based (independent of consumption) | Deleted after ACK |
| Throughput | Millions of events/s | Hundreds of thousands/s |
| Ordering | Per partition | Per queue |
| Consumer position | Consumer-controlled offset | Broker-controlled |
| Replay | Yes | No (once consumed, gone) |
| Use case | Event streaming, audit, replay | Task queues, RPC, routing |

## Related

- [[Message Queue]] — simpler, point-to-point alternative
- [[Publish-Subscribe Pattern]] — Kafka implements pub-sub via consumer groups
- [[Message Broker]] — infrastructure role; Kafka is a specialised log-based broker
- [[Event-Driven Architecture]] — Kafka is a common backbone
- [[Event Sourcing]] — Kafka as the durable event store
- [[CQRS]] — Kafka as the event log feeding read-model projections
- [[Backpressure]] — Kafka's pull model naturally handles back-pressure
- [[Enterprise Integration Patterns]] — EIP vocabulary maps to Kafka concepts
