---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/system-design/lazy-loading-design-pattern/"]
tags: [design-pattern, creational, gof]
---

# Lazy Initialization

Defers the creation or computation of an expensive resource until the first time it is actually needed.

## Problem

Eagerly initialising all resources at startup increases startup time, consumes memory even for resources that may never be used in a given session, and can cause failures (e.g. a missing configuration) to surface at startup rather than at the point of actual use. Applications with many optional subsystems or large data structures particularly suffer from this cost.

GfG's framing: "applications often create expensive or rarely-used resources upfront, wasting memory and increasing startup time." Real examples — an e-commerce site loading all product images at page load, a social media app fetching all comments on open, a game engine loading all high-resolution textures at launch.

## Solution

Guard the resource with a null check. On the first access, create the resource, store it in a field, and return it. On every subsequent access, return the already-created instance directly. The resource creation cost is paid exactly once, and only if the resource is needed.

```
class Service:
    _resource = null

    getResource():
        if _resource is null:
            _resource = expensiveCreate()
        return _resource
```

This is the core idiom. The pattern appears as a building block inside other patterns ([[Singleton Pattern]], [[Object Pool Pattern]], [[Proxy Pattern]]) rather than standing alone.

## Structure

Lazy Initialization is a technique, not a class structure. It typically manifests as:

| Role | Description |
|---|---|
| Holder field | A private field, initially null (or a sentinel value), that caches the resource after first creation |
| Guard condition | An `if` check on the holder field before each access |
| Initialiser | The code path that runs on first access to create and populate the holder field |
| Accessor | The method or property that encapsulates the guard + initialiser |

### Structural variations (GfG's four implementation forms)

**Virtual Proxy:** A lightweight proxy object stands in for the real heavyweight object. The proxy implements the same interface, but creates the real object only on the first method call. Callers never know whether they hold a proxy or the real thing.

````nclass ContactListProxyImpl implements ContactList {
    private ContactList contactList;

    public List<Employee> getEmployeeList() {
        if (contactList == null) {
            System.out.println("Fetching list of employees");
            contactList = new ContactListImpl();
        }
        return contactList.getEmployeeList();
    }
}
```

**Lazy Initialization (field-level):** An object checks a field's value on access; if null, loads before returning. The thread-safe variant uses double-checked locking.

**Value Holder:** A container object wraps a value. Its `get()` method triggers initialisation if the value hasn't been computed yet. Similar to `Lazy<T>` in .NET, `lazy` in Kotlin, and Guava's `Suppliers.memoize()` in Java. Drawback: users must know a value holder is expected, requiring API awareness.

````nclass ValueHolder:
    def __init__(self, value_retrieval):
        self.value = None
        self.value_retrieval = value_retrieval

    def get_value(self, parameter):
        if self.value is None:
            self.value = self.value_retrieval(parameter)
        return self.value
```

**Ghost / Partial Loading:** An object is loaded with partial data (e.g. only ID). Further data (e.g. full details of a database record) is loaded lazily when accessed for the first time. Common in ORMs (Hibernate lazy-loading of associations). The object exists but is not yet fully initialised — it is a "ghost."

## When to Use

- The resource is expensive to create (database connections, large data structures, heavy computation).
- The resource is not needed on every code path — many executions may not use it at all.
- You want to improve startup time by deferring initialisation until needed.
- You want errors related to a resource to surface at point of use, not at startup.
- Combined with [[Singleton Pattern]] when you want both a single instance and deferred creation.
- Web applications loading images/modules conditionally (lazy-loading images on scroll, comments on click, textures on zone entry in games).

Avoid lazy initialisation when:
- The resource is always needed — eagerness avoids the null-check overhead on every access.
- Thread safety of the initialisation is difficult to guarantee correctly.
- Fail-fast behaviour at startup is preferable to a runtime failure later.

## Trade-offs

**Pros:**
- Reduces startup time and memory footprint when many resources are conditionally used.
- Shifts cost to the first-use moment — closer to when the user performs the corresponding action.
- Integrates naturally with other creational patterns (Singleton, Proxy, Object Pool).
- Avoids unnecessary resource allocation.

