---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/joelparkerhenderson-architecture-decision-record.md'
source_date: ''
source_author: Joel Parker Henderson
tags:
  - source
  - adr
  - architecture-decision
  - documentation
  - templates
  - nygard
  - madr
---
# Architecture Decision Record - joelparkerhenderson

Comprehensive GitHub repository cataloging ADR templates, formats, examples, and tooling across multiple styles.

## Summary

This repository is the most complete publicly available collection of Architecture Decision Record resources. It covers multiple ADR formats suited to different team cultures: Nygard's original Context/Decision/Consequences format, Y-Statements (concise one-sentence decisions), RFC-style for larger decisions requiring broader review, and MADR (Markdown Architectural Decision Records) for structured lightweight documentation. Each format includes annotated examples showing how to apply the template in practice.

The repository includes real-world ADR examples covering technology choices (database selection, framework adoption), architectural pattern decisions (microservices vs. monolith, REST vs. gRPC), and team convention decisions (branching strategy, code review process). This diversity illustrates that ADRs are not limited to technology choices but capture any significant decision that future team members need to understand.

Tooling references include the adr-tools CLI for managing ADRs as files in version control, Log4brains for generating browsable ADR documentation sites, and ADR Manager for IDE integration. The emphasis on tooling reinforces the principle that ADRs should be versioned alongside code rather than stored in wikis or documentation systems separate from the codebase.

## Key Arguments

- ADRs should capture the context and reasoning behind decisions, not just the decision itself
- Different ADR formats serve different team cultures; no single format is universally correct
- ADRs should be versioned with the code they describe to preserve historical context
- Rejected alternatives are as important to document as the accepted decision
- ADR lifecycle states (Proposed → Accepted → Deprecated → Superseded) track how decisions evolve over time

## Concepts Covered

- [[Architectural Decision Records (ADR)]] — comprehensive templates, examples, and tooling reference
- [[Technical Debt]] — ADRs as a mechanism for documenting and communicating technical debt decisions
- [[Software Architecture Overview]] — decision documentation as a core architectural practice

## Quality Notes

Most comprehensive ADR template collection publicly available. Authoritative reference for ADR best practices and the most-cited resource on the topic. Joel Parker Henderson is a recognized practitioner and curator in this space.
