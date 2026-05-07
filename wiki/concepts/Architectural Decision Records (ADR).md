---
type: concept
created: 2026-05-03
updated: 2026-05-06
sources:
  - raw/articles/Software architecture 1.md
tags:
  - architecture
  - documentation
  - decision-making
---

# Architectural Decision Records (ADR)

A short document that captures an architecturally significant decision — the context, the options considered, the choice made, and the rationale — so that the reasoning remains accessible to future team members and can be revisited when circumstances change.

## Problem / Why It Matters

Architectural decisions are frequently forgotten, not documented, or not understood, leading to repeated discussions without resolution. When key personnel leave, the intent behind structural choices evaporates ("knowledge vaporization"). Teams then waste time relitigating settled questions, accidentally undo important constraints, or accrue [[Technical Debt]] because no one knows why the original design was chosen.

Email is a particularly poor medium for architectural decisions: it is non-searchable, audience-restricted, and ephemeral. The decision and its rationale end up scattered across inboxes.

## What an ADR Contains

A typical ADR (e.g., Michael Nygard's widely-adopted template) includes:

- **Title** — short noun phrase describing the decision
- **Status** — Proposed / Accepted / Deprecated / Superseded
- **Context** — the forces at play, the problem being solved, constraints
- **Decision** — the change being made and the option selected
- **Consequences** — what becomes easier, harder, or different as a result; trade-offs accepted

Enterprise formats (IBM UMF, CapitalOne) add:
- **Available options** — alternatives that were considered
- **Quality attributes** addressed — which non-functional requirements the decision serves
- **Design rationale** — why this option was selected over alternatives

A compact alternative is the **Y-statement** format:
> "In the context of [situation], facing [concern], we decided [option], to achieve [quality], accepting [downside]."

## Why ADRs Work

- Provide a single updated source of truth, accessible in a searchable repository (wiki, git, etc.)
- Separate communication of context (for stakeholders) from the record of the decision (for the team)
- Enable future teams to understand not just what was built, but why — making it easier to evolve or replace parts of the system
- Make deliberate [[Technical Debt]] visible and time-bound

## Storage and Lifecycle

ADRs are typically stored in version control alongside the code (e.g., `docs/decisions/`) or in a team wiki. A new ADR supersedes — rather than overwrites — an old one, preserving the history of thinking.

The architect's responsibility includes:
1. Providing both technical and business justifications in each ADR
2. Ensuring decisions have tangible business value
3. Communicating decisions to relevant stakeholders with a link to the centralized record

## ADR Templates

Several standard formats exist; choose the one that fits team culture:

| Format | Structure | Best For |
|--------|-----------|----------|
| **Nygard** (original) | Title, Status, Context, Decision, Consequences | Concise, minimal overhead |
| **Y-Statement** | One sentence: "In the context of X, facing Y, we decided Z, to achieve Q, accepting R" | Rapid capture, PR comments |
| **RFC** | Motivation, Detailed Design, Drawbacks, Alternatives, Unresolved Questions | Large cross-team proposals |
| **MADR** (Markdown ADR) | Title, Status, Context, Considered Options, Decision Outcome, Pros/Cons per option | Systematic option comparison |

### ADR Lifecycle States

- **Proposed** — under discussion; not yet decided.
- **Accepted** — decision is made and in effect.
- **Deprecated** — the decision is no longer relevant (superseded technology, changed requirements).
- **Superseded** — replaced by a newer ADR; the old ADR links to its successor.

New ADRs supersede old ones — the history of thinking is preserved, not overwritten.

## Tooling

- **adr-tools** (npryce/adr-tools) — CLI for creating, listing, and linking ADRs in a `doc/adr/` directory.
- **Log4brains** — generates a browsable web knowledge base from ADR markdown files.
- **ADR Manager** (VS Code extension) — GUI for creating and editing ADRs.

## Related

- [[Technical Debt]] — ADRs make deliberate debt visible and reversible
- [[Software Architecture Overview]] — architectural decision-making is a core activity
- [[Quality Attributes]] — ADRs document which attributes a decision serves or trades off
