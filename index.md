# Index

One line per wiki page: `- [[Page Name]] — summary`. Update on every ingest.

---

## Topics
*(synthesis pages — start here for queries)*

### Meta / Navigation
- [[Learning Path]] — 10-stage guided journey from design principles → distributed systems → observability

### Design Patterns
- [[Design Patterns Overview]] — GoF three-category taxonomy, 23 patterns, patterns vs. principles, learning sequence
- [[Creational Patterns Overview]] — when and how to choose among the 7 creational patterns
- [[Structural Patterns Overview]] — choosing among 7 structural patterns; structural vs. behavioral comparisons
- [[Behavioral Patterns Overview]] — choosing among 10 behavioral patterns; pattern family groupings

### Design Principles
- [[SOLID Principles]] — all five principles as a system; origins, interactions, criticisms
- [[Design Principles Overview]] — master synthesis: SOLID, DRY, KISS, YAGNI, Composition, coupling/cohesion
- [[Software Design Fundamentals]] — PHAME, structural principles, design mindset, relationship to architecture

### Software Architecture
- [[Software Architecture Overview]] — definitions, core concerns, styles, quality attributes, architect's role
- [[Architectural Styles Comparison]] — side-by-side profiles and decision guide for architectural styles
- [[Architectural Styles Reference]] — comprehensive comparison of all architectural styles with decision guide

### Microservices & Cloud
- [[Cloud Design Patterns Overview]] — cloud patterns organized by problem category (reliability, communication, data)
- [[Microservices Patterns Overview]] — challenge map → patterns for consistency, reliability, communication, observability

### Domain-Driven Design
- [[DDD Building Blocks]] — all strategic and tactical DDD patterns and how they fit together

### Testing & Quality
- [[Testing Strategies Overview]] — test pyramid, TDD, BDD, contract testing, doubles; decision table for test types

### Reactive Architecture
- [[Reactive Architecture Overview]] — Reactive Manifesto, Reactive Streams, framework comparison, when to use

### Messaging & Communication
- [[Messaging Patterns Overview]] — queues vs. topics vs. streams; broker vs. streaming platform; EIP taxonomy

### API Design
- [[API Design Overview]] — REST vs. GraphQL vs. gRPC decision guide; versioning, contracts, maturity model

---

## Concepts — Creational Patterns (GoF)

- [[Singleton Pattern]] — one instance per JVM/process; eager/lazy/DCL/holder/enum variants; thread-safety
- [[Factory Method Pattern]] — polymorphic factory method decouples creator from product; JDBC, Calendar examples
- [[Abstract Factory Pattern]] — creates families of related objects; AWT/Swing, Spring ApplicationContext examples
- [[Builder Pattern]] — step-by-step construction of complex objects; Director, Fluent, inner-builder variants
- [[Prototype Pattern]] — clones objects without depending on concrete class; shallow vs deep copy; registry
- [[Object Pool Pattern]] — acquire/release lifecycle for expensive resources; HikariCP, ThreadPoolExecutor examples
- [[Lazy Initialization]] — defers creation to first use; DCL, holder class, Kotlin lazy, Virtual Proxy variants

## Concepts — Structural Patterns (GoF)

- [[Adapter Pattern]] — translates one interface to another; also Wrapper; Java I/O, JDBC, Spring MVC examples
- [[Bridge Pattern]] — decouples abstraction from implementation; shapes×colours motivation; Java AWT examples
- [[Composite Pattern]] — part-whole trees; clients treat leaves and composites uniformly; HTML DOM, React
- [[Decorator Pattern]] — wraps objects to add behaviour dynamically; Java I/O streams, Spring Security chain
- [[Facade Pattern]] — simplified interface over a complex subsystem; Spring JdbcTemplate, AWS SDK examples
- [[Flyweight Pattern]] — shares intrinsic state; Java Integer cache, String pool, game engine particle pools
- [[Proxy Pattern]] — controls access via same-interface surrogate; 6 types; Spring AOP, Hibernate lazy loading

## Concepts — Behavioral Patterns (GoF)

