---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/threedotslabs-watermill.md'
source_date: ''
source_author: Three Dots Labs
tags:
  - source
  - go
  - golang
  - event-driven
  - pub-sub
  - message-routing
  - cqrs
  - watermill
  - middleware
---
# Watermill - Go Event-Driven Library

Go library for building event-driven applications with a broker-agnostic pub/sub abstraction, middleware support, and a CQRS component.

## Summary

Watermill solves the broker coupling problem in Go event-driven applications by providing a common Publisher/Subscriber interface implemented by adapters for Kafka, RabbitMQ, Google Pub/Sub, AWS SNS/SQS, NATS, and an in-memory implementation for testing. Application code depends only on the interface, enabling broker swapping without touching handler logic and enabling integration tests using the fast in-memory implementation.

The router is the central coordination component: it connects publishers and subscribers into processing pipelines and applies middleware chains. Middleware handles cross-cutting concerns (structured logging, Prometheus metrics, retry with backoff, poison message queues, OpenTelemetry tracing) without modifying individual message handlers. This separation keeps handlers focused on business logic while the middleware chain handles operational concerns.

The CQRS component provides explicit Command Bus and Event Bus abstractions, with command handlers and event handlers registered separately. This makes the command/query separation explicit at the framework level and prevents command handlers from accidentally publishing commands to the event bus or vice versa. The component integrates with the router's middleware infrastructure, so commands and events benefit from the same cross-cutting concern handling.

## Key Arguments

- Watermill abstracts message broker specifics behind a common Publisher/Subscriber interface
- Middleware chains enable cross-cutting concerns (logging, metrics, retry, tracing) without modifying handlers
- The CQRS component provides explicit command bus and event bus abstractions that enforce separation
- Router-based pipeline construction enables complex event processing topologies without custom wiring code
- Broker-agnostic design enables fast integration testing with the in-memory pub/sub implementation

## Concepts Covered

- [[Event-Driven Architecture]] — Go implementation demonstrating EDA patterns in practice
- [[Publish-Subscribe Pattern]] — Watermill's core abstraction over multiple broker implementations
- [[CQRS]] — command bus and event bus components
- [[Messaging Patterns Overview]] — broker-agnostic patterns and middleware chains

## Quality Notes

Well-designed library that demonstrates clean event-driven architecture in Go. Good reference for EDA implementation patterns that apply beyond Go. Three Dots Labs is an active Go consultancy with strong open-source track record.
