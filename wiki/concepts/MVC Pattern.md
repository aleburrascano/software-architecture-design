---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - "raw/Top 10 Software Architecture & Design Patterns for 2025.md"
  - "raw/How to Learn Software Design and Architecture  The  Full-stack Software Design & Architecture Map.md"
  - "raw/Software architecture 1.md"
  - "https://www.geeksforgeeks.org/system-design/mvc-design-pattern/"
tags:
  - architecture
  - architectural-pattern
  - ui-pattern
  - mvc
---

# MVC Pattern

Model-View-Controller (MVC) is an architectural pattern that separates an application's data and business logic (Model), its user interface rendering (View), and its input-handling and orchestration logic (Controller) into three distinct, collaborating components.

## Problem

Without separation, UI code, business logic, and data access become entangled. A change to how data is displayed requires touching the same code that validates business rules or queries the database. The result is brittle, hard-to-test, hard-to-maintain code — especially in user-interface-heavy applications. Parallel development of UI and business logic becomes impossible.

## Solution

Divide the application into three collaborating roles:

- **Model** — owns the data, business rules, and application state. Knows nothing about the UI. Responds to requests from View and Controller and notifies the View of state changes.
- **View** — renders the model's data for display. Knows how to present, but not how to fetch or modify data. Forwards user interactions to the Controller.
- **Controller** — acts as intermediary between Model and View. Receives user input (HTTP requests, UI events), validates data, interprets it, updates the Model, and selects the appropriate View to render.

Analogy: In a restaurant, the **kitchen** (Model) prepares the food; the **menu and plated dishes** (View) present it; the **waiter** (Controller) coordinates between the kitchen and the diners.

## Key Components / Structure

```
User Input → Controller → Model (fetch/mutate data)
                       ↓
                     View ← Model (data to render)
                       ↓
                    Response to User
```

**Full communication flow:**
User interaction → View receives input → Controller processes it → Controller updates Model → Model notifies View → View requests data → Controller refreshes View → UI re-renders

| Component | Responsibility |
|-----------|---------------|
| **Model** | Data, domain logic, validation, persistence interface; responds to controller and notifies view of changes |
| **View** | Template/rendering — turns model data into HTML, JSON, or UI; passively displays without direct data access |
| **Controller** | Route handling, request parsing, Model interaction, View selection; mediates all interactions |

## When to Use

- Web applications with clear request/response cycles (Rails, Django, Spring MVC, ASP.NET MVC).
- Applications where UI concerns need to be tested and changed independently of business logic.
- As a starting point for applications that may later need richer patterns ([[Domain-Driven Design]], [[Clean Architecture]]).
- Simpler CRUD applications where business logic is not complex.

## Trade-offs

**Pros:**
- Clear separation of concerns — UI, logic, and data each have a defined home.
- Easier to unit-test models independently of the UI.
- Enables parallel development of UI and business logic.
- Widely understood — nearly every web framework implements MVC or a variant.
- Changes to View (e.g., switching from HTML to JSON API) don't require touching the Model.

**Cons / pitfalls:**
- The Controller tends to accumulate logic as the application grows ("fat controller" anti-pattern).
- The Model boundary is under-specified: validation, business rules, use cases, and data access all compete for space in the "M", leading to an anemic model or a bloated service layer.
- Not sufficient for complex domains with rich business logic — typically needs supplementary patterns (services, repositories, domain events).
- Adds complexity to simple applications that don't need the separation.
- Steeper learning curve for teams unfamiliar with the pattern.

## MVC Variants

**MVP (Model-View-Presenter):** The Presenter fully mediates between View and Model; the View is entirely passive. Common in Android development.

**MVVM (Model-View-ViewModel):** The ViewModel exposes observable data streams the View binds to directly. Common in frontend frameworks (Angular, Vue, WPF, SwiftUI). Eliminates Controller in favor of data binding. See [[MVVM Pattern]].

## Real-World Usage

- **E-commerce search:** User submits search → Controller receives request → invokes `ProductModel.search(query)` → passes results to `ProductListView` → renders product grid.
- **Student information system:** Model stores name/roll number; View displays details; Controller mediates updates.
- Used in virtually every major web framework: Ruby on Rails, Django, Laravel, Spring MVC, ASP.NET Core MVC, Express.js (loosely).
- Implementation languages: Java, Python, C++, JavaScript — the pattern is language-agnostic.

## Related

- [[Layered Architecture]] — MVC is an application-level pattern that typically sits within a layered architecture
- [[Domain-Driven Design]] — DDD provides the enterprise constructs (Entities, Value Objects, Services) that fill out the Model layer when MVC is not enough
- [[Repository Pattern]] — repositories are frequently used within the Model layer to abstract data access
- [[Clean Architecture]] — Clean Architecture places Controllers and Views in the Interface Adapters layer, with use cases as explicit inner objects
