---
type: source
created: '2026-05-06'
updated: '2026-05-06'
source_path: 'raw/repos/etcd-io-etcd.md'
source_date: ''
source_author: etcd / CNCF
tags:
  - source
  - etcd
  - raft
  - distributed-systems
  - consensus
  - kubernetes
  - key-value-store
  - cncf
  - service-discovery
  - cp-system
---
# etcd Distributed Key-Value Store

CNCF-graduated distributed reliable key-value store providing strong consistency via Raft consensus; serves as Kubernetes' cluster state backing store.

## Summary

etcd is a distributed key-value store designed around a single principle: strong consistency is more important than availability. This CP positioning (in CAP terms) makes it appropriate for coordination tasks — cluster membership, leader election, distributed locking, service discovery — where returning stale or incorrect data would cause cascading failures. Raft consensus requires a majority quorum to commit writes, so writes block during network partitions rather than returning potentially inconsistent results.

The watch operation is etcd's most distinctive feature for distributed systems use: clients register watchers on specific keys or key prefixes and receive streaming notifications when values change. This enables reactive service discovery and configuration management without polling. The watch implementation is strongly consistent — a watcher will observe every change in order, never missing an update or observing updates out of order.

Leases provide TTL-based key expiry, enabling distributed locking patterns where a lock holder must periodically renew its lease or the lock expires automatically. Compare-and-swap transactions (multi-key, with preconditions) enable optimistic concurrency control for coordinating state changes across multiple keys atomically. Together these primitives cover most distributed coordination requirements.

## Key Arguments

- etcd prioritizes consistency over availability (CP in CAP); writes block during network partitions
- Raft consensus requires majority quorum for commits, providing linearizable read and write semantics
- Watch operations enable reactive, push-based service discovery and configuration without polling
- Leases provide distributed locking without external lock services via TTL-based key expiry
- Compare-and-swap transactions enable optimistic concurrency control across multiple keys atomically
- etcd's simplicity and strong consistency make it the coordination backbone for Kubernetes and many distributed systems

## Concepts Covered

- [[etcd]] — primary treatment
- [[Raft Consensus Algorithm]] — implementation; etcd is the reference Raft production system
- [[CAP Theorem]] — CP system example with concrete behavior description
- [[PACELC Theorem]] — PC/EC position (prefer consistency over latency during partitions)
- [[Service Mesh]] — service discovery backend used by many service mesh control planes

## Quality Notes

Official CNCF-graduated project documentation. Authoritative source for etcd capabilities, consistency guarantees, and operational characteristics. Widely deployed in production Kubernetes environments.
