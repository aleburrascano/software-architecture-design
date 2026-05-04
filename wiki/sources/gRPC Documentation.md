---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://grpc.io/docs/what-is-grpc/
source_date: 2024
source_author: gRPC Authors (Google / CNCF)
tags:
  - grpc
  - rpc
  - protobuf
  - http2
  - api-design
---

# gRPC Documentation — Introduction and Core Concepts

Official gRPC documentation covering the introduction and core concepts, at [https://grpc.io/docs/what-is-grpc/introduction/](https://grpc.io/docs/what-is-grpc/introduction/) and [https://grpc.io/docs/what-is-grpc/core-concepts/](https://grpc.io/docs/what-is-grpc/core-concepts/).

## Summary

gRPC is a high-performance, open-source RPC framework that uses Protocol Buffers as IDL and HTTP/2 as transport. It enables a client to call a server method as if it were local. Services and messages are defined in `.proto` files; `protoc` generates type-safe client stubs and server interfaces in 11+ languages.

## Key Takeaways

- **Four streaming modes**: Unary, server streaming, client streaming, bidirectional streaming.
- **HTTP/2 benefits**: multiplexing, header compression, binary framing, native flow control.
- **Protocol Buffers**: binary, compact, strongly typed; fields are numbered for schema evolution.
- **Deadlines and cancellation**: first-class features for timeout and lifecycle management.
- **Metadata**: arbitrary key-value pairs for auth tokens and call context.
- **Channels**: connection abstraction that supports load balancing and state management.
- **Supported languages**: Java, Go, Python, Ruby, C++, C#, Kotlin, Dart, Node, PHP, Objective-C, Swift.

## Wiki Pages That Cite This Source

- [[gRPC]] — full concept page
- [[API Design Overview]] — gRPC in the REST vs. GraphQL vs. gRPC comparison
- [[API Design Principles]] — contract-first design and schema evolution
