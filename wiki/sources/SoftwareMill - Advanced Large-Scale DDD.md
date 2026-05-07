---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Advanced Large-scale DDD - SoftwareMill.md'
source_date: ''
source_author: Michał Grabowski / SoftwareMill
tags:
  - source
  - ddd
  - responsibility-layers
  - knowledge-level
  - large-scale
  - domain-stratification
  - evans
---
# SoftwareMill - Advanced Large-Scale DDD

Advanced DDD patterns for large-scale systems — Responsibility Layers and the Knowledge Level pattern extending Evans' original DDD for enterprise scale.

## Summary

The article addresses a gap in standard DDD guidance: what happens when a bounded context grows large enough that a flat domain model becomes a tangled mess? SoftwareMill introduces two advanced structural patterns — Responsibility Layers and the Knowledge Level — as the answer. Both patterns come from the DDD literature (Evans' "Domain-Driven Design" and Fowler's "Analysis Patterns" respectively) but are rarely discussed in introductory DDD material.

Responsibility Layers provide horizontal stratification of the domain into five levels: Capabilities (what the system can do), Operations (current business activities), Policies (business rules that constrain operations), Commitments (obligations to outside parties), and Decision Support (analytics and reporting). Higher layers express intent and business meaning; lower layers express mechanism and implementation. A transport/logistics domain is used as the running example, demonstrating how a delivery scheduling system maps onto these layers.

The Knowledge Level pattern (from Fowler's "Analysis Patterns") introduces a two-level model: a base level containing runtime instances (actual deliveries, vehicles, routes) and a meta level containing the configurable rules that govern them (delivery types, pricing models, route constraints). This separation allows business rules to be changed at runtime without code changes — the meta level is data-driven configuration, not hard-coded logic.

## Key Arguments

- Single-level domain models become tangled in large systems; horizontal stratification prevents accidental coupling between different kinds of domain logic
- Responsibility Layers: higher layers express intent (what should happen), lower layers express mechanism (how it happens); dependencies only flow downward
- Knowledge Level separates runtime instances (base level) from configurable rules (meta level), allowing business rules to be changed without code deployment
- These patterns extend Evans' DDD for enterprise-scale systems where a flat model would become a big ball of mud
- The transport/logistics domain demonstrates how these patterns interact: scheduling policies live at the Policy layer, specific shipments at the Operations layer, pricing rules at the Knowledge Level meta layer

## Concepts Covered

- [[Responsibility Layers]] — primary treatment; definition, layers, and transport domain example
- [[Knowledge Level Pattern]] — primary treatment; two-level model and runtime configurability
- [[Domain-Driven Design]] — large-scale extension of Evans' foundational patterns
- [[Bounded Context]] — layer organization within and across bounded contexts
- [[DDD Advanced Patterns]] — this source is a primary reference for the advanced patterns topic

## Quality Notes

Excellent treatment of under-documented DDD patterns that are rarely covered in introductory material. The transport/logistics domain example is well chosen. References Evans' "Analysis Patterns" as the original source for Knowledge Level — following that reference provides mathematical rigor.
