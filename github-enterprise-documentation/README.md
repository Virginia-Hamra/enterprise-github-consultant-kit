# GitHub Enterprise Documentation

> Senior-consultant knowledge base covering everything to know — and currently manage — across a GitHub Enterprise estate. One folder per domain, each with current `docs.github.com` sources.

Last reviewed: April 2026.

---

## How to use this folder

- Each domain folder has a `README.md` with: Overview · Configuration · Usage · Best Practices · Common Pitfalls · Implementation Notes · Sources.
- Sources are real `docs.github.com` links — verify currency before quoting in customer deliverables.
- Companion knowledge:
  - [`../github-copilot-enablement/`](../github-copilot-enablement/) — Copilot rollout, governance, measurement.
  - [`../solution-architecture-knowledge/`](../solution-architecture-knowledge/) — broader enterprise + cloud architecture context.
  - [`_legacy-overview.md`](./_legacy-overview.md) — the original single-file overview (kept for reference).

---

## Domains

| # | Domain | Folder |
|---|--------|--------|
| 01 | Platform Options (GHEC / GHEC-DR / GHES / EMU) | [01-platform-options](./01-platform-options/README.md) |
| 02 | Identity & Access Management (SSO, SCIM, EMU, PATs) | [02-identity-and-access](./02-identity-and-access/README.md) |
| 03 | Org & Repo Governance (Rulesets, CODEOWNERS, Templates) | [03-org-and-repo-governance](./03-org-and-repo-governance/README.md) |
| 04 | GitHub Advanced Security (Code Security + Secret Protection) | [04-advanced-security](./04-advanced-security/README.md) |
| 05 | Compliance & Audit (audit log, streaming, evidence) | [05-compliance-and-audit](./05-compliance-and-audit/README.md) |
| 06 | Software Supply Chain (Dependabot, attestations, SBOM) | [06-software-supply-chain](./06-software-supply-chain/README.md) |
| 07 | GitHub Actions (workflows, OIDC, reusable, environments) | [07-github-actions](./07-github-actions/README.md) |
| 08 | CI/CD & Infrastructure (runners, ARC, deployment patterns) | [08-cicd-and-infrastructure](./08-cicd-and-infrastructure/README.md) |
| 09 | Copilot in the Enterprise | [09-copilot-in-the-enterprise](./09-copilot-in-the-enterprise/README.md) |
| 10 | Data & Privacy (residency, retention, GDPR) | [10-data-and-privacy](./10-data-and-privacy/README.md) |
| 11 | Networking (allow-lists, IP, private connectivity) | [11-networking](./11-networking/README.md) |
| 12 | Integrations & APIs (REST, GraphQL, Apps, webhooks) | [12-integrations-and-apis](./12-integrations-and-apis/README.md) |
| 13 | Operational Management (HA, backups, monitoring, upgrades) | [13-operational-management](./13-operational-management/README.md) |
| 14 | Migration & Adoption (GEI, ado2gh, bbs2gh, gl2gh) | [14-migration-and-adoption](./14-migration-and-adoption/README.md) |
| 15 | Cost & Licensing | [15-cost-and-licensing](./15-cost-and-licensing/README.md) |
| 16 | Risks, Tradeoffs & Decision Framework | [16-risks-and-tradeoffs](./16-risks-and-tradeoffs/README.md) |
| 17 | Additional Services & Features (Codespaces, Pages, Apps, Projects, Sponsors, ...) | [17-additional-services](./17-additional-services/README.md) |

---

## Cross-domain reading paths

- **New consultant onboarding:** 01 → 02 → 03 → 07 → 09 → 16
- **Security audit engagement:** 04 → 05 → 06 → 02 → 11 → 12
- **Migration project:** 14 → 02 → 03 → 07 → 13 → 15
- **Copilot rollout:** 09 → `../github-copilot-enablement/` → 04 → 05
- **Platform engineering build:** 03 → 07 → 08 → 12 → 17.3 (Codespaces)
- **FinOps review:** 15 → 07 → 08 → 17.3 → 17.4

---

## Conventions

- **Dates / versions:** GHES feature parity lags GHEC; check `docs.github.com/en/enterprise-server@latest` for the version your customer runs.
- **GHAS:** Treat as **two products** (Code Security, Secret Protection) — unbundled in 2024.
- **Rulesets vs Branch Protection:** Rulesets are the strategic direction; legacy protection still works but new design should use rulesets.
- **EMU vs Standard:** Decide early. Switching is disruptive.
