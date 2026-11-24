# Core

> Status: planned

## Purpose

Shared building blocks used by every layer. Depends on nothing else in the project.

## Contents (planned)

- **Config**: settings with sensible defaults (timeouts, TTL, base URL). Environment variables optional, no secrets required.
- **Logging**: structured JSON to stderr.
- **Errors**: typed exceptions shared across layers (for example `UpstreamUnavailable`, `RegionNotFound`, `SchemaChanged`).
- **Models**: small typed result objects carrying value plus metadata (source, period, last updated, stale flag).

## Rule

No layer imports from a layer above it. Core imports from none of them.
