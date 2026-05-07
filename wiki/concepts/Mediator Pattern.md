---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/mediator-design-pattern/", "https://refactoring.guru/design-patterns/mediator"]
tags: [design-pattern, behavioral, gof]
---

# Mediator Pattern

A behavioral pattern that reduces chaotic direct dependencies between objects by having them communicate only through a central mediator object, which encapsulates how they interact.

## Problem

When many objects communicate directly, the system develops a web of inter-object references. Every new component must know about all others it might interact with. This creates:

- An O(n²) coupling problem: n components may have n² interaction paths.
- Difficulty reusing components in isolation because they depend on each other.
- Brittle code where a change in one object forces changes in many others.

Classic examples: air traffic control (planes don't communicate directly — all go through the tower), chat room servers, UI dialog boxes where widgets interact (enable the "OK" button only when the form is valid), workflow orchestrators.

The air traffic control analogy is exact: "Rather than letting the pilots communicate among themselves about who lands first, the control tower handles all decisions. Without a tower, every pilot needs to be aware of every other plane — a coordination problem that scales catastrophically."

## Solution

Introduce a **Mediator** interface. All **Colleague** objects hold only a reference to the mediator. When a colleague needs to communicate or coordinate with others, it calls the mediator; the mediator routes the message or coordinates the reaction. The colleagues are decoupled from one another — they only know the mediator.

## Structure

| Participant | Role |
|---|---|
| **Mediator** (interface) | Declares the notification/coordination protocol, e.g., `notify(sender, event)` |
| **ConcreteMediator** | Implements coordination logic; knows all concrete colleagues |
| **Colleague** (component) | Knows only the mediator; notifies mediator on its events; reacts to mediator calls |
| **ConcreteColleague** | A specific component (button, text field, service) |

The mediator knows about all colleagues; colleagues know only the mediator. The complexity migrates from a distributed web into one centralized place.

## When to Use

- A set of objects communicate in complex, well-defined but tangled ways.
- Reusing a component is difficult because it references many other components.
- You want to customize the behavior of distributed objects without subclassing them.
- The interaction behavior should be independently variable from the components themselves.
- "Classes are tightly coupled, making changes difficult."
- Components need to be reusable in different contexts or programs.

## Trade-offs

**Pros:**
- Reduces coupling from O(n²) to O(n) — each component depends only on the mediator.
- Centralises interaction logic, making it easy to trace and modify.
- Components become more reusable — they depend only on the mediator interface.
- Supports the Single Responsibility Principle for communication.
- Supports Open/Closed for adding new interaction behaviors via a new mediator.

**Cons / pitfalls:**
- The mediator can become a "god object" — a monolithic class that knows everything and is hard to maintain. "Over time a mediator can evolve into a God Object."
- The centralization that simplifies the network of objects concentrates complexity in one place.
- Adding a new type of interaction requires changing the mediator.
- Can be harder to follow the flow of control than direct calls between objects.

## Variants

- **Event-based Mediator** — colleagues emit typed events; the mediator subscribes and dispatches. This is how many UI frameworks (Flux/Redux, Tkinter) work.
- **Message Bus** — an architectural-scale mediator where services publish to a bus and the bus routes messages. See also [[Event-Driven Architecture]].
- **Controller in MVC** — the controller acts as a mediator between models and views.

## Code Example

````nfrom abc import ABC, abstractmethod

class AirTrafficControl(ABC):
    @abstractmethod
    def request_takeoff(self, airplane): ...

    @abstractmethod
    def request_landing(self, airplane): ...

class AirportControlTower(AirTrafficControl):  # ConcreteMediator
    def __init__(self):
        self._runway_busy = False

    def request_takeoff(self, airplane):
        if not self._runway_busy:
            self._runway_busy = True
            print(f"Tower: {airplane.name} cleared for takeoff.")
        else:
            print(f"Tower: {airplane.name} hold — runway busy.")

    def request_landing(self, airplane):
        if not self._runway_busy:
            self._runway_busy = True
            print(f"Tower: {airplane.name} cleared to land.")
            self._runway_busy = False
        else:
            print(f"Tower: {airplane.name} enter holding pattern.")

class CommercialAirplane:  # Colleague
    def __init__(self, name, tower: AirTrafficControl):
        self.name = name
        self._tower = tower

    def take_off(self):
        self._tower.request_takeoff(self)

    def land(self):
        self._tower.request_landing(self)

# Usage
tower = AirportControlTower()
flight1 = CommercialAirplane("AA101", tower)
flight2 = CommercialAirplane("UA202", tower)

flight1.take_off()   # Tower: AA101 cleared for takeoff.
flight2.take_off()   # Tower: UA202 hold — runway busy.
flight1.land()       # Tower: AA101 cleared to land.
```

## Real-World Examples

- **Air traffic control** — the canonical example. All pilot communications go through the control tower; planes never communicate directly with each other about landing priority.
- **Chat room servers** — a chat server (mediator) routes messages between users (colleagues). Users send to the server; the server fans out to recipients. No user knows another user's network address directly.
- **UI dialog/form coordination** — in a login dialog, the Submit button is enabled only when both username and password fields are non-empty, and the "Forgot password" link only appears when username is filled. A `DialogMediator` reacts to field-change events and orchestrates widget states.
- **Flux/Redux store** — the store is a mediator that receives all state-mutation intents (actions) and dispatches updates to subscribed view components.
- **MVC controller** — the controller mediates between the model (state) and view (display), preventing them from referencing each other directly.
- **Workflow orchestrators** (Temporal, AWS Step Functions) — each activity/task is a colleague; the orchestrator (mediator) defines and manages the interaction sequence.
- **Classroom teaching** — students raise questions directed at the teacher (mediator), who decides when and how to relay information to the class, preventing cross-talk and ensuring order.

## Related

- [[Observer Pattern]] — Observer distributes communication: a subject broadcasts to all subscribers. Mediator centralizes it: all communication passes through one hub. When the communication topology is complex, prefer Mediator; for simple one-to-many broadcast, prefer Observer. A Mediator can be implemented using Observer as its internal mechanism (components subscribe to the mediator's events), though they remain conceptually distinct.
- [[Facade Pattern]] — Facade simplifies an interface to a subsystem (one-directional); Mediator coordinates bidirectional communication between components.
- [[Command Pattern]] — commands can be routed through a mediator.
- [[Event-Driven Architecture]] — scales Mediator to the service level.