- [[Observer Pattern]] — subject broadcasts state changes to subscribers; Django signals, GUI listeners
- [[Strategy Pattern]] — interchangeable algorithms; Java Comparator, Spring Security, payment systems
- [[Command Pattern]] — request as object; undo/queue/macro; text editor undo, AWS SQS, DB transactions
- [[Chain of Responsibility Pattern]] — sequential handler dispatch; web middleware, servlet filters, approval flows
- [[Template Method Pattern]] — algorithm skeleton with variable steps; JUnit lifecycle, JdbcTemplate, data export
- [[Iterator Pattern]] — uniform collection traversal; Java Iterable, Python generators, JDBC cursors
- [[State Pattern]] — delegate to state object; vending machines, media players, TCP states, order lifecycle
- [[Mediator Pattern]] — central hub eliminates O(n²) coupling; air traffic control, Redux store, MVC controller
- [[Memento Pattern]] — opaque state snapshot for undo; Git commits, Photoshop history, Terraform rollback
- [[Visitor Pattern]] — add operations to stable hierarchy via double dispatch; compilers, shopping cart tax

## Concepts — Design Principles

- [[Single Responsibility Principle]] — one reason to change; Uncle Bob actor framing; prevents god classes
- [[Open-Closed Principle]] — extend without modifying; plugin architectures (Eclipse, IntelliJ, Minecraft)
- [[Liskov Substitution Principle]] — subtypes behaviorally substitutable; Rectangle/Square, Bird/Penguin examples
- [[Interface Segregation Principle]] — role interfaces over fat interfaces; Fowler's role vs. header interfaces
- [[Dependency Inversion Principle]] — depend on abstractions; owns the interface at the high-level module
- [[DRY Principle]] — single authoritative representation; Hunt & Thomas; WET code consequences
- [[KISS Principle]] — simplest solution that works; accidental vs. essential complexity; Google Search example
- [[YAGNI Principle]] — don't build speculative features; XP origins; four cost categories
- [[Composition over Inheritance]] — favour object composition; fragile base class problem; ECS as embodiment
- [[Dependency Injection]] — supply dependencies from outside; constructor/setter/method forms; DI vs DIP vs IoC

## Concepts — Architectural Styles & Patterns

- [[Event-Driven Architecture]] — producers, brokers, consumers; choreography vs. orchestration; pub/sub vs streaming
- [[CQRS]] — separate read/write models; Fowler's caution; reporting database alternative; pairs with Event Sourcing
- [[Event Sourcing]] — append-only event log as system of record; Fowler's three capabilities; schema evolution pitfalls
- [[Repository Pattern]] — collection-like data access abstraction; DDD usage; Unit of Work pattern
- [[MVC Pattern]] — Model/View/Controller; fat controller anti-pattern; MVP/MVVM variants noted
- [[MVVM Pattern]] — ViewModel holds observable UI state; data binding; WPF, Angular, SwiftUI, Jetpack Compose
- [[Layered Architecture]] — horizontal concern separation; 3/4-layer schemes; sinkhole anti-pattern
- [[Microservices Architecture]] — vertical domain decomposition; Fowler/Lewis 9 characteristics; migration strategy
- [[Service-Oriented Architecture]] — business services + ESB; 7 guiding principles; orchestration vs choreography
- [[Clean Architecture]] — concentric layers; inward-only dependencies; synthesis of Hexagonal/Onion/DCI/BCE
- [[Hexagonal Architecture]] — ports (inbound/outbound) and adapters; Cockburn 2005; domain core isolation
- [[Onion Architecture]] — Jeffrey Palermo's concentric-layer variant; domain model at center
- [[Modular Monolith]] — single deployable with strict module boundaries; MonolithFirst strategy; path to microservices
- [[Vertical Slice Architecture]] — feature-oriented code organization; complements CQRS/MediatR
- [[Serverless Architecture]] — FaaS model; event-driven execution; cost model; cold start trade-offs
- [[Actor Model Architecture]] — actors + message passing; supervision hierarchies; Orleans/Akka.NET/Proto.Actor
- [[Domain-Driven Design]] — strategic (Bounded Context, Ubiquitous Language) and tactical building blocks
- [[Architectural Styles and Patterns]] — style vs. pattern hierarchy; monolith vs. distributed taxonomy

