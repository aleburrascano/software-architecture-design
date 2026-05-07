---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/DDD Resources - Domain Language.md'
source_date: ''
source_author: Eric Evans / Domain Language Inc.
tags:
  - source
  - ddd
  - eric-evans
  - resources
  - big-blue-book
  - legacy-modernization
  - ubiquitous-language
  - ddd-reference
---
# DDD Resources Domain Language

Eric Evans' official DDD resource hub at domainlanguage.com — canonical learning path, free reference materials, and legacy modernization strategies.

## Summary

Eric Evans' Domain Language website serves as the official hub for DDD learning resources. The primary reference is the "Big Blue Book" (Domain-Driven Design: Tackling Complexity in the Heart of Software, 2003), which remains the canonical text. Alongside it, Evans provides the free DDD Reference guide — a concise pattern summary document that functions as a quick-lookup companion without the book's full explanations and examples. The Manager's Guided Tour provides a non-technical introduction to DDD concepts for stakeholders and managers who need to understand why DDD matters without the implementation detail.

The resource hub also documents four strategies for dealing with legacy systems using DDD, which are among the most practically important and under-documented DDD patterns. The Bubble Context strategy creates a new DDD bounded context that is isolated from the legacy system, gradually extracting functionality. The Autonomous Bubble goes further — it is a bubble context that has been fully isolated and can evolve independently. Exposing Legacy as Services wraps existing legacy functionality behind a service interface without restructuring the internals. The Anti-Corruption Layer wraps the legacy system with a translation layer that prevents the legacy data model and terminology from contaminating the new domain model.

The site represents Evans' own framing of his work, making it the highest-authority reference for what DDD is and what resources are canonical.

## Key Arguments

- The Big Blue Book is the primary DDD reference; the free DDD Reference guide is the recommended quick-lookup companion
- The Manager's Guided Tour provides a non-technical path for stakeholders to understand DDD value
- Bubble Context: new DDD context isolated from legacy, enabling incremental migration
- Autonomous Bubble: fully isolated bubble context that can evolve without legacy coupling
- Exposing Legacy as Services: wrap legacy functionality as services without internal restructuring
- Anti-Corruption Layer: translation layer preventing legacy terminology and data model contamination of the new domain

## Concepts Covered

- [[Domain-Driven Design]] — primary resource hub; Evans' own framing of canonical DDD materials
- [[Bounded Context]] — the key strategic concept; all four legacy strategies involve bounded context thinking
- [[Ubiquitous Language]] — central to the ACL strategy: protecting the new language from legacy contamination
- [[Anti-Corruption Layer Pattern]] — legacy strategy; primary treatment in the legacy modernization context
- [[DDD Advanced Patterns]] — legacy modernization strategies as advanced DDD application

## Quality Notes

Authoritative — directly from Eric Evans. Primary source for canonical DDD resources and for the legacy modernization strategies, which are rarely covered in secondary DDD literature. Any claim about "what Evans says" should be verified against this source.
