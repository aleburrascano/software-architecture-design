---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/visitor-design-pattern/", "https://refactoring.guru/design-patterns/visitor"]
tags: [design-pattern, behavioral, gof]
---

# Visitor Pattern

A behavioral pattern that lets you add new operations to an object structure without modifying the classes of the elements in that structure, by separating the algorithm from the objects it operates on.

## Problem

You have a stable object hierarchy (e.g., AST nodes, document elements, composite shapes) and need to add many distinct operations on those objects over time — export to XML, pretty-print, type-check, evaluate — but:

- Adding each operation directly to each class violates Single Responsibility.
- Adding a new operation requires modifying every class in the hierarchy.
- Operations that span multiple types are scattered across unrelated classes.

The geographic graph example is concrete: a geographic data export task required adding XML export to each node class (City, Industry, SightSeeing). Modifying production node classes for an auxiliary feature "seemed risky," and future requirements (JSON export, graph analysis, distance calculation) would repeat the same invasive changes.

Similarly, adding area calculation for shapes (circles, squares, triangles) by modifying each shape class would violate OCP and scatter the related calculation logic across three unrelated files.

## Solution

Define a **Visitor** interface with an `visit(ElementType)` overload for every concrete element type in the hierarchy. Each **ConcreteVisitor** implements a specific operation across all element types. Each **Element** class implements `accept(visitor)`, which calls the correct overload on the visitor: `visitor.visit(this)`. This double-dispatch ensures the correct visitor method is called for each concrete element type at runtime.

## Structure

| Participant | Role |
|---|---|
| **Visitor** (interface) | Declares `visit(ConcreteElementA)`, `visit(ConcreteElementB)`, … |
| **ConcreteVisitor** | Implements a specific operation for each element type |
| **Element** (interface) | Declares `accept(Visitor)` |
| **ConcreteElement** | Implements `accept(v) { v.visit(this) }` — triggers double dispatch |
| **ObjectStructure** | A collection or composite that iterates elements, calling `accept()` |

### Double Dispatch

Standard single dispatch selects a method based on the receiver's type. Visitor needs dispatch on *both* the element type and the visitor type. The `accept()` call (dispatch 1: element type) calls `visit(this)` (dispatch 2: visitor's overload for that element type). Together they achieve double dispatch in languages that only support single dispatch natively.

"Instead of letting the client select a proper version of the method to call, how about we delegate this choice to objects we're passing to the visitor as an argument?"

## When to Use

- You have a stable class hierarchy but need to add many new operations over time.
- You need to perform an operation across all elements of a heterogeneous object structure.
- You want to avoid polluting element classes with unrelated operations.
- Operations need to accumulate state across elements (e.g., counting, collecting).
- "Extracting auxiliary behaviors" to maintain focused primary classes.
- A behavior applies only to specific classes within a hierarchy and should be localized.

## Trade-offs

**Pros:**
- Open/Closed for operations — add new visitors without touching element classes.
- Centralises related operations — all XML export logic lives in `XmlExportVisitor`.
- Can accumulate state across an element traversal.
- Visitor can work across unrelated class hierarchies.
- Type-safe method dispatch (at compile time in statically-typed languages).

**Cons / pitfalls:**
- Closed to new element types — adding a new element class requires updating every existing visitor.
- Breaks encapsulation — visitors may need access to the element's internal state (requires public or friend access, or exposing getters).
- Interface can become wide if the hierarchy has many element types.
- Overkill for small, stable hierarchies with few operations.
- "Must update all visitors when element classes change."

## Variants

- **Acyclic Visitor** — visitors implement only the element interfaces they care about; elements check with `instanceof`/`dynamic_cast` before calling. Avoids the "closed to new elements" problem at the cost of runtime type checks.
- **Hierarchical Visitor** — visits composite structures with `enterElement`/`leaveElement` hooks (common in XML/DOM processing).
- **Visitor with return value** — visitor methods return a value rather than accumulating state; useful for expression evaluators.

