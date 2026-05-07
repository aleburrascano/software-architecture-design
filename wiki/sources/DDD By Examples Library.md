---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/ddd-by-examples-library.md'
source_date: ''
source_author: ddd-by-examples (GitHub organization)
tags:
  - source
  - ddd
  - strategic-design
  - tactical-design
  - context-mapping
  - subdomain
  - library-example
  - java
---
# DDD By Examples Library

GitHub repository implementing a library domain using DDD, demonstrating full strategic and tactical design from problem space analysis through implementation.

## Summary

This repository uses a library domain (book lending, catalog management, patron management) as a neutral, approachable context for demonstrating the full DDD process. The work begins with problem space analysis — identifying subdomains, classifying them as Core, Supporting, or Generic based on strategic importance — before moving to solution space design. This sequence makes explicit that strategic design precedes tactical design.

The context mapping work reveals the integration complexity between bounded contexts before a line of implementation code is written. The library's contexts interact through explicit integration patterns: context maps show which contexts use Shared Kernel, which use Customer-Supplier relationships, and which require Anti-Corruption Layers to translate between models. This makes the architectural seams explicit and reasoned.

Tactical implementation focuses on aggregate design around true invariants rather than entity relationships. The documentation explicitly addresses aggregate design decisions — why certain entities are within the same aggregate root versus separate roots — making the reasoning visible. Domain events decouple bounded contexts at runtime, matching the loose coupling specified in the context map.

## Key Arguments

- Problem space analysis (subdomain classification) must precede solution space design
- Core Domains deserve full DDD investment; Supporting and Generic Subdomains may use simpler approaches
- Context mapping reveals integration complexity before implementation, reducing architectural surprises
- Aggregate boundaries should be designed around true invariants, not entity ownership relationships
- Domain events decouple bounded contexts at runtime, enabling independent evolution
- The library domain is neutral and accessible enough to focus attention on DDD concepts rather than domain complexity

## Concepts Covered

- [[Domain-Driven Design]] — strategic and tactical example with full problem-to-solution flow
- [[Bounded Context]] — context mapping with multiple real integration patterns
- [[Aggregate]] — invariant-based design reasoning made explicit
- [[Domain Event]] — context decoupling at the tactical level
- [[DDD Building Blocks]] — practical application of all major building blocks
- [[DDD Advanced Patterns]] — subdomain classification and strategic decision-making

## Quality Notes

Excellent reference implementation with architectural documentation as valuable as the code. The explicit documentation of design decisions distinguishes this from repositories that only show the final code without rationale.
