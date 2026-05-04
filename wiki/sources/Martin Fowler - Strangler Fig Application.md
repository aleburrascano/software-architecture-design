---
type: source
created: '2026-05-03'
updated: '2026-05-03'
source_path: 'https://martinfowler.com/bliki/StranglerFigApplication.html'
source_date: '2019'
source_author: Martin Fowler
tags:
  - source
  - migration
  - modernization
  - patterns
---
# Martin Fowler - Strangler Fig Application

Martin Fowler's bliki entry naming and defining the Strangler Fig Application pattern for incrementally migrating legacy systems.

## Summary

Fowler coins the term after observing strangler fig trees in Queensland, Australia (2001). The pattern involves incrementally building new system functionality alongside legacy code, gradually migrating behavior from old to new, until the legacy system can be decommissioned entirely.

## Key Arguments

- Big-bang rewrites are risky; the Strangler Fig approach reduces risk by making progress visible and reversible.
- The biological metaphor is apt: new growth wraps the old system and eventually replaces it while the host continues functioning.
- Four activities are essential: clarify desired outcomes, identify seams, replace components incrementally, drive organizational change.
- Transitional architecture must be accepted as a valid, valuable investment — not as technical debt.
- Conway's Law applies: organizational changes must accompany technical changes.
- Start with small component replacements to learn before tackling large ones.

## Concepts Covered

- [[Strangler Fig Pattern]] — the primary concept defined in this article
- [[Modular Monolith]] — often an intermediate destination in the migration
- [[Anti-Corruption Layer Pattern]] — needed at the seam between old and new systems

## Quality Notes

Authoritative primary source for the pattern name and concept. Short and conceptual — does not provide step-by-step implementation guidance (see the Microsoft Azure Docs article for that). Fowler updated the title from "Strangler Application" to "Strangler Fig Application."
