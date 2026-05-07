---
title: "CQRS and Event Sourcing: Implementation Guide"
source: "https://knowledgelib.io/software/system-design/cqrs-event-sourcing/2026"
author:
published: 2026-02-23
created: 2026-05-07
description: "Complete implementation guide for CQRS (Command Query Responsibility Segregation) and Event Sourcing patterns. Covers architecture components, decision trees for CQRS-only vs. CQRS+ES, step-by-step implementation with event store, projections, and snapshotting. Includes Python and Node.js code examples."
tags:
  - clippings
  - "vault-scout"
  - "source:tier2"
  - "kind:article"
  - "score:8"
---




  How do I implement CQRS and Event Sourcing?

  

  
  
  TL;DR
  
    Bottom line: CQRS separates read and write models for independent optimization; Event Sourcing stores state as an append-only sequence of domain events, enabling complete audit trails and temporal queries.
    Key tool/command: EventStoreDB (purpose-built event store), or Apache Kafka with log compaction disabled for event sourcing on existing infrastructure.
    Watch out for: Applying CQRS/ES globally instead of selectively to bounded contexts that actually benefit from it — most systems do not need this complexity.
    Works with: Any language/framework; dedicated support in Axon Framework (Java), Marten (C#/.NET), EventStoreDB (polyglot), Kafka Streams, and custom implementations.
  
  

  
  
  Constraints
  
    Never use CQRS as a top-level architecture for the entire system — apply it selectively to bounded contexts that benefit from separate read/write models [src1]
    Event schemas are your API contract — never modify published event schemas; only add new event types or version existing ones [src2]
    Event stores must be append-only — never update or delete events in production; use compensating events instead [src4]
    Projections must be rebuildable from scratch — never store data in read models that cannot be reconstructed from the event stream [src6]
    Do not use CQRS without operational maturity (monitoring, tracing, dead-letter queues) — eventual consistency bugs are invisible without observability [src3]
  
  

  
  
  Quick Reference
  
  
    
      ComponentRoleTechnology OptionsScaling Strategy
    
    
      Command APIAccepts write requests, validates business rulesREST/gRPC endpoint, message queue consumerHorizontal scaling behind load balancer
      Command HandlerProcesses commands, loads aggregate, emits eventsApplication service layerStateless; scale by adding instances
      AggregateEnforces business invariants, produces domain eventsDomain model (DDD aggregate root)One aggregate instance per ID (single-writer)
      Event StoreAppend-only persistence of domain eventsEventStoreDB, PostgreSQL + outbox, Kafka, DynamoDBPartition by aggregate ID / stream
      Event BusDistributes events to subscribersKafka, RabbitMQ, Amazon SNS/SQS, Azure Service BusPartition by event type or aggregate ID
      Projection EngineTransforms events into read-optimized viewsKafka Streams, custom subscribers, Axon projectionsOne consumer group per projection
      Read Model StoreServes denormalized query dataPostgreSQL, Redis, Elasticsearch, MongoDBRead replicas, caching, sharding
      Query APIServes read requests from materialized viewsREST/GraphQL endpointHorizontal scaling + CDN/cache
      Snapshot StoreCaches aggregate state at intervals to speed rebuildsSame DB as event store or separate cacheSnapshot every N events (100-500)
      Saga / Process ManagerCoordinates multi-aggregate workflowsStateful orchestrator consuming eventsPartition by saga ID
      Dead Letter QueueCaptures failed event processing for retryKafka DLQ, SQS DLQ, RabbitMQ dead-letter exchangeMonitor size; alert on growth
      Schema RegistryManages event schema evolution and compatibilityConfluent Schema Registry, AWS Glue, customCentral service; version all schemas
    
  
  
  

  
  
  Decision Tree
  START
├── Do you need different read and write models?
│   ├── NO → Standard CRUD is simpler; skip CQRS entirely
│   └── YES ↓
├── Do you need a complete audit trail / temporal queries?
│   ├── NO → Use CQRS only (separate read/write models, shared or separate DB)
│   └── YES ↓
├── CQRS + Event Sourcing
│   ├── Expected load < 1K concurrent users?
│   │   ├── YES → Single-node EventStoreDB or PostgreSQL event table
│   │   │         Synchronous projections acceptable
│   │   └── NO ↓
│   ├── Expected load 1K-100K concurrent users?
│   │   ├── YES → EventStoreDB cluster or Kafka + dedicated projection service
│   │   │         Async projections with consumer groups
│   │   └── NO ↓
│   └── Expected load > 100K concurrent users?
│       └── YES → Kafka event backbone + partitioned read stores
│                 (Elasticsearch, Redis, Cassandra)
│                 Multiple projection services + snapshotting
└── Framework available for your language?
    ├── Java/Kotlin → Axon Framework (batteries-included)
    ├── C#/.NET → Marten or EventStoreDB .NET client
    ├── Python/JS/Go → Custom implementation with EventStoreDB or Kafka
    └── Any → EventStoreDB gRPC client (polyglot)
  

  
  
  Step-by-Step Guide

  1. Define bounded context and aggregates
  Identify the domain boundary where CQRS/ES adds value. Map aggregates (consistency boundaries) using Domain-Driven Design. Each aggregate will have its own event stream. [src1]
  Example bounded context: Order Management
├── Aggregate: Order (OrderId)
│   Events: OrderCreated, ItemAdded, ItemRemoved, OrderConfirmed, OrderShipped
├── Aggregate: Inventory (ProductId)
│   Events: StockReserved, StockReleased, StockDepleted
└── Process Manager: OrderFulfillment
    Listens: OrderConfirmed → reserves stock → emits OrderReadyToShip
  Verify: Each aggregate has a clear consistency boundary. No two aggregates share the same invariant rules.

  2. Design event schemas
  Define immutable event types with all data needed to reconstruct state. Use past-tense naming. Include metadata (event ID, timestamp, aggregate ID, version). [src2]
  {
    "event_type": "OrderCreated",
    "event_id": "uuid-v4",
    "aggregate_id": "order-123",
    "aggregate_type": "Order",
    "version": 1,
    "timestamp": "2026-02-23T10:00:00Z",
    "data": {
        "customer_id": "cust-456",
        "items": [{"product_id": "prod-789", "quantity": 2, "price": 29.99}],
        "total": 59.98
    },
    "metadata": {
        "correlation_id": "req-abc",
        "causation_id": null,
        "user_id": "user-001"
    }
}
  Verify: Every event contains enough data to rebuild aggregate state without external lookups.

  3. Implement the event store
  Choose your storage backend. The event store must support append-only writes, optimistic concurrency control (expected version), and reading all events for a given aggregate. [src6]
  -- PostgreSQL event store table
CREATE TABLE events (
    event_id       UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    aggregate_id   UUID NOT NULL,
    aggregate_type VARCHAR(100) NOT NULL,
    event_type     VARCHAR(100) NOT NULL,
    version        INTEGER NOT NULL,
    data           JSONB NOT NULL,
    metadata       JSONB DEFAULT '{}',
    created_at     TIMESTAMPTZ DEFAULT NOW(),
    UNIQUE (aggregate_id, version)  -- optimistic concurrency
);

CREATE INDEX idx_events_aggregate ON events (aggregate_id, version);
CREATE INDEX idx_events_type ON events (event_type, created_at);
  Verify: INSERT with duplicate (aggregate_id, version) raises a unique constraint violation (concurrency conflict).

  4. Build the command side (write model)
  Implement command handlers that load an aggregate from events, apply business rules, and append new events. Use optimistic concurrency to prevent conflicting writes. [src2]
  def handle_confirm_order(command):
    # 1. Load events from store
    events = event_store.load(command.order_id)
    # 2. Rebuild aggregate state
    order = Order()
    for event in events:
        order.apply(event)
    # 3. Execute business logic
    new_events = order.confirm()  # raises if invalid state
    # 4. Persist new events (with expected version)
    event_store.append(
        aggregate_id=command.order_id,
        events=new_events,
        expected_version=order.version
    )
    # 5. Publish events to bus
    event_bus.publish(new_events)
  Verify: Sending the same command twice with the same expected version raises a concurrency error on the second attempt.

  5. Build projections (read model)
  Create event handlers that subscribe to events and update denormalized read models optimized for specific queries. Each projection is independent and rebuildable. [src3]
  // Projection: Order Summary
async function handleEvent(event) {
    switch (event.event_type) {
        case 'OrderCreated':
            await db.query(
                `INSERT INTO order_summary (order_id, customer_id, status, total, created_at)
                 VALUES ($1, $2, 'pending', $3, $4)`,
                [event.aggregate_id, event.data.customer_id,
                 event.data.total, event.timestamp]
            );
            break;
        case 'OrderConfirmed':
            await db.query(
                `UPDATE order_summary SET status = 'confirmed', confirmed_at = $2
                 WHERE order_id = $1`,
                [event.aggregate_id, event.timestamp]
            );
            break;
    }
}
  Verify: After publishing events, query the read model and confirm the projected data matches expected state.

  6. Handle eventual consistency in the UI
  The read model lags behind the write model. Implement strategies: return the command result with the new version, poll until the projection catches up, or use client-side optimistic updates. [src3]
  Strategies for eventual consistency:
1. Return write version → client polls read model until version >= write version
2. Causal consistency token → pass correlation ID, read model waits for that event
3. Optimistic UI → update client state immediately, reconcile when projection arrives
4. Read-your-writes → route reads to write model for N seconds after a command
  Verify: After a write, the UI shows updated data within the acceptable latency window (typically < 500ms for interactive UIs).

  7. Add snapshotting for large aggregates
  When aggregates accumulate hundreds of events, replaying from the beginning becomes slow. Snapshot aggregate state periodically and load from the latest snapshot plus subsequent events. [src6]
  def load_aggregate(aggregate_id):
    snapshot = snapshot_store.get_latest(aggregate_id)
    if snapshot:
        aggregate = deserialize(snapshot.state)
        events = event_store.load(aggregate_id,
                                  from_version=snapshot.version + 1)
    else:
        aggregate = Order()
        events = event_store.load(aggregate_id)
    for event in events:
        aggregate.apply(event)
    return aggregate
  Verify: Load time for an aggregate with 10,000 events + snapshotting should be < 50ms.
  

  
  
  Code Examples

  Python: Simple Event Store with Optimistic Concurrency
  // Input:  Domain events as dictionaries with aggregate_id and version
// Output: Persisted events with concurrency protection

import json
import uuid
from datetime import datetime, timezone
import psycopg2
from psycopg2.extras import RealDictCursor

class EventStore:
    """Append-only event store with optimistic concurrency control."""

    def __init__(self, dsn: str):
        self.conn = psycopg2.connect(dsn)

    def append(self, aggregate_id: str, events: list[dict],
               expected_version: int) -> None:
        """Append events. Raises on version conflict."""
        with self.conn.cursor() as cur:
            for i, event in enumerate(events):
                version = expected_version + i + 1
                cur.execute(
                    """INSERT INTO events
                       (event_id, aggregate_id, aggregate_type,
                        event_type, version, data, metadata)
                       VALUES (%s, %s, %s, %s, %s, %s, %s)""",
                    (str(uuid.uuid4()), aggregate_id,
                     event["aggregate_type"], event["event_type"],
                     version, json.dumps(event["data"]),
                     json.dumps(event.get("metadata", {})))
                )
        self.conn.commit()

    def load(self, aggregate_id: str,
             from_version: int = 0) -> list[dict]:
        """Load events for an aggregate, optionally from a version."""
        with self.conn.cursor(cursor_factory=RealDictCursor) as cur:
            cur.execute(
                """SELECT * FROM events
                   WHERE aggregate_id = %s AND version > %s
                   ORDER BY version""",
                (aggregate_id, from_version)
            )
            return cur.fetchall()

  Node.js: Read Model Projection with Event Subscription
  // Input:  Events from an event store (EventStoreDB or Kafka)
// Output: Denormalized read model in PostgreSQL

const { Pool } = require('pg');  // [email protected]
const pool = new Pool({ connectionString: process.env.DATABASE_URL });

const projectionHandlers = {
  async OrderCreated(event) {
    const { customer_id, items, total } = event.data;
    await pool.query(
      `INSERT INTO order_summary
       (order_id, customer_id, status, item_count, total, created_at, updated_at)
       VALUES ($1, $2, 'pending', $3, $4, $5, $5)
       ON CONFLICT (order_id) DO NOTHING`,
      [event.aggregate_id, customer_id, items.length, total, event.timestamp]
    );
  },

  async OrderConfirmed(event) {
    await pool.query(
      `UPDATE order_summary
       SET status = 'confirmed', updated_at = $2
       WHERE order_id = $1`,
      [event.aggregate_id, event.timestamp]
    );
  },

  async OrderShipped(event) {
    const { tracking_number, carrier } = event.data;
    await pool.query(
      `UPDATE order_summary
       SET status = 'shipped', tracking_number = $2,
           carrier = $3, updated_at = $4
       WHERE order_id = $1`,
      [event.aggregate_id, tracking_number, carrier, event.timestamp]
    );
  }
};

// Projection runner with checkpoint tracking
async function runProjection(eventStore, projectionName) {
  const checkpoint = await loadCheckpoint(projectionName);
  let position = checkpoint;
  const stream = eventStore.subscribeToAll({ fromPosition: position });
  for await (const resolvedEvent of stream) {
    const event = resolvedEvent.event;
    const handler = projectionHandlers[event.event_type];
    if (handler) {
      try {
        await handler(event);
        await saveCheckpoint(projectionName, resolvedEvent.position);
      } catch (err) {
        console.error(`Projection error:`, err);
        await sendToDeadLetter(projectionName, resolvedEvent, err);
      }
    }
  }
}
  

  
  
  Anti-Patterns

  Wrong: Mutable events in the store
  # BAD — modifying events after they are stored destroys the audit trail
def fix_order_total(order_id, correct_total):
    event = event_store.load_latest(order_id, "OrderCreated")
    event["data"]["total"] = correct_total  # MUTATING a stored event
    event_store.update(event)               # UPDATING instead of appending

  Correct: Use compensating events
  # GOOD — append a correction event; the original event remains immutable
def fix_order_total(order_id, correct_total, reason):
    event_store.append(order_id, [{
        "aggregate_type": "Order",
        "event_type": "OrderTotalCorrected",
        "data": {
            "previous_total": order.total,
            "corrected_total": correct_total,
            "reason": reason
        }
    }], expected_version=order.version)

  Wrong: One giant read model serving all queries
  -- BAD — single normalized read model defeats the purpose of CQRS
SELECT o.id, o.status, c.name, c.email,
       SUM(oi.price * oi.quantity) as total,
       s.tracking_number, s.carrier
FROM orders o
JOIN customers c ON o.customer_id = c.id
JOIN order_items oi ON o.id = oi.order_id
LEFT JOIN shipments s ON o.id = s.order_id
WHERE o.status = 'shipped'
GROUP BY o.id, o.status, c.name, c.email, s.tracking_number, s.carrier;

  Correct: Purpose-built denormalized projections
  -- GOOD — each projection is a flat, denormalized table for one use case
CREATE TABLE shipped_orders_view (
    order_id UUID PRIMARY KEY,
    customer_name TEXT,
    customer_email TEXT,
    item_count INTEGER,
    total DECIMAL(10,2),
    tracking_number TEXT,
    carrier TEXT,
    shipped_at TIMESTAMPTZ
);
-- Single-row lookup, no JOINs
SELECT * FROM shipped_orders_view WHERE order_id = $1;

  Wrong: Exposing domain events to external services
  # BAD — internal domain events leak aggregate internals to consumers
event_bus.publish_to_external("order-events-topic", domain_event)
# External consumers now depend on internal event schemas

  Correct: Translate domain events to integration events
  # GOOD — publish stable integration events for external consumers
def on_order_confirmed(domain_event):
    integration_event = {
        "type": "OrderConfirmedV1",
        "order_id": domain_event.data["order_id"],
        "total": domain_event.data["total"],
        "confirmed_at": domain_event.timestamp
    }
    integration_bus.publish("public.orders", integration_event)

  Wrong: Validating commands against the read model
  # BAD — read model may be stale; business rules must use the write model
async def handle_confirm_order(command):
    order_summary = await read_db.query(
        "SELECT status FROM order_summary WHERE order_id = $1",
        command.order_id
    )
    if order_summary.status == "pending":  # STALE data!
        event_store.append(...)

  Correct: Load aggregate from events for validation
  # GOOD — always load from the event store for write-side decisions
async def handle_confirm_order(command):
    events = event_store.load(command.order_id)
    order = Order()
    for event in events:
        order.apply(event)
    new_events = order.confirm()  # raises if not in valid state
    event_store.append(command.order_id, new_events, order.version)
  

  
  
  Common Pitfalls
  
    Eventual consistency surprises: Users submit a command, then immediately query the read model and see stale data. Fix: implement read-your-writes consistency via polling with version check or causal consistency tokens. [src3]
    Event schema evolution breaks projections: Adding/removing fields in events without versioning causes deserialization failures. Fix: use a schema registry (Confluent Schema Registry, Avro, Protobuf) with backward-compatible changes only. [src7]
    Unbounded event streams per aggregate: Aggregates with millions of events become impossibly slow to rebuild. Fix: snapshot every 100-500 events, or redesign aggregate boundaries. [src6]
    Projection replay takes hours: Rebuilding a projection from scratch is slow with millions of events. Fix: parallel projection with partitioned consumers and checkpoint-based resumption. [src7]
    Missing idempotency in projections: Network retries cause duplicate event delivery, leading to double-counted data. Fix: track last processed event ID per projection and skip duplicates. [src5]
    Treating CQRS as all-or-nothing: Applying CQRS + ES to the entire application instead of specific bounded contexts. Fix: start with one complex domain, keep the rest as simple CRUD. [src1]
    No dead letter queue for failed projections: A single bad event halts the entire projection pipeline. Fix: route failed events to a DLQ, alert on queue depth, continue processing. [src4]
    Synchronous projections in the command path: Updating projections inside the command handler creates tight coupling. Fix: project asynchronously via event subscriptions; accept eventual consistency. [src2]
  
  

  
  
  Diagnostic Commands
  # Check EventStoreDB health (Docker)
curl -s http://localhost:2113/health/live | jq .

# Count events per aggregate in PostgreSQL event store
psql -c "SELECT aggregate_id, COUNT(*) as event_count, MAX(version) as latest_version FROM events GROUP BY aggregate_id ORDER BY event_count DESC LIMIT 20;"

# Check projection lag
psql -c "SELECT e.max_id AS latest_event, p.position AS projected_to, (e.max_id - p.position) AS lag FROM (SELECT MAX(event_id) as max_id FROM events) e, projection_checkpoints p WHERE p.name = 'order_summary';"

# Monitor dead letter queue depth
psql -c "SELECT projection, COUNT(*) as failed_count, MIN(created_at) as oldest_failure FROM dead_letter_queue GROUP BY projection;"

# Check EventStoreDB stream stats
curl -s http://localhost:2113/streams/\$stats-0 -H "Accept: application/json" | jq .
  

  
  
  Version History & Compatibility
  
  
    
      Pattern / ToolStatusBreaking ChangesNotes
    
    
      CQRS (pattern)Stable since 2010NoneCoined by Greg Young, based on CQS (Bertrand Meyer)
      Event Sourcing (pattern)Stable since 2005NonePopularized by Greg Young and Martin Fowler
      EventStoreDB 24.xCurrentgRPC API is primary (HTTP deprecated)Docker: eventstore/eventstore:24.10
      EventStoreDB 23.xMaintained---Last version with full HTTP API
      Axon Framework 4.xCurrent (LTS)Axon Server required for clusteringJava 17+ required since 4.8
      Marten 7.xCurrentPostgreSQL 12+ required.NET 8+ required
      Kafka (as event store)Usable with caveatsNot a true event storeUse log compaction OFF for event streams
    
  
  
  

  
  
  When to Use / When Not to Use
  
  
    
      Use WhenDon't Use WhenUse Instead
    
    
      Read and write workloads have vastly different scaling requirementsSimple CRUD with balanced read/write loadStandard three-tier architecture
      You need a complete, immutable audit trail of all state changesAudit requirements satisfied by database change logsCDC (Change Data Capture) with Debezium
      Domain has complex business rules that benefit from DDD aggregatesDomain is anemic with simple property updatesActive Record or Repository pattern
      Multiple read models needed for different query patternsSingle query pattern serves all use casesStandard database views or materialized views
      You need temporal queries ("what was the state at time T?")Only current state mattersStandard database with updated_at timestamps
      High-throughput write workload benefiting from append-only storageWrite volume is moderate and relational ACID is sufficientPostgreSQL with proper indexing
      Microservices need to react to domain events across boundariesMonolithic application with shared databaseDatabase triggers or application events
    
  
  
  

  
  
  Important Caveats
  
    CQRS and Event Sourcing are independent patterns that complement each other but do not require each other. You can use CQRS with a traditional database, or event sourcing without CQRS (though this is rare). [src1]
    Martin Fowler explicitly warns: "the majority of cases I've run into have not been so good, with CQRS seen as a significant force for getting a software system into serious difficulties." Only apply where complexity is justified. [src1]
    Eventual consistency between write and read models is inherent. There will always be a window (typically milliseconds to seconds) where the read model is behind. Design your UX around this reality. [src3]
    Event versioning and schema evolution become critical at scale. Plan for upcasting (transforming old event formats to new ones during replay) from day one. [src2]
    GDPR and data deletion requirements conflict with immutable event stores. Solutions include crypto-shredding (encrypting PII with per-user keys, then deleting the key) or event store compaction with redaction. [src4]
  
  

  
  

  