## Concepts — Microservices & Cloud Patterns

- [[Circuit Breaker Pattern]] — Closed/Open/Half-Open state machine; Fowler's bliki; Resilience4j/Polly
- [[Saga Pattern]] — distributed transaction via choreography or orchestration; compensating transactions
- [[API Gateway Pattern]] — single external entry point; Microsoft's routing/aggregation/offloading trio
- [[Backend for Frontend Pattern]] — per-client-type gateway; solves API Gateway granularity mismatch
- [[Strangler Fig Pattern]] — incremental legacy migration via facade; Fowler's Queensland tree analogy
- [[Sidecar Pattern]] — co-deployed peripheral container; language-agnostic cross-cutting concerns
- [[Service Mesh]] — infrastructure layer for east-west service communication; data plane + control plane
- [[Ambassador Pattern]] — sidecar variant for outbound call management; proxy helper services
- [[Bulkhead Pattern]] — resource pool isolation to contain failure blast radius
- [[Outbox Pattern]] — atomic state + event publication in one DB transaction; reliable at-least-once delivery
- [[Inbox Pattern]] — consumer-side deduplication; exactly-once handler execution
- [[Anti-Corruption Layer Pattern]] — translation layer protecting domain from external model corruption
- [[Resiliency Patterns]] — Retry, Timeout, Fallback, Hedging; Polly/Simmy reference

## Concepts — DDD Building Blocks

- [[Bounded Context]] — explicit model boundary with ubiquitous language; strategic DDD cornerstone
- [[Aggregate]] — cluster of domain objects with single root enforcing invariants
- [[Domain Event]] — immutable record of a business fact; domain vs. integration event distinction
- [[Value Object]] — immutable, identity-free domain descriptor; solves primitive obsession
- [[Event Storming]] — collaborative workshop; Big Picture / Process Level / Design Level formats

## Concepts — Distributed Systems Fundamentals

- [[CAP Theorem]] — Consistency/Availability/Partition Tolerance trade-offs; CP vs AP systems
- [[Eventual Consistency]] — BASE vs ACID; consistency spectrum; conflict resolution strategies
- [[Idempotency]] — safe-to-retry operations; idempotency keys; idempotent consumers
- [[Backpressure]] — flow control; throttle/buffer/drop/reject strategies; Reactive Streams
- [[Distributed Transactions]] — 2PC mechanics and problems; Saga as the modern alternative

## Concepts — Messaging & Event Streaming

- [[Message Queue]] — AMQP model; exchange types; delivery guarantees; RabbitMQ specifics
- [[Publish-Subscribe Pattern]] — topics vs. queues; fan-out; distributed vs. in-process Observer
- [[Apache Kafka]] — distributed log; partitions; consumer groups; offsets; vs. RabbitMQ table
- [[Message Broker]] — broker role in EDA; push vs. pull; RabbitMQ vs. Kafka trade-off table
- [[Enterprise Integration Patterns]] — Hohpe & Woolf 65 patterns; channels, routers, translators, endpoints

## Concepts — API Design

- [[REST]] — Fielding's 6 constraints; resource model; HTTP verb semantics; Richardson Maturity Model
- [[GraphQL]] — query/mutation/subscription; type system; resolver model; N+1 problem; vs REST/gRPC
- [[gRPC]] — Protocol Buffers; HTTP/2; 4 streaming modes; contract-first; vs REST/GraphQL table
- [[API Design Principles]] — versioning strategies; backward compat rules; idempotency keys; HATEOAS
- [[Richardson Maturity Model]] — Level 0–3 with examples; HATEOAS link anatomy; design principles per level

## Concepts — Reactive Architecture

