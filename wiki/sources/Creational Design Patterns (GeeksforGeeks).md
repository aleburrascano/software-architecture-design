---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: raw/articles/Creational Design Patterns.md
source_date:
source_author: GeeksforGeeks / refactoring.guru
tags: [design-pattern, creational, source]
---

# Creational Design Patterns (GeeksforGeeks)

## Summary

A clipping from refactoring.guru (captured with GeeksforGeeks metadata) that introduces the five canonical GoF creational design patterns. The raw source is brief — essentially a catalogue card for each pattern with a one- or two-sentence description and an image link. It does not contain code, structure diagrams, or detailed analysis; it serves only as a pointer to deeper per-pattern pages on refactoring.guru.

## Concepts Covered

- [[Factory Method Pattern]] — "Provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created."
- [[Abstract Factory Pattern]] — "Lets you produce families of related objects without specifying their concrete classes."
- [[Builder Pattern]] — "Lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code."
- [[Prototype Pattern]] — "Lets you copy existing objects without making your code dependent on their classes."
- [[Singleton Pattern]] — "Lets you ensure that a class has only one instance, while providing a global access point to this instance."

Note: [[Object Pool Pattern]] and [[Lazy Initialization]] are not present in this source. They were incorporated from standard GoF-adjacent literature and the broader pattern community.

## Key Arguments / Claims

- Creational patterns "provide various object creation mechanisms, which increase flexibility and reuse of existing code."
- The five patterns are presented as a co-equal set without ranking or comparison guidance — selection rationale is left to the reader.
- Each pattern is described at the abstract/intent level only; no code, structural diagrams, or applicability heuristics are given in this source.

## Quality Notes

**Depth:** Very shallow. The raw file contains only short descriptions and image links — essentially a landing page index. All detailed content in the wiki concept pages was sourced from established GoF documentation and standard pattern literature rather than from this raw file.

**Coverage gaps:**
- No code examples.
- No When to Use / trade-off discussion.
- No discussion of relationships between patterns.
- Object Pool and Lazy Initialization are absent (they are not GoF originals but are widely recognised as creational patterns).
- No coverage of pattern variants (e.g. Double-Checked Locking Singleton, Fluent Builder, Parameterised Factory Method).

**Reliability:** refactoring.guru is a well-regarded, accurate source for GoF pattern descriptions. The definitions given are faithful to the original GoF book intent.
