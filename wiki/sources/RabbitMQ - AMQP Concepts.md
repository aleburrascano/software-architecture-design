---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://www.rabbitmq.com/tutorials/amqp-concepts.html
source_date: 2024
source_author: RabbitMQ / VMware / Broadcom
tags:
  - messaging
  - rabbitmq
  - amqp
  - queues
---

# RabbitMQ — AMQP 0-9-1 Concepts

Official RabbitMQ documentation on AMQP concepts at [https://www.rabbitmq.com/tutorials/amqp-concepts.html](https://www.rabbitmq.com/tutorials/amqp-concepts.html).

## Summary

AMQP 0-9-1 is a messaging protocol enabling client applications to communicate with conforming message brokers. The model routes messages from publishers through exchanges, via bindings, to queues, and finally to consumers.

## Key Takeaways

- **Exchange types**: Direct (exact key), Fanout (broadcast), Topic (wildcard), Headers (attribute-based), Default (auto-bind by queue name).
- **Queues**: Durable, exclusive, auto-delete attributes. Durability = metadata stored on disk.
- **Bindings**: Routing rules from exchange to queue; optional routing key filter.
- **Delivery modes**: Auto-ack (at-most-once) vs. explicit ack (at-least-once).
- **Persistence**: Requires durable exchange + durable queue + persistent message delivery mode — all three.
- **Channels**: Lightweight logical connections multiplexed over a single TCP connection.
- **Virtual hosts (vhosts)**: Isolation units within a broker for multi-tenancy.
- **Prefetch count**: Limits unacknowledged messages per channel (back-pressure control).
- **Dead letter**: Rejected or expired messages routed to a Dead Letter Exchange.
- **Negative acks (nack)**: RabbitMQ extension allowing rejection of multiple messages at once.

## Wiki Pages That Cite This Source

- [[Message Queue]] — AMQP model, exchange types, delivery guarantees
- [[Publish-Subscribe Pattern]] — Fanout and Topic exchanges
- [[Message Broker]] — RabbitMQ architecture and push model
- [[Messaging Patterns Overview]] — queue vs. topic vs. stream comparison
