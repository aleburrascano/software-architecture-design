---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://graphql.org/learn/
tags:
  - api-design
  - graphql
  - query-language
  - web
---

# GraphQL

A query language for APIs and a server-side runtime for executing those queries — developed by Facebook (2012, open-sourced 2015) — where clients specify exactly what data they need, and the server returns precisely that.

## Problem

REST APIs return fixed response shapes. Clients (especially mobile) often need only a subset of a resource's fields (over-fetching) or must make multiple requests to assemble a view (under-fetching). Versioning REST APIs as requirements evolve creates URI proliferation and maintenance burden.

## Solution / Explanation

GraphQL replaces multiple REST endpoints with a **single endpoint** (`/graphql`) that accepts structured queries. Clients describe the shape of the data they want; the server resolves and returns exactly that shape — no more, no less.

### Core Operations

**Query** — read data.
```graphql
query {
  user(id: "1") {
    name
    email
    posts {
      title
    }
  }
}
```
Returns only `name`, `email`, and each post's `title`.

**Mutation** — write data.
```graphql
mutation {
  createPost(title: "Hello", body: "World") {
    id
    title
  }
}
```

**Subscription** — real-time push via WebSocket.
```graphql
subscription {
  newComment(postId: "42") {
    author
    body
  }
}
```

### Type System and Schema

GraphQL is **strongly typed**. The schema is the contract between client and server:

```graphql
type User {
  id: ID!
  name: String!
  email: String
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  author: User!
}

type Query {
  user(id: ID!): User
  posts: [Post!]!
}
```

- `!` means non-nullable.
- Scalar types: `Int`, `Float`, `String`, `Boolean`, `ID`.
- Object types, interfaces, unions, enums, and input types are all supported.

### Resolvers

Each field in the schema is backed by a **resolver function** — a function that fetches the data for that field. The resolver for `User.posts` knows how to query the database for posts belonging to a user.

### The N+1 Problem

A classic GraphQL performance trap: fetching a list of N users, then for each user running a separate query to fetch their posts — resulting in N+1 database queries. The standard solution is the **DataLoader** pattern (batching and caching resolver calls within a request).

> [!question] Unverified
> DataLoader is the de-facto solution in JavaScript/TypeScript ecosystems. Equivalent batching libraries exist in other languages but vary in maturity.

### Introspection

GraphQL schemas are **introspectable** at runtime. Tools like GraphiQL and Apollo Studio use introspection to generate documentation and enable query auto-completion.

## Key Components

- **Schema** — the strongly-typed contract defining all available types and operations.
- **Query / Mutation / Subscription** — the three operation types.
- **Resolver** — server-side function that fetches data for a field.
- **Type System** — scalar, object, interface, union, enum, input types.
- **DataLoader** — batching pattern that solves N+1 resolver queries.
- **Introspection** — self-describing schema discoverable at runtime.

## When to Use

- Clients with diverse data needs (mobile app, web app, third-party integrations).
- Rapid front-end development where clients own their data shape.
- Aggregating data from multiple backend services into a single query (API gateway pattern).
- Applications where bandwidth is constrained (mobile): avoid over-fetching.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Eliminates over-fetching and under-fetching | N+1 problem requires DataLoader discipline |
| Single endpoint simplifies routing | HTTP caching is harder (POST requests) |
| Strongly typed schema is self-documenting | Schema design requires up-front investment |
| Clients evolve independently of server schema | Complex queries can be expensive — rate limiting/query complexity needed |
| Introspection enables great tooling | Learning curve for server-side resolver architecture |

## Comparison with REST and gRPC

| | REST | GraphQL | [[gRPC]] |
|-|------|---------|---------|
| Protocol | HTTP/1.1 | HTTP/1.1 | HTTP/2 |
| Payload | JSON/XML | JSON | Protobuf |
| Data fetching | Server-defined shape | Client-defined shape | Server-defined shape |
| Caching | Native HTTP | Requires custom cache | No native HTTP caching |
| Type safety | Optional (OpenAPI) | Built-in | Built-in (Protobuf) |
| Streaming | Webhooks / SSE | Subscriptions (WebSocket) | Native streaming |
| Best for | Resource CRUD | Flexible data fetching | Low-latency internal RPC |

## Related

- [[REST]] — the dominant alternative for resource-oriented APIs
- [[gRPC]] — binary, contract-first alternative for internal services
- [[API Design Principles]] — versioning, backward compatibility
- [[Microservices Architecture]] — GraphQL as an API gateway aggregating microservices
