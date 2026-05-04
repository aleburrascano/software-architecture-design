---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-03'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/flyweight
  - https://www.geeksforgeeks.org/system-design/flyweight-design-pattern/
tags:
  - design-pattern
  - structural
  - gof
---
# Flyweight Pattern

Reduces memory consumption by sharing as much state as possible between a large number of fine-grained objects, separating intrinsic (shared, immutable) state from extrinsic (context-specific) state.

## Problem

An application creates a very large number of objects that are similar but not identical — a text editor with millions of character objects, a game world with tens of thousands of tree or particle objects, a GUI with thousands of icon instances. Each object holds redundant copies of state that is identical across many instances (e.g., the glyph bitmap, the tree texture, the particle colour and sprite). The cumulative memory footprint becomes prohibitive and can crash the application on lower-memory hardware.

A concrete scenario: a game with bullets, missiles, and shrapnel. Each particle object might store colour, sprite, coordinates, and velocity. Coordinates and velocity are unique per particle, but colour and sprite are shared across thousands of objects of the same type. Storing them individually wastes significant RAM.

## Solution

Identify the *intrinsic state* — state that is constant across many objects and can be shared — and extract it into a `Flyweight` object. The *extrinsic state* — what varies per context (position, colour, owner) — is stored outside the flyweight and passed in by the client at call time. A `FlyweightFactory` maintains a pool/cache of flyweight instances and returns existing ones rather than creating duplicates.

```
class Flyweight:
    intrinsicState: ...     // shared, immutable; set only in constructor

    operation(extrinsicState):
        // use both intrinsic and extrinsic state

class FlyweightFactory:
    pool: Map<key, Flyweight>

    getFlyweight(key):
        if key not in pool:
            pool[key] = new Flyweight(key)
        return pool[key]
```

Implementation steps:
1. Divide class fields into intrinsic (shared) and extrinsic (unique) categories.
2. Keep intrinsic fields immutable in the Flyweight class; initialise only via constructor.
3. Refactor methods that use extrinsic state to accept those values as parameters.
4. Create a factory to manage the flyweight pool and return existing instances.
5. Store extrinsic state in Context objects (formerly the fat objects) alongside flyweight references.

## Structure

| Participant | Role |
|---|---|
| Flyweight | Stores intrinsic state (immutable); `operation()` receives extrinsic state as a parameter |
| ConcreteFlyweight | A concrete flyweight with specific shared data |
| UnsharedConcreteFlyweight | A flyweight subclass that is *not* shared (holds all its state); used when sharing is not appropriate for a particular instance |
| FlyweightFactory | Creates and caches flyweights; ensures sharing; returns existing instance or creates a new one |
| Context (Client) | Holds extrinsic state; calls flyweight methods by passing extrinsic state; each context object is lightweight — it contains only the unique data plus a flyweight reference |

## When to Use

Apply Flyweight only when all of the following hold:
- The application uses a very large number of objects that strain available memory.
- Most object state can be made extrinsic (moved out to context objects or computed on the fly).
- Many groups of objects can be replaced by relatively few shared objects once extrinsic state is removed.
- The application does not depend on object identity — many clients can share the same flyweight instance without noticing.

## Trade-offs

**Pros:**
- Dramatic memory savings when a large number of objects share significant intrinsic state.
- Can improve cache performance due to higher object reuse.
- Minimises object creation overhead for repeated types.

**Cons / pitfalls:**
- Trades memory for CPU time: extrinsic state must be computed or fetched on each call, adding overhead.
- Complicates the code: clients must track extrinsic state; the intrinsic/extrinsic split must be rigorously maintained.
- Flyweight objects must be immutable (intrinsic state must not change after creation), which can require upfront refactoring.
- If few objects actually share state, the overhead is not justified.
- Can be difficult for new team members to understand.

## Code Example

```python
class TreeType:
    """Flyweight: shared intrinsic state (species, texture, colour).
    Imagine self.texture is a large bitmap loaded from disk."""
    def __init__(self, name: str, colour: str, texture: str):
        self.name = name
        self.colour = colour
        self.texture = texture

    def draw(self, x: int, y: int):
        print(f"Drawing {self.name} ({self.colour}) at ({x}, {y})")

class TreeFactory:
    """FlyweightFactory: caches and returns shared TreeType instances."""
    _types: dict = {}

    @classmethod
    def get(cls, name: str, colour: str, texture: str) -> TreeType:
        key = (name, colour, texture)
        if key not in cls._types:
            cls._types[key] = TreeType(name, colour, texture)
            print(f"[factory] Created new TreeType: {name}")
        return cls._types[key]

class Tree:
    """Context: holds extrinsic state (position) + reference to flyweight."""
    def __init__(self, x: int, y: int, tree_type: TreeType):
        self.x = x
        self.y = y
        self.type = tree_type

    def draw(self):
        self.type.draw(self.x, self.y)

# Plant 10,000 oak trees — only ONE TreeType object is ever created
forest = [
    Tree(i * 3, i * 7, TreeFactory.get("Oak", "green", "oak_texture.png"))
    for i in range(10_000)
]
forest[0].draw()
print(f"TreeType instances: {len(TreeFactory._types)}")    # 1
print(f"Tree instances:     {len(forest)}")                 # 10000
```

Icon caching example (GfG illustration):

```java
// Flyweight: FileIcon stores shared intrinsic state
class FileIcon implements Icon {
    private final String type;
    private final String imageName;   // heavy: loaded from disk once

    public void draw(int x, int y) {
        System.out.println("Drawing " + type + " icon at (" + x + ", " + y + ")");
    }
}

// Factory: returns cached instance or creates new one
class IconFactory {
    private Map<String, Icon> cache = new HashMap<>();

    public Icon getIcon(String key) {
        return cache.computeIfAbsent(key, k -> new FileIcon(k, k + ".png"));
    }
}
```

## Real-World Examples

- **Text editors / word processors** — character glyph objects store font family, size, and style as intrinsic state (shared across all 'A' characters in the same font); position and paragraph context are extrinsic. Java's `String` interning applies the same principle.
- **Game engines** — particles, trees, grass blades, enemy units of the same type all share a single model/texture flyweight object; position, health, and rotation are extrinsic.
- **Java `Integer` cache** — the JVM caches `Integer` objects for values −128 to 127 (intrinsic: the integer value itself). `Integer.valueOf(100) == Integer.valueOf(100)` is `true` because the same flyweight is returned.
- **Java `String` pool** — string literals are interned; the JVM maintains a flyweight pool of unique string values.
- **GUI icon rendering** — a file manager displaying thousands of identical folder or document icons reuses a single shared `Icon` flyweight object; the position of each icon on screen is extrinsic.
- **Traffic signal systems** — signal type objects (red, amber, green) are shared flyweights; the intersection location is extrinsic.

## Related

- [[Composite Pattern]] — Flyweight leaf nodes are often used as leaves in a Composite tree; combining both patterns enables memory-efficient hierarchical structures
- [[Singleton Pattern]] — FlyweightFactory is often a Singleton; individual flyweight objects resemble Singletons keyed by their intrinsic state; the key difference is that Singleton has one mutable instance while Flyweight has multiple immutable instances, each unique by key
- [[Strategy Pattern]] — Flyweights can be passed as strategies (shared, stateless algorithm objects) when the algorithm has no mutable state
- [[Facade Pattern]] — structural contrast: Facade represents an entire subsystem as a single object; Flyweight represents many tiny objects sharing internal state
