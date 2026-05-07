---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/tmrts-go-patterns.md'
source_date: ''
source_author: tmrts (GitHub)
tags:
  - source
  - go
  - golang
  - design-patterns
  - goroutine
  - channel
  - concurrency
  - gof
  - idioms
---
# Go Design Patterns - tmrts

Curated collection of Go design patterns, idioms, and concurrency recipes demonstrating idiomatic Go implementations of GoF and Go-native patterns.

## Summary

This repository bridges the GoF pattern catalog with Go's distinctive language features, showing that patterns translate but look substantially different in Go than in classical OOP languages. Go's interface system — structural rather than nominal typing — means patterns like Strategy and Observer are implemented by satisfying interfaces implicitly, without explicit declarations of intent. Composition over inheritance replaces the subclassing that GoF patterns often rely on.

The Go-specific section is the most distinctive contribution: goroutine-based concurrency patterns not found in the GoF catalog. Pipeline patterns chain goroutines connected by channels, enabling composable data transformation stages. Fan-out distributes work across multiple goroutines; fan-in collects results. The done channel pattern provides a standardized cancellation mechanism that propagates through goroutine hierarchies. Worker pools bound resource consumption. Generator patterns create lazy sequences using goroutines and channels.

Go idioms are treated as patterns in their own right: functional options for flexible struct initialization, error as value for explicit error handling, the context package for cancellation and deadline propagation, and table-driven tests. These idioms reflect the community's accumulated wisdom about idiomatic Go design.

## Key Arguments

- GoF patterns translate to Go but their implementation differs significantly due to interfaces and composition
- Go's structural interface typing enables implicit pattern participation without inheritance hierarchies
- Go's goroutines enable concurrency patterns (pipeline, fan-in/out, worker pool) absent from the GoF catalog
- Channels replace locks for many synchronization scenarios, producing more composable concurrent designs
- Go idioms (functional options, error as value, context propagation) constitute patterns in their own right

## Concepts Covered

- [[Design Patterns Overview]] — Go idiomatic implementations showing language-specific translation
- [[Behavioral Patterns Overview]] — Observer, Strategy, and Command patterns in Go
- [[Creational Patterns Overview]] — Factory, Builder, and Singleton in Go idioms
- [[Reactive Architecture]] — pipeline and fan-out patterns as reactive data flow primitives

## Quality Notes

Useful for Go practitioners. Demonstrates language-idiomatic pattern implementation and Go-native concurrency patterns. Language-specific but the underlying patterns are universal. Well-organized with clear examples.
