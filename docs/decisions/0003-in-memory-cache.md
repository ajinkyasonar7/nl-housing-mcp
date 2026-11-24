# ADR 0003: Use an in-memory TTL cache

**Status:** Proposed

## Context

Workload is read-only and CBS data changes monthly or quarterly. The bottleneck is the upstream API.

## Decision

Use a simple in-process cache with a TTL. On upstream failure, serve stale data clearly flagged as stale. Redis is documented as a future step.

## Consequences

- Good: no extra service, easy to test.
- Bad: cache is lost on restart and not shared between instances.
- Bad: the TTL is a guess until we observe CBS update patterns.
