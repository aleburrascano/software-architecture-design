---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://grpc.io/docs/what-is-grpc/introduction/
  - https://grpc.io/docs/what-is-grpc/core-concepts/
tags:
  - api-design
  - grpc
  - rpc
  - protobuf
  - http2
  - microservices
---

# gRPC

A high-performance, open-source Remote Procedure Call (RPC) framework originally developed by Google — using Protocol Buffers as the IDL and HTTP/2 as transport — enabling contract-first, polyglot, low-latency service communication.

## Problem

REST APIs are text-heavy (JSON), lack a built-in contract, and use HTTP/1.1 (one request per connection). Internal microservice-to-microservice communication needs lower latency, stricter contracts, efficient binary serialisation, and support for streaming.

## Solution / Explanation

gRPC lets a client call a method on a remote server as if it were a local function. The service contract is defined in a `.proto` file using **Protocol Buffers**. The `protoc` compiler generates client **stubs** and server **interfaces** in any supported language. Transport uses **HTTP/2** for multiplexing, header compression, and native streaming.

### Protocol Buffers

`.proto` files define both the service interface and the message types:

```protobuf
syntax = "proto3";

service Greeter {
  rpc SayHello (HelloRequest) returns (HelloReply);
  rpc SayHelloServerStream (HelloRequest) returns (stream HelloReply);
}

message HelloRequest {
  string name = 1;
}

message HelloReply {
  string message = 1;
}
```

- Fields are numbered — old fields can be deprecated without breaking existing clients.
- Binary encoding is 3-10x smaller than equivalent JSON.
- `protoc` generates type-safe code in Java, Go, Python, C++, C#, Kotlin, Dart, Ruby, Node, PHP, Swift, and more.

### Four Streaming Modes

| Mode | Description | Use case |
|------|-------------|---------|
| **Unary** | One request → one response | Standard function call; most common |
| **Server streaming** | One request → stream of responses | Large result set, live feed |
| **Client streaming** | Stream of requests → one response | File upload, telemetry aggregation |
| **Bidirectional streaming** | Simultaneous request and response streams | Chat, real-time collaboration |

### HTTP/2 Advantages

- **Multiplexing** — multiple RPC calls over a single TCP connection (no head-of-line blocking).
- **Header compression** — HPACK reduces header overhead.
- **Binary framing** — lower parsing overhead than HTTP/1.1 text.
- **Flow control** — built-in backpressure.
- **Server push** — server can proactively send data.

### RPC Lifecycle

1. Client invokes a stub method; the gRPC runtime sends metadata, method name, and deadline to the server.
2. Server responds with initial metadata (optional), then the response message, then trailing metadata + status.
3. Client receives the response and the call completes.

### Key Operational Features

- **Deadlines/Timeouts** — clients specify the maximum wait time; gRPC propagates cancellation.
- **Cancellation** — either side can cancel an in-progress RPC.
- **Metadata** — arbitrary key-value pairs attached to a call (e.g., authentication tokens).
- **Channels** — manage connections to a server endpoint; support load balancing.
- **Error model** — standardised status codes (OK, NOT_FOUND, UNAVAILABLE, etc.).

## Key Components

- **Protocol Buffers (.proto)** — the IDL defining services and messages.
- **protoc** — compiler generating client stubs and server skeletons.
- **Stub / Channel** — client-side generated code and connection abstraction.
- **Server** — implements the service interface and handles requests.
- **Streaming** — four modes supporting unary, server, client, and bidirectional flows.
- **Interceptors** — middleware for logging, auth, tracing (analogous to HTTP middleware).

## When to Use

- Internal microservice-to-microservice communication where low latency and efficiency matter.
- Polyglot systems where different services are written in different languages.
- Streaming data between services (real-time logs, IoT sensor data).
- APIs with complex data models where Protobuf schema validation is valuable.
- Mobile-to-backend communication where bandwidth is constrained.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Binary Protobuf: efficient serialisation | Not human-readable; harder to debug with basic tools |
| HTTP/2 multiplexing: high throughput | Browser support requires gRPC-Web proxy |
| Strict schema: contract-first development | Schema evolution requires backward-compat discipline |
| Native streaming (4 modes) | More complex than REST for simple CRUD |
| Polyglot code generation | Adds build toolchain dependency (protoc) |

## Comparison

| | REST | [[GraphQL]] | gRPC |
|-|------|------------|------|
| Protocol | HTTP/1.1 | HTTP/1.1 | HTTP/2 |
| IDL / Contract | Optional (OpenAPI) | GraphQL Schema | Protocol Buffers |
| Payload | JSON/XML | JSON | Binary (Protobuf) |
| Streaming | Limited (SSE, WebSocket) | Subscriptions | Native (4 modes) |
| Browser support | Native | Native | Needs gRPC-Web proxy |
| Best for | Public APIs, CRUD | Flexible data queries | Internal RPC, streaming |

## Related

- [[REST]] — text-based, resource-oriented alternative for public APIs
- [[GraphQL]] — query-language alternative for flexible data fetching
- [[API Design Principles]] — versioning, backward compatibility across services
- [[Microservices Architecture]] — gRPC is common for synchronous inter-service calls
- [[Backpressure]] — HTTP/2 flow control provides native back-pressure
