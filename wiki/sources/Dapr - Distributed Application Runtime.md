---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/dapr-dapr.md'
source_date: ''
source_author: Dapr / CNCF
tags:
  - source
  - dapr
  - distributed-systems
  - event-driven
  - microservices
  - pub-sub
  - state-management
  - actors
  - workflow
  - cncf
  - sidecar
---
# Dapr - Distributed Application Runtime

CNCF project providing portable distributed application building blocks via a sidecar architecture, decoupling application code from infrastructure choices.

## Summary

Dapr addresses a fundamental portability problem in distributed applications: application code becomes tightly coupled to specific infrastructure choices (Redis for state, Kafka for pub/sub, specific secret stores), making it expensive to change infrastructure or run in different environments. The sidecar model solves this by placing all infrastructure interaction in a sidecar process that the application communicates with via a stable HTTP/gRPC API, regardless of what specific infrastructure is configured behind it.

Building blocks cover the primary concerns of distributed applications: pub/sub messaging, state management, service-to-service invocation with retries, input/output bindings for triggering from external events, secrets management, distributed actors (virtual actor model), durable workflow orchestration, application configuration, and cryptographic operations. Each building block is pluggable via component configuration, allowing infrastructure to be swapped without changing application code.

The workflow building block provides durable, fault-tolerant orchestration that survives process restarts. The actor model builds on state management to provide virtual actors that are automatically activated on demand and deactivated when idle. Together these building blocks cover most distributed systems requirements without any application-level infrastructure code.

## Key Arguments

- Dapr's sidecar model decouples application code from infrastructure choices via a stable API abstraction
- Building blocks are pluggable, enabling infrastructure replacement (e.g., Redis to DynamoDB) without code changes
- The workflow building block provides durable fault-tolerant orchestration for long-running processes
- The actor model provides virtual actors built on top of Dapr's state management building block
- Cloud-agnostic portability is the primary value proposition: the same application runs on any cloud or on-premises
- Infrastructure concerns move entirely to Dapr component configuration, not application code

## Concepts Covered

- [[Dapr]] — primary treatment
- [[Sidecar Pattern]] — architectural basis for all Dapr building blocks
- [[Event-Driven Architecture]] — pub/sub building block as EDA infrastructure
- [[Actor Model Architecture]] — virtual actor support built on state management
- [[Publish-Subscribe Pattern]] — Dapr pub/sub building block
- [[Choreography vs Orchestration]] — workflow building block for orchestration
- [[Service Mesh]] — sidecar overlap and complementary positioning

## Quality Notes

Official CNCF-graduated project documentation. Dapr represents a significant pattern for portable distributed application development. Actively maintained with major cloud vendor support.
