---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://www.enterpriseintegrationpatterns.com/patterns/messaging/
tags:
  - messaging
  - integration
  - patterns
  - eip
  - architecture
---

# Enterprise Integration Patterns

A canonical pattern language of 65 messaging-based integration patterns, codified by Gregor Hohpe and Bobby Woolf in their 2003 book *Enterprise Integration Patterns*, providing a shared vocabulary for designing message-driven systems.

## Problem

Enterprise systems are built from heterogeneous applications that must exchange data but differ in technology, data format, and timing requirements. Without a shared vocabulary and reusable solutions, integration code is ad-hoc, brittle, and hard to communicate. Teams repeatedly solve the same routing, transformation, and reliability problems from scratch.

## Solution / Explanation

Hohpe & Woolf identified 65 recurring problems and their solutions across seven categories. The patterns build on four integration styles (file transfer, shared database, remote procedure invocation, messaging) and focus primarily on **messaging** as the most flexible and scalable style.

The fundamental building blocks are:

- **Message**: a discrete packet of data exchanged between applications.
- **Channel**: the transport pipe through which messages flow.
- **Router**: a component that directs messages to the appropriate channel.
- **Transformer**: a component that converts message format or content.
- **Endpoint**: the connection point between an application and the messaging system.

## Pattern Taxonomy

### 1. Message Construction
Defines what a message is and how it is structured.

| Pattern | Summary |
|---------|---------|
| Command Message | Directs a receiver to perform an action |
| Document Message | Transfers data between systems |
| Event Message | Notifies receivers of a significant occurrence |
| Request-Reply | Two-way messaging with a reply channel |
| Return Address | Specifies where to send the reply |
| Correlation Identifier | Links request and reply messages |
| Message Sequence | Maintains ordering across a set of messages |
| Message Expiration | Sets a validity time-limit on a message |
| Format Indicator | Identifies the message data format/version |

### 2. Messaging Channels
Describes transport mechanisms.

| Pattern | Summary |
|---------|---------|
| Point-to-Point Channel | Exactly one consumer receives each message |
| Publish-Subscribe Channel | All subscribers receive a copy |
| Datatype Channel | Single channel per message type |
| Dead Letter Channel | Holds undeliverable/expired messages |
| Guaranteed Delivery | Persists messages so none are lost |
| Channel Adapter | Connects an external system to the message channel |
| Messaging Bridge | Joins two separate messaging systems |
| Message Bus | Central hub for all system communication |

### 3. Message Routing
Directs messages to the right destination.

| Pattern | Summary |
|---------|---------|
| Pipes and Filters | Chains processing steps sequentially |
| Content-Based Router | Routes based on message content |
| Message Filter | Selectively passes or discards messages |
| Dynamic Router | Routes based on runtime-configured rules |
| Recipient List | Sends to a dynamic list of destinations |
| Splitter | Breaks a message into individual parts |
| Aggregator | Combines related messages into one |
| Resequencer | Restores the correct message order |
| Scatter-Gather | Broadcasts and consolidates responses |
| Routing Slip | Message carries its own routing instructions |
| Process Manager | Orchestrates multi-step workflows |
| Message Broker | Decouples sender routing knowledge |

### 4. Message Transformation
Modifies message content for compatibility.

| Pattern | Summary |
|---------|---------|
| Message Translator | Converts between data formats |
| Envelope Wrapper | Adds routing metadata around the payload |
| Content Enricher | Adds missing data from an external source |
| Content Filter | Removes irrelevant fields |
| Claim Check | Stores large payloads externally; passes a ticket |
| Normalizer | Converts varying formats to a canonical form |
| Canonical Data Model | Shared intermediate format for multi-system translation |

### 5. Messaging Endpoints
How applications connect to the messaging system.

| Pattern | Summary |
|---------|---------|
| Messaging Gateway | Encapsulates messaging code from application logic |
| Transactional Client | Ensures reliable, atomic message sending |
| Polling Consumer | Actively pulls messages from the channel |
| Event-Driven Consumer | Passively notified when a message arrives |
| Competing Consumers | Multiple instances consume the same queue (load-balancing) |
| Durable Subscriber | Retains messages while subscriber is offline |
| Idempotent Receiver | Handles duplicate messages safely |
| Service Activator | Triggers domain logic via a message |

### 6. Systems Management
Operational patterns for monitoring and control.

| Pattern | Summary |
|---------|---------|
| Control Bus | Separate administrative message channel |
| Wire Tap | Non-intrusive copy for monitoring |
| Message History | Records the path a message has taken |
| Message Store | Persists messages for auditing or replay |
| Smart Proxy | Intercepts messages for tracking |
| Test Message | Validates system health |
| Channel Purger | Removes test/stale messages from a channel |

## Key Contributors

- **Gregor Hohpe & Bobby Woolf** — authors of the book (Addison-Wesley, 2003).
- Popularised by frameworks: Apache Camel, Spring Integration, Mule ESB, NServiceBus, MassTransit all implement this vocabulary.

## When to Use

- Designing or documenting an integration between systems.
- Reviewing or naming existing integration code (giving it a shared vocabulary).
- Deciding how to route, transform, or aggregate messages between services.
- Building an [[Event-Driven Architecture]] with well-defined message contracts.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Shared vocabulary reduces miscommunication | Large pattern set has a learning curve |
| Solutions proven across decades of enterprise integration | Patterns from 2003; some assume heavyweight ESB — apply selectively |
| Framework support reduces boilerplate | Over-engineering risk: not every system needs all patterns |

## Related

- [[Message Queue]] — implements Point-to-Point Channel
- [[Publish-Subscribe Pattern]] — implements Publish-Subscribe Channel
- [[Message Broker]] — implements the Message Broker pattern
- [[Apache Kafka]] — many EIP patterns map to Kafka concepts
- [[Event-Driven Architecture]] — EIP provides the vocabulary for EDA
- [[Outbox Pattern]] — specialisation of Transactional Client + Guaranteed Delivery
- [[Idempotency]] — required by Idempotent Receiver pattern