## Code Example

```java
// Visitor interface — one visit method per concrete element type
interface ShapeVisitor {
    void visit(Circle circle);
    void visit(Square square);
    void visit(Triangle triangle);
}

// Element interface
interface Shape {
    void accept(ShapeVisitor visitor);
}

// Concrete elements — each triggers double dispatch via accept()
class Circle implements Shape {
    public final double radius;
    public Circle(double radius) { this.radius = radius; }
    public void accept(ShapeVisitor visitor) { visitor.visit(this); }
}

class Square implements Shape {
    public final double side;
    public Square(double side) { this.side = side; }
    public void accept(ShapeVisitor visitor) { visitor.visit(this); }
}

class Triangle implements Shape {
    public final double base, height;
    public Triangle(double base, double height) {
        this.base = base; this.height = height;
    }
    public void accept(ShapeVisitor visitor) { visitor.visit(this); }
}

// Concrete visitor — area calculation operation
class AreaCalculator implements ShapeVisitor {
    private double totalArea = 0;

    public void visit(Circle c)   { totalArea += Math.PI * c.radius * c.radius; }
    public void visit(Square s)   { totalArea += s.side * s.side; }
    public void visit(Triangle t) { totalArea += 0.5 * t.base * t.height; }

    public double getTotalArea() { return totalArea; }
}

// Another visitor — perimeter calculation (no element changes needed)
class PerimeterCalculator implements ShapeVisitor {
    private double totalPerimeter = 0;

    public void visit(Circle c)   { totalPerimeter += 2 * Math.PI * c.radius; }
    public void visit(Square s)   { totalPerimeter += 4 * s.side; }
    public void visit(Triangle t) { /* simplified */ totalPerimeter += t.base * 3; }

    public double getTotalPerimeter() { return totalPerimeter; }
}

// Client
List<Shape> shapes = List.of(new Circle(5), new Square(4), new Triangle(3, 6));
AreaCalculator areaCalc = new AreaCalculator();
for (Shape shape : shapes) {
    shape.accept(areaCalc);
}
System.out.printf("Total area: %.2f%n", areaCalc.getTotalArea());
```

## Real-World Examples

- **Compiler AST traversal** — an Abstract Syntax Tree has stable node types (Literal, BinaryOp, FunctionCall, IfStatement). Operations like type checking, code generation, constant folding, and dead code elimination are each a separate Visitor over the same tree.
- **Tax / discount calculation on shopping carts** — cart items (Book, Electronics, Clothing) implement `accept(TaxVisitor)`. Different tax rules per category live in the visitor, not in the item classes. Adding a discount visitor requires no item changes.
- **File system operations** — a directory tree of File and Directory nodes accepts visitors for operations: calculate total size, list all `.log` files, backup changed files.
- **XML/DOM processing** — a DOM tree accepts SAX-style visitors that accumulate or transform content; XPath evaluation is effectively a visitor over the document tree.
- **Export in design tools** — shapes in a vector graphics app accept `SvgExportVisitor`, `PdfExportVisitor`, or `PngRenderVisitor` without modifying shape classes.
- **Geographic data graph** (Refactoring Guru's example) — City, Industry, and SightSeeing nodes in a graph accept `XmlExportVisitor`, `JsonExportVisitor` independently.
- **Java `javax.lang.model` API** — the `ElementVisitor` and `TypeVisitor` interfaces in the Java compiler API use the Visitor pattern for processing AST elements.

## Related

- [[Iterator Pattern]] — Iterator provides the traversal mechanism that Visitor operations use to walk an object structure.
- [[Composite Pattern]] — object structures Visitor operates on are often Composites.
- [[Command Pattern]] — both encapsulate an operation; Command targets one receiver, Visitor operates across an entire hierarchy.
- [[Strategy Pattern]] — Strategy swaps algorithms on a single object; Visitor applies an operation uniformly across a structure of objects.
