---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://graphql.org/learn/
source_date: 2024
source_author: GraphQL Foundation
tags:
  - graphql
  - api-design
  - query-language
  - schema
---

# GraphQL — Learn Documentation

Official GraphQL learning documentation at [https://graphql.org/learn/](https://graphql.org/learn/).

> [!question] Unverified
> The graphql.org site returned HTTP 403 during the fetch, so these notes are based on the gRPC and Richardson sources plus general knowledge of GraphQL. Content should be cross-checked against the live documentation if a detailed refresh is needed.

## Summary

GraphQL is a query language for APIs and a server-side runtime developed by Facebook (open-sourced 2015). Clients describe the exact shape of data they need; the server resolves and returns precisely that. A single endpoint replaces multiple REST routes.

## Key Concepts

- **Operations**: Query (read), Mutation (write), Subscription (real-time push via WebSocket).
- **Type System**: Scalar types (Int, Float, String, Boolean, ID), object types, interfaces, unions, enums, input types.
- **Schema**: The contract between client and server; all available types and operations are defined here.
- **Resolvers**: Server-side functions that return data for each field in the schema.
- **Introspection**: The schema is self-describing; tools like GraphiQL auto-generate docs and enable query completion.
- **N+1 problem**: Naive resolver implementations issue N database queries for N items; solved with DataLoader (batching).
- **Fragments**: Reusable field sets for composing queries.
- **Variables**: Parameterise queries at runtime without string interpolation.
- **Directives**: `@deprecated`, `@skip`, `@include` modify execution or schema documentation.

## Wiki Pages That Cite This Source

- [[GraphQL]] — full concept page
- [[API Design Overview]] — GraphQL in the REST vs. GraphQL vs. gRPC comparison
- [[API Design Principles]] — versionless evolution, schema-driven backward compatibility
