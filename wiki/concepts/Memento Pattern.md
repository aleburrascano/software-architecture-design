---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/memento-design-pattern/", "https://refactoring.guru/design-patterns/memento"]
tags: [design-pattern, behavioral, gof]
---

# Memento Pattern

A behavioral pattern that captures and externalises an object's internal state into a memento object so that the object can be restored to that state later, without violating encapsulation.

## Problem

You need to implement undo/redo, history, or checkpointing for an object, but exposing its internal fields so that an external caretaker can save and restore them breaks encapsulation. Making the fields public or providing many getters couples the caretaker to the originator's implementation details.

The text editor undo problem illustrates this precisely: to implement Ctrl+Z the editor must save its state before each change, but "if you make all these fields public, you'd end up with a rather fragile editor because any code could access the internals." The conflict is between needing external storage (so history management doesn't pollute the editor) and needing internal access (so the snapshot is complete and consistent).

## Solution

Let the **Originator** (the object whose state must be saved) create **Memento** objects that store a snapshot of its own private state. The memento is opaque to the outside world — external objects (the **Caretaker**) hold and manage mementos but cannot inspect or modify their contents. Only the originator knows how to pack state into a memento and unpack it back.

## Structure

| Participant | Role |
|---|---|
| **Originator** | Creates mementos (`createMemento()`); restores from them (`restore(memento)`) |
| **Memento** | Stores the originator's internal state snapshot; exposes no public setters to outsiders |
| **Caretaker** | Holds one or more mementos (e.g., a history stack); never operates on memento contents |

The key invariant: the caretaker stores mementos but treats them as black boxes. Only the originator reads/writes the state inside.

Three implementation approaches exist for achieving this:
- **Nested classes** (Java, C#) — the Memento is a private inner class; the Caretaker only sees a marker interface. Strongest encapsulation.
- **Intermediate interface** — the Caretaker works with a narrow interface that exposes no state; the Originator uses a wider concrete type.
- **Strict encapsulation** — multiple originator–memento pairs; each originator-class produces only mementos it knows how to restore.

## When to Use

- You need undo/redo or checkpoint/rollback functionality.
- Direct access to an object's state would violate its encapsulation.
- You want to take a snapshot before a risky operation (transaction-like safety net).
- You need to implement persistence or state serialization without exposing internals.
- Version history management where you need to restore to any prior version.

## Trade-offs

**Pros:**
- Preserves encapsulation — state is saved and restored without exposing private internals.
- Clean separation: originator handles state logic; caretaker handles storage.
- Enables undo/redo, version history, and snapshots.
- Simplifies originator code by offloading history management to the caretaker.

**Cons / pitfalls:**
- Memory cost — storing many large snapshots can be expensive. "Can consume a lot of memory if many states are stored."
- In dynamically-typed or garbage-collected languages, the "opaque" contract depends on discipline — there's no compile-time guarantee that caretakers won't inspect the memento.
- Deep-copying mutable object graphs (collections, nested objects) for a snapshot can be complex and slow.
- Originator must be able to recreate full state from a memento — any lazy-loaded or computed state must either be included or re-derivable.
- Caretakers must manage the memento lifecycle; failing to discard old mementos causes memory leaks.

## Variants

- **Incremental Memento** — stores only the diff from the previous state rather than a full copy. Reduces memory at the cost of requiring sequential replay for restore.
- **Serialized Memento** — state is serialized (JSON, binary) for persistence across process restarts.
- **Command + Memento** — [[Command Pattern]] is responsible for executing operations; Memento provides the state snapshots commands use for undo.
- **Prototype-based Memento** — if the originator implements `clone()`, a cloned instance acts as the memento (simpler but potentially more expensive).

## Code Example

````nclass DocumentMemento:
    """Opaque to the Caretaker — only Document calls get_saved_content()."""
    def __init__(self, content: str):
        self._content = content

    def get_saved_content(self) -> str:
        return self._content


class Document:  # Originator
    def __init__(self, content: str = ""):
        self._content = content

    def write(self, text: str):
        self._content += text

    def create_memento(self) -> DocumentMemento:
        return DocumentMemento(self._content)

    def restore_from_memento(self, memento: DocumentMemento):
        self._content = memento.get_saved_content()

    def __str__(self) -> str:
        return self._content


class History:  # Caretaker
    def __init__(self):
        self._stack: list[DocumentMemento] = []

    def push(self, memento: DocumentMemento):
        self._stack.append(memento)

    def pop(self) -> DocumentMemento:
        return self._stack.pop()

    def is_empty(self) -> bool:
        return len(self._stack) == 0


# Usage
doc = Document("Initial text.\n")
history = History()

doc.write("Added paragraph 1.\n")
history.push(doc.create_memento())   # save checkpoint

doc.write("Added paragraph 2.\n")
history.push(doc.create_memento())   # save checkpoint

doc.write("Typo goes here.\n")
print(doc)
# Initial text.
# Added paragraph 1.
# Added paragraph 2.
# Typo goes here.

doc.restore_from_memento(history.pop())  # undo last write
print(doc)
# Initial text.
# Added paragraph 1.
# Added paragraph 2.
```

## Real-World Examples

- **Text editor undo/redo** — every edit operation saves a memento. Ctrl+Z pops the history stack and calls `restore()`. VSCode, Vim, and Microsoft Word all use this model internally.
- **Video game save systems** — the player's position, health, inventory, and world state are snapshotted at save points. Loading a save restores the game to the exact moment the snapshot was taken.
- **Database transactions** — a transaction's before-image (the state before a DML operation) is saved as a memento in the undo log. Rolling back replays the before-images in reverse order.
- **Version control systems** — each Git commit is a snapshot memento of the repository's file tree. `git checkout <sha>` restores to any saved state.
- **Graphic design tools** (Photoshop, Figma) — the "History" panel shows a list of mementos. Clicking any history item restores the canvas to that state.
- **Configuration rollback** — infrastructure-as-code tools (Terraform state files, Kubernetes `kubectl rollout undo`) save deployment state snapshots and restore on failure.

## Related

- [[Command Pattern]] — Commands implement undo by storing Mementos; the two patterns are complementary. Commands describe *what changed*; Mementos describe *what state was before the change*.
- [[Iterator Pattern]] — Memento can snapshot iterator state to allow rollback of traversal position.
- [[Prototype Pattern]] — cloning the originator is an alternative way to create a memento.
- [[State Pattern]] — states can be checkpointed with Mementos for long-running workflows.
