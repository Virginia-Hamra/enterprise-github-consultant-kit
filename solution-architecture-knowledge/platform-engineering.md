# Platform Engineering & Internal Developer Platforms

> Treat the platform as a product. Pave the road. Measure the friction.

---

## 1. What an IDP Is (and Isn't)

**Is:** A self-service product that consolidates infrastructure, tooling, and standards behind a developer-facing surface — typically a portal (Backstage), CLI, or Git-native experience.

**Isn't:** A re-skinned ticket queue, a CMDB, or a kubernetes cluster.

The IDP exists to **shorten the path from idea to production** while keeping security, cost, and reliability default-on.

---

## 2. Team Topologies (The Reference)

| Team | Purpose |
|------|---------|
| **Stream-aligned** | Owns a value stream end-to-end |
| **Platform** | Provides self-service capabilities |
| **Enabling** | Coaches stream-aligned teams in new skills |
| **Complicated subsystem** | Owns deeply specialized components |

Interactions: collaboration, X-as-a-service, facilitating. The **platform team's success metric is stream-aligned team velocity**, not platform feature count.

---

## 3. Golden Paths

A golden path is the **opinionated, secure-by-default** route from new repo → production. Components:

- Repo template (with CI, CODEOWNERS, security defaults)
- Reusable workflows for build/test/deploy
- IaC modules for the standard topology
- Observability instrumentation baked in
- Documentation generated from templates
- A 1-command bootstrap (`gh repo create --template ...` or portal click)

KPI: **Time from repo creation to first production deploy** (target: hours, not weeks).

---

## 4. Backstage (or equivalent) Anatomy

| Plugin / Component | Purpose |
|--------------------|---------|
| Software Catalog | Single source of truth for services, owners |
| TechDocs | Docs-as-code rendering |
| Software Templates | Scaffolders (golden paths) |
| Scorecards | Maturity & compliance posture |
| Cost Insights | FinOps integration |
| Kubernetes / ArgoCD | Operational visibility |
| GitHub plugin | PRs, Actions, Insights inline |

Avoid plugin sprawl — pick the 6–10 that drive 90% of value.

---

## 5. Self-Service Surface Patterns

| Action | Self-Service Surface |
|--------|---------------------|
| Create repo from template | Portal / `gh repo create` |
| Provision dev DB | Crossplane / Terraform module |
| Onboard a new service | Backstage scaffolder |
| Request access | SCIM-driven group membership |
| Create environment | GitOps PR to platform repo |
| Promote to prod | Approval in Environments |

If a request takes more than ~2 platform engineer hours, it's a candidate for self-service.

---

## 6. GitOps as the Backbone

- Declarative state in Git (configs, IaC, policies)
- Pull-based reconciliation (Argo CD, Flux, Crossplane)
- Drift detection alerts
- Promote via PR, not console
- Rollback = revert PR

Pair with environment promotion: dev → staging → prod, each with rulesets + required reviewers.

---

## 7. Maturity Model (Scorecards)

Define service tiers:

| Tier | Examples |
|------|----------|
| Bronze | Has CODEOWNERS, README, CI |
| Silver | + tests > 60% cov, on-call, runbook |
| Gold | + DR tested, SLOs, SBOM, signed artifacts |
| Platinum | + chaos testing, multi-region, automated DR |

Gate features (e.g., access to prod) on tier. Display in Backstage. Refresh quarterly.

---

## 8. Cost & FinOps in the Platform

- Tag every resource with `service`, `team`, `cost-center`, `env`
- Surface cost per service in Backstage
- Right-size GitHub Actions runners; enforce timeouts
- Use larger / ARM runners only when justified
- Reclaim Codespaces, Copilot seats, idle infra monthly

---

## 9. Anti-Patterns

- Building a portal nobody uses (no PMing of the platform)
- Forcing every team onto the platform on day 1
- Confusing the IDP with the cluster
- "We don't need a roadmap, it's internal"
- Ignoring DX research (no surveys, no shadowing)

---

## 10. Measurement

- Adoption: % services on golden path
- Friction: time-to-prod for a new service
- Reliability: % services meeting their SLO
- Satisfaction: quarterly developer NPS
- Cost: $ / service / month

---

## 11. References

- [DevOps & SRE](./devops-and-sre.md)
- [Cloud Strategy](./cloud-strategy.md)
- [Knowledge Base — Architectural Styles](./knowledge-base.md)
