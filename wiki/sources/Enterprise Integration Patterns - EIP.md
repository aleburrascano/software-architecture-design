---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://www.enterpriseintegrationpatterns.com/patterns/messaging/
source_date: 2003 (book), site ongoing
source_author: Gregor Hohpe, Bobby Woolf
tags:
  - messaging
  - integration
  - eip
  - patterns
---

# Enterprise Integration Patterns — EIP Catalogue

The official EIP patterns site and the companion book *Enterprise Integration Patterns* (Addison-Wesley, 2003) by Gregor Hohpe and Bobby Woolf, at [https://www.enterpriseintegrationpatterns.com/patterns/messaging/](https://www.enterpriseintegrationpatterns.com/patterns/messaging/).

## Summary

A catalogue of 65 messaging patterns organised across seven categories: Message Construction, Messaging Channels, Message Routing, Message Transformation, Messaging Endpoints, and Systems Management. Provides a shared vocabulary for designing and communicating about message-driven integration architectures.

## Key Takeaways

- **65 named patterns** covering the full lifecycle of a message-based system.
- Fundamental abstractions: Message, Channel, Router, Transformer, Endpoint.
- **Four integration styles**: file transfer, shared database, RPC, messaging — the catalogue focuses on messaging.
- Widely implemented in: Apache Camel, Spring Integration, Mule ESB, NServiceBus, MassTransit.
- Pattern icons and names are licensed under Creative Commons Attribution.
- The book predates cloud-native but the vocabulary remains the canonical reference for enterprise messaging.

## Key Patterns Referenced in This Wiki

- Point-to-Point Channel → [[Message Queue]]
- Publish-Subscribe Channel → [[Publish-Subscribe Pattern]]
- Message Broker → [[Message Broker]]
- Dead Letter Channel → reliability in [[Message Queue]]
- Competing Consumers → [[Messaging Patterns Overview]]
- Idempotent Receiver → [[Idempotency]]
- Guaranteed Delivery → [[Outbox Pattern]]
- Content-Based Router, Scatter-Gather, Aggregator → [[Enterprise Integration Patterns]]

## Wiki Pages That Cite This Source

- [[Enterprise Integration Patterns]] — full taxonomy
- [[Message Queue]] — Point-to-Point Channel
- [[Publish-Subscribe Pattern]] — Publish-Subscribe Channel
- [[Message Broker]] — Message Broker pattern
- [[Messaging Patterns Overview]] — EIP vocabulary synthesis