- [[Reactive Architecture]] — Reactive Manifesto: Responsive, Resilient, Elastic, Message-Driven
- [[Reactive Streams]] — Publisher/Subscriber/Subscription/Processor; backpressure via request(n); JDK 9 Flow
- [[Reactive Programming]] — ReactiveX/Observable model; operators; Project Reactor; Spring WebFlux

## Concepts — Testing

- [[Test Pyramid]] — unit/integration/E2E layers; ice cream cone anti-pattern; narrow vs. broad integration
- [[Test-Driven Development]] — Red-Green-Refactor; Kent Beck; sociable vs. solitary tests
- [[Behavior-Driven Development]] — Given/When/Then; Three Amigos; Gherkin/Cucumber; collaboration tool
- [[Consumer-Driven Contract Testing]] — Pact workflow; provider/consumer contracts; microservices testing
- [[Test Double]] — Meszaros taxonomy: Dummy, Fake, Stub, Spy, Mock; state vs. behaviour verification
- [[Integration Testing]] — narrow vs. broad; Testcontainers; test one seam at a time
- [[Property-Based Testing]] — generative testing; QuickCheck; Hypothesis; shrinking; edge case discovery

## Concepts — DevOps & Infrastructure

- [[Twelve-Factor App]] — all 12 factors; Heroku origins; cloud-native app principles
- [[Infrastructure as Code]] — declarative vs. imperative; immutable infrastructure; Terraform/Pulumi/CDK
- [[Continuous Integration and Delivery]] — CI vs CD; pipeline stages; DORA metrics; deployment strategies
- [[Blue-Green Deployment]] — zero-downtime environment switching; instant rollback; expand-contract for DBs
- [[Canary Release]] — gradual traffic shifting; automated canary analysis; feature flag relationship

## Concepts — Observability

- [[Observability]] — three pillars (logs/metrics/traces); observability vs. monitoring; OpenTelemetry
- [[Distributed Tracing]] — spans, traces, context propagation; W3C Trace Context; Jaeger/Zipkin

## Concepts — Software Design Fundamentals

