# Resilience Patterns

> Build systems that degrade gracefully under partial failure. (See also `knowledge-base.md` §4 for distributed systems fundamentals.)

---

## 1. The Reality

In any non-trivial distributed system: dependencies fail, networks partition, queues back up, deploys go bad, clouds hiccup. Resilience is **designing for these as the normal case**, not the exception.

The two foundations:

- **Bulkheads** — failure isolation
- **Backpressure** — controlled load shedding

Everything else is implementation detail.

---

## 2. Core Patterns

| Pattern | Purpose |
|---------|---------|
| **Timeout** | Give up before the caller does |
| **Retry (with jitter + backoff)** | Survive transient errors |
| **Circuit Breaker** | Stop hammering a sick dependency |
| **Bulkhead** | Isolate resource pools per dependency |
| **Rate Limit** | Protect yourself + downstreams |
| **Throttling / Load Shedding** | Drop low-priority work first |
| **Fallback** | Return a degraded but safe answer |
| **Cache (with TTL)** | Reduce dependency on dependency |
| **Idempotency** | Safe retries |
| **Saga / Compensation** | Recover from partial multi-step failure |

Default libraries: Resilience4j (JVM), Polly (.NET), failsafe-go, Tenacity (Py), Hystrix (deprecated).

---

## 3. The Golden Defaults

| Knob | Default |
|------|---------|
| Connect timeout | 1–2 s |
| Read timeout | request budget − overhead |
| Retry attempts | 3 |
| Retry backoff | exponential + full jitter |
| Circuit breaker opens after | 5–10 failures in 30 s |
| Half-open probe | every 30 s |

Tune from real metrics, not gut feel.

---

## 4. Don't Retry These

- Non-idempotent writes without idempotency key
- 4xx client errors (except 408, 425, 429)
- Auth failures
- Validation errors

Retry **only** what you can prove is safe.

---

## 5. Caching Patterns

| Pattern | Use |
|---------|-----|
| Read-through | Cache miss triggers fetch & store |
| Write-through | Update cache + DB synchronously |
| Write-behind | Buffer writes; risk on crash |
| Cache-aside | App manages it; most common |
| Negative caching | Cache "not found" briefly |

Add: **stale-while-revalidate** to keep serving while refreshing async.

Watch out for thundering herd → use single-flight / request coalescing.

---

## 6. Queues, Backpressure, and Poison

- Bounded queues; reject when full
- Visible queue depth metrics
- DLQ for messages that fail max retries
- Per-tenant fairness when multi-tenant
- Replay tooling for DLQ → recovery

Unbounded queues = unbounded latency = invisible failure.

---

## 7. Dependency Topology

Minimize **cross-service synchronous depth**. A request that touches 8 services synchronously has the failure surface of all 8 multiplied.

Aim for ≤ 3 sync hops; convert deeper paths to async events.

---

## 8. Multi-Region Resilience

| Pattern | Trade-off |
|---------|-----------|
| Active/passive | Simpler, RTO minutes |
| Active/active read | Reads scale; writes still single-region |
| Active/active write (CRDTs / quorum) | Complex; latency penalty |

Test region failover at least annually. Document RPO/RTO per service.

---

## 9. Chaos Engineering (See DevOps & SRE)

Hypothesis-driven failure injection:

- Latency, error injection (Toxiproxy, Litmus, Gremlin)
- Pod / instance termination
- Region isolation
- Dependency outages

Run game days quarterly; promote to scheduled chaos when mature.

---

## 10. Anti-Patterns

- Retries on retries (call graph multiplies attempts exponentially)
- No timeouts (the silent killer)
- Circuit breakers without fallbacks
- Fallbacks that secretly return wrong data
- Single-region "high-availability"

---

## 11. Resilience Checklist (per service)

- [ ] Timeouts on every external call
- [ ] Bounded retries with jitter
- [ ] Circuit breaker on flaky deps
- [ ] Bulkheads per dependency
- [ ] DLQ + replay for async consumers
- [ ] Idempotency on all mutating endpoints
- [ ] Documented RTO/RPO
- [ ] DR test in last 12 months
- [ ] Chaos test in CI or scheduled

---

## 12. References

- [DevOps & SRE](./devops-and-sre.md)
- [Observability](./observability.md)
- [Knowledge Base — Distributed Systems Fundamentals](./knowledge-base.md)
