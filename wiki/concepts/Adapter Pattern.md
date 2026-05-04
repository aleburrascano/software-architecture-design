---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-03'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/adapter
  - https://www.geeksforgeeks.org/system-design/adapter-pattern/
tags:
  - design-pattern
  - structural
  - gof
---
# Adapter Pattern

Allows objects with incompatible interfaces to collaborate by wrapping one object in an adapter that translates calls between the two interfaces. Also known as the **Wrapper** pattern.

## Problem

You have an existing class (the *adaptee*) whose interface doesn't match the interface that client code expects. You cannot change the adaptee — it may be a third-party library, a legacy component, or simply code you should not touch. You need both to work together without modifying either.

Classic real-world analogy: a US laptop plug that doesn't fit a European socket. You need a physical adapter — you don't rewire the building or redesign the laptop.

A concrete software example: a stock market app that receives XML data needs to integrate a third-party analytics library that expects JSON. Modifying the library is impossible; rewriting your infrastructure is expensive. An adapter bridges them.

## Solution

Introduce an adapter class that implements the target interface the client expects and internally holds a reference to the adaptee. When the client calls a target-interface method, the adapter translates that call into the appropriate method call on the adaptee.

```
interface Target:
    request() -> Result

class Adaptee:
    specificRequest() -> Result   // incompatible method name / signature

class Adapter implements Target:
    adaptee: Adaptee

    request():
        return adaptee.specificRequest()   // translation
```

The adapter acts as a translator: client calls it via the target interface; the adapter maps the call into the adaptee's format; the adaptee processes it; the client receives results without knowing any adaptation occurred.

## Structure

| Participant | Role |
|---|---|
| Client | Contains existing business logic; works only through the Target interface |
| Target (Client Interface) | Protocol that the Client expects; the interface the Adapter must implement |
| Adaptee (Service) | The useful but incompatible class — often third-party or legacy; cannot be changed |
| Adapter | Implements Target; wraps Adaptee; translates Target calls into Adaptee method calls |

### Object Adapter vs. Class Adapter

**Object adapter** (composition, preferred): The Adapter holds a reference to an Adaptee instance. Works in any language; the adaptee can be subclassed without affecting the adapter.

**Class adapter** (multiple inheritance): The Adapter subclasses both Target and Adaptee simultaneously. Only available in languages that support multiple inheritance (e.g. C++). Allows the adapter to override adaptee behaviour directly.

**Two-way adapter**: Functions bidirectionally — each side can use the other's interface. Useful when two systems both need to treat each other as their own type.

**Interface adapter (default adapter)**: Provides default no-op implementations of all interface methods so subclasses only need to override the methods they actually care about.

## When to Use

- You want to use an existing class but its interface doesn't match what you need.
- You want to create a reusable class that cooperates with classes whose interfaces you can't predict or control.
- You need to integrate several existing subclasses that lack common functionality — adapt the parent rather than every subclass.
- You are integrating a third-party library and cannot modify its source code.
- You want to centralise all compatibility changes in a single place for easier maintenance.

## Trade-offs

**Pros:**
- Single Responsibility Principle: separates interface conversion from business logic.
- Open/Closed Principle: introduce new adapters without changing client or adaptee code.
- Works with legacy or third-party code without touching it.
- Supports multiple target interfaces through interchangeable adapters.
- Decouples systems from specific implementations.

**Cons / pitfalls:**
- Increases overall complexity — a new class plus indirection.
- For a simple name mismatch, a thin wrapper may feel like overkill; a direct delegation method can be simpler.
- Multiple adapters for many interfaces increase maintenance burden.
- Performance overhead from the extra indirection layer on each call.
- Handling many incompatible interfaces may require complex adapter chains.

## Variants

| Variant | Mechanism | Best When |
|---|---|---|
| Object Adapter | Composition (wraps instance) | Default choice; works in all languages |
| Class Adapter | Multiple inheritance | C++; need to override adaptee methods directly |
| Two-way Adapter | Composition in both directions | Mutual adaptation between two systems |
| Interface Adapter | Default implementations on abstract class | Interface has many methods; only a few need custom behaviour |

## Code Example

```python
from abc import ABC, abstractmethod

# Target interface — what our application expects
class Printer(ABC):
    @abstractmethod
    def print(self, text: str): ...

# Adaptee — third-party legacy class we cannot change
class LegacyPrinter:
    def print_document(self, content: str):
        print(f"[Legacy] Printing: {content}")

# Object Adapter — wraps LegacyPrinter, implements Printer
class PrinterAdapter(Printer):
    def __init__(self, legacy_printer: LegacyPrinter):
        self._adaptee = legacy_printer

    def print(self, text: str):
        self._adaptee.print_document(text)   # translation

# Client code — only knows Printer
def send_report(printer: Printer, report: str):
    printer.print(report)

adapter = PrinterAdapter(LegacyPrinter())
send_report(adapter, "Q4 Financial Report")
# Output: [Legacy] Printing: Q4 Financial Report
```

## Real-World Examples

- **Java I/O** — `InputStreamReader` is an adapter converting a byte-oriented `InputStream` into the character-oriented `Reader` interface. `Arrays.asList()` adapts an array to the `List` interface.
- **JDBC** — database drivers act as adapters between the generic JDBC API and vendor-specific database protocols (MySQL, PostgreSQL, Oracle).
- **Spring MVC** — `HandlerAdapter` adapts various handler types (annotated controllers, `HttpRequestHandler`, `Servlet`) to the uniform `DispatcherServlet` dispatch mechanism.
- **SLF4J** — logging facade adapters (`slf4j-log4j12`, `slf4j-jdk14`) adapt different underlying logging frameworks to the common `Logger` interface.
- **Device drivers** — OS drivers adapt hardware-specific interfaces to a standard OS API, so application code never changes when hardware changes.
- **File format converters** — tools that convert between CSV, JSON, and XML are object adapters between incompatible data format interfaces.
- **Power plug adapters** — the physical-world canonical example: US electronics in European sockets.

## Related

- [[Bridge Pattern]] — also decouples two interfaces, but by upfront design; Adapter retrofits incompatible *existing* interfaces after the fact
- [[Decorator Pattern]] — also wraps an object, but maintains the original interface to add behaviour rather than translate it; Decorator adds; Adapter converts
- [[Facade Pattern]] — defines a *new* simpler interface over a subsystem; Adapter makes an *existing* interface match another existing interface; Facade typically manages entire subsystems while Adapter typically wraps a single object
- [[Proxy Pattern]] — also wraps an object, but maintains the *same* interface to control access rather than translate it
