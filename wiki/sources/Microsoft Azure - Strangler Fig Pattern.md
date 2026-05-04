---
type: source
created: '2026-05-03'
updated: '2026-05-03'
source_path: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig'
source_date: '2025-02-13'
source_author: Microsoft / Ovais Mehboob
tags:
  - source
  - migration
  - modernization
  - microsoft
---
# Microsoft Azure - Strangler Fig Pattern

Microsoft Azure Architecture Center documentation for the Strangler Fig Pattern.

## Summary

Step-by-step documentation of the Strangler Fig pattern showing how to incrementally migrate a legacy system using a façade/proxy. Includes a concrete database migration example with shadow writes and a detailed walkthrough of the four migration phases. Also includes Azure WAF pillar guidance.

## Key Arguments

- Introduce a façade (proxy) early that initially routes all requests to legacy.
- Iteratively migrate features to the new system; the façade shifts routing accordingly.
- After full migration, decommission the legacy system; optionally retain the façade as an adapter.
- Critical considerations: concurrent data store access, façade becoming a bottleneck or SPOF, keeping façade routing in sync with actual migration state.
- Database migration example: shadow writes (new system writes to both DBs), validation via read comparison, then cutover to new DB as system of record.

## Concepts Covered

- [[Strangler Fig Pattern]] — comprehensive primary coverage with step-by-step guidance
- [[Anti-Corruption Layer Pattern]] — mentioned as useful alongside the façade
- [[Microservices Architecture]] — typical target architecture

## Quality Notes

High quality; official and current (updated 2025). More implementation-focused than Fowler's original bliki entry. The database migration example is particularly valuable.
