---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://www.reactivemanifesto.org/
source_date: 2014-09-16
source_author: Jonas Bonér, Dave Farley, Roland Kuhn, Martin Thompson
tags:
  - reactive
  - architecture
  - source
---

# The Reactive Manifesto

Foundational document defining the four properties of reactive systems, published by key practitioners from the distributed systems community.

## Key Takeaways

- Motivated by modern demands: millisecond response times, 100% uptime, petabyte-scale data, mobile-to-cloud deployment.
- **Responsive**: system responds in a timely manner; foundation of usability and trust; enables quick problem detection.
- **Resilient**: stays responsive in the face of failure; achieved via replication, containment, isolation, and delegation; distinct from simple fault tolerance.
- **Elastic**: stays responsive under varying workload; scales by adding/removing resources; eliminates bottlenecks via sharding and replication.
- **Message-Driven**: relies on asynchronous message-passing; establishes boundaries between components; enables loose coupling, location transparency, and failure delegation; enables backpressure via queue monitoring.
- The four properties are interdependent and composable. They apply at all scales.

## Referenced Concepts

- [[Reactive Architecture]]
- [[Backpressure]]
- [[Reactive Architecture Overview]]

## Confidence

High — read directly from source.
