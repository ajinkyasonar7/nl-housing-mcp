# Cache

> Status: planned

## Purpose

Protect CBS from repeated requests and keep answers fast. The data changes monthly or quarterly, so most requests can be served from memory.

## Responsibilities

- In-memory storage with a TTL.
- Serve stale entries, flagged as stale, if the upstream call fails.
- Expose hit and miss counts for logging.

## Does not

- Decide what is correct for the business.
- Share state between processes (see docs/scaling.md for Redis).

## Design notes

- Keys include table ID, region and period so different questions never collide.
- Initial TTL is a guess. Revisit once CBS update patterns are observed.

## Tests

TTL expiry, stale fallback, key collisions. Use a controllable clock, not real sleeps.