**Cons / pitfalls:**
- **Thread safety:** Without synchronisation, two threads can simultaneously see the holder as null, each entering the initialiser, and create duplicate instances. Use locks, double-checked locking, or language-level constructs (`std::once_flag` in C++, `Lazy<T>` in .NET, `lazy` in Kotlin, inner holder class in Java) to protect initialisation.
- **First-call latency spike:** The first caller incurs the full creation cost, which may be unacceptable in latency-sensitive paths. Eager initialisation (at startup or in a background thread) is better for those cases.
- **Obscured dependencies:** Lazy resources hide the fact that a component depends on them, making dependency graphs harder to reason about and test.
- **Null pointer risk:** If the lazy accessor is bypassed or the guard condition is written incorrectly, callers can receive null.
- Harder debugging due to deferred initialisation — errors surface far from their cause.

## Thread Safety Approaches

**Synchronized method (simple, but contended):**
````nsynchronized Resource getResource() {
    if (resource == null) resource = new Resource();
    return resource;
}
```

**Double-checked locking (DCL) with `volatile`:**
````nvolatile Resource resource;

Resource getResource() {
    if (resource == null) {
        synchronized (this) {
            if (resource == null) resource = new Resource();
        }
    }
    return resource;
}
```
Requires `volatile` to prevent the JVM from reordering the write to `resource` before the constructor completes. GfG recommends this for high-concurrency scenarios: "check before acquiring lock, re-check after acquiring to prevent race conditions."

**Initialization-on-demand holder (Java — preferred):**
````nclass Holder {
    private static class Inner {
        static final Resource INSTANCE = new Resource();
    }
    static Resource get() { return Inner.INSTANCE; }
}
```
The JVM class loader guarantees `Inner` is initialised exactly once, thread-safely, without explicit locking.

**.NET `Lazy<T>`:**
````nLazy<Resource> _lazy = new Lazy<Resource>(() => new Resource());
Resource r = _lazy.Value;   // thread-safe by default
```

**Kotlin `lazy` delegate:**
````nval resource: Resource by lazy { Resource() }   // default: SYNCHRONIZED mode
```

## Code Example

Python — lazy-initialised template in a report generator:

````nclass ReportGenerator:
    def __init__(self):
        self._template = None  # lazily initialised

    @property
    def template(self):
        if self._template is None:
            self._template = self._load_template()  # expensive
        return self._template

    def _load_template(self):
        print("Loading template (once)...")
        return "<html>...</html>"

    def generate(self, data):
        return self.template.replace("...", str(data))
```

Java — thread-safe lazy init with double-checked locking (car type cache, from GfG):

````npublic static Car getCarByTypeNameHighConcurrentVersion(CarType type) {
    if (!types.containsKey(type)) {
        synchronized (types) {
            if (!types.containsKey(type)) {
                types.put(type, new Car(type));
            }
        }
    }
    return types.get(type);
}
```

## Real-World Examples

- **ORMs (Hibernate, SQLAlchemy)** — lazy-loading of associations: a `User.orders` collection is not fetched from the database until it is first accessed. This is the Ghost/Partial Loading variant.
- **Spring Framework `@Lazy`** — beans annotated with `@Lazy` are not instantiated until first requested from the context.
- **Browser image lazy-loading** (`loading="lazy"` attribute) — images below the fold are fetched only when the user scrolls to them.
- **E-commerce product pages** — product images load on scroll; comments load on "View Comments" click; related products load when visible.
- **Game engines** — high-resolution textures and level geometry load when the player enters a specific zone, not at game start.
- **Python `importlib`** — modules can be imported lazily; PEP 562 enables lazy module-level attributes.
- **Java `Class.forName()`** — class loading is inherently lazy; a class is loaded only when first referenced.

## Related

- [[Singleton Pattern]] — lazy initialisation is the standard implementation strategy for lazy Singletons
- [[Object Pool Pattern]] — pools commonly use lazy initialisation to create pool members on first demand up to max size
- [[Proxy Pattern]] — the Virtual Proxy structural pattern is the architectural embodiment of lazy initialisation
- [[Factory Method Pattern]] — a factory method is often what the lazy accessor calls to create the resource
