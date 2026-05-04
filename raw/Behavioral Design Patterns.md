---
title: "Behavioral Design Patterns"
source: "https://refactoring.guru/design-patterns/behavioral-patterns"
author:
published:
created: 2026-05-03
description: "Design Patterns are typical solutions to commonly occurring problems in software design. They are blueprints that you can customize to solve a particular design problem in your code."
tags:
  - "clippings"
---
Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects.![Chain of Responsibility](https://refactoring.guru/images/patterns/cards/chain-of-responsibility-mini.png)

Chain of Responsibility

[

Lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

](https://refactoring.guru/design-patterns/chain-of-responsibility)[![Command](https://refactoring.guru/images/patterns/cards/command-mini.png)

Turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request's execution, and support undoable operations.

](https://refactoring.guru/design-patterns/command)[![Iterator](https://refactoring.guru/images/patterns/cards/iterator-mini.png)

Lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

](https://refactoring.guru/design-patterns/iterator)[![Mediator](https://refactoring.guru/images/patterns/cards/mediator-mini.png)

Lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

](https://refactoring.guru/design-patterns/mediator)[![Memento](https://refactoring.guru/images/patterns/cards/memento-mini.png)

Lets you save and restore the previous state of an object without revealing the details of its implementation.

](https://refactoring.guru/design-patterns/memento)[![Observer](https://refactoring.guru/images/patterns/cards/observer-mini.png)

Lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they're observing.

](https://refactoring.guru/design-patterns/observer)[![State](https://refactoring.guru/images/patterns/cards/state-mini.png)

Lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

](https://refactoring.guru/design-patterns/state)[![Strategy](https://refactoring.guru/images/patterns/cards/strategy-mini.png)

Lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

](https://refactoring.guru/design-patterns/strategy)[![Template Method](https://refactoring.guru/images/patterns/cards/template-method-mini.png)

Template Method

Defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

](https://refactoring.guru/design-patterns/template-method)[![Visitor](https://refactoring.guru/images/patterns/cards/visitor-mini.png)

Lets you separate algorithms from the objects on which they operate.

](https://refactoring.guru/design-patterns/visitor)