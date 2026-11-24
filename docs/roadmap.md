# Roadmap

## Phase 0: design (current)

- [x] Repo created, MIT licence
- [x] Requirements and failure analysis (docs/design.md)
- [x] Architecture and component docs
- [ ] Review and curate these docs

## Phase 1: verify the data

- [ ] Make one real call to the CBS OData API and record the response shape
- [ ] Confirm exact table IDs and fields for price index and regional prices
- [ ] Check whether CBS publishes usage limits

## Phase 2: build the layers

- [ ] Project setup (pyproject, ruff, pytest)
- [ ] CBS data client with timeouts and retries
- [ ] Cache with TTL and stale fallback
- [ ] Service layer
- [ ] MCP layer with the first tool
- [ ] Remaining tools

## Phase 3: production-shaped polish

- [ ] Structured logging and health check
- [ ] Dockerfile
- [ ] GitHub Actions CI
- [ ] README demo (screen recording or GIF)

## Phase 4: optional

- [ ] Second data source (for example grid congestion, if a clean free download exists)
- [ ] Remote HTTP transport
