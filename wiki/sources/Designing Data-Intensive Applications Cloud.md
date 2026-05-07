---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/articles/Designing Data-Intensive Applications- the Cloud & Doing the Right Thing.md'
source_date: ''
source_author: Gergely Orosz / Martin Kleppmann
tags:
  - source
  - data-intensive
  - cloud-native
  - ethical-engineering
  - scalability
  - kleppmann
  - ddia
---
# Designing Data-Intensive Applications Cloud

Excerpt and commentary on the updated DDIA edition, focusing on cloud-native design and the ethical dimensions of data engineering.

## Summary

This piece covers Kleppmann's updated perspective on designing data-intensive applications in the cloud era, drawing from both the revised DDIA content and Gergely Orosz's commentary. The shift to managed cloud services (RDS, DynamoDB, BigQuery, Kafka-as-a-service) fundamentally changes architectural choices: failure modes differ from bare-metal assumptions, latency profiles are different, and the operational responsibility boundary has moved. Cloud-native design means embracing ephemeral infrastructure, treating managed services as primitives, and designing for failure patterns specific to cloud environments.

The "doing the right thing" dimension of the article addresses the ethical responsibilities of engineers who design data-intensive systems. Privacy, responsible data use, and the downstream societal consequences of large-scale data infrastructure are framed not as compliance concerns but as engineering concerns. Kleppmann argues that engineers who build these systems have a professional responsibility to understand their impact.

The article also examines how cloud economics affect architectural decisions — when to use managed streaming services versus self-hosted Kafka, how to reason about data locality in a multi-region deployment, and how the abstraction layers cloud providers offer can both simplify and obscure important failure mode reasoning.

## Key Arguments

- Cloud changes assumptions about hardware failure modes; the abstractions of managed services hide failure patterns that matter architecturally
- Managed services shift architectural choices upstream: picking a service means accepting its consistency and availability characteristics
- Data-intensive applications must consider ethical implications; privacy and responsible data use are engineering concerns, not just legal ones
- Cloud-native design means embracing ephemeral infrastructure and stateless services where possible
- Distributed data systems have societal consequences that responsible engineers should reason about explicitly

## Concepts Covered

- [[Microservices Architecture]] — cloud-native decomposition changes microservices assumptions
- [[CAP Theorem]] — cloud consistency trade-offs; managed services make CAP choices for you
- [[Quality Attributes]] — reliability and ethical responsibility as quality attributes
- [[Twelve-Factor App]] — cloud-native principles; ephemeral infrastructure alignment

## Quality Notes

Useful for breadth — Kleppmann's updated perspective on cloud-native concerns and ethical dimensions — but less useful for depth on any single topic. Best read alongside the DDIA book itself or the interview source for more substantive technical content.
