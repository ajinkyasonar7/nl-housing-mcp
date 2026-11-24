# Scaling: from POC to production

These are **described, not built**. Each item is tied to the failure it would address, so nothing is added just to collect components.

| Next step | Failure or limit it addresses | Trade-off |
|---|---|---|
| Shared cache (e.g. Redis) | Several server instances each keep their own cache, multiplying load on CBS | Another service to run and secure |
| Remote HTTP transport instead of local stdio | Users cannot all run the server locally | Adds auth, TLS and abuse protection |
| Stateless horizontal scaling behind a load balancer | One instance cannot serve many concurrent users | Needs the shared cache first |
| Scheduled cache refresh | Cold requests are slow right after CBS publishes new data | Extra moving part and a job to monitor |
| Metrics and tracing | Cannot see slow tools or rising error rates | Setup and storage cost |
| Rate limiting per client | One heavy client could exhaust the upstream budget | Needs client identity |
| Versioned tool schemas | Changing a tool breaks existing assistants | Maintenance burden |

## Why the bottleneck is CBS, not us

The workload is read-only and the data changes monthly or quarterly. Most requests can be answered from cache, so the limiting factor is how politely we treat the upstream API, not our own CPU.
