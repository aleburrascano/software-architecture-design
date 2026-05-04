---
type: person
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://en.wikipedia.org/wiki/David_Parnas
tags:
  - people
  - software-architecture
---

# David Parnas

Canadian software engineer and academic who originated Information Hiding and modular decomposition criteria — the theoretical foundations of encapsulation, component design, and interface-based programming.

## Background

David Lorge Parnas (born 10 February 1941, Plattsburgh, New York) earned his PhD in electrical engineering from Carnegie Mellon University. He held professorships at Carnegie Mellon, the University of North Carolina at Chapel Hill, Technische Universität Darmstadt (Germany), University of Victoria, Queen's University (Kingston, Ontario), McMaster University (Hamilton, Ontario), and the University of Limerick (Ireland). He has been a Fellow of the ACM (1994), a Fellow of the IEEE (2009), and a Fellow of the Royal Society of Canada (1992). He received the Norbert Wiener Award for Social and Professional Responsibility (1987) and the IEEE Computer Society Harlan Mills Award (1999).

## Key Contributions

- **[[Information Hiding]]** — introduced in his 1972 paper "On the Criteria To Be Used in Decomposing Systems into Modules"; the principle that each module should hide a design decision that the rest of the system need not know about; directly underpins encapsulation in object-oriented programming and interface-based design
- **Modular Decomposition Criteria** — challenged the prevailing flowchart-based decomposition by arguing that modules should be defined by the secrets they hide (design decisions subject to change), not by processing steps; this framing transformed how software systems are divided into components
- **[[Modularity]]** — formalised the benefits of modular design: independent development, comprehensibility, and changeability; his decomposition criteria remain the clearest statement of what makes a module boundary good
- **Software Requirements and Specifications** — developed a tabular notation for software requirements (Parnas Tables) making requirements precise and verifiable
- **Program Families** — the concept of designing software as a family of related programs sharing as much as possible, a precursor to product-line engineering and feature-based design
- **Software Aging** — coined the "software aging" metaphor: software that is never redesigned progressively loses its structure and becomes harder to change, analogous to biological aging
- **Technical Activism** — publicly and prominently opposed the Strategic Defense Initiative ("Star Wars") in the mid-1980s, arguing that software of sufficient reliability for nuclear defense was technically impossible; one of the first high-profile cases of a computer scientist taking a public ethical stand

## Key Works

- "On the Criteria To Be Used in Decomposing Systems into Modules" (CACM, 1972) — one of the most cited papers in software engineering
- "On the Design and Development of Program Families" (IEEE Transactions on Software Engineering, 1976)
- "A Rational Design Process: How and Why to Fake It" (IEEE Transactions on Software Engineering, 1986) — with David Weiss
- "Software Aging" (ICSE, 1994)
- "Stop the Numbers Game" (CACM, 2007) — critique of publication-metric-based academic evaluation

## Influence

Parnas's 1972 paper is among the most influential in software engineering. Information hiding is the intellectual foundation of encapsulation in object-oriented languages: the private keyword in Java, C++, and C# is a direct implementation of his principle. Interface-based programming, dependency inversion, and the distinction between what a module exposes vs. what it implements all trace back to his decomposition criteria. His work on program families prefigured software product lines and modern feature-flag-based development. The phrase "the secret of a module" remains one of the clearest design heuristics available.

## Quotes

> "The purpose of software engineering is to control complexity, not to create it."

> "Every module is characterised by its knowledge of a design decision which it hides from all others."

> [!question] Unverified
> Direct quote attribution above requires confirmation against Parnas's original papers.

## Related

- [[Information Hiding]] — originated by Parnas in his 1972 CACM paper
- [[Modularity]] — Parnas defined the criteria by which modules should be decomposed
