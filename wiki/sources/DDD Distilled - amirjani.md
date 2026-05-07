---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/amirjani-domain-driven-design-distilled.md'
source_date: ''
source_author: amirjani (GitHub)
tags:
  - source
  - ddd
  - ubiquitous-language
  - bounded-context
  - subdomain
  - aggregate
  - event-storming
  - context-mapping
  - layered-architecture
---
# DDD Distilled - amirjani

Comprehensive DDD guide covering all three subdomain types, all context mapping relationships, all tactical building blocks, and Event Storming as the primary discovery technique.

## Summary

This guide provides end-to-end coverage of DDD from strategic classification through tactical implementation. The strategic section covers all three subdomain types with implementation guidance: Core Domain (invest in DDD; this is your competitive advantage), Supporting Subdomain (simpler patterns acceptable; may outsource), and Generic Subdomain (buy or use open-source; don't build). This classification prevents teams from over-engineering supporting and generic concerns.

Context mapping receives thorough treatment with all major integration relationships: Shared Kernel (shared model between two contexts), Customer-Supplier (one context's output drives another's input), Conformist (downstream accepts upstream's model), Anti-Corruption Layer (downstream translates upstream's model into its own), and Open Host Service (upstream provides a published API for multiple downstreams). Each relationship carries different coupling implications and maintenance costs.

The tactical section covers all building blocks with their specific responsibilities: Entity (identity continuity), Value Object (equality by attributes, immutable), Aggregate (consistency boundary with a root), Domain Service (stateless domain logic spanning aggregates), Domain Event (something that happened), Repository (aggregate lifecycle management), Application Service (use case coordination), and Factory (complex construction). Event Storming is presented as the primary workshop technique for discovering these building blocks collaboratively before implementation begins.

## Key Arguments

- Subdomain classification drives strategic DDD investment decisions: only Core Domains warrant full DDD complexity
- Context Maps are the primary tool for visualizing and reasoning about integration architecture
- All tactical building blocks have specific, non-overlapping responsibilities; conflating them produces muddled models
- Event Storming is the most effective way to discover domain events and aggregate boundaries collaboratively
- CQRS and hexagonal architecture complement DDD tactical patterns by reinforcing boundary enforcement

## Concepts Covered

- [[Domain-Driven Design]] — comprehensive strategic and tactical guide
- [[Bounded Context]] — strategic design with all major context mapping relationships
- [[Aggregate]] — tactical patterns with invariant-based design guidance
- [[Event Storming]] — discovery technique with workshop structure
- [[DDD Building Blocks]] — complete building block coverage with responsibilities
- [[DDD Advanced Patterns]] — subdomain classification and strategic design

## Quality Notes

Well-structured comprehensive reference for practitioners learning DDD. Covers the full breadth in a single source. Good complement to the canonical books for practitioners who want an accessible single-document overview before diving deeper.
