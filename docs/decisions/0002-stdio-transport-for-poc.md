# ADR 0002: Use stdio transport for the POC

**Status:** Proposed

## Context

The goal is a free, quick demo with Claude Desktop or another MCP client.

## Decision

Run the server locally over stdio. Describe remote HTTP transport as a future step.

## Consequences

- Good: zero hosting cost, no auth or TLS to build.
- Bad: not reachable by other users without running it themselves.
- Rule: stdout is reserved for the protocol, so all logs go to stderr.
