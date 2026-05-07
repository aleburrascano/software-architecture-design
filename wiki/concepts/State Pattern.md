---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/state-design-pattern/", "https://refactoring.guru/design-patterns/state"]
tags: [design-pattern, behavioral, gof]
---

# State Pattern

A behavioral pattern that allows an object to alter its behavior when its internal state changes, by delegating state-dependent behavior to separate state objects, so that the object appears to change its class.

## Problem

Objects that behave differently depending on their current state typically manage this with large conditional blocks (`if state == "X"` / `switch`). Problems:

- Every new state requires updating conditionals scattered across many methods.
- State transition logic is mixed with behavior logic.
- The class grows with each additional state, making it hard to maintain.

Classic examples: vending machines, traffic lights, order lifecycle (pending → paid → shipped → delivered), TCP connection states, smartphone lock states.

The smartphone example illustrates this clearly: pressing the same button on a locked phone shows the unlock screen, on an unlocked phone opens the selected app, and on a low-battery phone shows a charging screen. The same stimulus produces completely different behavior depending on current state. Without the pattern, a single method contains a large switch statement that must be updated every time a new state is added.

## Solution

Create a **State** interface with a method for each context behavior that varies by state. Each **ConcreteState** implements this interface with behavior appropriate for that state. The **Context** holds a reference to the current state object and delegates all state-dependent calls to it. State transitions are handled either by the Context or by the ConcreteState objects themselves, by replacing the context's state reference.

"Instead of implementing all behaviors on its own, the original object, called context, stores a reference to one of the state objects that represents its current state, and delegates all the state-related work to that object."

## Structure

| Participant | Role |
|---|---|
| **Context** | Holds a reference to the current `State`; delegates requests to it; exposes `setState()` |
| **State** (interface) | Declares the operation(s) that differ by state |
| **ConcreteStateA/B/…** | Implements the behavior for a specific state; may trigger transitions |

The Context and State objects may reference each other: states need to trigger transitions on the context.

## When to Use

- An object's behavior depends on its state and must change at runtime.
- Operations have many branches depending on the object's state (large conditional blocks are a code smell pointing to the State pattern).
- State transitions are complex and need to be explicitly modeled.
- States are well-defined and bounded (finite state machine).
- "Classes polluted with massive conditionals checking field values."

## Trade-offs

**Pros:**
- Eliminates complex conditionals — each state is a self-contained class.
- Single Responsibility — state-specific behavior lives in one place.
- Open/Closed — add new states without modifying existing states or the context.
- State transitions are explicit and auditable.
- Improves readability by making each valid state and its behavior discoverable as a class.

**Cons / pitfalls:**
- Class proliferation — each state requires its own class, even for simple machines.
- If states share behavior, avoiding duplication requires careful base classes or mixins.
- Tight coupling between State and Context if states trigger their own transitions.
- For simple finite state machines with few states, a state table or enum-based switch may be simpler and more readable.
- State transitions harder to track in large systems with many states.

## Variants

- **State-driven transitions** — the ConcreteState object calls `context.setState(newState)` to trigger transitions internally; the context simply delegates.
- **Context-driven transitions** — the Context contains the transition logic; states only implement behavior. Cleaner separation but context can become complex.
- **Hierarchical State Machine** — states have sub-states; a common pattern in UML statecharts and game AI.
- **State table / enum-based** — for simple cases, a dictionary mapping (state, event) → (new_state, action) may suffice without full OOP.

## Code Example

````ninterface VendingMachineState {
    void insertCoin(VendingMachine machine);
    void selectProduct(VendingMachine machine);
    void dispense(VendingMachine machine);
}

class IdleState implements VendingMachineState {
    public void insertCoin(VendingMachine machine) {
        System.out.println("Coin inserted.");
        machine.setState(new HasCoinState());
    }
    public void selectProduct(VendingMachine machine) {
        System.out.println("Please insert a coin first.");
    }
    public void dispense(VendingMachine machine) {
        System.out.println("No coin inserted.");
    }
}

class HasCoinState implements VendingMachineState {
    public void insertCoin(VendingMachine machine) {
        System.out.println("Coin already inserted.");
    }
    public void selectProduct(VendingMachine machine) {
        System.out.println("Product selected.");
        machine.setState(new DispensingState());
    }
    public void dispense(VendingMachine machine) {
        System.out.println("Please select a product first.");
    }
}

class DispensingState implements VendingMachineState {
    public void insertCoin(VendingMachine machine) {
        System.out.println("Wait, dispensing in progress.");
    }
    public void selectProduct(VendingMachine machine) {
        System.out.println("Already dispensing.");
    }
    public void dispense(VendingMachine machine) {
        System.out.println("Dispensing product.");
        machine.setState(new IdleState());
    }
}

class VendingMachine {  // Context
    private VendingMachineState state = new IdleState();
    public void setState(VendingMachineState state) { this.state = state; }
    public void insertCoin()    { state.insertCoin(this); }
    public void selectProduct() { state.selectProduct(this); }
    public void dispense()      { state.dispense(this); }
}

// Usage
VendingMachine vm = new VendingMachine();
vm.insertCoin();      // Coin inserted.
vm.selectProduct();   // Product selected.
vm.dispense();        // Dispensing product.
```

## Real-World Examples

- **Vending machines** — the classic example. Idle, HasCoin, ProductSelected, OutOfStock are all states with different behavior for the same button presses.
- **Media players** — a video player behaves differently when Stopped, Playing, Paused, or Buffering. Each state handles play/pause/stop events differently.
- **Traffic lights** — Red, Yellow, Green states each have different allowed/blocked actions and automatic timed transitions.
- **Order lifecycle** — an e-commerce order moves through Pending → Confirmed → Paid → Shipped → Delivered → Cancelled. Each state determines which operations are valid.
- **Document workflows** — Draft → In Review → Published → Archived. A "publish" action in Draft state moves to In Review; in Published state it is a no-op or error.
- **TCP connections** — `CLOSED`, `LISTEN`, `SYN_SENT`, `ESTABLISHED`, `FIN_WAIT`, `TIME_WAIT` are formal TCP states; the same `send()` call behaves differently in each.
- **Smartphone lock screen** — locked, unlocked, and low-battery states all respond differently to button presses.
- **Game character states** — Idle, Running, Jumping, Attacking, Dead each animate and respond to input differently.

## Related

- [[Strategy Pattern]] — structurally identical; the difference is intent. Strategy is selected by the client and rarely changes; State changes itself as the object's lifecycle progresses. "In the State pattern, the particular states may be aware of each other and initiate transitions from one state to another, whereas strategies almost never know about each other." Strategy describes different ways of doing the same thing; State describes an object whose identity changes with context.
- [[Command Pattern]] — commands can trigger state transitions.
- [[Observer Pattern]] — state changes can notify observers.
- [[Flyweight Pattern]] — if state objects hold no intrinsic state, they can be shared (flyweights) across many context instances.
