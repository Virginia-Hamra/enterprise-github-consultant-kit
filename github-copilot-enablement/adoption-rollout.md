# Copilot Adoption & Rollout Playbook

> The senior-consultant playbook for taking an enterprise from zero to mature Copilot adoption.

---

## 1. The 5-Phase Model

```
1. Discover  →  2. Pilot  →  3. Expand  →  4. Optimize  →  5. Sustain
   (4 wk)        (6–8 wk)     (8–12 wk)     (continuous)   (continuous)
```

Each phase has explicit **entry criteria**, **deliverables**, and **exit criteria** — refuse to skip.

---

## 2. Phase 1 — Discover

**Goal:** Confirm fit, identify risks, baseline metrics.

Activities:

- Stakeholder map: VPE, Security, Legal, Procurement, Platform, EM leads
- Identify regulatory constraints (sector, region, data residency)
- Inventory IDE + language landscape
- Baseline DORA + developer satisfaction survey
- Define success metrics (with the customer, not at them)

Deliverables:

- Engagement charter
- Risk register
- Success-metrics document
- SKU recommendation (Business vs Enterprise — see [licensing](./licensing-and-policies.md))

Exit criteria:

- Executive sponsor signed
- Pilot cohort + control cohort selected
- Legal & security pre-approval

---

## 3. Phase 2 — Pilot

**Goal:** Prove value with a controlled cohort.

Activities:

- Provision seats via SCIM groups
- Apply baseline policies (public-code block, content exclusions)
- Configure Copilot Code Review on 3–5 high-traffic repos
- Set up 1 Knowledge Base on architecture/runbooks
- Run weekly office hours
- Run a "prompt clinic" mid-pilot

Deliverables:

- Pilot policy baseline (versioned)
- Custom instructions for top repos
- Mid-pilot and end-of-pilot survey
- Metrics report (Activity + Behavior + early Outcome signals)

Exit criteria:

- ≥ 70% engaged users in cohort
- ≥ 25% acceptance rate (early indicator)
- Measurable lead-time or PR-throughput uplift
- No critical security findings

---

## 4. Phase 3 — Expand

**Goal:** Scale across the org with consistent quality.

Activities:

- Roll seat assignment automation (SCIM groups → seat)
- Standardize repo template with `.github/copilot-instructions.md`
- Org-level rulesets requiring instruction files in tier-1 repos
- Enable Copilot Coding Agent on selected repos
- Extend Knowledge Bases (per domain)
- Train **champions** (1 per ~50 devs)

Deliverables:

- Champions program charter
- Internal portal page with FAQs and quickstarts
- Adoption dashboard (Looker / Power BI / Datadog)
- Updated DORA baseline

Exit criteria:

- ≥ 60% enterprise-wide engaged users
- Champions network active in ≥ 80% of teams
- Coding Agent producing merged PRs on ≥ 5 repos

---

## 5. Phase 4 — Optimize

**Goal:** Lift from "used" to "transformative".

Activities:

- Quarterly seat reclaim
- Curate Knowledge Bases — prune stale content
- Build [Copilot Extensions](./copilot-extensions.md) for top internal systems
- Standardize MCP servers across teams
- Iterate custom instructions based on review noise feedback
- Run prompt-engineering masterclasses

Deliverables:

- Cost-of-ownership model
- ROI report (see [metrics](./metrics-and-measurement.md))
- Internal MCP / extension catalog

---

## 6. Phase 5 — Sustain

**Goal:** Make Copilot indistinguishable from "how we build software here".

Continuous practices:

- Onboarding includes Copilot day-1
- New repos inherit instruction templates
- Quarterly model & policy review (new SKUs, new models)
- Annual customer-style "report card"
- Community of practice meets monthly

---

## 7. Persona Enablement Tracks

| Persona | Focus |
|---------|-------|
| Developer | Inline, chat, prompt files, instructions |
| Maintainer | Code review, custom instructions, agent PR triage |
| Tech lead | Custom agents, Spaces, Knowledge Bases |
| Architect | Workspace design, ADR-grounded chat, MCP catalog |
| Security | Content exclusions, audit, IP indemnity |
| Platform | SCIM, policy-as-code, MCP infra, metrics pipeline |
| EM / leader | Metrics, ROI, change management |

Each track: 1 short async module + 1 live workshop + 1 office-hour follow-up.

---

## 8. Anti-Patterns

- "Big bang" rollout without a pilot
- Buying Enterprise but not enabling Knowledge Bases / Code Review
- No champions program → adoption plateaus around 40%
- No reclaim cadence → license bloat
- Treating Copilot as a tool, not a workflow change
- Skipping security review because "it's just an IDE plugin"

---

## 9. Templates

Customer-facing:

- Engagement charter
- Pilot success criteria
- Champions program kickoff deck
- Quarterly business review template

(Place these in your engagement-specific fork of this kit.)

---

## 10. References

- [Licensing & Policies](./licensing-and-policies.md)
- [Metrics & Measurement](./metrics-and-measurement.md)
- [Responsible AI](./responsible-ai.md)
