---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/command-pattern/", "https://refactoring.guru/design-patterns/command"]
tags: [design-pattern, behavioral, gof]
---

# Command Pattern

A behavioral pattern that encapsulates a request as a standalone object containing all the information needed to execute it, enabling deferred execution, queuing, logging, and undoable operations.

## Problem

You need to issue requests to objects without knowing:
- Which object will handle the request.
- The exact operation to perform.
- When the request should be executed.

Hardcoding the request logic into the caller couples the invoker tightly to the receiver, prevents undo/redo, and makes it impossible to queue or log requests generically.

A concrete illustration: a text editor with buttons, menus, and keyboard shortcuts all triggering the same operations. Naively, each UI control subclass implements the action directly — leading to an enormous number of subclasses, tight coupling between GUI and business logic, and duplication when the same operation appears in multiple UI entry points (copy available via button, menu, and Ctrl+C). The pattern solves this by extracting "all of the request details into a separate command class with a single method that triggers this request," decoupling senders from receivers uniformly.

## Solution

Turn each request into a **Command** object with an `execute()` method (and optionally `undo()`). The **Invoker** holds command objects and calls `execute()` at the appropriate time — it has no knowledge of what the command does. The **Receiver** contains the actual business logic; the command calls receiver methods in its `execute()` body. A **Client** instantiates commands and injects receivers into them.

## Structure

| Participant | Role |
|---|---|
| **Command** (interface) | Declares `execute()` (and optionally `undo()`) |
| **ConcreteCommand** | Implements `execute()`; wires action to a Receiver |
| **Receiver** | Holds the domain logic that the command delegates to |
| **Invoker** | Calls `execute()` on commands; may maintain a history list |
| **Client** | Creates ConcreteCommand instances and sets their Receivers |

## When to Use

- You want to parameterize objects with operations (e.g., toolbar buttons, menu items).
- You need to queue, schedule, or log operations.
- You need undoable operations — store executed commands in a history stack.
- You need transactional behavior — roll back failed operations by calling `undo()`.
- You want to assemble complex operations from simple primitives (Macro Command / Composite Command).
- Commands should be serializable, transmittable across a network, or persisted to storage.
- Operations need deferred or scheduled execution (job queues, task schedulers).

## Trade-offs

**Pros:**
- Single Responsibility: the invoker is decoupled from the operation logic.
- Open/Closed: add new commands without changing the invoker.
- Enables undo/redo, macro recording, audit logs.
- Commands can be serialized and transmitted across a network or persisted.
- Supports the Null Object pattern — a no-op command replaces null checks.
- Enables operation composition and deferred execution.

**Cons / pitfalls:**
- Class proliferation — a new class for every distinct action.
- Added indirection makes control flow harder to trace.
- If state needs to be restored for undo, the command may need to snapshot significant amounts of data (consider [[Memento Pattern]] for state capture).
- Macro commands with many steps can complicate rollback logic.
- Adds complexity overhead that is not justified for simple one-shot invocations.

## Variants

- **Macro Command (Composite Command)** — a command that contains a list of commands and executes them in sequence. Combines with [[Composite Pattern]].
- **Queued / Asynchronous Command** — commands placed on a queue and processed by a worker thread (common in event loops, job queues like AWS SQS, RabbitMQ).
- **Event Sourcing** — an architectural extension where every state change is stored as a command (event); replay reconstructs state.
- **Transactional Command** — `execute()` and `undo()` form a transactional unit; a failed batch is rolled back.
- **Null Command** — a no-op implementation used to avoid null checks on unset command slots (Null Object pattern applied to commands).

## Code Example

```java
// Command Interface
interface Command {
    void execute();
    void undo();
}

// Receiver
interface Device {
    void turnOn();
    void turnOff();
}

class TV implements Device {
    public void turnOn()  { System.out.println("TV is now ON"); }
    public void turnOff() { System.out.println("TV is now OFF"); }
}

// Concrete Commands
class TurnOnCommand implements Command {
    private Device device;
    public TurnOnCommand(Device device) { this.device = device; }
    public void execute() { device.turnOn(); }
    public void undo()    { device.turnOff(); }
}

class TurnOffCommand implements Command {
    private Device device;
    public TurnOffCommand(Device device) { this.device = device; }
    public void execute() { device.turnOff(); }
    public void undo()    { device.turnOn(); }
}

// Invoker with undo history
class RemoteControl {
    private Deque<Command> history = new ArrayDeque<>();

    public void press(Command cmd) {
        cmd.execute();
        history.push(cmd);
    }

    public void undoLast() {
        if (!history.isEmpty()) {
            history.pop().undo();
        }
    }
}

// Client
TV tv = new TV();
RemoteControl remote = new RemoteControl();
remote.press(new TurnOnCommand(tv));   // TV is now ON
remote.undoLast();                     // TV is now OFF
```

## Real-World Examples

- **Text editor undo/redo** — every keystroke, paste, or format change is a Command stored on a history stack. Ctrl+Z pops and calls `undo()`.
- **GUI frameworks** — Swing's `Action` interface, JavaFX `EventHandler`, and Qt's signals are all forms of Command. A toolbar button and a menu item can share the same Command object.
- **Job / message queues** — AWS SQS, RabbitMQ, and Celery tasks are serialized Commands processed asynchronously by worker consumers.
- **Database transaction managers** — each DML operation is a Command; the transaction rolls back by executing reverse Commands on failure.
- **Macro recording** — in spreadsheets and IDEs, recorded macros are sequences of Commands that can be replayed.
- **Smart home systems** — a single remote control issues device-agnostic Commands (TurnOn, SetBrightness, SetTemperature) to heterogeneous receivers (lights, thermostats, TVs) without knowing their API.
- **Restaurant order tickets** — the paper order is literally a Command object: it encapsulates what to prepare, sits in the kitchen queue, and the chef (receiver) executes it when ready.

## Related

- [[Chain of Responsibility Pattern]] — Chain passes the request along a handler chain to find a handler; Command wraps it in an object destined for a single receiver. They are often combined: commands travel down a chain to find their handler.
- [[Strategy Pattern]] — both use an object to encapsulate behavior; Command encapsulates *a request* (with receiver, parameters, and optional undo); Strategy encapsulates *an algorithm*.
- [[Memento Pattern]] — often used with Command to snapshot state needed for undo. Commands describe *what changed*; Mementos describe *what state was before the change*.
- [[Template Method Pattern]] — provides a skeleton for commands that share setup/execute/cleanup lifecycles.
