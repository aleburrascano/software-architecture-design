---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-06'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/proxy
  - https://www.geeksforgeeks.org/system-design/proxy-design-pattern/
tags:
  - design-pattern
  - structural
  - gof
---
# Proxy Pattern

Provides a surrogate or placeholder for another object, controlling access to it and optionally adding behaviour before or after the request reaches the real object.

## Problem

You need to control access to an object — because it is expensive to create, lives on a remote server, requires access control, or should support lazy initialisation — but you cannot or should not change the real object's class or its client code. You need a stand-in that the client treats identically to the real object.

A concrete example: a `ThirdPartyYouTubeLib` object downloads videos on every request. When the same video is requested repeatedly, the redundant downloads waste bandwidth. Modifying the library is impossible. Checking every call site for caching creates code duplication. The solution is a proxy that caches results transparently.

Real-world analogy: a credit card is a proxy for a bank account, which is a proxy for a bundle of cash. Both implement the same interface (they can be used for payment). The proxy adds convenience, security, and deferred access without changing the underlying resource.

## Solution

Create a `Proxy` class that implements the same interface as the `RealSubject`. The proxy holds a reference to a `RealSubject` instance (or creates one lazily). Client code talks to the proxy through the `Subject` interface; the proxy intercepts the request, performs whatever pre- or post-processing is needed, then delegates to the `RealSubject`.

```
interface Subject:
    request()

class RealSubject implements Subject:
    request(): // real work

class Proxy implements Subject:
    realSubject: RealSubject   // reference (may be lazy)

    request():
        // pre-processing (auth check, cache lookup, logging…)
        realSubject.request()
        // post-processing (logging, metric recording…)
```

## Structure

| Participant | Role |
|---|---|
| Service Interface | Declares the interface that both the Proxy and RealSubject must implement |
| RealSubject (Service) | The actual object that provides business logic; the real work |
| Proxy | Implements Service Interface; holds a reference to RealSubject; controls access; manages lifecycle; performs cross-cutting concerns |
| Client | Works through the Service Interface; unaware of whether it talks to Proxy or RealSubject |

## Proxy Types

**Virtual proxy (lazy initialisation):** Delays expensive creation of the RealSubject until it is actually needed. Example: a document viewer that shows placeholder thumbnails until images are scrolled into view; a `ProxyImage` that loads the real image from disk only on first `display()` call.

**Protection (access) proxy:** Checks that the caller has the required permissions before forwarding the request. Example: service layer enforcing role-based access control; only admins can call `delete()`.

**Remote proxy:** Represents an object in a different address space (another process, machine, or service). Handles marshalling and unmarshalling transparently. Example: RPC stubs, SOAP proxies, Java RMI.

**Caching proxy:** Caches results of expensive operations and returns the cached result for repeated requests. Example: a YouTube library proxy that caches downloaded video metadata; a database query proxy that caches `SELECT` results.

**Logging / auditing proxy:** Records all method calls for debugging, compliance, or monitoring, without modifying the RealSubject.

**Smart reference:** Performs extra bookkeeping when the real object is accessed — e.g. reference counting, locking/unlocking a heavyweight resource when no clients are using it.

## When to Use

- **Lazy initialisation (virtual proxy):** The RealSubject is heavy and you want to defer creation until the first real use.
- **Access control (protection proxy):** Different clients should have different levels of access to the same underlying object.
- **Remote access (remote proxy):** The real object lives in a different address space; the proxy handles all network/IPC communication.
- **Caching:** Repeated requests for the same result should be served from a cache without reaching the real object.
- **Logging, monitoring, or auditing:** You need to record requests around an existing interface without modifying it.
- **Lifecycle management:** You want to dismiss heavyweight objects when they are no longer referenced.

## Trade-offs

**Pros:**
- Open/Closed Principle: adds behaviour (caching, access control, logging) without modifying the RealSubject.
- Lazy initialisation defers cost to first use.
- Transparent to client — same interface as the real object.
- Remote proxy hides all network and serialisation details.
- Works even when the service object is not yet ready or available.

**Cons / pitfalls:**
- Adds an extra level of indirection on every call, which may introduce latency.
- Increases the number of classes in the design.
- Can hide errors if the proxy is not carefully implemented.
- If the proxy is too "smart", it becomes harder to reason about what actually happens on each call.
- Debugging becomes harder due to multiple abstraction layers.

## Code Example

````nfrom abc import ABC, abstractmethod

# Service Interface
class Image(ABC):
    @abstractmethod
    def display(self): ...

# RealSubject — expensive to create (loads from disk)
class RealImage(Image):
    def __init__(self, filename: str):
        self.filename = filename
        self._load()   # expensive operation at construction

    def _load(self):
        print(f"[RealImage] Loading from disk: {self.filename}")

    def display(self):
        print(f"[RealImage] Displaying: {self.filename}")

# Virtual Proxy — defers loading until display() is first called
class ProxyImage(Image):
    def __init__(self, filename: str):
        self.filename = filename
        self._real: RealImage = None   # not loaded yet

    def display(self):
        if self._real is None:
            self._real = RealImage(self.filename)   # lazy init
        self._real.display()

# Client
image = ProxyImage("photo.jpg")
print("Image object created — no loading yet")
image.display()   # loads now
image.display()   # no loading; delegates directly
```

Caching proxy example:

````nclass CachingDatabaseProxy:
    def __init__(self, real_db):
        self._real = real_db
        self._cache: dict = {}

    def execute(self, sql: str) -> list:
        if sql not in self._cache:
            print("[proxy] Cache miss — forwarding to DB")
            self._cache[sql] = self._real.execute(sql)
        else:
            print("[proxy] Cache hit")
        return self._cache[sql]
```

## Real-World Examples

- **Java RMI / CORBA stubs** — remote proxies that represent objects running in a different JVM. The stub implements the same interface and handles all network communication transparently.
- **Spring AOP (Aspect-Oriented Programming)** — Spring wraps beans in dynamic proxies to add cross-cutting concerns (transaction management, security checks, caching, logging) without modifying the service class. `@Transactional`, `@Cacheable`, `@Secured` all work via proxy interception.
- **Hibernate lazy loading** — `@OneToMany` relationships are initially represented by proxy collections that load from the database only when first accessed (virtual proxy).
- **Java `java.lang.reflect.Proxy`** — the JDK dynamic proxy API creates proxies at runtime for any interface, used extensively in frameworks (Spring, Mockito).
- **Web browsers loading images** — a placeholder proxy is displayed immediately; the real image is loaded asynchronously and replaces the proxy when ready (virtual proxy).
- **CDN and reverse proxies** (nginx, Cloudflare) — act as caching + protection proxies in front of origin web servers.
- **Mockito mocks** — test doubles are proxies that record method invocations and return configured responses without touching real dependencies.

## Related

- [[Decorator Pattern]] — also wraps an object with the same interface; Decorator's purpose is to *add behaviour*; Proxy's purpose is to *control access*. The structure is identical; the intent differs. Proxy typically manages object lifecycle; Decorator composition is client-controlled.
- [[Adapter Pattern]] — changes the *interface* of the wrapped object; Proxy keeps the *same* interface as the subject
- [[Facade Pattern]] — provides a *different* (simpler) interface to a subsystem; Proxy keeps the *same* interface; both buffer access to complex entities
