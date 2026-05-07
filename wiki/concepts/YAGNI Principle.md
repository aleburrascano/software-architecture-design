---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/articles/What Makes a Good Software Design Mindset.md"
  - "https://www.geeksforgeeks.org/software-engineering/what-is-yagni-principle-you-arent-gonna-need-it/"
tags:
  - design-principle
---

# YAGNI Principle

You aren't gonna need it — don't add functionality until you actually need it.

## Problem

Software teams frequently build features, abstractions, configuration options, and generalization layers based on *anticipated* future requirements. These speculative additions:

- Increase current development time and cost.
- Add code that must be maintained, tested, and understood — even if it is never used.
- Often turn out to be wrong: when the real requirement arrives, it is different from what was anticipated, and the speculative code must be reworked or removed anyway.
- Slow down the team by cluttering the codebase with dead weight.

Speculative generality is one of Martin Fowler's named code smells precisely because it happens so commonly and has real, measurable costs. YAGNI frames this through four concrete cost categories:

1. **Cost of building**: time, effort, and resources invested in features nobody asked for, including planning, coding, and testing.
2. **Cost of delay**: shipping the real feature later because speculative work consumed capacity.
3. **Cost of carry**: the ongoing complexity tax — every unused abstraction and configuration parameter slows everyone who touches the code.
4. **Cost of repair**: when the speculation was wrong and the code must be rewritten or removed, you pay twice.

## Statement

> "You Aren't Gonna Need It."
> — Ron Jeffries, Extreme Programming (XP) community; popularized via Martin Fowler

The directive: implement things when you actually need them, never when you merely foresee that you might need them.

## Explanation

YAGNI is fundamentally about **deferring decisions** until the moment you have the information to make them well. It recognizes that:

1. **Predictions are unreliable**: software requirements change. Today's "obviously needed" feature may be irrelevant in two months.
2. **The cost of adding later is often lower than the cost of carrying unused code now**: a feature built speculatively must be designed, coded, reviewed, tested, and maintained indefinitely. A feature built when needed benefits from better context.
3. **Simpler codebases are faster to work in**: every unused abstraction, every configuration parameter, every generalization layer slows down the team's ability to understand and change the code.

### The critical distinction: deferring vs. ignoring

YAGNI is about **delaying implementation**, not avoiding good design. It does not mean:
- Ignoring scalability or architecture quality — a poorly designed foundation is not "simpler," it is just incomplete.
- Rejecting all future planning — thinking through system boundaries and interfaces is different from implementing speculative features.
- Removing abstractions that serve current requirements — only those that serve hypothetical ones.

The key question: "Is this needed *right now* by a real, current requirement?" If no, defer it.

### YAGNI and incremental design

YAGNI is a core XP practice. Combined with continuous refactoring, it argues that the right time to add generalization is *after* you see a second or third concrete use case — not before the first. Good test coverage and refactoring discipline make "add it later" safe. Without them, deferral can accumulate into technical debt.

### YAGNI vs. DRY and OCP

YAGNI creates productive tension with [[DRY Principle]] and [[Open-Closed Principle]]:
- DRY says "don't duplicate existing knowledge." YAGNI says "don't abstract for hypothetical future knowledge."
- OCP says "design so new behavior can be added without modification." YAGNI says "don't add extension seams until there is a second extension to make."

The resolution: add abstractions and extension points when you have concrete evidence you need them (two or more real use-cases), not for one anticipated future use-case.

### Practical implementation steps

1. Gather requirements and categorize them strictly as "needed now" vs. "can wait."
2. Align with the team on current goals to avoid individuals adding speculative features in isolation.
3. Break objectives into the minimal tasks that deliver the current requirement.
4. Explicitly reject scope additions that don't serve the current goal.
5. Document deferred ideas in a backlog, not in code comments or unused branches.

## Example

### Violation — speculative plugin system

````nclass ReportGenerator:
    """Generates reports. Supports pluggable formatters for future formats."""
    def __init__(self, formatter=None, cache_backend=None,
                 output_channel=None, locale=None, theme=None):
        # Five optional parameters for features nobody asked for yet
        self.formatter = formatter or DefaultFormatter()
        self.cache = cache_backend or NoOpCache()
        self.output = output_channel or StdoutChannel()
        self.locale = locale or "en_US"
        self.theme = theme or "default"

    def generate(self, data: dict) -> str:
        # complex pipeline nobody currently uses more than one step of
        ...
```

The team spent a week building the plugin system. The product currently has one report format, one output, and no locale/theme requirements. That work — and every future developer who has to understand it — pays the cost of carry indefinitely.

### YAGNI-conforming

````ndef generate_report(data: dict) -> str:
    """Generate a plain-text report from data."""
    lines = [f"{k}: {v}" for k, v in data.items()]
    return "\n".join(lines)
```

When a second format is actually needed, the abstraction can be introduced with confidence because both use-cases are known. The refactoring will be guided by real constraints, not guesses.

### Violation — generic base class for a single implementation

````npublic abstract class AbstractReportService {
    protected abstract String format(Map<String, Object> data);
    protected abstract void validate(Map<String, Object> data);
    // ... eight more abstract methods

    public final String generate(Map<String, Object> data) {
        validate(data);
        return format(data);
    }
}

// The only actual implementation
public class PlainTextReportService extends AbstractReportService {
    @Override
    protected String format(Map<String, Object> data) { ... }

    @Override
    protected void validate(Map<String, Object> data) { ... }
}
```

### YAGNI-conforming

````npublic class ReportService {
    public String generate(Map<String, Object> data) {
        validate(data);
        return format(data);
    }
    private void validate(Map<String, Object> data) { ... }
    private String format(Map<String, Object> data) { ... }
}
```

Introduce the abstract base class when there are two concrete implementations with genuinely shared behavior.

## Common Violations

- **Unused configuration parameters**: adding `mode`, `strategy`, or `version` arguments when only one value is ever passed.
- **Generic base classes for single implementations**: creating `AbstractFooService` when only one `FooService` exists.
- **"We might need this later" comments**: code committed with a note that it is not currently used.
- **Premature plugin/extension architectures**: hook systems, event buses, or strategy registries built before there are multiple strategies.
- **Over-parameterized functions**: functions that accept optional arguments to control behavior that has never been varied.
- **Dead feature flags**: `if feature_enabled("new_billing_model")` for a feature that was never shipped and the flag is never `true`.

## Trade-offs

- **Refactoring cost**: deferring an abstraction until it is needed means retrofitting it into existing code, which may be harder than building it in from the start if the codebase has grown. Good test coverage is the mitigation.
- **Context-sensitivity**: for library/framework authors (who *cannot* easily change their API once published), YAGNI applies with less force — thinking ahead about extension points is more justified.
- **Team expertise**: teams with strong refactoring discipline find YAGNI liberating; teams without it may find the retrofit work becomes technical debt.
- **Long-lived architectural decisions**: some decisions (database choice, API shape, deployment model) are costly to reverse. YAGNI does not mean ignoring these — it applies to *features and code abstractions*, not to foundational architectural constraints.

## Related

- [[KISS Principle]] — YAGNI is the feature-scope complement to KISS; together they argue for doing the simplest thing now
- [[DRY Principle]] — the productive tension: DRY consolidates known duplication, YAGNI defers speculative generalization
- [[Open-Closed Principle]] — OCP favors extension seams; YAGNI says earn them with concrete need
- [[Design Principles Overview]] — YAGNI in context with all other design principles
