# 18 — Discovery & Assessment Methodology

> The repeatable method to understand a customer's GitHub estate before recommending anything. Inputs to every other domain.

---

## Advisory Gist

**TL;DR.** Treat discovery as a **structured intake**, not a conversation. Capture scale, regulation, identity, current SCM/CI, security maturity, FinOps maturity, change-management appetite. Produce a 1-page current-state synthesis before any recommendation. Never recommend without an artefact backing it.

**Decisions you will be asked to make**

- Depth: lightweight (1-week diagnostic) vs deep (4–8 week assessment).
- Evidence sources: interviews only / config exports / API pulls / scanner output / all.
- Rubric: CIS GitHub baseline / GitHub Well-Architected / internal / customer-supplied.
- Output: written assessment vs decision matrix vs roadmap.
- Hand-off: who carries the assessment forward post-engagement.

**Top edges**

- "Discovery by interview only" → confirmation bias; pull data.
- Stakeholder list missing the budget owner → recommendations land flat.
- Maturity rated by gut feel → unrepeatable; document the rubric.
- Findings without owner + date → aspirational, not actionable.

**Connects to**

- [deliverables/README.md](../../deliverables/README.md) → 🔍 Technical Assessment deliverable type.
- [16 Risks & Tradeoffs](../16-risks-and-tradeoffs/README.md) → frames the decisions discovery feeds.
- [.github/skills/customer-advisory/](../../.github/skills/customer-advisory/SKILL.md) → workflow.
- [.github/skills/gist-extraction/](../../.github/skills/gist-extraction/SKILL.md) → compress source artefacts.

**Customer-fit questions**

- What triggered this engagement, and what does success look like in 90 days?
- What evidence are you allowed to share?
- Who decides, who influences, who blocks?

---

## Overview

Discovery is the upstream of every domain in this folder. A weak discovery produces weak recommendations everywhere downstream.

Three depths:

| Depth | Duration | Outputs |
|-------|----------|---------|
| **Lightweight** | 3–5 days | Current-state 1-pager + top-5 risks + ranked next steps |
| **Standard** | 2–4 weeks | Full assessment + roadmap + decision frameworks for top 3 questions |
| **Deep** | 4–8 weeks | Above + target architecture + cost model + adoption plan |

---

## Configuration

### Intake rubric

Capture once, version per engagement.

- **Scale.** Active devs · repos · orgs · monthly Actions minutes · monthly egress.
- **Regulation.** SOC2 / ISO27001 / PCI / HIPAA / GDPR / DORA / NIS2 / sectoral (PRA / FFIEC / TISAX / GxP).
- **Identity.** IdP · SSO protocol · SCIM provider · group-mapping practice · break-glass.
- **Current SCM.** GitHub.com / GHEC / GHES / Bitbucket / GitLab / ADO / SVN / multi.
- **Current CI.** Jenkins / ADO / GitLab CI / Bamboo / TeamCity / mixed · runner topology.
- **Cloud footprint.** AWS / Azure / GCP / OCI / on-prem · OIDC trust today.
- **Security maturity.** SAST / SCA / secret scanning / SBOM / signing today.
- **FinOps.** Cost-allocation maturity · chargeback model · budget owner.
- **Change appetite.** Mandate strength · sponsor seniority · prior failed migrations.

### Evidence sources

| Layer | Source |
|-------|--------|
| Identity / access | IdP export · SCIM logs · GitHub `/orgs/{org}/members` API · audit log |
| Repo / org config | `gh api`, `gh ruleset list`, `gh repo list`, custom-properties API |
| Security posture | Code scanning / Secret scanning / Dependabot APIs · GHAS dashboards |
| Actions usage | Billing API · workflow runs API · self-hosted runner inventory |
| CI baseline | Jenkinsfile / `.gitlab-ci.yml` / azure-pipelines.yml inventories |
| Networking | Egress allow-lists · firewall configs · GitHub IP meta API consumer |
| Cost | Billing portal export · enhanced billing API |

---

## Usage

1. **Charter** the assessment in writing — scope, depth, evidence access, deliverable.
2. **Build the stakeholder map** — DACI / RACI per workstream.
3. **Pull evidence first**, then interview to disambiguate.
4. **Score against the rubric** — never gut-feel.
5. **Produce the artefact** — assessment, decision framework, or recommendation memo.
6. **Walk it back** — a findings-review meeting; capture pushback.
7. **Hand off** — name the customer-side owner of follow-through.

---

## Best Practices

- One-pager **before** the deep report. If the one-pager doesn't land, the deep one won't.
- **Numbers, not adjectives.** "Ruleset coverage 38% of production repos" beats "ruleset coverage is low".
- Quote configs verbatim where they matter. Paraphrasing config = drift introduced.
- Distinguish **observation · risk · recommendation · effort**.
- Risk-rank with a documented rubric (likelihood × impact, with definitions).

---

## Common Pitfalls

- Findings as opinions ("this seems risky").
- Recommendations without effort estimates → not prioritisable.
- Roadmap with > 30 items → unactionable.
- Single-stakeholder discovery → political blind spots.
- Skipping the charter step → scope creep mid-engagement.

---

## Implementation Notes

- Build a reusable **discovery script** (REST/GraphQL collectors) → reduce per-engagement effort.
- Standardise the **assessment template** (see deliverables folder) and reuse across engagements.
- Anonymise + retain prior assessments as a benchmarking corpus (with customer permission).

---

## Sources

- [GitHub Well-Architected (community)](https://wellarchitected.github.com/)
- [CIS Benchmarks for GitHub](https://www.cisecurity.org/benchmark/github)
- [Audit log REST API](https://docs.github.com/en/rest/orgs/orgs#get-the-audit-log-for-an-organization)
- [Enterprise reporting via API](https://docs.github.com/en/rest/enterprise-admin)
- [`gh` CLI](https://cli.github.com/manual/)
