# DevOps & SRE

> The operating model that keeps shipped software working — and the metrics that prove it.

---

## 1. The Three Ways (Phoenix / DevOps Handbook)

1. **Flow** — fast, smooth, left-to-right work
2. **Feedback** — fast, amplified, right-to-left signals
3. **Continuous learning** — experimentation, failure as fuel

Every DevOps practice maps to one of these. Use it to diagnose: which Way is broken?

---

## 2. DORA Metrics (Outcome)

| Metric | Elite | High | Medium | Low |
|--------|-------|------|--------|-----|
| Deployment Frequency | On demand | Daily–weekly | Weekly–monthly | Monthly–6m |
| Lead Time for Changes | < 1h | 1d–1w | 1w–1m | 1–6m |
| Change Failure Rate | 0–15% | 16–30% | 16–30% | 16–30%+ |
| MTTR | < 1h | < 1d | < 1w | 1w–1m |

Track per **service**, not org-wide average — averages hide reality.

---

## 3. SPACE Metrics (Behavioral)

DORA tells you outcomes; SPACE tells you why.

- **S**atisfaction & well-being
- **P**erformance (quality, customer satisfaction)
- **A**ctivity (PRs, deploys — caveat: not productivity)
- **C**ommunication & collaboration
- **E**fficiency & flow (interruptions, focus time)

---

## 4. SLI / SLO / SLA / Error Budgets

| Term | Definition |
|------|------------|
| SLI | A measured indicator (latency, success rate) |
| SLO | The internal target on an SLI (e.g., 99.9%) |
| SLA | Customer-facing contractual commitment |
| Error budget | 1 − SLO; the allowed unreliability |

Operating rule: **when error budget is exhausted, freeze risky changes** until reliability is restored.

Pick 2–4 SLIs per critical user journey; refuse SLO sprawl.

---

## 5. Incident Management

| Phase | Practice |
|-------|----------|
| Detect | Alerts on SLOs, not on every metric |
| Triage | Severity matrix, on-call rotation |
| Mitigate | Runbooks, feature flags, rollback |
| Resolve | Restore service, then root cause |
| Learn | Blameless postmortem within 5 business days |

Postmortem template:

- What happened (timeline)
- Impact (users, $, duration)
- Detection & response timing
- Contributing factors (multiple — never one root cause)
- Action items with owners and dates
- What went well

---

## 6. Progressive Delivery

- Feature flags (LaunchDarkly / Unleash / OpenFeature)
- Canary deployments (1% → 10% → 100%)
- Blue/green (instant cutover)
- Dark launches (run new code without exposing it)
- A/B testing (when measuring user impact)

GitHub Environments + rulesets enforce the gates between these stages.

---

## 7. Observability — The Three Pillars (and one)

| Pillar | Use |
|--------|-----|
| Metrics | Aggregated, low cardinality |
| Logs | High-context, structured |
| Traces | Cross-service request paths |
| (Profiles) | Hot-spot analysis |

Standard stack: **OpenTelemetry SDK** + collector → vendor backend (Datadog, New Relic, Honeycomb, Grafana).

Anti-pattern: alerting on logs when SLO-based metrics would do.

---

## 8. Chaos Engineering

Maturity ladder:

1. Game days (manual disruptions in non-prod)
2. Scheduled chaos in pre-prod
3. Continuous chaos in prod (Chaos Monkey-style)
4. Chaos in CI (resilience tests run on every PR)

Always: clear hypothesis, blast radius, abort criteria.

---

## 9. On-Call Practices

- Rotation: ≤ 1 week per engineer
- Healthy alerting: every page must be actionable, with a runbook link
- Post-rotation handoff doc
- Compensation / time-in-lieu policy
- Quarterly on-call retrospective

A page rate > 2/day is a signal of alert fatigue, not heroism.

---

## 10. Release Engineering

- Trunk-based development
- Short-lived branches (≤ 2 days)
- Feature flags hide unfinished work
- Every commit to main is potentially shippable
- Versioning: SemVer for libs, calendar / build-number for apps

---

## 11. Anti-Patterns

- DevOps team that gates all deploys
- DORA reported as org-wide average
- SLOs invented in a vacuum, not from user needs
- Postmortems as performance reviews
- "We don't need feature flags, we have CI"

---

## 12. References

- [Platform Engineering](./platform-engineering.md)
- [Resilience Patterns](./resilience-patterns.md)
- [Observability](./observability.md)
