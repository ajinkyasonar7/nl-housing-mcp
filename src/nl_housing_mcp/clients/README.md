# CBS data client

> Status: planned

## Purpose

The only place that knows about CBS StatLine. Fetches data over the OData API and returns a clean internal model.

## Responsibilities

- Build requests: table IDs, filters, pagination.
- Timeouts and retries with backoff.
- Map cryptic CBS fields and codes to readable internal names.
- Raise typed errors (upstream unavailable, unexpected schema, not found).

## Does not

- Cache (that is the cache layer's job).
- Do business calculations.

## Failure modes handled

- CBS slow or down: timeout, retry, then raise a typed error.
- Schema change: detect and fail loudly rather than return wrong numbers.

## Open questions

- Exact tables and fields (must be verified with a real call).
- Whether CBS documents usage limits.

## Tests

Recorded or mocked responses in CI. One optional live test, clearly marked, to catch upstream changes.
