# Copilot Metrics & Measurement

> How a senior consultant proves and grows ROI. Combines the **Copilot Metrics API**, **DORA / SPACE**, and **developer experience surveys**.

---

## 1. The Three Layers of Measurement

| Layer | Question | Source |
|-------|----------|--------|
| Activity | Are people using it? | Metrics API, seat usage |
| Behavior | Are workflows changing? | Repo events, PR data |
| Outcome | Is the business better off? | DORA, satisfaction, throughput |

A common mistake: stopping at Layer 1.

---

## 2. Copilot Metrics API

Endpoints (use the **org** or **enterprise** scope):

```bash
# Daily aggregated metrics (90d retention)
gh api /orgs/$ORG/copilot/metrics --paginate

# Team breakdown
gh api /orgs/$ORG/team/$SLUG/copilot/metrics --paginate

# Enterprise rollup
gh api /enterprises/$ENT/copilot/metrics --paginate
```

Returned dimensions (subset):

- Total active / engaged users
- IDE completions: shown, accepted, lines accepted
- IDE chats: turns, acceptance
- github.com chat usage
- Pull request summaries / Code Review usage
- Per-language and per-editor breakdowns

### Nuance

- "Acceptance" is a **proxy**, not truth — measure outcomes too
- A drop in acceptance can mean Copilot got smarter (fewer redundant suggestions) — investigate before alarming
- Compare cohorts (Copilot vs. control) when possible during rollout

---

## 3. DORA Metrics (the Outcome Layer)

| Metric | Definition | Tooling |
|--------|------------|---------|
| Deployment Frequency | Deploys / day per service | Actions deployment events |
| Lead Time for Changes | First commit → prod | PR + deploy joins |
| Change Failure Rate | % deploys causing incident | Incident system + deploy events |
| MTTR | Time to restore | Incident system |

Use the **GitHub Insights** dashboard or build a warehouse pipeline (Webhook → Event Hub → BigQuery / Snowflake).

Look for **inflection** at Copilot rollout, not absolute numbers.

---

## 4. SPACE Framework

Beyond DORA, measure developer impact across:

- **S**atisfaction & well-being
- **P**erformance
- **A**ctivity
- **C**ommunication & collaboration
- **E**fficiency & flow

Survey cadence: quarterly, anonymous, ≤ 8 questions, with a free-text "what would you change about Copilot?".

---

## 5. Cohort & A/B Methodology

When deploying Copilot to a regulated customer:

1. Pick a **pilot cohort** (60–120 devs) and a comparable **control cohort**
2. Baseline 30 days of DORA + survey
3. Enable Copilot for pilot only
4. Measure 30 / 60 / 90 days
5. Roll out broadly when uplift is statistically meaningful

---

## 6. Reporting Cadence

| Audience | Cadence | Content |
|----------|---------|---------|
| Engineering leaders | Weekly | Active usage, acceptance trend, top blockers |
| VPE / CTO | Monthly | DORA delta, satisfaction, cost vs. value |
| Procurement / FinOps | Quarterly | Seat utilization, reclaim list, cost/dev/month |
| Board / Sponsor | Quarterly | Outcome story with 1–2 customer quotes |

---

## 7. Sample Reclaim Query

```bash
# List inactive seats (pseudocode using API)
gh api /orgs/$ORG/copilot/billing/seats --paginate \
  | jq '.seats[] | select(.last_activity_at == null or
                         (.last_activity_at | fromdate) < (now - 30*24*3600))
        | .assignee.login'
```

Run monthly. Reassign or remove.

---

## 8. ROI Modeling

A defensible model:

```
Annual value =
  (avg developer fully-loaded cost / hour)
  × (mean hours saved / dev / week)
  × 50 weeks
  × #active devs
  × confidence factor (0.5–0.8)
```

Source `hours saved` from your survey (typical reported range: 2–6 h/week). Always present **net of license cost** and apply the confidence factor — skeptical CFOs are won by conservatism.

---

## 9. Common Pitfalls

- Measuring only acceptance → vanity metric
- Comparing across very different teams without normalization
- Ignoring context features (Knowledge Bases, Spaces) in attribution
- Stopping measurement after rollout — adoption is a continuous curve

---

## 10. References

- [Premium Requests & Models](./premium-requests-and-models.md)
- [Strategic Workflows](./strategic-workflows.md)
- [Adoption Rollout](./adoption-rollout.md)
