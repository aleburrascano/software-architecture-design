---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/papers/2605.03143.md'
source_date: ''
source_author: Kiran Gopinathan, Jack Feser, Michelangelo Naim, Zenna Tavares, Eli Bingham
tags:
  - source
  - choreography
  - agent-coordination
  - game-theory
  - distributed-protocols
  - agentic-systems
  - formal-methods
---
# arXiv 2605.03143 - Pact Choreographic Language

Introduces Pact, a choreographic programming language for AI agentic ecosystems grounded in game-theoretic foundations.

## Summary

Traditional choreographic programming assumes all participants are cooperative and follow the protocol faithfully. Pact challenges this assumption by designing for AI agentic ecosystems where agents may have conflicting self-interests, private information, and untrusted counterparts. The language extends choreographic programming with game-theoretic reasoning, enabling protocol designers to specify multi-party interactions that remain correct even when participants are rational actors pursuing individual goals.

Pact models protocols as formal games, allowing the type system and compiler to verify that protocols are incentive-compatible — meaning rational agents will follow the protocol because deviating does not benefit them. This correct-by-construction approach prevents protocol violations at the language level rather than relying on runtime enforcement or trust assumptions.

The work sits at the intersection of programming language theory, distributed systems, and AI agent coordination, making it particularly relevant as AI systems increasingly interact with each other in semi-autonomous ways. It provides formal foundations for the emerging problem of designing multi-agent protocols that hold even without full mutual trust.

## Key Arguments

- Traditional choreographic programming assumes cooperative participants, an assumption that breaks down for AI agents
- AI agents may have conflicting interests, private information, and rational incentives to deviate from protocols
- Pact models protocols as formal games, enabling game-theoretic correctness verification
- Incentive-compatibility guarantees that rational agents follow the protocol because deviation does not benefit them
- Correct-by-construction design prevents protocol violations at the language level

## Concepts Covered

- [[Choreography vs Orchestration]] — choreographic programming extension with game-theoretic foundations
- [[Event-Driven Architecture]] — agent communication patterns and message-passing protocols
- [[Microservices Architecture]] — multi-party protocol design for distributed services

## Quality Notes

Academic research at the frontier of AI agent architecture. Most relevant for the Choreography vs. Orchestration concept page's theoretical foundations section. High novelty; citation count and community adoption not yet established as of the paper's filing date.
