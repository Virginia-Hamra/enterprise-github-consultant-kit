# Observability

> "If you can't see it, you can't run it." OpenTelemetry-first, SLO-driven, signal-rich.

---

## 1. The Pillars (and the Goal)

The **goal** is not to collect telemetry — it's to **answer questions** about system behavior fast enough to act.

| Pillar | Best Question |
|--------|---------------|
| Metrics | "Is the system healthy right now?" |
| Logs | "What exactly happened in this request?" |
| Traces | "Where is the latency / failure?" |
| Profiles | "Why is this hot path slow?" |
| Events | "What changed?" (deploys, flags, config) |

A mature pipeline correlates all five via a common **trace ID**.

---

## 2. OpenTelemetry as the Default

Adopt **OpenTelemetry SDKs + Collector** for all new services. Benefits:

- Vendor-portability (swap Datadog ↔ Honeycomb ↔ Grafana without re-instrumenting)
- Single semantic conventions
- Auto-instrumentation for major frameworks

Collector deployment: **agent per node** + **gateway** for fan-out, sampling, redaction.

---

## 3. Metrics: RED + USE

| Method | Use |
|--------|-----|
| **RED** (Rate, Errors, Duration) | Request-driven services |
| **USE** (Utilization, Saturation, Errors) | Resources (CPU, disk, queues) |

Pair them. RED on the service, USE on its dependencies.

Cardinality discipline: every label is a budget item. Avoid user-id, request-id as labels.

---

## 4. Structured Logging

- JSON, single line per event
- Standard fields: `timestamp`, `level`, `service`, `env`, `trace_id`, `span_id`, `tenant_id`, `event`
- Levels used as designed: `error` actionable, `warn` notable, `info` business events, `debug` off in prod
- Sample debug logs; never sample errors
- Redact PII at source, not at the backend

---

## 5. Distributed Tracing

- W3C Trace Context propagation everywhere
- Sample intelligently: head-based for cost, **tail-based** for incidents
- Span names: `<verb> <resource>` (`POST /orders`, `db.query users`)
- Add attributes for `user.tier`, `feature.flag`, `region`, `tenant_id`
- Visualize service maps from traces

---

## 6. SLO-Based Alerting

Alert on **symptoms**, not causes:

- Page when SLO burn rate is high
- Multi-window, multi-burn-rate (Google SRE workbook)
- Avoid threshold alerts on infra metrics that don't map to user impact

Each alert must have:

- Severity
- Runbook link
- Owner team
- Expected response action

---

## 7. Continuous Profiling

Tools: Pyroscope, Parca, Datadog APM Continuous Profiler, Pixie.

Reveals what aggregate APM cannot: per-function CPU/memory contributions over time.

Default-on for hot paths and critical services.

---

## 8. Cost Control for Observability

Observability bills can rival cloud bills. Patterns:

- Tail-based sampling (keep all errors, sample successes)
- Tier metrics: hi-cardinality only for paid tiers / critical journeys
- Roll up old data (1-hour, then 1-day aggregates)
- Drop debug-level logs in prod via collector
- Right-size retention by signal

---

## 9. Incident-Time Workflow

```
Alert →  open incident channel  →  acknowledge
      →  pull dashboard (links in alert)
      →  trace ID → relevant traces
      →  diff with last 24h baseline
      →  feature flag / rollback if needed
      →  declare mitigated when SLO recovers
```

Every step should be one click from the alert.

---

## 10. Anti-Patterns

- Alerting on every metric "just in case"
- 50-graph dashboards no one reads
- Logs without structure
- Traces without context propagation across queues
- Observability owned by a central team only — services must own their signals

---

## 11. References

- [DevOps & SRE](./devops-and-sre.md)
- [Resilience Patterns](./resilience-patterns.md)
