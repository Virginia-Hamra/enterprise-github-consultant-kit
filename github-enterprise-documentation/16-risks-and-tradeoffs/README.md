# 16 — Risks, Tradeoffs & Decision Framework

> Every architectural choice in GitHub Enterprise is a tradeoff. Make the choices explicit and document them.

---

## Advisory Gist

**TL;DR.** This domain is the **decision spine** of every engagement. Pair every architectural choice with: a documented matrix, an ADR, and an automation that *enforces* the choice (rulesets / OPA / required workflows). Review annually or on major org change.

**Decisions catalogued here**

- GHEC vs GHEC-DR vs GHES (tenancy + residency).
- EMU vs Standard (identity).
- Hosted vs Larger vs ARC vs static self-hosted (runners).
- Rulesets vs legacy branch protection.
- REST vs GraphQL.

**Top edges**

- "Evaluate later" → undefined defaults become technical debt.
- Decisions made in meetings without ADRs → lost.
- GHES chosen for control then unstaffed.
- EMU adopted without OSS-contribution analysis.

**Connects to**

- All other 16 domains — this folder is the cross-cut.
- [solution-architecture-knowledge/enterprise-architecture.md](../../solution-architecture-knowledge/enterprise-architecture.md) — ADR pattern.
- [deliverables/README.md](../../deliverables/README.md) — Decision Framework deliverable type.

**Customer-fit questions**

- Where do architectural decisions live, and who reads them?
- When was the last decision-review cycle?
- For each major decision: what would change the answer?

---

## Overview

Documented architectural decisions every enterprise must make, with the trade-off space and a recommended default.

---

## Decision Matrix

### GHEC vs GHES

| Factor | GHEC | GHEC + Data Residency | GHES |
|--------|------|-----------------------|------|
| Data residency | GitHub-managed | EU / AU dedicated | Your DC |
| Operational overhead | Low | Low | High |
| Feature currency | Current | Current | Lags GHEC |
| Air-gap | No | No | Yes |
| HA / DR | GitHub | GitHub | You |

**Default:** GHEC. Use Data Residency for EU / AU sovereignty. GHES only when air-gap or specific regulation forces it.

### EMU vs Standard

| Factor | EMU | Standard |
|--------|-----|----------|
| Identity ownership | Enterprise-owned | Linked to personal account |
| Public OSS contribution | Restricted | Yes |
| Provisioning | SCIM mandatory | SCIM recommended |
| Isolation | Complete | SSO-linked |

**Default:** Standard with enforced SSO. EMU when complete identity isolation required.

### Hosted vs Self-hosted Runners

| Factor | GitHub-hosted | Larger | ARC (self-hosted, K8s) | Static self-hosted |
|--------|---------------|--------|------------------------|--------------------|
| Operational overhead | None | None | Medium | High |
| Internal network access | None | VNet | Direct | Direct |
| Cost (high volume) | High | High | Low | Low |
| Security isolation | Best | Best | Good (ephemeral) | Risky if persistent |

**Default:** GitHub-hosted for security-sensitive + low volume. **ARC** for scale and internal-network workloads.

### Rulesets vs Branch Protection

| Factor | Rulesets | Legacy branch protection |
|--------|----------|--------------------------|
| Org-level scope | Yes | No |
| Bypass actors | Yes | No |
| Pattern application | Yes | No (per repo only) |
| `evaluate` mode | Yes | No |

**Default:** Rulesets for all new configurations.

### REST vs GraphQL

| Factor | REST | GraphQL |
|--------|------|---------|
| Stability | High | High |
| Round trips | Many | One |
| Complex shapes | Painful | Natural |
| Rate limits | Per-request | Point budget |

**Default:** REST for management ops, GraphQL for reporting / aggregation.

---

## Best Practices

- Document each decision as an **ADR** in a GitHub repo: decision, context, options, rationale, consequences, owner.
- Maintain a 1-page **enterprise architecture summary** for new platform engineers.
- Review decisions **annually** or on major org change (M&A, regulation).
- Maintain a **platform risk register** with quarterly review.

---

## Common Pitfalls

- "Evaluate later" — undefined defaults become technical debt.
- Decisions made in meetings with no documentation.
- GHES chosen for control, then unstaffed.
- EMU adopted without OSS-contribution analysis.

---

## Implementation Notes

- Use a public-internal GitHub repo (`{company}-platform-decisions`) for ADRs.
- Tag decisions by domain (`identity`, `runners`, `network`).
- Pair every architectural decision with an automation that **enforces** it (rulesets, OPA, required workflows).

---

## Sources

- [Architecture Decision Records (Michael Nygard)](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [GitHub Well-Architected (community resource)](https://wellarchitected.github.com/)
- See cross-folder: [solution-architecture-knowledge/enterprise-architecture.md](../../solution-architecture-knowledge/enterprise-architecture.md)
