---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/chain-responsibility-design-pattern/", "https://refactoring.guru/design-patterns/chain-of-responsibility"]
tags: [design-pattern, behavioral, gof]
---

# Chain of Responsibility Pattern

A behavioral pattern that passes a request along a chain of handlers, where each handler decides either to process the request or to forward it to the next handler in the chain.

## Problem

A request may need to be handled by one of several objects, but the sender should not be coupled to any specific handler. The set of eligible handlers and their order may need to vary at runtime. Classic examples:

- Middleware pipelines in web frameworks (authentication → logging → rate limiting → routing).
- Exception handling hierarchies — catch at the innermost scope or let it propagate.
- Approval workflows — junior manager approves small amounts, escalates larger ones.
- Event bubbling in UI frameworks.
- Customer support escalation — basic queries handled at Level 1, complex ones escalated to Level 2 or Level 3.

Without the pattern, each validation or handling check accumulates in a single bloated class. As the codebase grows, "changing one check sometimes affected the others," creating fragile, tightly coupled code.

## Solution

Arrange handlers in a linked chain. Each handler holds a reference to the **next** handler. When a request arrives, the handler either handles it or calls `next.handle(request)`. The sender holds only a reference to the first handler and is unaware of the chain's length or which handler ultimately processes the request.

## Structure

| Participant | Role |
|---|---|
| **Handler** (interface/abstract) | Declares `handle(request)` and holds a reference to the next handler |
| **BaseHandler** (optional) | Abstract class providing default chaining logic (`if next: next.handle()`) |
| **ConcreteHandlerA/B/…** | Decides whether to handle the request or pass it on |
| **Client** | Assembles the chain and sends requests to the first handler |

## When to Use

- More than one object may handle a request, and the handler is not known a priori.
- You want to issue a request to one of several objects without specifying the receiver explicitly.
- The set of objects that can handle a request should be specified dynamically.
- You want to add or reorder handlers without touching request-sending code.
- Processing requests requires executing handlers in a specific, configurable order.
- You need to be able to modify the pipeline at runtime (add, remove, reorder handlers).

## Trade-offs

**Pros:**
- Decouples sender from receivers.
- Single Responsibility — each handler focuses on one concern.
- Open/Closed — add or reorder handlers without modifying existing code or the client.
- Runtime configurability of the processing pipeline.
- Enables new handlers to be added without modifying the client or existing handlers.

**Cons / pitfalls:**
- No guarantee that a request is handled — it may fall off the end of the chain silently. Requires a default/fallback handler or explicit error raising.
- Hard to observe/debug: the flow is implicit and spread across multiple classes.
- Can introduce latency if the chain is long and the matching handler is near the end.
- Each handler adds a layer of indirection, making the codebase harder to follow.
- Improper chain configuration may leave requests unhandled entirely.

## Variants

- **Pure chain** — only one handler processes the request; once handled, propagation stops.
- **Impure / pipeline chain** — every handler processes the request in sequence (middleware pattern). All handlers run regardless.
- **Tree of responsibility** — handlers form a tree rather than a linear chain (e.g., UI component hierarchies where a click bubbles from child to parent).

## Code Example

```python
from abc import ABC, abstractmethod

class SupportHandler(ABC):
    def __init__(self):
        self._next = None

    def set_next(self, handler):
        self._next = handler
        return handler  # allows chaining: a.set_next(b).set_next(c)

    @abstractmethod
    def handle(self, request): ...

class Level1SupportHandler(SupportHandler):
    def handle(self, request):
        if request == "BASIC":
            print("Level 1 Support handled the request.")
        elif self._next:
            self._next.handle(request)

class Level2SupportHandler(SupportHandler):
    def handle(self, request):
        if request == "INTERMEDIATE":
            print("Level 2 Support handled the request.")
        elif self._next:
            self._next.handle(request)

class Level3SupportHandler(SupportHandler):
    def handle(self, request):
        if request == "CRITICAL":
            print("Level 3 Support handled the request.")
        else:
            print(f"Request '{request}' could not be handled.")

# Assemble chain
l1 = Level1SupportHandler()
l2 = Level2SupportHandler()
l3 = Level3SupportHandler()
l1.set_next(l2).set_next(l3)

l1.handle("BASIC")         # Level 1 Support handled the request.
l1.handle("INTERMEDIATE")  # Level 2 Support handled the request.
l1.handle("CRITICAL")      # Level 3 Support handled the request.
l1.handle("UNKNOWN")       # Request 'UNKNOWN' could not be handled.
```

## Real-World Examples

- **Web framework middleware** — Express.js, ASP.NET Core, and Django middleware chains: each middleware calls `next()` to pass the request forward. Authentication, CORS, logging, compression, and routing are all distinct handlers in sequence.
- **Customer support escalation** — Level 1 support handles basic questions; unresolved tickets pass to Level 2 (intermediate), then Level 3 (critical/engineering). Each level decides to resolve or escalate.
- **GUI event bubbling** — a mouse click on a button propagates upward through the component tree: button → panel → window → application. The first handler that consumes the event stops propagation.
- **Multi-level logging** — a logging framework routes log entries by severity: DEBUG → handler that writes to file; WARN → handler that sends alerts; ERROR → handler that pages on-call engineers.
- **Security permission checks** — a request passes through authentication → authorization → rate limiting → input validation handlers before reaching the business logic.
- **Approval workflows** — a purchase order is approved by a team lead (small amounts), manager (medium), director (large), or CFO (very large), with each approver escalating if the amount exceeds their limit.
- **Java Servlet filters** — `javax.servlet.Filter` chain processes HTTP requests through a sequence of filters before reaching the servlet.

## Related

- [[Command Pattern]] — Command wraps a request as an object to be dispatched; Chain decides *who* handles it. They are often combined: commands travel down a chain.
- [[Decorator Pattern]] — also uses a chain of wrappers, but all wrappers execute; Chain stops at the first matching handler (in pure chain mode).
- [[Composite Pattern]] — tree-shaped variants of Chain resemble Composite hierarchies.
- [[Mediator Pattern]] — Mediator centralises routing; Chain distributes it along a sequence. Both decouple senders from receivers but through opposite structural means.
