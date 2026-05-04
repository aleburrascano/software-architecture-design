---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: []
tags: [design-pattern, architectural-pattern, ui]
---

# MVVM Pattern

Model-View-ViewModel separates UI logic from business logic by introducing a ViewModel that exposes observable state, enabling the View to bind declaratively rather than imperatively.

## Problem

In [[MVC Pattern]], controllers grow large and UI state management becomes entangled with business logic. Views often have too much code manipulating the model directly, and testing UI logic requires instantiating views. This is the "fat controller" / "fat view" problem.

## Solution

Introduce a ViewModel — a presentation model that holds all the state and logic the View needs, expressed as observable properties and commands. The View binds to the ViewModel's properties; the ViewModel knows nothing about the View.

## Structure

| Participant | Responsibility |
|-------------|----------------|
| **Model** | Domain data and business rules. Same as in MVC. |
| **View** | Purely declarative UI. Binds to ViewModel properties. No business logic. |
| **ViewModel** | Holds UI state as observable properties. Exposes commands. Talks to Model. Has no reference to View. |

Data flow: `View ←(data binding)→ ViewModel ←→ Model`

The binding is bidirectional: the View reflects ViewModel state changes, and user interactions update ViewModel state (which may then call Model).

## When to Use

- UI frameworks with first-class data binding (WPF, Angular, SwiftUI, Jetpack Compose, Vue, Knockout)
- When UI logic needs to be unit-tested without instantiating views
- When the same ViewModel should drive multiple View representations
- Rich client applications with complex UI state

## Trade-offs

**Pros:**
- View is a thin, declarative shell — easy to redesign without touching logic
- ViewModel is testable without a UI
- Clean separation of concerns across View / presentation logic / domain
- Works naturally with reactive programming (RxJS, Combine, StateFlow)

**Cons / pitfalls:**
- Data binding overhead — debugging binding errors can be opaque
- ViewModel can grow large if not decomposed carefully (fat ViewModel problem)
- Overkill for simple CRUD UIs with no real UI state
- Each framework's binding mechanism differs; the pattern is consistent but implementation varies widely

## Variants

- **MVU (Model-View-Update)** — Elm/Redux style; unidirectional data flow; state is immutable; update function produces new state
- **MVP (Model-View-Presenter)** — Presenter replaces ViewModel; View is more active (calls Presenter); no data binding required; common in Android pre-Compose

## Real-World Examples

- **WPF / UWP (Microsoft)** — MVVM's origin environment; INotifyPropertyChanged, commands
- **Angular** — Components act as ViewModels; templates bind to component properties
- **SwiftUI** — `@StateObject` / `@ObservedObject` are ViewModel containers; `@Published` properties drive binding
- **Jetpack Compose (Android)** — ViewModel + StateFlow/LiveData + Compose UI
- **Vue.js** — Component data + computed properties form the ViewModel; template is the View

## Related

- [[MVC Pattern]] — parent pattern; MVVM evolved from MVC to support data binding
- [[Separation of Concerns]] — MVVM is a direct application of SoC to UI
- [[Observer Pattern]] — the data binding mechanism underlying MVVM is Observer
- [[Command Pattern]] — ViewModel commands are Command objects
