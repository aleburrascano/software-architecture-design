---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-03'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/composite
  - https://www.geeksforgeeks.org/system-design/composite-method-software-design-pattern/
tags:
  - design-pattern
  - structural
  - gof
---
# Composite Pattern

Composes objects into tree structures to represent part-whole hierarchies, and lets clients treat individual objects and compositions uniformly through a common interface.

## Problem

You are building a system with objects that can contain other objects of the same kind — file systems, UI widget trees, expression trees, organisational charts, or order systems where boxes contain products and other boxes. Client code must constantly distinguish between leaf nodes and container nodes: "if it's a file, call `size()`; if it's a folder, iterate and sum." This distinction leaks everywhere and makes extension painful.

A related problem: when calculating a total (order price, directory size, military strength), you must know recursively whether the thing you're looking at is a leaf or a container. Without a uniform interface, this branching logic appears at every call site.

## Solution

Define a common `Component` interface implemented by both leaf objects and composite objects (containers). The composite holds a collection of `Component` children and implements its operations by delegating to them recursively. Client code operates on `Component` and never needs to know whether it is talking to a leaf or a composite.

```
interface Component:
    operation()

class Leaf implements Component:
    operation():
        // do real work

class Composite implements Component:
    children: List<Component>

    add(c: Component)
    remove(c: Component)

    operation():
        for child in children:
            child.operation()
```

## Structure

| Participant | Role |
|---|---|
| Component | Common interface (or abstract class) for both leaves and composites; declares operations for the whole hierarchy |
| Leaf | Has no children; performs actual work; the basic building block of the tree |
| Composite | Stores child Components; delegates operations to them recursively; may aggregate results (e.g. sum prices, concatenate output) |
| Client | Works only through Component; doesn't distinguish leaf from composite |

### Child management placement

GoF discusses two options:

- **Declare child-management in Component** (`add`/`remove` on the interface): maximises transparency — client treats all nodes identically — but leaves must implement no-op or error methods for operations that don't apply to them.
- **Declare child-management only in Composite**: safer (leaves cannot be added to; better type safety), but client must downcast to Composite to manage the structure.

## When to Use

- You need to represent part-whole hierarchies of objects.
- You want client code to be able to ignore the difference between compositions of objects and individual objects — clients treat all objects in the structure uniformly.
- The application model can naturally be represented as a tree.
- You need to introduce new element types (new leaf or composite) without breaking existing client code.

## Trade-offs

**Pros:**
- Simplifies client code — no type-checking or branching on leaf vs. composite.
- Open/Closed Principle: adding new component types doesn't require changing existing client code.
- Recursive structures are natural to implement.
- Flexible addition/removal of objects at any level of the hierarchy.

**Cons / pitfalls:**
- The common interface may force leaves to implement methods that don't make sense for them (e.g., `add()`, `getChildren()`), reducing type safety.
- Deep trees can make debugging harder as an operation cascades many levels.
- Performance overhead when traversing deep or wide hierarchies.
- Increased memory consumption from storing child-reference collections in every composite node.

## Code Example

```python
from abc import ABC, abstractmethod
from typing import List

class FileSystemComponent(ABC):
    @abstractmethod
    def get_size(self) -> int: ...

    @abstractmethod
    def display(self, indent: int = 0): ...

class File(FileSystemComponent):
    def __init__(self, name: str, size: int):
        self.name = name
        self._size = size

    def get_size(self) -> int:
        return self._size

    def display(self, indent: int = 0):
        print(" " * indent + f"{self.name} ({self._size} bytes)")

class Directory(FileSystemComponent):
    def __init__(self, name: str):
        self.name = name
        self._children: List[FileSystemComponent] = []

    def add(self, item: FileSystemComponent):
        self._children.append(item)

    def get_size(self) -> int:
        return sum(c.get_size() for c in self._children)

    def display(self, indent: int = 0):
        print(" " * indent + f"{self.name}/")
        for child in self._children:
            child.display(indent + 2)

# Client treats File and Directory uniformly through FileSystemComponent
root = Directory("root")
src = Directory("src")
src.add(File("main.py", 1200))
src.add(File("utils.py", 800))
root.add(src)
root.add(File("README.md", 300))
root.display()
print(f"Total: {root.get_size()} bytes")
# root/
#   src/
#     main.py (1200 bytes)
#     utils.py (800 bytes)
#   README.md (300 bytes)
# Total: 2300 bytes
```

Order pricing example: a `Box` can contain `Product` objects or other `Box` objects. Both implement `get_price()`. Containers sum their children's prices (plus optional packaging cost). Client calls `get_price()` on the root without knowing the nesting depth.

## Real-World Examples

- **Java AWT / Swing** — `Component` is the composite interface; `Container` is the Composite class; individual widgets (`Button`, `Label`) are leaves. The UI tree is built and painted recursively.
- **XML / HTML DOM** — `Element` and `TextNode` both implement `Node`. DOM traversal and query operations work uniformly over the entire tree regardless of depth.
- **Apache Ant / Maven build scripts** — build tasks can contain sub-tasks, which can contain further sub-tasks. The build runner executes the tree without distinguishing leaves from containers.
- **React / Qt / Flutter component trees** — containers (panels, rows, stacks) and leaves (text, images, buttons) implement the same rendering interface. The framework renders the whole tree uniformly.
- **Military hierarchies** — armies contain divisions, divisions contain brigades, brigades contain platoons, platoons contain squads. An order or a headcount query propagates uniformly through every level.
- **Organisation charts** — departments contain teams, teams contain employees. "Total headcount" or "total salary budget" is computed identically at any level.

## Related

- [[Decorator Pattern]] — also uses recursive composition; Decorator wraps a *single* component to add behaviour; Composite aggregates *many* children to form a tree; Composite has one-to-many children, Decorator has exactly one wrapped component
- [[Iterator Pattern]] — commonly used to traverse Composite trees uniformly without exposing their internal structure
- [[Visitor Pattern]] — used to apply an operation across a Composite tree without modifying the component classes themselves
- [[Flyweight Pattern]] — can reduce memory when a Composite tree has many repeated leaf nodes; leaf flyweights share intrinsic state
- [[Builder Pattern]] — can construct complex Composite trees step-by-step
- [[Chain of Responsibility Pattern]] — often implemented on top of a Composite tree, passing requests from parent components to child components
