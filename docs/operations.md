# Operations

> Status: planned.

## Observability

- **Structured logging** (JSON lines to stderr). Over stdio transport, stdout belongs to the MCP protocol, so logs must never go to stdout.
- Log per request: tool name, duration, cache hit or miss, upstream status, error type. No personal data.
- **Health check**: a lightweight function or tool that reports server version and whether CBS is reachable.

## Testing in CI

- Lint and format with ruff.
- Unit tests for services, cache and validation.
- Data client tested against recorded or mocked CBS responses so CI never depends on the live API.
- One optional, separately-marked live test to detect upstream changes.

## Docker

- Small multi-stage image, non-root user, pinned dependencies.
- Docker Desktop is free for personal use and open source projects. Check current terms before relying on this.

## GitHub Actions

- Workflow: lint, test, build image on every push and pull request.
- Optional: publish the image to GitHub Container Registry for the public repo.
- Free for public repositories. Check current terms.

## Not in the POC

Metrics, tracing, alerting and hosted deployment. See [scaling.md](scaling.md).
