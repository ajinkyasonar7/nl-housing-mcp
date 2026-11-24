# Service layer

> Status: planned

## Purpose

Business logic: turn raw observations into answers a person can use.

## Responsibilities

- Trend calculation (for example year-on-year change).
- Region comparison.
- Plain-language summaries.
- Enforce response size limits so results fit in an AI context window.
- Always attach metadata: source table ID, period, last-updated date, and a stale flag when relevant.

## Does not

- Know about HTTP, OData or CBS field names.
- Know about the MCP protocol.

## Failure modes handled

- Missing data for a period: say so explicitly instead of guessing.
- Partial results: return what exists with a clear note.

## Tests

Pure unit tests with a fake data client and fake cache. No network.
