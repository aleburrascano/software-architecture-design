---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - raw/articles/What Makes a Good Software Design Mindset.md
tags:
  - design
  - principles
  - coupling
---

# Law of Demeter

A design guideline stating that a module should have limited knowledge about other modules — it should only talk to its direct associates, never to "strangers."

Also called the **Principle of Least Knowledge**.

## Problem / Why It Matters

When code chains through multiple objects to reach what it needs (`order.getCustomer().getAddress().getCity()`), it becomes tightly coupled to the entire chain of internal structures. Changing any intermediate object — its type, its internal structure, how it stores its data — breaks every caller that navigates through it. This fragility is a common and subtle source of [[Coupling and Cohesion|tight coupling]].

## Formal Statement

In object-oriented programming, a method `m` of object `a` may only invoke methods on:

1. `a` itself
2. Objects passed as parameters to `m`
3. Objects instantiated within `m`
4. `a`'s direct attributes (fields)
5. Global variables accessible to `a`

In dot-notation terms: **use only one dot**. `a.doSomething()` is acceptable; `a.getPart().doSomething()` is a violation.

## Origin

Ian Holland proposed the Law of Demeter in 1987 while working on the Demeter Project at Northeastern University, a project focused on aspect-oriented and adaptive programming.

## Examples

**Violation:**
````n// Navigates through Customer to Address to get the city
String city = order.getCustomer().getAddress().getCity();
```

**Compliant:**
````n// Order asks for the city directly; internal navigation is hidden inside
String city = order.getCustomerCity();
```

The compliant version hides how `Order` knows the city. If the internal model changes, only `Order` changes — not every caller.

## Trade-offs

Following LoD reduces coupling and makes systems more resilient to internal restructuring. However:

- It can require writing numerous thin wrapper methods, increasing the surface area of classes
- Wide class interfaces can become harder to navigate at the design level
- Taken too strictly, it can create indirection that obscures intent

Aspect-oriented programming is sometimes proposed as a way to manage the resulting interface proliferation through higher-level abstraction.

## Related

- [[Coupling and Cohesion]] — LoD is a specific rule for reducing coupling
- [[Separation of Concerns]] — LoD enforces information hiding across concerns
- [[Single Responsibility Principle]] — complementary constraint on what a class should expose
- [[Dependency Inversion Principle]] — another mechanism for reducing dependency on implementation details
