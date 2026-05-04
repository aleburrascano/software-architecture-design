---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: [Creational Design Patterns.md, "https://www.geeksforgeeks.org/java/object-pool-design-pattern/"]
tags: [design-pattern, creational, gof]
---

# Object Pool Pattern

Manages a set of initialised, reusable objects so that clients can acquire one, use it, and return it rather than constructing and destroying objects on every request.

## Problem

Some objects are expensive to create — establishing a database connection, opening a file handle, spawning a thread, or allocating a large buffer — yet they are needed repeatedly for short bursts of work. As GfG states: "creating a connection is an expensive operation. When there are too many connections opened it takes longer to create a new one and the database server will become overloaded."

Creating a new instance on every request adds latency and increases GC pressure (in managed runtimes); destroying the instance immediately after use throws away the initialisation work. Under high load, the cost of rapid creation/destruction can dominate application performance.

## Solution

Pre-allocate (or lazily allocate, up to a configured maximum) a pool of instances. When a client needs an object, it calls `acquire()` (or `takeOut()`) on the pool. If an idle instance is available, the pool returns it immediately; otherwise it creates one (up to the pool limit) or blocks/throws. When the client is done, it calls `release(object)` (or `takeIn()`) to return the object to the pool rather than discarding it. The pool resets or validates the object before making it available again.

```
class ObjectPool<T>:
    available: List<T>
    inUse:     Set<T>
    maxSize:   int

    acquire() -> T:
        if available is not empty:
            obj = available.pop()
        elif total < maxSize:
            obj = createNew()
        else:
            throw PoolExhaustedException   // or block, or queue
        inUse.add(obj)
        return obj

    release(obj: T):
        validate(obj)   // reset state; discard if broken
        inUse.remove(obj)
        available.push(obj)
```

## Structure

| Participant | Role |
|---|---|
| ObjectPool | Manages available and in-use instance lists; implements `acquire()`/`takeOut()` and `release()`/`takeIn()`; enforces pool size limits |
| PooledObject | The expensive resource being managed; should be resettable to a clean state |
| Client | Calls `acquire()` before use and `release()` after; must not use an object after releasing it |
| PooledObjectFactory (optional) | Encapsulates creation, validation, and destruction logic; used in Apache Commons Pool and similar libraries |

### Object lifecycle stages
1. **Creation** — object is initialised and added to the pool's available list.
2. **Borrowing** — client calls `acquire()`; object moves to the in-use set.
3. **Usage** — client performs work with the object.
4. **Returning** — client calls `release()`; pool validates and resets the object.
5. **Rejection/Destruction** — expired or invalid objects are discarded (not returned to available list).

## When to Use

- Object creation is expensive (database connections, thread creation, socket connections, graphics contexts).
- Objects are needed frequently for short durations and creating/destroying on each use would dominate runtime cost.
- The number of simultaneously-used instances is bounded and known — you can set a meaningful pool size.
- Objects can be safely reset to a clean state before reuse.

Do **not** use Object Pool when:
- Objects are cheap to create (plain data objects, value types).
- Objects carry state that cannot be fully reset between uses, making reuse unsafe.
- The pool size would effectively be one — use [[Singleton Pattern]] instead.

## Trade-offs

**Pros:**
- Dramatically reduces per-request latency when creation is expensive.
- Caps total resource usage — pool max size acts as a back-pressure mechanism.
- Reduces GC pressure in managed runtimes by reusing long-lived objects.
- Can improve cache locality because the same objects are reused repeatedly.
- Controls maximum concurrent object instances, preventing resource exhaustion.

**Cons / pitfalls:**
- **Stale state:** If an object is not properly reset before reuse, a client sees leftover state from a previous user — a subtle and dangerous bug.
- **Leaked objects:** If a client acquires an object and never releases it (exception, early return, forgot), the pool shrinks over time until it is exhausted. Use try/finally or RAII patterns to enforce release.
- **Thread safety:** The pool data structures (`available`, `inUse`) must be protected with synchronisation in multi-threaded environments. Naive implementations introduce race conditions.
- **Sizing complexity:** Too small a pool causes contention and blocking; too large wastes memory and initialisation overhead.
- **Complexity cost:** Adds lifecycle management that is unnecessary when creation is cheap.
- Potential memory bloat if pool size is oversized.

## Thread Safety

In concurrent environments, acquire/release must be atomic. Common approaches:

