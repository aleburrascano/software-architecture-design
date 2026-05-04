---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://kafka.apache.org/intro
source_date: 2024
source_author: Apache Software Foundation
tags:
  - messaging
  - kafka
  - event-streaming
  - distributed-systems
---

# Apache Kafka — Introduction

Official Apache Kafka introduction documentation at [https://kafka.apache.org/intro](https://kafka.apache.org/intro).

## Summary

Apache Kafka is an event streaming platform combining durable publish-subscribe messaging with a distributed commit log. It can publish, subscribe to, store, and process event streams in real time.

## Key Takeaways

- **Events**: Records with key, value, timestamp, and optional headers.
- **Topics**: Durable, named log streams supporting multiple producers and consumers simultaneously. Events are not deleted on consumption.
- **Partitions**: Topics split into ordered, append-only logs on individual brokers. Events with the same key route to the same partition (ordered delivery per key).
- **Replication**: Topics replicated across brokers (commonly factor 3) for fault tolerance. Automatic leader failover.
- **Consumer Groups**: A group of consumers divides partition assignment among members. Multiple independent groups each read the full topic independently.
- **Retention**: Events retained by time (e.g., 7 days) or size — independent of consumption. Enables replay.
- **Brokers**: Kafka cluster nodes; deployable on bare metal, VMs, containers, or cloud.
- **APIs**: Producer, Consumer, Kafka Streams (stream processing), Kafka Connect (external system integration), Admin.
- **Use cases**: Real-time financial processing, fleet/IoT tracking, healthcare monitoring, microservices integration, data pipeline integration.

## Wiki Pages That Cite This Source

- [[Apache Kafka]] — full concept page
- [[Message Broker]] — Kafka vs. RabbitMQ comparison
- [[Messaging Patterns Overview]] — queue vs. topic vs. stream decision guide
- [[Event-Driven Architecture]] — Kafka as the event streaming backbone
