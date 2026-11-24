# Architecture

> Status: planned. Nothing here is implemented yet.

## Layers

```mermaid
flowchart TB
    subgraph Client["MCP client"]
        A["Claude Desktop / Cursor"]
    end
    subgraph Server["nl-housing-mcp"]
        B["MCP layer\n(tool definitions, input validation)"]
        C["Service layer\n(business logic)"]
        D["Cache\n(TTL, in-memory)"]
        E["CBS data client\n(HTTP, retries, timeouts)"]
        X["Core\n(config, logging, errors)"]
    end
    F[("CBS StatLine\nOData API")]
    A -->|"stdio"| B
    B --> C
    C --> D
    D -.->|"miss"| E
    E -->|"HTTPS"| F
    X -.-> B
    X -.-> C
    X -.-> E
```

## Responsibilities

| Layer | Owns | Must not |
|---|---|---|
| MCP layer | Tool names, schemas, input validation, turning errors into readable messages | Contain business logic or call CBS |
| Service layer | Comparisons, trends, plain-language summaries, response size limits | Know HTTP or OData details |
| Cache | TTL storage, stale-while-error behaviour | Decide what is business-correct |
| CBS data client | All knowledge of CBS: URLs, table IDs, field names, retries, timeouts | Leak CBS field names upward |
| Core | Settings, logging, shared error types | Depend on other layers |

Dependency direction is one-way: MCP layer, then services, then cache and client. Core is used by all.

## Request flow

```mermaid
sequenceDiagram
    participant AI as AI assistant
    participant M as MCP layer
    participant S as Service layer
    participant C as Cache
    participant D as CBS client
    participant CBS as CBS StatLine
    AI->>M: compare_regions("Utrecht", "Eindhoven")
    M->>M: validate input
    M->>S: compare(region_a, region_b)
    S->>C: get(table, region)
    alt cache hit
        C-->>S: data + metadata
    else cache miss
        C->>D: fetch(table, region)
        D->>CBS: GET (timeout, retry)
        CBS-->>D: observations
        D-->>C: normalised data
        C-->>S: data + metadata
    end
    S-->>M: result + source, period, last_updated
    M-->>AI: readable answer
```

## Trust rule

Every result includes the **source table ID, the period covered and the last-updated date**. If data is served from a stale cache because CBS failed, the response says so explicitly.

## Related

- [design.md](design.md): requirements behind these choices
- [decisions/](decisions/): why each major choice was made
