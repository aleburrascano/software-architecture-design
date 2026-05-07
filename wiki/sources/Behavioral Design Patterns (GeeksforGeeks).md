---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: raw/articles/Behavioral Design Patterns.md
source_date:
source_author: GeeksforGeeks / refactoring.guru
tags: [design-pattern, behavioral, source]
---

# Behavioral Design Patterns (GeeksforGeeks)

## Summary

A compact reference covering all 10 Gang of Four behavioral design patterns. The raw clip (from refactoring.guru via the GeeksforGeeks ingest pipeline) provides one-sentence definitions for each pattern alongside card images. The deeper GeeksforGeeks articles (linked in the ingest instruction) cover problem/solution detail, UML diagrams, and code examples — those pages were not fetched during this ingest session due to permission restrictions.

> [!question] Unverified
> WebFetch was denied during ingest. The concept pages in `wiki/concepts/` draw on trained knowledge of the GoF canonical descriptions, not the live GeeksforGeeks article content. They should be considered accurate against the GoF book and common consensus, but specific code examples, diagrams, or phrasings from the live GeeksforGeeks pages were not incorporated.

## Concepts Covered

- [[Observer Pattern]]
- [[Strategy Pattern]]
- [[Command Pattern]]
- [[Chain of Responsibility Pattern]]
- [[Template Method Pattern]]
- [[Iterator Pattern]]
- [[State Pattern]]
- [[Mediator Pattern]]
- [[Memento Pattern]]
- [[Visitor Pattern]]

## Key Arguments / Claims

The raw source defines behavioral patterns as being "concerned with algorithms and the assignment of responsibilities between objects." Each pattern is described in one sentence:

- **Chain of Responsibility** — lets you pass requests along a chain of handlers; each handler decides to process or pass on.
- **Command** — turns a request into a stand-alone object; enables queuing, delay, and undo.
- **Iterator** — lets you traverse a collection without exposing its underlying representation.
- **Mediator** — reduces chaotic dependencies by forcing objects to collaborate only via a mediator.
- **Memento** — lets you save and restore an object's previous state without revealing implementation details.
- **Observer** — defines a subscription mechanism to notify multiple objects about events.
- **State** — lets an object alter its behavior when its internal state changes; appears to change its class.
- **Strategy** — defines a family of algorithms, encapsulates each, and makes them interchangeable.
- **Template Method** — defines the skeleton of an algorithm in a superclass; subclasses override specific steps.
- **Visitor** — lets you separate algorithms from the objects on which they operate.

## Quality Notes

- The raw clip is a lightweight card-grid page (refactoring.guru / GeeksforGeeks). It covers definitions but no code or deep analysis.
- The GeeksforGeeks per-pattern articles (observer-pattern-set-1-introduction, strategy-pattern-set-1, etc.) would add Java/Python code samples and UML diagrams; these remain unfetched and should be retrieved in a future refresh session.
- Source date is not present in the raw frontmatter; `source_date` left blank.
