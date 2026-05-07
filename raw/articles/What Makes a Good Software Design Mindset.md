---
title: "What Makes a Good Software Design Mindset?"
source: "https://medium.com/better-programming/what-makes-a-good-software-design-mindset-677c82c8adf1"
author:
  - "[[Duy Pham]]"
published: 2020-03-17
created: 2026-05-03
description: "How to orient our thinking so we can solve any problem we face"
tags:
  - "clippings"
---
## How to orient our thinking so we can solve any problem we face

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Xi3Cg8e1wprApVD3-kesoA.jpeg)

Photo by Denise Jans on Unsplash

As a software engineer, have you been afraid of technology changes?

I have, to be honest. I’ve been lost from the first decision of which programming language I should start to learn, to which tech stack would be the best direction for my career (back end, front end, mobile or something else). Regardless of my decisions, all of them are changing rapidly every day.

I’ve read a lot. One day I came upon [Martin Fowler’s blog](https://martinfowler.com/) and found the most interesting quote for me:

> “While specifics of technology change rapidly in our profession, fundamental practices and patterns are more stable.”

I can’t agree more with this. Having a solid understanding of fundamentals has been the best direction for me. It helps me to adapt to any technology changes quickly since everything, including tech evolutions, should not go far away from fundamentals.

More specifically, I realized that software engineering is not only writing code but more about solving problems through good designs. Building a design mindset is more important than gaining any specific technical skills.

## Design Mindset

Creating software and shipping the very first release are just beginning steps in software development. Continuous adoption is even more important and challenging for any software, along with business and technology changes.

Adding functionality requires changing existing functionality. Replacing a database management system requires rebuilding the whole system, including all business logic. Fixing a small bug causes another, bigger one you don’t have a clue about. Scaling up a team makes conflicts all over the place, which slows down the efficiency of continuous integration. Such situations demonstrate bad designs, eventually making [Agile development](https://en.wikipedia.org/wiki/Agile_software_development) unrealistic.

A good design mindset means having a direction of highly maintainable, extensible, and scalable solutions by strongly believing in fundamental practices and patterns to develop software that can adapt to changes quickly, at low risk.

No matter which kind of software you’re developing (including operation systems, web pages, mobile apps, or even the new cloud computing paradigm), and no matter which programming language you’re using, there are fundamentals, principles, and design patterns that have been proven for decades.

But it’s a bit difficult to understand all of those principles at the beginning without experiencing real problems, falling into pains, and then realizing your mistakes, such as maintaining legacy code. So having a good design mindset is really important to shape your direction of learning, thinking, and eventually developing good software.

In this article, I won’t go deeply into each fundamental but will list out the most important points for each. There are many articles on the internet that have concrete examples including code demo and illustrations. In this article, you’ll find an overview of software design fundamentals.

## High Cohesion, Loose Coupling

![](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*eNFLzZOWJvg0rjPHgtd0_A.png)

a short definition of coupling and cohesion

This is the most important fundamental.

If the code that makes up some functionality is spread out all over the place in multiple modules, then it’s hard to write tests or maintain that functionality independently. If there are changes required, you will also need to adapt those changes in multiple modules, which potentially carries a risk of creating side effects. So you need high-cohesion modules that contain elements that are functionally related.

If changing one module in a program requires changing another module, then coupling exists.

If a module is tightly coupled to other modules in some way, such as module A depends on the concrete implementation of module B, then changing module B will require a lot of changes to module A as well. And what if modules C and D also depend on the implementation of module A, and so on? Tight coupling also makes modules hard to be unit-tested, reused, or composed, which are also important to make the development lifecycle safer and more efficient. Therefore, loose coupling should be the first factor to consider in measuring the design quality of any modular system.

Loose coupling means the module can be changed with fewer side effects and can even be moved or deployed to other tiers easily, increasing scalability (for example, as in [multitier architecture](https://en.wikipedia.org/wiki/Multitier_architecture) and [service-oriented architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture)).

Decoupling is also a well-known technique in software refactoring.

There are many types of coupling and cohesion that are described clearly in this [Geeks for Geeks overview](https://www.geeksforgeeks.org/software-engineering-coupling-and-cohesion/). Further down in this article, you’ll see most of the other principles are based on these fundamentals.

## Separation of Concerns

> **“Separation of concerns** (**SoC**) is a design principle for separating a computer program into distinct sections such that each section addresses a separate concern”
> 
> — [Wikipedia](https://en.wikipedia.org/wiki/Separation_of_concerns)

Perhaps you have a modular system. Here each module is a concern that should be as separate as possible. Each module should hide its detailed implementation by well-designed interfaces. Functional encapsulation protects the module from any changes in the other modules so that it can be developed, deployed, upgraded, and so on, independently, thus significantly improving the maintainability of the system.

Separation of concerns also applies in layered architectures, where the system is divided into layers (presentation, business logic, data access, etc.). Each layer has its own responsibility and only exposes its contract interfaces, whereas the concrete implementation is encapsulated and protected internally. Each layer can be unit-tested and changes adopted independently. Combined with suitable dependency rules, the business layer can be safe from any changes in the database or UI. For example, applying a new design concept that requires changing the whole presentation layer shouldn’t affect the business rules or persistent logic of the system.

In other words, we can also say that good separation of concerns = high cohesion + loose coupling.

## SOLID Principles

A set of principles that every software engineer should understand solidly was promoted by the legendary [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin) (Uncle Bob) in his 2000 paper “ [Design Principles and Design Patterns](http://www.cvc.uab.es/shared/teach/a21291/temes/object_oriented_design/materials_adicionals/principles_and_patterns.pdf).”

### Single responsibility principle (SRP)

> “A class or module should have one, and only one, reason to be changed.”

Mixing responsibilities into a single class/module is the state of low cohesion (described above). For example, a class that implements business logic as well as having persistent logic (e.g., retrieving/storing data into the database) obviously violates SRP, as changing database logic possibly creates side effects for the business logic, which is bad design.

Multi-purpose classes, especially the *god object*, are also hard to write unit tests for, or to reuse or compose.

### Open-closed principle

> “Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.”

Extending the system with more functionality requires modifying existing functionality and is horrible for any development team.

The idea is that, when new requirements are added, we should not modify existing code but extend it by using the most common technique: designing abstractions/interfaces with different implementations for different purposes that can be switched at runtime. This is called the [*strategy pattern*](https://dzone.com/articles/the-openclosed-principle).

Let’s see another use case. We run the app under automated test cases that are integrated into our CI system to ensure that functionality is working after every commit. But under tests, we don’t need to verify image-loading functionality that consumes network data and slows down the tests yet reduces the efficiency of our CI server. So do we need to adjust production code to add some testing logic, like `if "underTest" then "do not load image"`? No, the solution should be defining an interface like `ImageLoader` and providing an implementation named `OptOutImageLoader`, which does nothing for the tests using the [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) technique.

Thus, adhering to the open-closed principle is very important to make the system highly extensible at low risk.

### Liskov substitution principle (LSP)

> “Subclass/derived classes should be substitutable for their base/parent class.”

In object-oriented programming, if a child class violates the contract or changes the behavior of its parent class, then it’s un-substitutable for the parent class. This introduces code smell — deep problems in the system that aren’t clearly visible at the beginning but appear later in the maintenance stage.

For example, we define an abstraction of `Bird` (abstract class) that can `fly` (abstract method). All the bird’s child classes, such as `Pigeon`, `Eagle`, and so on, implement `fly` method since they can fly in their own way differently. An exception is `Penguin`, which throws an `Exception` indicating that it can’t fly. Somewhere in the codebase, we execute `fly()` method on a list of birds. If the list doesn’t contain `Penguin`, the app can run without any problem. But if one day we add `Penguin` into the list, it will crash. We have to try to catch the exception that introduces coupling to the concrete implementation of `Penguin` class or violates the open-closed principle above.

<iframe src="https://medium.com/media/010f53affe384c08e6c1489d756a6f07" allowfullscreen="" frameborder="0" height="287" width="680" title="LspViolationExample.kt"></iframe>

An example of the LSP violation

We probably have the wrong abstraction of `Bird` since not all bird breeds can fly. So `fly` should not be an abstract method in `Bird` but could be in the `Flyable` interface.

## Get Duy Pham’s stories in your inbox

Join Medium for free to get updates from this writer.

This contract violation is often introduced when we extend or modify an existing system whose abstraction wasn’t designed well. So LSP reminds us that the abstraction design is important.

### Interface segregation principle

> “Clients should not be forced to implement methods they do not use.”

We don’t really see a big problem if a class implements an interface with a blank method containing only `//do nothing` comment.

But the real problem is that something must be wrong in our abstractions. Abstraction design is more of an art than coding is. Designing correct abstractions is challenging for any domain. It’s super important for further extension and maintenance of the system.

This principle suggests that we should split interfaces that are very large into smaller and more specific ones so that clients will only have to know about the methods they are interested in, thus making them more cohesive and solid.

### Dependency inversion principle (DIP)

> “High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g. interfaces).”
> 
> “Abstractions should not depend on details. Details (concrete implementations) should depend on abstractions.”

Inversion of control (IoC), or interface-based programming (also known as interface-based architecture), that embodies DIP is a well-known approach to help software engineers design complex systems focusing on the high-level abstraction of businesses and functionality without caring about concrete implementation at designing stage.

The key is that when building interfaces, we should not think about how their methods will be implemented. For example, we have a repository interface having `getAllUsers(): List<UserEntity>` methods. We need this interface since it serves our business logic. But when designing our domain layer, we don’t care about how user data is stored, whether in a relational DBMS or in a NoSql server; we’re fine as long as it can get a list of all users.

This principle also helps to reduce tight coupling between components/modules by building dependencies based on abstractions while all detailed implementation is definitely hidden from the big picture of the system.

## Domain-Driven Design (DDD)

Most products are born to solve customer problems, and software is no exception. Thus products have their own business rules that are solutions for those problems in specific domains.

In software development, *domain* means “sphere of knowledge and activity around which the application logic revolves,” or simply business logic.

DDD focuses on three core principles:

- Focus on the core domain and domain logic.
- Base complex designs on models of the domain.
- Constantly collaborate with domain experts to improve the application model and resolve any emerging domain-related issues.

Domain-centric architectures (e.g., [Uncle Bob’s Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)) that embody DDD, believe that `domain` is the heart of the system since it is the most stable part. Domain encapsulates the most general and high-level rules, as well as all of the use cases of the system that are least likely to change when something external changes. If we change the domain, we probably want to build another application.

Even though the domain-centric approach has some drawbacks of data consistency or code reuse compared to the data-centric approach (as expressed in the article “ [Domain-centric vs data-centric approaches to software development](https://enterprisecraftsmanship.com/posts/domain-centric-vs-data-centric-approaches/) ”), it has been proven as a good choice in the long run for any system that involves changes or complexity growth over time.

![domain-centric vs data-centric complexity growth](https://miro.medium.com/v2/resize:fit:1248/format:webp/1*tUPqQalFbPVznDzWbNbhJw.png)

Image source: “ Domain-centric vs data-centric approaches to software development ”

## Composition Over Inheritance

Having graduated from a top university with a solid understanding of OOP, a software engineer was confident they could build an application with a well-designed inheritance tree having five layers of derived classes. Yeah, it worked pretty well, and the codebase looked professional.

Afterward, that genius left the company. Another engineer took over the work of maintaining this project. Requirements were changing and being added, so some specific parts of the project had to be adapted. They needed to change something in a class in the inheritance tree to change the behavior of an important feature. Ok, it worked. It passed quality control and then eventually went live.

The day after, there were many customer complaints about broken features. The whole team didn’t recognize that the engineer had broken another feature by extending that class. Then a hotfix had to be done very quickly by some hacking code to allow the broken feature to behave differently with its parent class.

Time after time, through changes the codebase had been messed up with a lot of contract violations between child classes and parent classes (LSP violations). Much more time was needed for maintenance, and more bugs were introduced, even after delivery. The team became so tired and felt the pain of inheritance deeply.

I’m pretty sure that most OOP programmers have been in similar situations, including me. That funny (but factual) story tells us how painful inheritance is in software development, despite the fact that it has been taught as one of the basic fundamentals of OOP in schools.

On the other hand, the *c* omposition approach has been proven as the best direction for highly maintainable software, better than inheritance. Unity, the best game engine ever, is a clear example of embodying composition.

In short:

> “Composition over inheritance (or composite reuse principle) in OOP is the principle that classes should achieve polymorphic behavior and code reuse by their composition (by containing instances of other classes that implement the desired functionality) rather thaninheritance from a base or parent class.”
> 
> — [Wikipedia](https://en.wikipedia.org/wiki/Composition_over_inheritance)

But sometimes, in some cases, inheritance still has the benefits of increased code reuse and less programming effort. However, maintenance matters more. At the end of the above Wikipedia page, you can find some examples of composition drawbacks, and there are techniques to avoid those drawbacks.

## Design Patterns

Besides the fundamentals of software design, we also need to know, understand, and practice the well-known design patterns described clearly in the book “ [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.goodreads.com/book/show/85009.Design_Patterns) ” by the Gang of Four (i.e., Erich Gamma et al).

In this book, there are three types of design patterns:

- **Creational** — builder, factory method, abstract factory, prototype, singleton
- **Structural** —adapter, flyweight, proxy, composite, decorator…
- **Behavioral** — strategy, mediator, observer, template, chain of responsibility, etc.

I have nothing to write here except to recommend that you read the book and practice those patterns in the meanwhile.

## Conclusion

There is no complete formula for good designs. Just follow fundamental practices and you will be alright. But understanding all of them and then applying them to real problems is really challenging, even for senior engineers. Having a good mindset helps you to focus on the right things to learn, and to accumulate valuable experiences and skills along the way.

From my point of view, I can sum up important fundamentals that make good designs for most of the software (but not all):

well-designed abstractions + high cohesive classes/modules + loose coupling dependencies + composition over inheritance + domain-driven + good design pattens + the last thing…

The last thing to remember is that we build software to serve user needs. It doesn’t make any sense at all if we have a very good architectural design acccording to our own perspective, but the final output doesn’t fit the user’s expectation in their use cases, or if the system is very low-performance yet unusable. So, besides all the above, always think about users to build great software.

Thanks for reading!