# GitHub Enterprise Documentation

> Senior-consultant knowledge base covering everything to know — and currently manage — across a GitHub Enterprise estate. One folder per domain, each with an **Advisory Gist** at the top (TL;DR · decisions · top edges · cross-links · customer-fit questions) followed by full domain detail and current `docs.github.com` sources.

Last reviewed: April 2026.

---

## How to use this folder

Every domain README is layered:

1. **Advisory Gist** — what a senior consultant must remember in 30 seconds. Decisions, edges, cross-links.
2. **Overview · Configuration · Usage** — the operational substance.
3. **Best Practices · Common Pitfalls · Implementation Notes** — applied wisdom.
4. **Sources** — current `docs.github.com` links; verify currency before quoting in customer deliverables.

Companion knowledge:

- [`../github-copilot-enablement/`](../github-copilot-enablement/) — Copilot rollout, governance, measurement.
- [`../solution-architecture-knowledge/`](../solution-architecture-knowledge/) — broader enterprise + cloud architecture context.
- [`../deliverables/`](../deliverables/) — the five deliverable types (📄 🌳 📅 🔍 📐) with structure and quality bars.
- [`../.github/skills/`](../.github/skills/) — agent skills (`customer-advisory`, `gist-extraction`, plus 6 domain skills).
- [`_legacy-overview.md`](./_legacy-overview.md) — original single-file overview (kept for reference).

---

## Domains

| # | Domain | Folder |
|---|--------|--------|
| 01 | Platform Options (GHEC / GHEC-DR / GHES / EMU) | [01-platform-options](./01-platform-options/README.md) |
| 02 | Identity, Access & Governance (SSO, SCIM, EMU, Apps, PATs) | [02-identity-and-access-management](./02-identity-and-access-management/README.md) |
| 03 | Organization & Repository Governance (Rulesets, CODEOWNERS, custom properties) | [03-organization-and-repo-governance](./03-organization-and-repo-governance/README.md) |
| 04 | Security: GitHub Advanced Security (Code Security + Secret Protection) | [04-security-ghas](./04-security-ghas/README.md) |
| 05 | Compliance & Audit (audit log streaming, SIEM, evidence) | [05-compliance-and-audit](./05-compliance-and-audit/README.md) |
| 06 | Software Supply Chain (Dependabot, attestations, SBOM, SLSA) | [06-software-supply-chain](./06-software-supply-chain/README.md) |
| 07 | GitHub Actions (workflows, OIDC, reusable, environments) | [07-github-actions](./07-github-actions/README.md) |
| 08 | CI/CD & Infrastructure (runners, ARC, deployment patterns) | [08-cicd-and-infrastructure](./08-cicd-and-infrastructure/README.md) |
| 09 | Copilot in the Enterprise (governance, agents, MCP, DR) | [09-copilot-in-the-enterprise](./09-copilot-in-the-enterprise/README.md) |
| 10 | Data & Privacy (residency, retention, GDPR, DSAR) | [10-data-and-privacy](./10-data-and-privacy/README.md) |
| 11 | Networking & Connectivity (allow-lists, VNet, WAF, TLS, K8s) | [11-networking-and-connectivity](./11-networking-and-connectivity/README.md) |
| 12 | Integrations & APIs (REST, GraphQL, Apps, webhooks, ITSM) | [12-integrations-and-apis](./12-integrations-and-apis/README.md) |
| 13 | Operational Management (HA, backups, monitoring, upgrades) | [13-operational-management](./13-operational-management/README.md) |
| 14 | Migration & Adoption (GEI, ado2gh, bbs2gh, gl2gh) | [14-migration-and-adoption](./14-migration-and-adoption/README.md) |
| 15 | Cost & Licensing | [15-cost-and-licensing](./15-cost-and-licensing/README.md) |
| 16 | Risks, Tradeoffs & Decision Framework | [16-risks-and-tradeoffs](./16-risks-and-tradeoffs/README.md) |
| 17 | Additional Services & Features (Codespaces, Pages, Apps, Projects, Sponsors, …) | [17-additional-services](./17-additional-services/README.md) |
| 18 | **Discovery & Assessment Methodology** | [18-discovery-and-assessment](./18-discovery-and-assessment/README.md) |
| 19 | **Learning & Knowledge Management** | [19-learning-and-knowledge-management](./19-learning-and-knowledge-management/README.md) |
| 20 | **Platform-Specific Knowledge** (per-customer patterns) | [20-platform-specific-knowledge](./20-platform-specific-knowledge/README.md) |

Domains 18–20 are the **advisory practice** layer — how a senior consultant operates around the technical domains 01–17.

---

## Cross-domain reading paths

- **New consultant onboarding:** 18 → 01 → 02 → 03 → 07 → 09 → 16 → 19
- **Security audit engagement:** 18 → 04 → 05 → 06 → 02 → 11 → 12
- **Migration project:** 18 → 14 → 02 → 03 → 07 → 13 → 15 → 20
- **Copilot rollout:** 18 → 09 → `../github-copilot-enablement/` → 04 → 05 → 19
- **Platform engineering build:** 18 → 03 → 07 → 08 → 12 → 17 (Codespaces) → 13
- **FinOps review:** 18 → 15 → 07 → 08 → 17 (Codespaces, Packages)
- **Regulatory advisory (DORA / NIS2 / PCI):** 18 → 10 → 05 → 04 → 02 → 11 → 20

---

## Cross-cutting themes

These cut across multiple domains. When a customer asks about one, expect to touch 3+ folders:

| Theme | Touches |
|-------|---------|
| **Network / proxy / egress** | 01 · 07 · 08 · 11 · 12 |
| **Data residency & sovereignty** | 01 · 05 · 10 · 11 |
| **Identity at scale** | 01 · 02 · 03 · 12 · 14 |
| **Supply-chain security** | 04 · 06 · 07 |
| **Real-time / WebSocket / streaming** | 05 · 09 · 11 · 12 |
| **Kubernetes** | 08 · 11 · 17 |
| **Scale / performance / rate limits** | 07 · 08 · 12 · 13 |
| **Security baselines** | 02 · 03 · 04 · 05 · 06 · 11 |

---

## Conventions

- **Dates / versions:** GHES feature parity lags GHEC; check `docs.github.com/en/enterprise-server@<version>` for the customer's pinned version.
- **GHAS:** Treat as **two products** (Code Security, Secret Protection) — unbundled in 2024.
- **Rulesets vs branch protection:** Rulesets are the strategic direction; legacy protection works but new design uses rulesets.
- **EMU vs Standard:** Decide early. Switching is disruptive.
- **Advisory Gist first.** Every domain leads with the gist; deep content follows for those who need it.
- **Sources are real.** No fabricated URLs. If a doc page can't be located, mark `[needs source verification]` as an open question.
