# ADR 0001: Use CBS open data as the source

**Status:** Proposed

## Context

The project must be free and reliable. Live rental listing APIs found during research were commercial (for example one charged a monthly fee and required a payment method).

## Decision

Use the CBS StatLine open data (OData) API, which is public and needs no registration or key.

## Consequences

- Good: free, official, regularly updated, long history.
- Good: no secrets to manage.
- Bad: statistics only, not live listings.
- To verify: exact tables, fields and any usage limits.
