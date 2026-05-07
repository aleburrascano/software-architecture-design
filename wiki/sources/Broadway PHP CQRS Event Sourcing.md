---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Broadway-examples at Master · Broadway-broadway.md'
source_date: ''
source_author: qandidate-labs
tags:
  - source
  - cqrs
  - event-sourcing
  - broadway
  - php
  - framework
  - testing-helpers
---
# Broadway PHP CQRS Event Sourcing

GitHub examples repository for Broadway, a PHP infrastructure library for CQRS and Event Sourced applications — demonstrating the infrastructure separation pattern.

## Summary

Broadway is a PHP library developed by qandidate-labs that provides the infrastructure plumbing for CQRS and Event Sourcing, allowing developers to focus on domain logic rather than persistence and messaging infrastructure. The examples repository demonstrates the key Broadway abstractions through working code: command handlers receive commands from the command bus and invoke aggregate methods; event sourced aggregates record domain events and reconstruct state by applying them; projections consume events and build read models; the event store persists and replays event histories.

The testing helpers are a notable aspect of Broadway's design: the Given-When-Then testing style allows aggregate behavior to be specified and tested without infrastructure dependencies. "Given these past events, when this command is executed, then these new events should be raised" — the test describes behavior entirely in terms of domain events, making tests readable as business specifications and completely decoupled from storage or messaging concerns.

Broadway is historically significant as one of the earliest PHP CQRS/ES frameworks, influencing subsequent PHP domain-driven design tooling. While PHP-specific, the architectural patterns Broadway implements are directly transferable to any language and framework: the separation of command bus, event store, and projection handling is a language-neutral design.

## Key Arguments

- Broadway separates domain logic (aggregates, command handlers, projections) from infrastructure (event store, command bus, event routing)
- Command bus routes commands to handlers; this decouples command issuers from command handlers
- Event store persists domain events and replays them for aggregate reconstruction; persistence is event-centric, not state-centric
- Projections build read models by consuming events; each projection serves a specific query pattern
- Given-When-Then testing helpers enable behavior-driven aggregate testing without infrastructure dependencies
- Broadway is one of the earliest PHP CQRS/ES frameworks; its patterns are transferable to other languages

## Concepts Covered

- [[CQRS]] — PHP reference implementation; Broadway as a concrete CQRS framework
- [[Event Sourcing]] — Broadway as an Event Sourcing framework; event store and aggregate patterns
- [[Event Sourcing and CQRS Integration]] — Broadway combines both in a single framework

## Quality Notes

Useful as a concrete implementation reference demonstrating the infrastructure separation pattern. PHP-specific implementation but patterns are transferable. The Given-When-Then testing helpers are a noteworthy design that influences how aggregate behavior testing should be approached in any CQRS/ES system.
