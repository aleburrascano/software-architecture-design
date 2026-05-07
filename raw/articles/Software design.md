---
title: "Software design"
source: "https://en.wikipedia.org/wiki/Software_design"
author:
  - "[[Wikipedia]]"
published:
created: 2026-05-03
description:
tags:
  - "clippings"
---
**Software design** is the process of [conceptualizing](https://en.wikipedia.org/wiki/Design "Design") how a [software system](https://en.wikipedia.org/wiki/Software_system "Software system") will work before it is [implemented](https://en.wikipedia.org/wiki/Implementation "Implementation") or modified.[^1] Software design also refers to the direct result of the design process – the concepts of how the software will work which may be formally [documented](https://en.wikipedia.org/wiki/Software_documentation "Software documentation") or may be maintained less formally, including via [oral tradition](https://en.wikipedia.org/wiki/Oral_tradition "Oral tradition").

The design process enables a designer to model aspects of a software system before it exists with the intent of making the effort of writing the code more efficiently. Creativity, past experience, a sense of what makes "good" software, and a commitment to quality are success factors for a competent design.

A software design can be compared to an [architected plan for a house](https://en.wikipedia.org/wiki/Blueprint "Blueprint"). High-level plans represent the totality of the house (e.g., a three-dimensional rendering of the house). Lower-level plans provide guidance for constructing each detail (e.g., the plumbing lay). Similarly, the software design model provides a variety of views of the proposed software solution.

## Part of the overall process

In terms of the [waterfall development process](https://en.wikipedia.org/wiki/Waterfall_model "Waterfall model"), software design is the activity that occurs after [requirements analysis](https://en.wikipedia.org/wiki/Software_requirements_analysis "Software requirements analysis") and before [coding](https://en.wikipedia.org/wiki/Computer_programming "Computer programming").[^2] Requirements analysis determines *what* the system needs to do without determining *how* it will do it, and thus, multiple designs can be imagined that satisfy the requirements. The design can be created while coding, without a plan or requirements analysis,[^3] but for more complex projects this is less feasible. Completing a design prior to coding allows for [multidisciplinary](https://en.wikipedia.org/wiki/Academic_discipline#Multidisciplinary "Academic discipline") designers and [subject-matter experts](https://en.wikipedia.org/wiki/Subject-matter_expert "Subject-matter expert") to collaborate with programmers to produce software that is useful and technically sound.

Sometimes, a [simulation](https://en.wikipedia.org/wiki/Simulation "Simulation") or [prototype](https://en.wikipedia.org/wiki/Prototype "Prototype") is created to model the system in an effort to determine a valid and good design.

## Code as design

A common point of confusion with the term *design* in software is that the process applies at multiple levels of [abstraction](https://en.wikipedia.org/wiki/Abstraction "Abstraction") such as a high-level [software architecture](https://en.wikipedia.org/wiki/Software_architecture "Software architecture") and lower-level components, [functions](https://en.wikipedia.org/wiki/Function_\(computing\) "Function (computing)") and [algorithms](https://en.wikipedia.org/wiki/Algorithm "Algorithm"). A relatively formal process may occur at high levels of abstraction but at lower levels, the design process is almost always less formal where the only artifact of design may be the code itself. To the extent that this is true, *software design* refers to the design of the design. [Edsger W. Dijkstra](https://en.wikipedia.org/wiki/Edsger_W._Dijkstra "Edsger W. Dijkstra") referred to this layering of semantic levels as the "radical novelty" of computer programming,[^4] and [Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth "Donald Knuth") used his experience writing [TeX](https://en.wikipedia.org/wiki/TeX "TeX") to describe the futility of attempting to design a program prior to implementing it:

> T <sub>E</sub> X would have been a complete failure if I had merely specified it and not participated fully in its initial implementation. The process of implementation constantly led me to unanticipated questions and to new insights about how the original specifications could be improved.[^5]

## Artifacts

![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f0/Software_watchdog_timer_flowchart.svg/250px-Software_watchdog_timer_flowchart.svg.png)

Software flowchart example

A design process may include the production of art [Software design documentation](https://en.wikipedia.org/wiki/Software_design_document "Software design document") such as [flow chart](https://en.wikipedia.org/wiki/Flow_chart "Flow chart"), [use case](https://en.wikipedia.org/wiki/Use_case "Use case"), [Pseudocode](https://en.wikipedia.org/wiki/Pseudocode "Pseudocode"), [Unified Modeling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language "Unified Modeling Language") model and other [Fundamental modeling concepts](https://en.wikipedia.org/wiki/Fundamental_modeling_concepts "Fundamental modeling concepts"). For [user centered](https://en.wikipedia.org/wiki/User_centered_design "User centered design") software, design may involve [user experience design](https://en.wikipedia.org/wiki/User_experience_design "User experience design") yielding a [storyboard](https://en.wikipedia.org/wiki/Storyboard "Storyboard") to help determine those specifications. Documentation may be reviewed to allow constraints, specifications and even requirements to be adjusted prior to coding.

## Iterative design

Software systems inherently deal with uncertainties, and the size of software components can significantly influence a system's outcomes, both positively and negatively. Neal Ford and Mark Richards propose an iterative approach to address the challenge of identifying and right-sizing components. This method emphasizes continuous refinement as teams develop a more nuanced understanding of system behavior and requirements.[^6]

The approach typically involves a cycle with several stages:[^6]

- A high-level partitioning strategy is established, often categorized as technical or domain-based. Guidelines for the smallest meaningful deployable unit, referred to as "quanta," are defined. While these foundational decisions are made early, they may be revisited later in the cycle if necessary.
- Initial components are identified based on the established strategy.
- Requirements are assigned to the identified components.
- The roles and responsibilities of each component are analyzed to ensure clarity and minimize overlap.
- Architectural characteristics, such as scalability, fault tolerance, and maintainability, are evaluated.
- Components may be restructured based on feedback from development teams.

This cycle serves as a general framework and can be adapted to different domains.

## Design principles

Design principles enable a software engineer to navigate the design process. Davis [^7] suggested principles which have been refined over time as:

The design process should not suffer from "tunnel vision"

A good designer should consider alternative approaches, judging each based on the requirements of the problem, the resources available to do the job.

The design should be traceable to the analysis model

Because a single element of the design model can often be traced back to multiple requirements, it is necessary to have a means for tracking how requirements have been satisfied by the design model.

The design should not reinvent the wheel

Systems are constructed using a set of design patterns, many of which have likely been encountered before. These patterns should always be chosen as an alternative to reinvention. Time is short and resources are limited; design time should be invested in representing (truly new) ideas by integrating patterns that already exist (when applicable).

The design should "minimize the intellectual distance" between the software and the problem as it exists in the real world

That is, the structure of the software design should, whenever possible, mimic the structure of the problem domain.

The design should exhibit uniformity and integration

A design is uniform if it appears fully coherent. In order to achieve this outcome, rules of style and format should be defined for a design team before design work begins. A design is integrated if care is taken in defining interfaces between design components.

The design should be structured to accommodate change

The design concepts discussed in the next section enable a design to achieve this principle.

The design should be structured to degrade gently, even when aberrant data, events, or operating conditions are encountered

Well-designed software should never "bomb"; it should be designed to accommodate unusual circumstances, and if it must terminate processing, it should do so in a graceful manner.

Design is not coding, coding is not design

Even when detailed procedural designs are created for program components, the level of abstraction of the design model is higher than the source code. The only design decisions made at the coding level should address the small implementation details that enable the procedural design to be coded.

The design should be assessed for quality as it is being created, not after the fact

A variety of design concepts and design measures are available to assist the designer in assessing quality throughout the development process.

The design should be reviewed to minimize conceptual (semantic) errors

There is sometimes a tendency to focus on minutiae when the design is reviewed, missing the forest for the trees. A design team should ensure that major conceptual elements of the design (omissions, ambiguity, inconsistency) have been addressed before worrying about the syntax of the design model.

## Design concepts

Design concepts provide a designer with a foundation from which more sophisticated methods can be applied. Design concepts include:

[Abstraction](https://en.wikipedia.org/wiki/Abstraction_\(computer_science\) "Abstraction (computer science)")

Reducing the information content of a concept or an observable phenomenon, typically to retain only information that is relevant for a particular purpose. It is an act of Representing essential features without including the background details or explanations.

[Architecture](https://en.wikipedia.org/wiki/Architecture "Architecture")

The overall structure of the software and the ways in which that structure provides conceptual integrity for a system. Good software architecture will yield a good return on investment with respect to the desired outcome of the project, e.g. in terms of performance, quality, schedule and cost.

Control hierarchy

A program structure that represents the organization of a program component and implies a hierarchy of control.

[Data structure](https://en.wikipedia.org/wiki/Data_structure "Data structure")

Representing the logical relationship between elements of data.

[Design pattern](https://en.wikipedia.org/wiki/Design_pattern_\(computer_science\) "Design pattern (computer science)")

A designer may identify a design aspect of the system that has solved in the past. The reuse of such patterns can increase software development velocity.[^8]

[Information hiding](https://en.wikipedia.org/wiki/Information_hiding "Information hiding")

Modules should be specified and designed so that information contained within a module is inaccessible to other modules that have no need for such information.

[Modularity](https://en.wikipedia.org/wiki/Modularity "Modularity")

Dividing the solution into parts (modules).

[Refinement](https://en.wikipedia.org/wiki/Program_refinement "Program refinement")

The process of elaboration. A hierarchy is developed by decomposing a macroscopic statement of function in a step-wise fashion until programming language statements are reached. In each step, one or several instructions of a given program are decomposed into more detailed instructions. Abstraction and Refinement are complementary concepts.

Software procedure

Focuses on the processing of each module individually.

Structural partitioning

The program structure can be divided horizontally and vertically. Horizontal partitions define separate branches of modular hierarchy for each major program function. Vertical partitioning suggests that control and work should be distributed top-down in the program structure.

[Grady Booch](https://en.wikipedia.org/wiki/Grady_Booch "Grady Booch") mentions [abstraction](https://en.wikipedia.org/wiki/Abstraction_\(computer_science\) "Abstraction (computer science)"), [encapsulation](https://en.wikipedia.org/wiki/Encapsulation_\(computer_programming\) "Encapsulation (computer programming)"), [modularization](https://en.wikipedia.org/wiki/Modularity "Modularity"), and [hierarchy](https://en.wikipedia.org/wiki/Hierarchy "Hierarchy") as fundamental software design principles.[^9] The phrase *principles of hierarchy, abstraction, modularization, and encapsulation* (PHAME) refers to these principles.[^10]

## Design considerations

There are many aspects to consider in the design of software. The importance of each should reflect the goals and expectations to which the software is being created. Notable aspects include:

[Compatibility](https://en.wikipedia.org/wiki/Software_compatibility "Software compatibility")

The software is able to operate with other products that are designed for interoperability with another product. For example, a piece of software may be backward-compatible with an older version of itself.

[Extensibility](https://en.wikipedia.org/wiki/Extensibility "Extensibility")

New capabilities can be added to the software without major changes to the underlying architecture.

[Fault-tolerance](https://en.wikipedia.org/wiki/Fault-tolerance "Fault-tolerance")

The software is resistant to and able to recover from component failure.

[Maintainability](https://en.wikipedia.org/wiki/Maintainability "Maintainability")

A measure of how easily bug fixes or functional modifications can be accomplished. High maintainability can be the product of modularity and extensibility.

[Modularity](https://en.wikipedia.org/wiki/Modularity "Modularity")

The resulting software comprises well defined, independent components which leads to better maintainability. The components could be then implemented and tested in isolation before being integrated to form a desired software system. This allows division of work in a software development project.

[Overhead](https://en.wikipedia.org/wiki/Overhead_\(engineering\) "Overhead (engineering)")

How the consumption of resources required for overhead impacts the resources needed to achieve system requirements.

[Performance](https://en.wikipedia.org/wiki/Computer_performance "Computer performance")

The software performs its tasks within a time-frame that is acceptable for the user, and does not require too much memory.

[Portability](https://en.wikipedia.org/wiki/Software_portability "Software portability")

The software should be usable across a number of different conditions and environments.

[Reliability](https://en.wikipedia.org/wiki/Software_durability "Software durability")

The software is able to perform a required function under stated conditions for a specified period of time.

[Reusability](https://en.wikipedia.org/wiki/Reusability "Reusability")

The ability to use some or all of the aspects of the preexisting software in other projects with little to no modification.

[Robustness](https://en.wikipedia.org/wiki/Fault-tolerant_system "Fault-tolerant system")

The software is able to operate under stress or tolerate unpredictable or invalid input. For example, it can be designed with resilience to low memory conditions.

[Scalability](https://en.wikipedia.org/wiki/Scalability "Scalability")

The software adapts well to increasing data or added features or number of users. According to Marc Brooker: "a system is scalable in the range where [marginal cost](https://en.wikipedia.org/wiki/Marginal_cost "Marginal cost") of additional workload is nearly constant." [Serverless](https://en.wikipedia.org/wiki/Serverless_computing "Serverless computing") technologies fit this definition but you need to consider total cost of ownership not just the infra cost.[^11]

[Security](https://en.wikipedia.org/wiki/Computer_security "Computer security")

The software is able to withstand and resist hostile acts and influences.

[Usability](https://en.wikipedia.org/wiki/Usability "Usability")

The software [user interface](https://en.wikipedia.org/wiki/User_interface "User interface") must be usable for its target user/audience. Default values for the parameters must be chosen so that they are a good choice for the majority of the users.[^12]

## Modeling language

A [modeling language](https://en.wikipedia.org/wiki/Modeling_language "Modeling language") can be used to express information, knowledge or systems in a structure that is defined by a consistent set of rules. These rules are used for interpretation of the components within the structure. A modeling language can be graphical or textual. Examples of graphical modeling languages for software design include:

[Architecture description language](https://en.wikipedia.org/wiki/Architecture_description_language "Architecture description language") (ADL)

A language used to describe and represent the [software architecture](https://en.wikipedia.org/wiki/Software_architecture "Software architecture") of a [software system](https://en.wikipedia.org/wiki/Software_system "Software system").

[Business Process Modeling Notation](https://en.wikipedia.org/wiki/Business_Process_Modeling_Notation "Business Process Modeling Notation") (BPMN)

An example of a [Process Modeling](https://en.wikipedia.org/wiki/Process_Modeling "Process Modeling") language.

[EXPRESS](https://en.wikipedia.org/wiki/EXPRESS_\(data_modeling_language\) "EXPRESS (data modeling language)") and EXPRESS-G (ISO 10303-11)

An international standard general-purpose [data modeling](https://en.wikipedia.org/wiki/Data_modeling "Data modeling") language.

[Extended Enterprise Modeling Language](https://en.wikipedia.org/wiki/Extended_Enterprise_Modeling_Language "Extended Enterprise Modeling Language") (EEML)

Commonly used for business process modeling across a number of layers.

[Flowchart](https://en.wikipedia.org/wiki/Flowchart "Flowchart")

Schematic representations of algorithms or other step-wise processes.

[Fundamental Modeling Concepts](https://en.wikipedia.org/wiki/Fundamental_Modeling_Concepts "Fundamental Modeling Concepts") (FMC)

A modeling language for software-intensive systems.

[IDEF](https://en.wikipedia.org/wiki/IDEF "IDEF")

A family of modeling languages, the most notable of which include [IDEF0](https://en.wikipedia.org/wiki/IDEF0 "IDEF0") for functional modeling, [IDEF1X](https://en.wikipedia.org/wiki/IDEF1X "IDEF1X") for information modeling, and [IDEF5](https://en.wikipedia.org/wiki/IDEF5 "IDEF5") for modeling [ontologies](https://en.wikipedia.org/wiki/Ontology_\(information_science\) "Ontology (information science)").

[Jackson Structured Programming](https://en.wikipedia.org/wiki/Jackson_Structured_Programming "Jackson Structured Programming") (JSP)

A method for structured programming based on correspondences between data stream structure and program structure.

LePUS3

An object-oriented visual Design Description Language and a [formal specification](https://en.wikipedia.org/wiki/Formal_specification "Formal specification") language that is suitable primarily for modeling large object-oriented ([Java](https://en.wikipedia.org/wiki/Java_\(programming_language\) "Java (programming language)"), [C++](https://en.wikipedia.org/wiki/C%2B%2B "C++"), [C#](https://en.wikipedia.org/wiki/C_Sharp_\(programming_language\) "C Sharp (programming language)")) programs and [design patterns](https://en.wikipedia.org/wiki/Design_patterns "Design patterns").

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Component-based-Software-Engineering-example2.png/250px-Component-based-Software-Engineering-example2.png)

UML component diagram example

[Unified Modeling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language "Unified Modeling Language") (UML)

A general modeling language to describe software both structurally and behaviorally. It has a graphical notation and allows for extension with a [Profile (UML)](https://en.wikipedia.org/wiki/Profile_\(UML\) "Profile (UML)").

[Alloy (specification language)](https://en.wikipedia.org/wiki/Alloy_\(specification_language\) "Alloy (specification language)")

A general purpose specification language for expressing complex structural constraints and behavior in a software system. It provides a concise language base on first-order relational logic.

[Systems Modeling Language](https://en.wikipedia.org/wiki/Systems_Modeling_Language "Systems Modeling Language") (SysML)

A [general-purpose modeling](https://en.wikipedia.org/wiki/General-purpose_modeling "General-purpose modeling") language for systems engineering.

[^1]: Ralph, P. and Wand, Y. (2009). A proposal for a formal definition of the design concept. In Lyytinen, K., Loucopoulos, P., [Mylopoulos, J.](https://en.wikipedia.org/wiki/John_Mylopoulos "John Mylopoulos"), and Robinson, W., editors, Design Requirements Workshop (LNBIP 14), pp. 103–136. Springer-Verlag, p. 109 [doi](https://en.wikipedia.org/wiki/Doi_\(identifier\) "Doi (identifier)"):[10.1007/978-3-540-92966-6\_6](https://doi.org/10.1007%2F978-3-540-92966-6_6).

[^2]: Freeman, Peter; David Hart (2004). "A Science of design for software-intensive systems". *Communications of the ACM*. **47** (8): 19–21 \[20\]. [doi](https://en.wikipedia.org/wiki/Doi_\(identifier\) "Doi (identifier)"):[10.1145/1012037.1012054](https://doi.org/10.1145%2F1012037.1012054). [S2CID](https://en.wikipedia.org/wiki/S2CID_\(identifier\) "S2CID (identifier)") [14331332](https://api.semanticscholar.org/CorpusID:14331332).

[^3]: Ralph, P., and Wand, Y. A Proposal for a Formal Definition of the Design Concept. In, Lyytinen, K., Loucopoulos, P., Mylopoulos, J., and Robinson, W., (eds.), Design Requirements Engineering: A Ten-Year Perspective: Springer-Verlag, 2009, pp. 103-136

[^4]: [Dijkstra, E. W.](https://en.wikipedia.org/wiki/Edsger_Dijkstra "Edsger Dijkstra") (1988). ["On the cruelty of really teaching computing science"](http://www.cs.utexas.edu/~EWD/transcriptions/EWD10xx/EWD1036.html). Retrieved 2014-01-10.

[^5]: [Knuth, Donald E.](https://en.wikipedia.org/wiki/Donald_Knuth "Donald Knuth") (1989). ["Notes on the Errors of TeX"](http://www.tug.org/TUGboat/tb10-4/tb26knut.pdf) (PDF).

[^6]: *Fundamentals of Software Architecture: An Engineering Approach*. O'Reilly Media. 2020. [ISBN](https://en.wikipedia.org/wiki/ISBN_\(identifier\) "ISBN (identifier)") [978-1492043454](https://en.wikipedia.org/wiki/Special:BookSources/978-1492043454 "Special:BookSources/978-1492043454").

[^7]: Davis, A:"201 Principles of Software Development", McGraw Hill, 1995.

[^8]: Judith Bishop. ["C# 3.0 Design Patterns: Use the Power of C# 3.0 to Solve Real-World Problems"](http://msdn.microsoft.com/en-us/vstudio/ff729657). C# Books from O'Reilly Media. Retrieved 2012-05-15. If you want to speed up the development of your.NET applications, you're ready for C# design patterns -- elegant, accepted and proven ways to tackle common programming problems.

[^9]: Booch, Grady; et al. (2004). [*Object-Oriented Analysis and Design with Applications*](http://dl.acm.org/citation.cfm?id=975416) (3rd ed.). MA, US: Addison Wesley. [ISBN](https://en.wikipedia.org/wiki/ISBN_\(identifier\) "ISBN (identifier)") [0-201-89551-X](https://en.wikipedia.org/wiki/Special:BookSources/0-201-89551-X "Special:BookSources/0-201-89551-X"). Retrieved 30 January 2015.

[^10]: Suryanarayana, Girish (November 2014). *Refactoring for Software Design Smells*. Morgan Kaufmann. p. 258. [ISBN](https://en.wikipedia.org/wiki/ISBN_\(identifier\) "ISBN (identifier)") [978-0128013977](https://en.wikipedia.org/wiki/Special:BookSources/978-0128013977 "Special:BookSources/978-0128013977").

[^11]: *Building Serverless Applications on Knative*. O'Reilly Media. [ISBN](https://en.wikipedia.org/wiki/ISBN_\(identifier\) "ISBN (identifier)") [9781098142049](https://en.wikipedia.org/wiki/Special:BookSources/9781098142049 "Special:BookSources/9781098142049").

[^12]: Carroll, John, ed. (1995). *Scenario-Based Design: Envisioning Work and Technology in System Development*. New York: John Wiley & Sons. [ISBN](https://en.wikipedia.org/wiki/ISBN_\(identifier\) "ISBN (identifier)") [0471076597](https://en.wikipedia.org/wiki/Special:BookSources/0471076597 "Special:BookSources/0471076597").

[^13]: Bell, Michael (2008). "Introduction to Service-Oriented Modeling". *Service-Oriented Modeling: Service Analysis, Design, and Architecture*. Wiley & Sons. [ISBN](https://en.wikipedia.org/wiki/ISBN_\(identifier\) "ISBN (identifier)") [978-0-470-14111-3](https://en.wikipedia.org/wiki/Special:BookSources/978-0-470-14111-3 "Special:BookSources/978-0-470-14111-3").