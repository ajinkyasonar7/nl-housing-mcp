# Design: requirements and altitude

This follows the "clarify before you design" approach for system design. The answers below are **proposed** by the author, since there is no external stakeholder. They are the design pillars for everything else.

## Altitude

High-level design (components, data flow, failure handling) for the whole system. Low-level design only for the data client and service layers, where the interesting logic lives.

## The five questions

| Question | Proposed answer | What it drives |
|---|---|---|
| **Scale and growth** | Today: one developer and a few demo users. Stated "what if": about 10,000 users via AI assistants on a remote server. The real bottleneck is CBS, the upstream source, not our compute. | Cache hit rate matters more than raw throughput. |
| **Read vs. write** | Essentially 100 percent reads. Users never write. CBS updates monthly or quarterly. | Aggressive caching is justified. No database in the MVP. |
| **Data durability** | Nothing we hold is irreplaceable: CBS is the source of truth. A lost cache only means a slow first request. What is critical is **trust**: never return a wrong or silently stale number. | Every response carries table ID, period and last-updated date. |
| **Latency** | Targets (to be measured, not facts): cached under about 50 ms, cold fetch under about 2 s, hard timeout at about 5 s with a clear error. | Timeouts and a cache. |
| **Cost** | Zero euros. | Local stdio transport, free CBS API, public GitHub with free Actions, no always-on hosting. Trade-off: not live 24/7. |

## Thinking in failures

Each component exists because of a failure it prevents.

| Failure | Component that earns its place |
|---|---|
| CBS is slow or down | Timeout, retry with backoff, stale-cache fallback clearly flagged as stale |
| CBS changes a table or field | Data client is the only layer that knows CBS, plus contract tests |
| AI sends a bad or ambiguous region name | Input validation and an error that lists close matches |
| Response too large for the AI's context | Limits and summarisation in the service layer |
| Too many requests hit CBS | Cache and a rate limiter |

## Deliberately absent

No Redis, load balancer or database. At POC scale they would solve no failure. They appear in [scaling.md](scaling.md) as the next steps, each tied to the failure it would address.

## Open questions

- Does CBS publish usage limits for its OData API? (Not yet verified.)
- Which exact StatLine tables and fields do we use? (Must be verified with a real call before coding.)
