---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/system-design/builder-design-pattern/", "https://refactoring.guru/design-patterns/builder"]
tags: [design-pattern, creational, gof]
---

# Builder Pattern

Separates the construction of a complex object from its representation so that the same construction process can produce different representations.

## Problem

Some objects require many configuration options or a multi-step assembly sequence. Handling this through a constructor creates the "telescoping constructor" anti-pattern — a proliferation of overloaded constructors with different parameter combinations — or a constructor with a large, fragile parameter list where the order of arguments is easy to get wrong.

Refactoring Guru's illustration: building a `House` object could require walls, floor, roof, windows, doors, a garage, a swimming pool. The straightforward approach is a constructor with every possible parameter, most of them `null` most of the time. The alternative — subclassing for every combination — leads to an explosion of subclasses.

GfG's illustration: building a custom computer with optional CPU, RAM, storage, GPU, and cooling configurations. Each valid combination should be expressible without a separate constructor.

The problem is especially acute when: some parameters are optional, valid combinations are constrained (walls must precede roof), and the construction process may need to produce objects with quite different internal representations while following the same sequence of steps.

## Solution

Extract the construction steps into a separate Builder interface (`buildCPU()`, `buildRAM()`, `buildStorage()`). Each ConcreteBuilder implements the interface and tracks the partially-built object. An optional Director class encodes a specific construction sequence by calling builder methods in the right order. The client retrieves the finished product by calling a terminal method (`build()` or `getResult()`). Different ConcreteBuilders plugged into the same Director produce different product types.

```
interface ComputerBuilder:
    buildCPU()
    buildRAM()
    buildStorage()
    getResult() -> Computer

class GamingComputerBuilder(ComputerBuilder):
    // builds a high-end gaming machine

class OfficeComputerBuilder(ComputerBuilder):
    // builds a budget office machine

class Director:
    builder: ComputerBuilder

    constructStandardBuild():
        builder.buildCPU()
        builder.buildRAM()
        builder.buildStorage()
```

## Structure

| Participant | Role |
|---|---|
| Builder | Interface declaring construction steps |
| ConcreteBuilder | Implements steps; keeps track of the product being assembled; provides `getResult()` |
| Director | Defines the order of construction steps; works via the Builder interface; optional |
| Product | The complex object being assembled; often has no common interface across ConcreteBuilders |
| Client | Creates a builder, optionally associates it with a director, retrieves the product |

The Director is optional. When the construction sequence is simple or the client wants fine-grained control, the client can call builder steps directly. Modern applications frequently prefer the **Fluent Builder** variant without a Director.

## When to Use

- Constructing a complex object requires many steps or a specific sequence of steps.
- The same construction process must produce different representations of the product.
- You want to isolate the construction logic from the business logic that uses the product (Single Responsibility Principle).
- You need fine-grained control over the construction process (e.g. build only walls and roof, skip windows).
- You want to avoid a telescoping constructor with many optional parameters — the **Fluent Builder** variant is extremely common for this case.
- You need deferred or recursive construction (e.g. building a Composite tree step by step).

## Trade-offs

**Pros:**
- Constructs objects step-by-step; lets you defer or reuse construction steps.
- Reuses the same construction code for different product representations.
- Isolates complex construction logic from business logic (Single Responsibility Principle).
- Produces immutable products naturally — the builder accumulates all data before calling `build()`.
- Fluent Builder variant is self-documenting: named methods make long construction calls readable ("improves readability by avoiding constructors with many parameters").

**Cons / pitfalls:**
- Adds significant boilerplate: a separate Builder class (and optionally a Director) for each product family.
- If the product is simple, a plain constructor or static factory method is sufficient — Builder is overkill.
- The Director introduces an extra layer of indirection that can obscure what is being built.
- In languages without named parameters (Java, C++), keeping Builder and Product in sync requires disciplined maintenance.
- Requires modifications when product structure changes significantly.

## Variants

