---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-06'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/decorator
  - https://www.geeksforgeeks.org/system-design/decorator-pattern/
tags:
  - design-pattern
  - structural
  - gof
---
# Decorator Pattern

Attaches additional responsibilities to an object dynamically by wrapping it in decorator objects that implement the same interface, providing a flexible alternative to subclassing for extending functionality.

## Problem

You need to add behaviour to individual objects, not to the entire class. Subclassing is one approach, but it's static — you fix the combination of behaviours at compile time and end up with a class explosion for every combination. You also can't add or remove behaviours at runtime.

Classic example: a text input stream that might need buffering, compression, and encryption. Three features, eight possible combinations — with inheritance you'd need seven subclasses for every combination, plus the base. A notification library that can send alerts via email, SMS, Facebook, and Slack would require exponentially growing subclass permutations.

Another analogy: layering clothing. Wearing a sweater, jacket, and raincoat each "extends" protection without being inherent to the person underneath. You can add or remove layers at any time.

## Solution

Define a `Component` interface. Create a `ConcreteComponent` that does the base work. Create a `BaseDecorator` abstract class that also implements `Component` and holds a wrapped `Component` reference. Each concrete decorator adds behaviour before and/or after forwarding the call to the wrapped component. Decorators stack: wrap a concrete component, then wrap that in another decorator, and so on.

```
interface Component:
    operation() -> Result

class ConcreteComponent implements Component:
    operation(): // base work

class BaseDecorator implements Component:
    wrapped: Component

    operation():
        return wrapped.operation()    // forward; subclasses add before/after

class ConcreteDecoratorA(BaseDecorator):
    operation():
        // before
        result = wrapped.operation()
        // after
        return result
```

A wrapper is an object linked to a target object. The wrapper contains the same set of methods as the target and delegates to it all requests it receives, adding behaviour around the delegation.

## Structure

| Participant | Role |
|---|---|
| Component | Interface for both the concrete component and all decorators |
| ConcreteComponent | The base object to which behaviours are added |
| BaseDecorator | Abstract class implementing Component; holds a reference to a wrapped Component; delegates all operations by default |
| ConcreteDecorator | Adds specific behaviour before/after calling `wrapped.operation()` |
| Client | Constructs and combines multiple decorator layers at runtime |

## When to Use

- You need to add responsibilities to individual objects at runtime without affecting other objects of the same class.
- You need combinations of behaviours that would produce a subclass explosion if handled via inheritance.
- Extension by subclassing is impractical (e.g., the base class is sealed/final or in a third-party library).
- You want to structure business logic into composable layers, each handling one responsibility.

## Trade-offs

**Pros:**
- Behaviours can be combined, stacked, and removed at runtime in any order.
- Open/Closed Principle: add new decorators without changing existing code.
- Single Responsibility Principle: each decorator handles exactly one concern.
- Avoids feature-laden superclasses.
- More flexible than inheritance: inheritance adds a feature to the whole class; decorators target individual instances.

**Cons / pitfalls:**
- Many small objects in the design — harder to debug when a chain is long.
- Order of decoration matters and can be non-obvious.
- Removing a specific decorator from the middle of a stack is awkward.
- Identity checks (`instanceof` / `is`) against a decorated object may give unexpected results.
- Initial configuration code that constructs a deeply nested chain can be complex to read.

## Code Example

````nfrom abc import ABC, abstractmethod

# Component interface
class Coffee(ABC):
    @abstractmethod
    def get_cost(self) -> float: ...

    @abstractmethod
    def get_description(self) -> str: ...

# ConcreteComponent
class PlainCoffee(Coffee):
    def get_cost(self) -> float:
        return 1.00

    def get_description(self) -> str:
        return "Plain coffee"

# BaseDecorator
class CoffeeDecorator(Coffee):
    def __init__(self, coffee: Coffee):
        self._coffee = coffee

    def get_cost(self) -> float:
        return self._coffee.get_cost()

    def get_description(self) -> str:
        return self._coffee.get_description()

# ConcreteDecorators
class MilkDecorator(CoffeeDecorator):
    def get_cost(self) -> float:
        return self._coffee.get_cost() + 0.50

    def get_description(self) -> str:
        return self._coffee.get_description() + ", milk"

class SugarDecorator(CoffeeDecorator):
    def get_cost(self) -> float:
        return self._coffee.get_cost() + 0.25

    def get_description(self) -> str:
        return self._coffee.get_description() + ", sugar"

class WhipDecorator(CoffeeDecorator):
    def get_cost(self) -> float:
        return self._coffee.get_cost() + 0.75

    def get_description(self) -> str:
        return self._coffee.get_description() + ", whipped cream"

# Stack decorators at runtime — any combination, any order
order = WhipDecorator(SugarDecorator(MilkDecorator(PlainCoffee())))
print(order.get_description())  # Plain coffee, milk, sugar, whipped cream
print(f"${order.get_cost():.2f}")  # $2.50
```

Data source example (from prior Code Sketch — preserved for completeness):

````n# Stacking encryption + compression decorators over a file data source
source = CompressionDecorator(EncryptionDecorator(FileDataSource("data.txt")))
source.write("hello world")
print(source.read())   # "hello world"
```

## Real-World Examples

- **Java I/O** — the canonical real-world example: `BufferedInputStream(new GZIPInputStream(new FileInputStream("file.gz")))`. `BufferedInputStream`, `GZIPInputStream`, `DataInputStream` are all decorators wrapping other `InputStream` objects. The same interface (`InputStream`) is implemented at every layer.
- **Java I/O Writer hierarchy** — `BufferedWriter` decorates `FileWriter`, adding buffering; `PrintWriter` decorates `BufferedWriter`, adding convenience methods.
- **Spring Security** — `HttpSecurity` uses a decorator chain to add authentication, authorisation, CSRF protection, and session management around the core `HttpServletRequest`.
- **Python `functools.wraps` / function decorators** — Python's `@decorator` syntax applies the Decorator pattern at the function level.
- **Text processors** — formatting decorators (bold, italic, underline) stack over a base text renderer; each decorator adds one formatting layer without modifying the others.
- **Video streaming platforms** — subtitle decorator, audio description decorator, and quality-selector decorator stack over a base video player.
- **Notification systems** — email, SMS, Slack, and push-notification behaviours implemented as decorators over a base notifier; any combination is assembled at runtime based on user preferences.

## Related

- [[Composite Pattern]] — also uses recursive composition; Composite aggregates *many* children to form a tree; Decorator wraps a *single* component to augment it; Composite has one-to-many children, Decorator has exactly one wrapped component
- [[Adapter Pattern]] — also wraps an object; Adapter *changes* the interface; Decorator *maintains* (and extends) the same interface
- [[Strategy Pattern]] — changes the *guts* of an object (replaces internal algorithm); Decorator changes its *skin* (adds behaviour from the outside while keeping the interface the same)
- [[Chain of Responsibility Pattern]] — also chains objects that pass requests along; Chain of Responsibility is about *handling* a request (passing it until handled); Decorator is about *augmenting* behaviour transparently (every decorator in the chain processes the call)
- [[Proxy Pattern]] — similar structure (wraps with same interface); Proxy *controls access* to the subject; Decorator *adds behaviour*; Proxy typically manages lifecycle, Decorator composition is client-controlled
