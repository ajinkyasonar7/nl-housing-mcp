# Test strategy

> Status: planned

| Level | Scope | Network |
|---|---|---|
| Unit | Services, cache, validation | None |
| Client | CBS client against recorded or mocked responses | None |
| Tool | MCP tool schemas and error messages | None |
| Live (optional) | One real call to CBS to detect upstream changes | Yes, marked and excluded from default CI |

## Principles

- CI must never depend on the live CBS API.
- Use a controllable clock for cache tests.
- Each failure listed in docs/design.md should have at least one test.