**Fluent Builder (method chaining):** Each setter returns `this` (the builder), enabling chained calls: `new PersonBuilder().name("Alice").age(30).build()`. Ubiquitous in Java (e.g. `StringBuilder`, `HttpRequest.Builder`, Lombok `@Builder`), Kotlin DSL builders, and many JavaScript/TypeScript libraries. The GfG source notes: "modern applications prefer a fluent builder approach."

**Inner Builder:** The builder is a static nested class of the product. The product's constructor is private and accepts only the builder. Common in Java when you want to enforce that the product can only be created through the builder.

**Director-less Builder:** The Director is omitted; the client drives the construction sequence directly. Appropriate when the client must compose products from arbitrary step subsets.

**Director with multiple configurations:** A single Director can offer multiple construction methods (`constructMinimalProduct()`, `constructFullFeaturedProduct()`) for the same product type, each encoding a different step sequence.

## Code Example

Python — computer builder with Director:

````n# Product
class Computer:
    def __init__(self):
        self.cpu = None
        self.ram = None
        self.storage = None

    def display_info(self):
        print(f"CPU: {self.cpu}, RAM: {self.ram}, Storage: {self.storage}")

# Builder Interface
class Builder:
    def build_cpu(self): pass
    def build_ram(self): pass
    def build_storage(self): pass
    def get_result(self): pass

# Concrete Builder
class GamingComputerBuilder(Builder):
    def __init__(self):
        self.computer = Computer()

    def build_cpu(self):
        self.computer.cpu = "Gaming CPU i9"

    def build_ram(self):
        self.computer.ram = "32GB DDR5"

    def build_storage(self):
        self.computer.storage = "2TB NVMe SSD"

    def get_result(self):
        return self.computer

# Director
class ComputerDirector:
    def construct(self, builder: Builder):
        builder.build_cpu()
        builder.build_ram()
        builder.build_storage()

# Usage
builder = GamingComputerBuilder()
director = ComputerDirector()
director.construct(builder)
pc = builder.get_result()
pc.display_info()  # CPU: Gaming CPU i9, RAM: 32GB DDR5, Storage: 2TB NVMe SSD
```

Fluent Builder (Python pizza example):

````nclass Pizza:
    def __init__(self, size, crust, toppings):
        self.size = size
        self.crust = crust
        self.toppings = toppings

    def __repr__(self):
        return f"Pizza({self.size}, {self.crust}, {self.toppings})"

class PizzaBuilder:
    def __init__(self, size):
        self._size = size
        self._crust = "thin"
        self._toppings = []

    def crust(self, crust):
        self._crust = crust
        return self   # fluent

    def topping(self, t):
        self._toppings.append(t)
        return self   # fluent

    def build(self):
        return Pizza(self._size, self._crust, self._toppings)

pizza = (PizzaBuilder("large")
         .crust("thick")
         .topping("mozzarella")
         .topping("pepperoni")
         .build())
```

## Real-World Examples

- **String builders** — standard libraries across languages expose a mutable string-building object that accumulates parts step by step before producing the final immutable string.
- **HTTP request builders** — HTTP client libraries expose a builder API for constructing requests with optional headers, query parameters, body, and timeout before sending.
- **ORM query builders** — data access frameworks expose a fluent builder API to compose queries step by step, adding filters, joins, and ordering before execution.
- **Document generators** — PDF/HTML generation libraries use builder-style APIs to assemble documents from sections, headers, and pages.
- **UI component builders** — GUI frameworks accumulate widget configuration (size, color, event handlers) before rendering the final component.

## Related

- [[Abstract Factory Pattern]] — Abstract Factory returns an object immediately; Builder assembles it over multiple steps
- [[Prototype Pattern]] — Prototype copies an existing object; Builder constructs a new one from scratch
- [[Composite Pattern]] — Directors often build Composite trees using a Builder
- [[Singleton Pattern]] — Builders themselves are occasionally Singletons when shared across threads (but usually they are short-lived)
- Bridge — pairs well with Builder: Director acts as the abstraction, builders as implementations
