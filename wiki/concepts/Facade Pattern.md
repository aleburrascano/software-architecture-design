---
type: concept
created: '2026-05-03T00:00:00.000Z'
updated: '2026-05-06'
sources:
  - Structural Design Patterns.md
  - https://refactoring.guru/design-patterns/facade
  - https://www.geeksforgeeks.org/system-design/facade-design-pattern-introduction/
tags:
  - design-pattern
  - structural
  - gof
---
# Facade Pattern

Provides a simplified, unified interface to a complex subsystem — a library, framework, or set of classes — without hiding the subsystem entirely.

## Problem

A subsystem grows complex: many classes with intricate dependencies, initialisation ordering requirements, and low-level operations. Client code must understand and coordinate all of this. The result is high coupling between clients and subsystem internals; changing any subsystem class can break many callers. Most clients only need a handful of common operations, not the full subsystem API.

A concrete example: integrating a complex video conversion framework requires initialising codec objects, setting format parameters, managing audio streams, and handling buffering — in a precise sequence. Most clients just want `convert("input.mp4", "output.avi")`.

## Solution

Create a `Facade` class that sits in front of the subsystem. It exposes a small, coherent, high-level interface covering the most-used operations. Internally it delegates to the appropriate subsystem classes in the right order. Clients that need only the common use cases talk to the Facade. Clients that need fine-grained control can still access subsystem classes directly — the Facade does not hide or restrict them.

```
class Facade:
    subsystemA: SubsystemA
    subsystemB: SubsystemB
    subsystemC: SubsystemC

    simpleOperation():
        subsystemA.init()
        subsystemB.prepare(subsystemA.getData())
        subsystemC.execute()
```

## Structure

| Participant | Role |
|---|---|
| Facade | Knows which subsystem classes handle a request; delegates client calls to the right objects in the right order; may initialise and manage subsystem lifecycle |
| Additional Facade | Optional; prevents the main facade from becoming too large; can be used by clients and by other facades |
| Subsystem classes | Implement the actual work; have no knowledge of the Facade; remain fully accessible directly |
| Client | Uses Facade for common tasks; may bypass Facade for advanced use cases |

There is no formal interface requirement for Facade — it is typically a concrete class. Multiple facades over the same subsystem are fine (e.g. one per distinct client type or feature area).

## When to Use

- You want to provide a simple interface to a complex subsystem.
- You want to layer a subsystem: facades serve as entry points between layers, and layers communicate only via facades.
- There is a lot of coupling between clients and internal classes of a subsystem, and you want to reduce it.
- You are wrapping a poorly structured legacy system behind a clean interface.
- A subsystem needs to grow and evolve independently; clients should be shielded from those changes.

## Trade-offs

**Pros:**
- Reduces coupling between clients and subsystem internals.
- Makes common operations easy; complex operations remain available directly.
- Isolates clients from subsystem changes — update subsystem internals without changing the facade's public API.
- Simplifies testing of client code (mock the facade instead of the whole subsystem).
- Improves maintainability by providing a stable public API over a changing internal structure.

**Cons / pitfalls:**
- Risk of becoming a "god object" that knows too much and delegates too little — keep it thin.
- If the facade exposes too much of the subsystem, it provides little benefit; if it exposes too little, clients bypass it anyway.
- Adding every feature to the facade is tempting but undermines its purpose.
- Extra indirection layer may add minor performance overhead.
- Can hide complexity rather than resolve it — facade over a poorly-designed subsystem is not a substitute for fixing the subsystem.

## Code Example

````n# Complex subsystem classes — each has its own interface
class VideoDecoder:
    def decode(self, file: str) -> str:
        print(f"[VideoDecoder] Decoding {file}")
        return f"frames:{file}"

class AudioDecoder:
    def decode(self, file: str) -> str:
        print(f"[AudioDecoder] Decoding {file}")
        return f"audio:{file}"

class VideoRenderer:
    def render(self, frames: str):
        print(f"[VideoRenderer] Rendering {frames}")

class AudioPlayer:
    def play(self, audio: str):
        print(f"[AudioPlayer] Playing {audio}")

class BitrateAdjuster:
    def adjust(self, file: str, bitrate: int):
        print(f"[BitrateAdjuster] Adjusting {file} to {bitrate}kbps")

# Facade — hides all of the above behind one simple interface
class VideoPlayerFacade:
    def __init__(self):
        self._video = VideoDecoder()
        self._audio = AudioDecoder()
        self._renderer = VideoRenderer()
        self._player = AudioPlayer()
        self._bitrate = BitrateAdjuster()

    def play(self, filename: str, quality: int = 1080):
        self._bitrate.adjust(filename, quality)
        frames = self._video.decode(filename)
        audio  = self._audio.decode(filename)
        self._renderer.render(frames)
        self._player.play(audio)

# Client — one call instead of five
player = VideoPlayerFacade()
player.play("movie.mp4", quality=720)
```

Hotel example (GfG illustration):

````nclass VegRestaurant:
    def get_menus(self) -> list:
        return ["Dal", "Paneer", "Roti"]

class NonVegRestaurant:
    def get_menus(self) -> list:
        return ["Chicken", "Fish", "Mutton"]

class HotelKeeper:
    """Facade — client never contacts restaurants directly."""
    def get_veg_menu(self) -> list:
        return VegRestaurant().get_menus()

    def get_non_veg_menu(self) -> list:
        return NonVegRestaurant().get_menus()

keeper = HotelKeeper()
print(keeper.get_veg_menu())     # ['Dal', 'Paneer', 'Roti']
print(keeper.get_non_veg_menu()) # ['Chicken', 'Fish', 'Mutton']
```

## Real-World Examples

- **Home automation** — a single "Good morning" scene triggers lights, thermostat, and security system through one facade call rather than three separate API calls.
- **Video streaming platforms** — a `StreamingService.play(movieId)` facade encapsulates encoding pipeline, CDN selection, DRM licensing, buffering, and adaptive bitrate switching.
- **Banking systems** — `account.transfer(from, to, amount)` hides balance checks, fraud detection, ledger entries, notification dispatch, and audit logging behind a single method.
- **Spring Framework** — `JdbcTemplate` is a facade over raw JDBC: it hides connection management, statement preparation, result set iteration, and exception translation.
- **Operating system APIs** — POSIX file I/O (`open`, `read`, `write`, `close`) is a facade over device drivers, filesystem layers, page cache, and VFS abstractions.
- **AWS SDK** — high-level SDK clients (e.g. `S3Client.putObject()`) facade complex HTTP signing, retry logic, multipart chunking, and error parsing.
- **Phone shop operator analogy** — the operator is a facade: they provide a simple voice interface to ordering systems, payment gateways, and delivery services without requiring customers to interact with each department directly.

## Related

- [[Adapter Pattern]] — Adapter makes an *existing* interface compatible with another existing interface; Facade defines a *new* simpler interface over an existing subsystem; Adapters typically wrap a single object, Facades manage entire subsystems
- [[Mediator Pattern]] — also reduces coupling between many objects, but Mediator coordinates *peer* objects that know each other; Facade is one-directional (clients call the facade; the facade calls the subsystem; the subsystem does not know about the facade or the client)
- [[Abstract Factory Pattern]] — can serve as an alternative to Facade for hiding object creation complexity; can be used inside a Facade to create subsystem objects
- [[Singleton Pattern]] — Facades are often implemented as Singletons since only one facade object is usually needed per subsystem
- [[Proxy Pattern]] — both buffer access to a complex entity; Proxy maintains the *same* interface; Facade provides a *simplified* interface
