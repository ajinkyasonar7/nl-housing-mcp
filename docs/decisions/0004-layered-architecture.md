# ADR 0004: Use a strict layered architecture

**Status:** Proposed

## Context

CBS field names and table codes are cryptic and may change. Tool definitions should stay stable for AI assistants.

## Decision

Separate MCP layer, service layer, cache and CBS data client. Only the data client knows CBS. Dependencies point one way.

## Consequences

- Good: swapping or adding a data source touches one layer. Each layer is testable alone.
- Bad: more files and indirection than a single script. Justified here because testability and clarity are part of the goal.
