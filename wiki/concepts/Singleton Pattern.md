---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/system-design/singleton-design-pattern/", "https://refactoring.guru/design-patterns/singleton"]
tags: [design-pattern, creational, gof]
---

# Singleton Pattern

Ensures a class has exactly one instance and provides a global access point to that instance.

## Problem

Some resources must exist exactly once in a system — a configuration registry, a logger, a connection pool, or a hardware interface. Using a plain global variable allows the instance to be replaced accidentally; constructing the object wherever it is needed risks creating multiple instances that get out of sync or waste resources.

The pattern addresses two distinct concerns simultaneously: controlling how many instances exist, and giving every part of the system a consistent way to reach that single instance. As Refactoring Guru puts it: "a class has only one instance, while providing a global access point to this instance."

A regular constructor always returns a **new** object; Singleton breaks that contract through a private constructor and a static accessor.

## Solution

Make the constructor private (or protected) so no external code can call `new`. Hold the single instance in a private static field. Expose a static method (conventionally `getInstance()`) that creates the instance on the first call and returns the cached reference on every subsequent call.

```
class Singleton:
    _instance = null

    private constructor()

    static getInstance():
        if _instance is null:
            _instance = new Singleton()
        return _instance
```

## Structure

The Singleton class owns three elements that work together:

| Participant | Role |
|---|---|
| Private static field `_instance` | Holds the one and only instance; `null` until first access |
| Private constructor | Prevents external code from calling `new Singleton()` directly |
| Public static `getInstance()` | Lazy-initialises on first call; returns the cached reference on all subsequent calls |

There are no other participants — the class is both its own factory and its own product.

## When to Use

- Exactly one object is needed to coordinate across the system (e.g. a logging service, an event bus, a configuration store, a database connection manager).
- The single instance should be accessible from a well-known point without passing it through every call chain.
- The instance is expensive to create and should be reused (can combine with [[Object Pool Pattern]] when multiple reusable instances are acceptable).
- A shared resource (hardware device, file handle, registry) must be accessed through a single controlled point.

Do **not** reach for Singleton merely because a class feels "global" — this often signals a need for proper dependency injection instead.

## Trade-offs

**Pros:**
- Guarantees a single instance — eliminates "who owns the resource" ambiguity.
- Lazy initialisation defers the creation cost until first use.
- Provides a well-known access point without threading it through every API.
- Saves memory by avoiding multiple object creation.

**Cons / pitfalls:**
- Introduces global state, which couples every consumer to the class and makes unit testing hard (you cannot easily substitute a mock — private constructors defeat reflection-based testing frameworks unless explicitly handled).
- Violates the Single Responsibility Principle: the class manages both its own domain logic and its own lifecycle.
- In multithreaded environments, naive implementations create race conditions — two threads can simultaneously see `_instance == null` and each create a separate instance.
- Inheritance is awkward because the constructor is private.
- Makes it difficult to detect which parts of the codebase depend on the instance (hidden dependency).
- Can mask poor design — "many components depend on Singleton" is often a symptom of missing abstraction layers.
- Java serialisation can create a second instance unless `readResolve()` is overridden; reflection can invoke the private constructor.

## Variants

**Eager initialisation (Static field):** Create the instance at class-load time, not on first call. Thread-safe by definition in languages with class-loading guarantees (e.g. Java static field initialiser), but incurs the creation cost even if the instance is never used.

```java
class Singleton {
    private static Singleton obj = new Singleton();
    private Singleton() {}
    public static Singleton getInstance() { return obj; }
}
```

**Thread-safe synchronized method:** Synchronize `getInstance()` so only one thread enters at a time. Simple, but every call acquires the lock — high contention overhead after the instance exists.

```java
public static synchronized Singleton getInstance() {
    if (obj == null)
        obj = new Singleton();
    return obj;
}
```

**Double-checked locking (DCL):** Check `_instance == null` without a lock, enter a `synchronized` block, check again, then create. Avoids the lock overhead on every call after creation. Requires `volatile` in Java to prevent instruction reordering.

```java
class Singleton {
    private static volatile Singleton obj = null;
    public static Singleton getInstance() {
        if (obj == null) {
            synchronized (Singleton.class) {
                if (obj == null)
                    obj = new Singleton();
            }
        }
        return obj;
    }
}
```

**Initialization-on-demand holder (Java — preferred):** A private static inner class holds the instance. The JVM guarantees its static initialiser runs exactly once when first accessed, providing lazy initialisation without explicit synchronisation.

```java
public class Singleton {
    private Singleton() {}
    private static class SingletonInner {
        private static final Singleton INSTANCE = new Singleton();
    }
    public static Singleton getInstance() {
        return SingletonInner.INSTANCE;
    }
}
```

**Enum Singleton (Java — safest):** Declare a single-element `enum`. The JVM serialisation mechanism and reflection cannot break it. Considered the most robust Java implementation (Effective Java, Bloch). Handles serialisation and reflection attacks automatically.

```java
public enum Singleton {
    INSTANCE;
    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```

**Monostate:** All fields are static; multiple instances share the same state. Achieves behavioural singularity without restricting instantiation count.

## Code Example

Python — basic lazy Singleton using `__new__`:

```python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super(Singleton, cls).__new__(cls)
        return cls._instance
```

Python — thread-safe double-checked locking:

```python
import threading

class ThreadSafeSingleton:
    _instance = None
    _lock = threading.Lock()

    @classmethod
    def get_instance(cls):
        if cls._instance is None:
            with cls._lock:
                if cls._instance is None:   # double-checked
                    cls._instance = cls()
        return cls._instance
```

## Real-World Examples

- **`java.lang.Runtime`** — `Runtime.getRuntime()` returns the single JVM runtime instance.
- **`java.awt.Desktop`** and **`java.awt.Toolkit`** — single system-level interface objects.
- **Logging frameworks** (Log4j, SLF4J, Python `logging` module) — a single logger registry per application.
- **Spring Framework** — beans scoped as `singleton` (the default scope) are managed as Singletons within the application context.
- **OS-level resources** — file system managers, window managers, and print spoolers are commonly modelled as Singletons.
- **Government analogy (Refactoring Guru):** A country can have only one official government — regardless of who comprises it, the title "Government of X" is a global access point to that single entity.

## Related

- [[Object Pool Pattern]] — generalises Singleton to a fixed set of reusable instances
- [[Factory Method Pattern]] — can be used to centralise instance creation without enforcing singularity
- [[Abstract Factory Pattern]] — factories are frequently implemented as Singletons
- [[Lazy Initialization]] — the deferred-creation technique used in lazy Singleton implementations
- Facade — a Facade object often becomes a Singleton since a single facade object is usually sufficient
