---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources: ["Behavioral Design Patterns.md", "https://www.geeksforgeeks.org/system-design/observer-pattern-set-1-introduction/", "https://refactoring.guru/design-patterns/observer"]
tags: [design-pattern, behavioral, gof]
---

# Observer Pattern

A behavioral pattern that defines a one-to-many dependency so that when one object (the subject) changes state, all its dependents (observers) are notified and updated automatically.

## Problem

A system has objects that need to stay consistent with the state of another object, but tight coupling between them would make the system brittle and hard to extend. Adding new dependents should not require modifying the subject. Classic scenarios:

- A spreadsheet UI where multiple chart views must update whenever underlying data changes.
- An event system where many listeners react to the same source event.
- A model in MVC that must push updates to an arbitrary number of view components.
- A weather station broadcasting temperature, humidity, and pressure to multiple display devices simultaneously.

Without the pattern, each dependent must either poll the subject repeatedly (wasteful) or the subject must know and call every dependent explicitly (brittle).

## Solution

Introduce two abstractions: **Subject** (also called Observable or Publisher) and **Observer** (also called Subscriber or Listener). The subject maintains a list of observers and provides `attach`/`detach` methods. When its state changes it calls `notify()`, which iterates over the list and calls each observer's `update()` method. Observers register themselves; the subject has no compile-time knowledge of the concrete observer types.

## Structure

| Participant | Role |
|---|---|
| **Subject** | Holds state, maintains observer list, fires `notify()` on change |
| **Observer** (interface) | Declares `update()` — the callback contract |
| **ConcreteSubject** | Implements Subject; stores state of interest |
| **ConcreteObserver** | Implements Observer; reacts to state changes, may query Subject |

Flow: Subject state changes → `notify()` → `observer.update()` for each registered observer → observers pull updated state (pull model) or receive it as an argument (push model).

## When to Use

- One object's change must trigger updates in an unknown or variable number of other objects.
- You want loose coupling between subjects and observers (neither knows the concrete type of the other).
- You are implementing an event system, pub/sub mechanism, or reactive data pipeline.
- Changes to one object require changing others and you don't know how many.
- Subscription should be dynamic — observers joining or leaving at runtime.

## Trade-offs

**Pros:**
- Open/Closed Principle — add new observers without modifying the subject.
- Loose coupling: subject and observers interact only through abstract interfaces.
- Supports broadcast communication; one notification reaches all subscribers.
- Observers can be added and removed at runtime.
- Enables event-driven architectures at the object level.

**Cons / pitfalls:**
- Unexpected updates — observers don't know about each other, so a change can cascade in unexpected ways.
- Memory leaks — if observers are not explicitly detached, the subject holds references indefinitely (the "lapsed listener" problem).
- Update order is unspecified and can be hard to reason about; "subscribers are notified in random order."
- If the notification is not fine-grained enough, observers do unnecessary work.
- Cycles are possible if observers modify the subject during `update()`.
- Performance can degrade with a large number of observers or frequent state changes.

## Variants

- **Push model** — subject passes the new state as arguments to `update(data)`. Observers need no reference to the subject but receive possibly irrelevant data.
- **Pull model** — subject calls `update()` with no data; observers call back to query specific state. More flexible but requires observers to hold a subject reference.
- **Event Bus / Message Broker** — a third-party mediator decouples subject and observer entirely; they communicate only via typed events (common in frameworks like Android's EventBus or Spring's `ApplicationEventPublisher`).
- **Reactive Streams** — Observer generalised with backpressure and composable pipelines (RxJava, Project Reactor, Swift Combine).
- **Filtered notifications** — observers register with a filter/predicate so they only receive events that match their interest.

## Code Example

````nimport java.util.*;

interface Observer {
    void update(String weather);
}

interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

class WeatherStation implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String weather;

    public void addObserver(Observer observer) { observers.add(observer); }
    public void removeObserver(Observer observer) { observers.remove(observer); }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(weather);
        }
    }

    public void setWeather(String newWeather) {
        this.weather = newWeather;
        notifyObservers();
    }
}

class PhoneDisplay implements Observer {
    private String weather;
    public void update(String weather) {
        this.weather = weather;
        System.out.println("Phone Display: Weather updated - " + weather);
    }
}

class TVDisplay implements Observer {
    public void update(String weather) {
        System.out.println("TV Display: Current weather - " + weather);
    }
}

// Client
WeatherStation station = new WeatherStation();
station.addObserver(new PhoneDisplay());
station.addObserver(new TVDisplay());
station.setWeather("Sunny");
// => Phone Display: Weather updated - Sunny
// => TV Display: Current weather - Sunny
```

## Real-World Examples

- **Social media notifications** — following a user subscribes you to their post events; the platform fan-outs one post to millions of follower feeds.
- **Stock market tickers** — a price feed notifies all registered trading dashboards, alert systems, and analytics engines when a stock price changes.
- **GUI event listeners** — a button (subject) broadcasts click events to all registered action listeners; no button code changes when a new listener is added.
- **Weather apps** — a central weather data service pushes updates to phone displays, web dashboards, and IoT devices simultaneously.
- **Spreadsheets** — changing a cell value triggers recalculation in all dependent cells and chart views (Excel's cell dependency graph is a form of Observer).
- **Built-in observer utilities** — most runtimes and frameworks provide built-in event/listener APIs that implement the Observer pattern natively.
- **ORM lifecycle hooks** — data access frameworks broadcast model lifecycle events (before/after save, delete) to registered signal handlers.

## Related

- [[Mediator Pattern]] — Mediator also decouples objects, but routes communication through a central hub rather than broadcasting; prefer Mediator when many-to-many communication is complex. Observer establishes dynamic one-way subscriptions; Mediator eliminates mutual dependencies through a centralized coordinator. A Mediator can be implemented using Observer as its underlying mechanism.
- [[Strategy Pattern]] — often combined: strategy objects may be swapped and observers notified of the change.
- [[Command Pattern]] — commands can be queued and observers notified when they execute.
- [[Event-Driven Architecture]] — architectural style that scales the Observer idea across services.
