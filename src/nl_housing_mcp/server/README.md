# MCP layer

> Status: planned

## Purpose

The only layer that speaks the MCP protocol. Declares tools, validates input and turns results and errors into readable responses.

## Responsibilities

- Define tools and their JSON schemas: `get_price_trend`, `compare_regions`, `get_market_headline`, `list_datasets`, `explain_dataset`.
- Validate and normalise input (region names, periods) before calling services.
- Convert internal errors into clear messages the assistant can relay.

## Does not

- Contain business logic.
- Call CBS or touch the cache directly.

## Failure modes handled

- Unknown or ambiguous region: return an error with close matches.
- Invalid period: explain the accepted format.
- Upstream failure: friendly message, never a stack trace.

## Notes

- Over stdio, never print to stdout. Logs go to stderr.
- Tool descriptions are part of the product: they are what the AI reads to decide when to call a tool.

## Tests

Tool schema tests and input validation tests. Services are mocked.
