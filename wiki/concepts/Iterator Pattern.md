---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/iterator-pattern/", "https://refactoring.guru/design-patterns/iterator"]
tags: [design-pattern, behavioral, gof]
---

# Iterator Pattern

A behavioral pattern that provides a uniform way to sequentially access the elements of a collection without exposing the collection's underlying data structure.

## Problem

Traversal logic for a collection (list, tree, graph, stack) is often embedded inside the collection itself or duplicated across client code. Clients that need to traverse in different ways (forward, backward, breadth-first) are tightly coupled to the collection's internal representation. Adding a second traversal order requires modifying the collection class.

A company storing employees across different data structures (arrays, linked lists, trees per department) needs a uniform traversal mechanism that works regardless of internal representation. Adding traversal directly to each collection "blurs their primary responsibility" and creates an ever-growing interface as new traversal algorithms accumulate.

## Solution

Extract traversal logic into a separate **Iterator** object. The iterator maintains a pointer into the collection and exposes a uniform interface (`hasNext()`, `next()`). The collection implements a method (`getIterator()`) that returns an appropriate iterator. Clients iterate through the collection via the iterator without knowing whether the backing structure is an array, linked list, or tree.

## Structure

| Participant | Role |
|---|---|
| **Iterator** (interface) | Declares `hasNext()`, `next()`, and optionally `remove()` |
| **ConcreteIterator** | Implements traversal for a specific collection; tracks current position |
| **IterableCollection** (interface) | Declares `createIterator()` / `__iter__()` |
| **ConcreteCollection** | Implements `createIterator()`, returns the matching ConcreteIterator |
| **Client** | Uses only Iterator and IterableCollection interfaces |

## When to Use

- You want to traverse a collection without knowing its internal structure.
- You need multiple simultaneous traversals of the same collection.
- You want to provide a uniform traversal interface across different collection types.
- You need to decouple iteration algorithms from the collection (e.g., different traversal orders via different iterators over the same tree).
- You want to reduce duplication of traversal code across the application.
- The collection has a complex internal structure that should stay hidden from clients.

## Trade-offs

**Pros:**
- Single Responsibility: traversal logic is isolated in the iterator.
- Open/Closed: add new iterators or new collections independently.
- Parallel iterations are straightforward — each iterator is independent.
- Uniform client code regardless of collection type.
- Supports delayed iteration continuation (pause and resume traversal).
- "Cleans up client code and collections by extracting bulky traversal algorithms."

**Cons / pitfalls:**
- Overkill if you only have simple list traversal — a `for` loop suffices.
- Iterator state may become stale if the underlying collection is modified during iteration (concurrent modification exception in Java).
- Stateful iterators are not thread-safe by default; external synchronization may be needed.
- Additional classes for simple use cases add complexity.
- May be less efficient than direct indexed access for simple collections.

## Variants

- **Internal iterator** — the collection drives the iteration; clients pass a function/callback (e.g., `forEach`, `map` in functional languages).
- **External iterator** — the client drives the iteration; holds the iterator and calls `next()` explicitly. More control, required for interleaved iterations.
- **Lazy / generator-based iterator** — values are computed on demand rather than stored (Python generators, Java `Stream`, C# `IEnumerable` with `yield`).
- **Reverse iterator** — traverses in reverse order; `std::reverse_iterator` in C++.
- **Filtered iterator** — wraps another iterator and skips elements not matching a predicate.
- **Tree traversal iterators** — specialized iterators for depth-first (pre-order, in-order, post-order) and breadth-first traversal of tree structures.

## Code Example

````nimport java.util.*;

class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public String getName()   { return name; }
    public double getSalary() { return salary; }
}

interface Iterator<T> {
    boolean hasNext();
    T next();
}

class EmployeeIterator implements Iterator<Employee> {
    private int currentIndex = 0;
    private List<Employee> employees;

    public EmployeeIterator(List<Employee> employees) {
        this.employees = employees;
    }

    public boolean hasNext() { return currentIndex < employees.size(); }

    public Employee next() {
        if (!hasNext()) throw new NoSuchElementException();
        return employees.get(currentIndex++);
    }
}

class Company {
    private List<Employee> employees;

    public Company(List<Employee> employees) { this.employees = employees; }

    public Iterator<Employee> createIterator() {
        return new EmployeeIterator(employees);
    }
}

// Client
List<Employee> staff = new ArrayList<>();
staff.add(new Employee("Alice", 50000));
staff.add(new Employee("Bob", 60000));
staff.add(new Employee("Carol", 70000));

Company company = new Company(staff);
Iterator<Employee> it = company.createIterator();

double total = 0;
while (it.hasNext()) {
    total += it.next().getSalary();
}
System.out.println("Total salary: " + total);  // Total salary: 180000.0
```

## Real-World Examples

- **Java `Iterator<E>` / `Iterable<E>`** — the foundation of Java's for-each loop. Every `Collection` implementation returns an iterator that the compiler desugars `for (T x : collection)` into.
- **Python generators** — `yield`-based functions are lazy iterators; `for x in generator` exhausts them one value at a time without materializing the full sequence.
- **TV remote channel surfing** — pressing next/previous navigates channels without the viewer knowing the internal channel-list representation (broadcast order, satellite numbering).
- **Database cursor** — a JDBC `ResultSet` or SQLAlchemy cursor is an iterator over query results, fetching rows on demand from the database.
- **File system traversal** — `os.walk()` in Python and `Files.walk()` in Java are iterators over a directory tree; clients don't need to understand the filesystem structure.
- **Social network graph** — a Facebook-style social graph can expose separate iterators for "friends" and "coworkers," allowing different filtered traversals over the same underlying graph structure.
- **Infinite sequences** — Haskell's lazy lists, Python's `itertools.count()`, and Java's `Stream.generate()` create iterators over conceptually infinite sequences evaluated only as far as needed.
- **C++ STL iterators** — the entire STL algorithm library (`std::sort`, `std::find`, `std::for_each`) operates on iterator pairs, decoupling algorithms from container types.

## Related

- [[Composite Pattern]] — Iterator is frequently used to traverse Composite trees.
- [[Factory Method Pattern]] — `createIterator()` is a Factory Method.
- [[Memento Pattern]] — can snapshot iterator state to allow rollback of traversal position.
- [[Visitor Pattern]] — Visitor traverses a structure performing operations; Iterator provides the traversal mechanism Visitor can leverage.
