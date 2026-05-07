---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/patriceckhart-cqrskit.md'
source_date: ''
source_author: Patrick Eckhart
tags:
  - source
  - cqrs
  - event-sourcing
  - typescript
  - event-upcasting
  - schema-evolution
  - read-models
  - projections
  - decorator-api
---
# CQRSKit - TypeScript CQRS ES Framework

TypeScript CQRS and Event Sourcing framework with first-class support for event upcasting, schema evolution, and Given-When-Then testing fixtures.

## Summary

CQRSKit addresses a common gap in CQRS/ES frameworks: schema evolution. As systems evolve, the structure of historical events must change without rewriting the event store. CQRSKit treats event upcasting as a first-class framework concern — upcasters transform older event versions to the current version on read, so aggregate state rebuilding and projection processing always work against the current schema without requiring event store migration.

The decorator-based API reduces the boilerplate typically associated with CQRS implementation in TypeScript. Command handlers, event handlers, and projections are declared with decorators, and the framework wires them together at startup. This keeps handler code focused on business logic rather than infrastructure registration.

Read model projections are explicitly separated from aggregate logic in the framework design. Projections subscribe to event streams and build query-optimized representations, with the framework managing the projection offset tracking and replay infrastructure. Event partitioning enables horizontal scaling of projection processing by distributing events across partitions based on aggregate ID or other criteria. Given-When-Then testing fixtures make aggregate behavior specification and testing natural, supporting the practice of specifying aggregate behavior as event sequences before implementing.

## Key Arguments

- Decorator-based API reduces boilerplate for command handlers, event handlers, and projections
- Event upcasting as a first-class concept enables schema evolution without rewriting event store history
- Read model projections are explicitly separated from aggregate logic to reinforce the read/write separation
- Event partitioning enables horizontal scaling of projection processing for high-volume event streams
- Given-When-Then testing fixtures make aggregate behavior specification and testing natural and expressive

## Concepts Covered

- [[CQRS]] — TypeScript implementation with explicit command/query separation
- [[Event Sourcing]] — aggregate persistence via event streams with replay support
- [[Event Upcasting]] — primary practical reference for schema evolution in event-sourced systems
- [[Event Sourcing and CQRS Integration]] — combined framework showing how the patterns work together

## Quality Notes

Good reference for event upcasting patterns, which are underrepresented in the general CQRS/ES literature. TypeScript-specific but the upcasting patterns are transferable to any language. Useful for teams facing schema evolution in event-sourced systems.
