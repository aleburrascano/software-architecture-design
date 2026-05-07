---
type: concept
created: '2026-05-06'
updated: '2026-05-06'
sources:
  - 'SoftwareMill - Advanced Large-Scale DDD.md'
tags:
  - knowledge-level
  - ddd
  - meta-model
  - base-level
  - design-pattern
  - large-scale-ddd
  - analysis-patterns
---
# Knowledge Level Pattern

A structural design pattern that splits the domain model into two levels: a **base level** of concrete operational instances and a **knowledge level** of types and rules that govern how those instances behave — enabling business rules to evolve independently of the objects they govern.

## Problem

Business rules about how objects behave change frequently and independently of the objects themselves. An `Employee` object needs to know what roles it can have, what benefits it's entitled to, what its work schedule constraints are. These rules vary by `EmployeeType` — but the `Employee` itself is a concrete instance. If you put the type logic in the `Employee` class, every rule change requires touching all employees. If you hardcode it, you lose flexibility.

## Solution / Explanation

Martin Fowler introduced the Knowledge Level in *Analysis Patterns* (1996). Evans referenced it in *Domain-Driven Design* as a large-scale structure pattern.

Split the model into two levels:

### Base Level (Operational Instances)
Concrete runtime objects: specific employees, specific policies, specific orders. These know what they **are** — their current state and identity.

### Knowledge Level (Types and Rules)
Descriptive objects that define what base-level objects **can do** and **how they behave**. These encode the configurable rules.

```
Knowledge Level:
  EmployeeType
    - allowedRoles: [Engineer, Manager]
    - vacationPolicy: accrual_rate = 1.5 days/month
    - workSchedule: 9-to-5, Mon-Fri
    - benefitsTier: STANDARD

Base Level:
  Employee
    - id, name, hireDate
    - employeeType → EmployeeType (knowledge level)
```

When rules change ("Engineers now get 2.0 days/month vacation"), you update `EmployeeType`, not every `Employee` instance.

### Insurance Example

```
Knowledge Level:
  PolicyType
    - coverageRules: [...]
    - deductibleRange: [$500, $5000]
    - allowedEndorsements: [...]

Base Level:
  Policy
    - policyNumber, holder, startDate
    - policyType → PolicyType  ← governs behavior
```

The `Policy` object knows what it is; the `PolicyType` knows what policies of that type can do.

### Configurable Behavior

Knowledge Level enables **configurable behavior without code changes**:
- New `EmployeeType` "ContractorUSA" added via database entry, not code deployment
- Insurance product variations defined in `PolicyType` records
- Pricing tiers, approval workflows, SLA configurations

This is the "data-driven behavior" pattern taken to the domain model level.

## When to Apply

**Apply when:**
- Business rules vary by "type" and change more frequently than the objects they govern
- You have many variants of a base concept (many EmployeeTypes, many PolicyTypes)
- Business users need to configure behavior without code deployments

**Don't apply when:**
- Only one or two types exist (overkill)
- Rules are stable and don't vary by type
- The meta-model adds complexity without enabling meaningful configuration

## Trade-offs

| Benefit | Cost |
|---|---|
| Rules evolve without touching instances | Two-level model is harder to understand |
| New types added via configuration, not code | Queries span two levels (join cost) |
| Insulates base objects from rule volatility | Knowledge level must be well-designed upfront |
| Enables non-developer rule configuration | Debugging requires understanding both levels |

## Related

- [[Domain-Driven Design]] — Knowledge Level is an Evans large-scale structure pattern
- [[Responsibility Layers]] — companion pattern for organizing domain by abstraction level
- [[Aggregate]] — base-level objects are often aggregates governed by knowledge-level types
- [[DDD Advanced Patterns]] — topic page with broader context