- [[Abstraction]] — control vs. data abstraction; PHAME; leaky abstractions (Spolsky)
- [[Encapsulation]] — data hiding + behavioral bundling; invariant enforcement; vs. information hiding
- [[Information Hiding]] — Parnas 1972; design secret as the module's reason for existence
- [[Modularity]] — PHAME; horizontal vs. vertical partitioning; trade-offs of over-modularization
- [[Coupling and Cohesion]] — inverse relationship; coupling types (content→data); cohesion types
- [[Separation of Concerns]] — Dijkstra 1974; applied at class/layer/aspect/protocol levels
- [[Law of Demeter]] — principle of least knowledge; Ian Holland 1987; train wreck anti-pattern
- [[Software Design vs Software Architecture]] — locality criterion; Eden/Kazman; the continuum
- [[Conceptual Integrity]] — Fred Brooks; single architect vision; the most important system quality
- [[Technical Debt]] — Ward Cunningham's debt metaphor; four-quadrant model; management strategies
- [[Conway's Law]] — org structure shapes system architecture; Inverse Conway Maneuver
- [[Quality Attributes]] — the "-ilities"; ISO/IEC 25010; execution vs. evolution qualities; fitness functions
- [[Architecture Erosion]] — Perry and Wolf 1992; causes, detection, Mozilla case study
- [[Architectural Decision Records (ADR)]] — Nygard template; Y-statements; why email fails; storage lifecycle

---

## People

- [[Gang of Four]] — Gamma, Helm, Johnson, Vlissides; 1994 GoF book; 23 canonical patterns
- [[Martin Fowler]] — named CQRS, Event Sourcing, Microservices patterns; Refactoring; PoEAA; Thoughtworks Chief Scientist
- [[Robert C. Martin]] — SOLID principles, Clean Architecture, Clean Code; founding Agile Manifesto signatory
- [[Eric Evans]] — Domain-Driven Design; Ubiquitous Language, Bounded Context, Aggregate, Domain Events
- [[Alistair Cockburn]] — Hexagonal Architecture (Ports & Adapters); Crystal methodology; Agile Manifesto
- [[Ward Cunningham]] — coined Technical Debt; invented Wiki; co-created Extreme Programming with Kent Beck
- [[Kent Beck]] — Test-Driven Development; Extreme Programming; YAGNI; JUnit; four rules of simple design
- [[Gregor Hohpe]] — Enterprise Integration Patterns; 65 messaging patterns; event-driven architecture
- [[Chris Richardson]] — Microservices Patterns; Saga pattern; microservices.io; distributed data management
- [[Edsger Dijkstra]] — Separation of Concerns; structured programming; semaphores; Turing Award
- [[David Parnas]] — Information Hiding; modular decomposition criteria; 1972 CACM paper

---

## Sources

- [[Design Patterns Tutorial (GeeksforGeeks)]] — high-level GoF index + 8-week learning roadmap
- [[Creational Design Patterns (GeeksforGeeks)]] — refactoring.guru card-index; pattern names and descriptions
- [[Structural Design Patterns]] — refactoring.guru card-index for all 7 GoF structural patterns
- [[Behavioral Design Patterns (GeeksforGeeks)]] — refactoring.guru clip; behavioral pattern names
- [[Software Architecture Guide (GeeksforGeeks)]] — Martin Fowler's architecture guide; definitions and quality
- [[What is Software Architecture - Comprehensive Guide]] — design process, layered/microservices/EDA with examples
- [[Top 10 Software Architecture Patterns for 2025]] — survey of 10 patterns with analogies and pros/cons
- [[Full-stack Software Design & Architecture Map]] — Khalil Stemmler's 9-stage learning roadmap
- [[Software Architecture (Overview Source)]] — Wikipedia; two laws, style vs. pattern, Conway's Law
- [[Software Development Source]] — ByteByteGo dev guide index
- [[Software Architecture 1 (Wikipedia)]] — comprehensive Wikipedia article on software architecture
- [[What Makes a Good Software Design Mindset (Source)]] — Duy Pham's Medium article on design mindset
- [[Software Design (Wikipedia Source)]] — Wikipedia software design article
- [[Awesome Software Architecture (Curated List)]] — mehdihadeli's ~60-category curated resource list
- [[Martin Fowler - Circuit Breaker]] — Fowler's canonical circuit breaker bliki entry
- [[Martin Fowler - Strangler Fig Application]] — Fowler's naming and definition of the Strangler Fig pattern
- [[Microsoft Azure - Sidecar Pattern]] — Azure Architecture Center docs for Sidecar
- [[Microsoft Azure - Strangler Fig Pattern]] — Azure Architecture Center docs with database migration example
- [[microservices.io - Saga Pattern]] — Chris Richardson's canonical Saga pattern catalog entry
- [[Martin Fowler - Practical Test Pyramid]] — Fowler's test pyramid article; unit/integration/E2E balance
- [[Martin Fowler - Test Double]] — Fowler's TestDouble taxonomy bliki entry
- [[Martin Fowler - Consumer-Driven Contracts]] — Ian Robinson's original CDC article
- [[Pact - Consumer-Driven Contract Testing]] — docs.pact.io; Pact workflow and broker
- [[Reactive Manifesto]] — reactivemanifesto.org; four properties of reactive systems
- [[Reactive Streams Specification]] — reactive-streams.org; Publisher/Subscriber/Subscription interfaces
- [[12factor.net - The Twelve-Factor App]] — Adam Wiggins / Heroku; all 12 factors
- [[Martin Fowler - Richardson Maturity Model]] — levels 0–3; HATEOAS
- [[gRPC Documentation]] — gRPC intro and core concepts
- [[GraphQL Learn]] — GraphQL.org learn section (403 on fetch; flagged unverified)
- [[RabbitMQ - AMQP Concepts]] — AMQP 0-9-1 concepts; exchanges, queues, bindings
- [[Apache Kafka - Introduction]] — kafka.apache.org/intro; distributed log overview
- [[Enterprise Integration Patterns - EIP]] — Hohpe & Woolf catalogue overview