- Use a `BlockingQueue` (Java) or equivalent: `acquire()` calls `queue.poll()` (non-blocking) or `queue.take()` (blocking).
- Use `synchronized` blocks or a `Lock` around pool operations (GfG's abstract implementation uses `synchronized` on `takeOut()` and `takeIn()`).
- Per-thread pools (thread-local storage) eliminate contention at the cost of memory.

Validate objects on acquisition (not just on release) because objects can become invalid while idle (e.g. a TCP connection times out in the pool). GfG's implementation includes an expiry check: objects idle longer than `deadTime` (50 seconds by default) are expired before a new object is returned.

## Code Example

Java — generic ObjectPool with expiry (GfG):

```java
abstract class ObjectPool<T> {
    private long deadTime = 50000; // 50 seconds
    private Hashtable<T, Long> locked = new Hashtable<>();
    private Hashtable<T, Long> unlocked = new Hashtable<>();

    protected abstract T create();
    protected abstract boolean validate(T o);
    protected abstract void expire(T o);

    public synchronized T takeOut() {
        long now = System.currentTimeMillis();
        if (!unlocked.isEmpty()) {
            for (T t : unlocked.keySet()) {
                if ((now - unlocked.get(t)) > deadTime) {
                    unlocked.remove(t);
                    expire(t);
                } else if (validate(t)) {
                    unlocked.remove(t);
                    locked.put(t, now);
                    return t;
                } else {
                    unlocked.remove(t);
                    expire(t);
                }
            }
        }
        T t = create();
        locked.put(t, now);
        return t;
    }

    public synchronized void takeIn(T t) {
        locked.remove(t);
        unlocked.put(t, System.currentTimeMillis());
    }
}
```

Python — thread-safe pool with BlockingQueue:

```python
import threading
from queue import Queue

class DatabaseConnection:
    def __init__(self, id):
        self.id = id
        self._active = True

    def query(self, sql):
        return f"[Conn {self.id}] result of: {sql}"

    def reset(self):
        pass  # clear any transaction state

    def is_valid(self):
        return self._active

class ConnectionPool:
    def __init__(self, max_size: int):
        self._pool  = Queue()
        self._lock  = threading.Lock()
        self._count = 0
        self._max   = max_size

    def acquire(self) -> DatabaseConnection:
        try:
            conn = self._pool.get_nowait()
            if not conn.is_valid():
                conn = self._create()
            return conn
        except Exception:
            with self._lock:
                if self._count < self._max:
                    return self._create()
            return self._pool.get(timeout=5)  # block until one is returned

    def release(self, conn: DatabaseConnection):
        conn.reset()
        self._pool.put(conn)

    def _create(self):
        with self._lock:
            self._count += 1
        return DatabaseConnection(self._count)

# Usage
pool = ConnectionPool(max_size=5)
conn = pool.acquire()
print(conn.query("SELECT 1"))
pool.release(conn)
```

## Real-World Examples

- **JDBC Connection Pools** — HikariCP, Apache DBCP, c3p0 all implement Object Pool for database connections. HikariCP is the default pool in Spring Boot.
- **Java `ThreadPoolExecutor`** — the JVM's standard thread pool is an Object Pool for `Thread` objects; `Executors.newFixedThreadPool(n)` creates a pool of n reusable threads.
- **Apache Commons Pool** — generic pooling library with `PooledObjectFactory` interface, used by Jedis (Redis client), Apache HttpClient, and others.
- **HTTP connection pools** — `requests.Session` in Python, `HttpClient` in .NET, and `OkHttpClient` in Java all pool TCP connections to avoid reconnecting on every request.
- **Game engines** — bullet pools, particle pools, enemy pools. Creating and destroying objects per frame is too expensive; pools keep objects alive and reset them on reuse.
- **`ByteBuffer` pools** in Netty — Netty's `PooledByteBufAllocator` pools memory buffers to avoid GC overhead in high-throughput network I/O.

## Related

- [[Singleton Pattern]] — the pool itself is typically a Singleton; Object Pool generalises Singleton to N reusable instances
- [[Factory Method Pattern]] — pool uses a factory to create new instances when the pool is empty
- [[Prototype Pattern]] — prototype cloning can serve as the creation strategy inside a pool
- [[Lazy Initialization]] — a pool can lazily create objects up to its max size rather than pre-allocating all of them at startup
