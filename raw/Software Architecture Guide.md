---
title: "Software Architecture Guide"
source: "https://martinfowler.com/architecture/"
author:
  - "[[Martin Fowler]]"
published: 2019-08-01
created: 2026-05-03
description: "Software Architecture is the important aspects of a software system's internal design, usually its major components and aspects that are hard to change."
tags:
  - "clippings"
---
When people in the software industry talk about “architecture”, they refer to a hazily defined notion of the most important aspects of the internal design of a software system. A good architecture is important, otherwise it becomes slower and more expensive to add new capabilities in the future.

Like many in the software world, I’ve long been wary of the term “architecture” as it often suggests a separation from programming and an unhealthy dose of pomposity. But I resolve my concern by emphasizing that good architecture is something that supports its own evolution, and is deeply intertwined with programming. Most of my career has revolved about the questions of what good architecture looks like, how teams can create it, and how best to cultivate architectural thinking in our development organizations. This page outlines my view of software architecture and points you to more material about architecture on this site.

A guide to material on martinfowler.com about software architecture.

## What is architecture?

People in the software world have long argued about a definition of architecture. For some it's something like the fundamental organization of a system, or the way the highest level components are wired together. My thinking on this was shaped by [an email exchange with Ralph Johnson](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf), who questioned this phrasing, arguing that there was no objective way to define what was fundamental, or high level and that a better view of architecture was **the shared understanding that the expert developers have of the system design.**

![](https://martinfowler.com/architecture/ralph.png)

Ralph Johnson, speaking at QCon

A second common style of definition for architecture is that it's “the design decisions that need to be made early in a project”, but Ralph complained about this too, saying that it was more like **the decisions you wish you could get right early in a project**.

His conclusion was that **“Architecture is about the important stuff. Whatever that is”**. On first blush, that sounds trite, but I find it carries a lot of richness. It means that the heart of thinking architecturally about software is to decide what is important, (i.e. what is architectural), and then expend energy on keeping those architectural elements in good condition. For a developer to become an architect, they need to be able to recognize what elements are important, recognizing what elements are likely to result in serious problems should they not be controlled.

[![](https://martinfowler.com/architecture/ieee-arch.png)](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf)

Ralph's email formed the core of [my column for IEEE software](https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf), which discussed the meaning of software architecture and the role of an architect.

## Why does architecture matter?

Architecture is a tricky subject for the customers and users of software products - as it isn't something they immediately perceive. But a poor architecture is a major contributor to the growth of cruft - elements of the software that impede the ability of developers to understand the software. Software that contains a lot of cruft [is much harder to modif](https://martinfowler.com/articles/is-quality-worth-cost.html) y, leading to features that arrive more slowly and with more defects.

[![](https://martinfowler.com/articles/is-quality-worth-cost/card.png)](https://martinfowler.com/articles/is-quality-worth-cost.html)

This situation is counter to our usual experience. We are used to something that is “high quality” as something that costs more. For some aspects of software, such as the user-experience, this can be true. But when it comes to the architecture, and other aspects of internal quality, this relationship is reversed. **High internal quality leads to faster delivery of new features**, because there is less cruft to get in the way.

While it is true that we can sacrifice quality for faster delivery in the short term, before the build up of cruft has an impact, people underestimate how quickly the cruft leads to an overall slower delivery. While this isn't something that can be objectively measured, experienced developers reckon that **attention to internal quality pays off in weeks not months.**

[![](https://martinfowler.com/articles/is-quality-worth-cost/both.png)](https://martinfowler.com/articles/is-quality-worth-cost.html)

[Read more…](https://martinfowler.com/articles/is-quality-worth-cost.html)

[![](https://martinfowler.com/architecture/oscon.png)](https://martinfowler.com/videos.html#2015-oscon)

At OSCON in 2015 I gave a [brief talk](https://martinfowler.com/videos.html#2015-oscon) (14 min) on what architecture is and why it matters.

---

## Application Architecture

The important decisions in software development vary with the scale of the context that we're thinking about. A common scale is that of an application, hence “application architecture”.

The first problem with defining application architecture is that there's no clear definition of what an application is. My view is that [applications are a social construction](https://martinfowler.com/bliki/ApplicationBoundary.html):

- A body of code that's seen by developers as a single unit
- A group of functionality that business customers see as a single unit
- An initiative that those with the money see as a single budget

Such a loose definition leads to many potential sizes of an application, varying from a few to a few hundred people on the development team. (You'll notice I look at size as the amount of people involved, which I feel is the most useful way of measuring such things.) The key difference between this and enterprise architecture is that there is a significant degree of unified purpose around the social construction.

---

## Enterprise Architecture

While application architecture concentrates on the architecture within some form of notional application boundary, enterprise architecture looks architecture across a large enterprise. Such an organization is usually too large to group all its software in any kind of cohesive grouping, thus requiring coordination across teams with many codebases, that have developed in isolation from each other, with funding and users that operate independently of each other.

Much of enterprise architecture is about understanding what is worth the costs of central coordination, and what form that coordination should take. At one extreme is a central architecture group that must approve all architectural decision for every software system in the enterprise. Such groups slow down decision making and cannot truly understand the issues across such a wide portfolio of systems, leading to poor decision-making. But the other extreme is no coordination at all, leading to teams duplicating effort, inability for different system to inter-operate, and a lack of skills development and cross-learning between teams.

Like most people with an agile mindset, I prefer to err on the side of decentralization, so will head closer to the rocks of chaos rather than suffocating control. But being on that side of the channel still means we have to avoid the rocks, and a way to maximize local decision making in a way that minimizes the real costs involved.