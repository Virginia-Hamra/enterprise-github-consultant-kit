# 20 — Platform-Specific Knowledge

> Customer-shaped truths that don't generalise: the IdP they use, the cloud they're on, the regulator they answer to, the legacy CI they migrate from. Capture per-customer; reuse the *patterns*, not the specifics.

---

## Advisory Gist

**TL;DR.** Maintain a per-customer "platform profile" capturing IdP / cloud / regulator / SCM-source / CI-source / network posture. Reuse the *pattern library* below, not customer specifics. Cross-customer pattern recognition is a senior advisor's biggest accelerator — keep it confidential and abstracted.

**Decisions you will be asked to make**

- Per-customer profile template + storage (private repo per customer).
- Pattern-vs-specifics separation rule (what generalises, what doesn't).
- Cross-customer benchmarking — opt-in only, anonymised.
- Confidentiality + retention controls.

**Top edges**

- Customer-specific config copied into reusable templates → confidentiality breach.
- Pattern library outdated for a customer's specific IdP version.
- Regulator interpretations vary regionally — same regulation, different audits.

**Connects to**

- [18 Discovery & Assessment](../18-discovery-and-assessment/README.md) → builds the profile.
- [19 Learning & Knowledge Management](../19-learning-and-knowledge-management/README.md) → where this lives.
- [16 Risks & Tradeoffs](../16-risks-and-tradeoffs/README.md) → decisions reference the profile.

**Customer-fit questions**

- Which platform-specific quirk has bitten 3+ engagements?
- What's worth promoting from CUST → INV → REF?
- Which customer specifics must never leave their repo?

---

## Overview

Most advisory work splits into:

- **General knowledge** — applies to any GitHub Enterprise customer (the other 19 domains).
- **Platform-specific knowledge** — only true for *this* customer or *this* class of customer.

This domain is the discipline of separating the two without losing the value of either.

---

## Customer Profile Template

```markdown
# Customer Profile: {Customer Name}

**Engagement:** {scope} · **Started:** {date} · **Confidentiality:** {classification}

## Platform footprint
- GitHub: {GHEC / GHEC-DR / GHES} · {version} · {orgs} · {seats}
- IdP: {Entra ID / Okta / Ping / ...} · SCIM: {yes/no} · groups → teams: {policy}
- Cloud: {AWS / Azure / GCP / OCI / on-prem} · regions: {…}
- Network: {egress allow-list / private link / VNet / air-gap}
- SCM source (if migrating): {ADO / Bitbucket / GitLab / SVN / mixed}
- CI source: {Jenkins / ADO / GitLab CI / mixed}

## Regulatory posture
- Frameworks in scope: {SOC2 / ISO / PCI / HIPAA / GDPR / DORA / NIS2 / sectoral}
- Auditor: {who / when}
- Data residency: {region(s) + the clause that says so}

## Decisions on file (ADRs)
- {link} — {decision} — {date}

## Open questions
- {…}

## Risks accepted
- {…}

## Last reviewed: {date} · Next review: {date}
```

---

## Pattern Library (anonymised, reusable)

The patterns below recur across engagements. Capture *what* and *when*; never *who*.

### Identity patterns

- **Entra ID + EMU + cross-tenant guests** → SCIM dance + B2B trust + license counting trap.
- **Okta + multiple IdPs (M&A)** → bridge-IdP pattern; consolidation roadmap.
- **Ping + classic SAML + manual team mapping** → SCIM-first migration.

### Cloud patterns

- **Azure-first with OIDC to multiple subscriptions** → federated credential per env, per repo.
- **AWS hub-and-spoke + cross-account roles** → OIDC trust on hub, role-chain to spokes.
- **Multi-cloud with central CI** → ARC on K8s, OIDC to each cloud independently.

### Network patterns

- **Strict egress allow-list + GitHub Actions** → meta-API consumer; alternative: ARC on internal.
- **WAF in front of GHES** → tune for git protocol; common false-positive on chunked encoding.
- **Webhooks behind firewall** → reverse-proxy or eventing-bridge pattern.

### Regulatory patterns

- **DORA + NIS2 (EU finance)** → supplier obligations cascade to GitHub itself; DPF + SCC.
- **PCI + audit log retention** → 1-year minimum; streaming + S3 lifecycle.
- **Sectoral (PRA / FFIEC / TISAX / GxP)** → reference internal interpretations; regulator-specific.

### Migration patterns

- **ADO (heavy pipelines) → GitHub** → modernisation > 1:1 port; ARC for self-hosted continuity.
- **Bitbucket DC + LFS + Jenkins** → SSH 7999 access + LFS pre-stage + Jenkins archetypes.
- **GitLab self-hosted + GitLab CI** → API export gaps + CI rewrite scope.

### Cost patterns

- **macOS-heavy CI** → Linux migration; Apple silicon ARC consideration.
- **GHAS active-committer overshoot** → repo scoping + reclamation cadence.
- **Codespaces leakage** → idle + retention enforcement.

---

## Configuration

### Storage

- **Per-customer profile**: private repo, encrypted at rest, retention-scheduled.
- **Pattern library**: this README + sibling INV notes.
- Never copy customer-specific values into the pattern library.

### Cross-customer benchmarking

- Opt-in only, with explicit customer consent in writing.
- Strip identifying detail before aggregation.
- Use anonymised buckets ("EU-finance customer @ ~3000 devs") not names.

---

## Usage

1. New customer → spin up profile from template.
2. Match observed shape to one or more pattern entries above.
3. Borrow the pattern, never the specifics.
4. After engagement: extract any *new* pattern, anonymise, propose addition.

---

## Best Practices

- **Confidentiality classification on every customer doc.**
- **One customer = one repo.** No mixing.
- **Retention = engagement length + N months**, automated.
- **Pattern review** quarterly; promote / retire.
- **No PII** in pattern entries. Ever.

---

## Common Pitfalls

- Customer name leaking into the pattern library.
- "All my customers do X" → over-generalisation; capture the *condition* under which they do X.
- Stale profile → recommendations diverge from reality.
- Cross-customer "war stories" delivered to a different customer → trust break.

---

## Implementation Notes

- Use `CODEOWNERS` + ruleset on the customer repo to enforce review before merge of confidential edits.
- Encrypt particularly sensitive sections with `git-crypt` or external KMS.
- Log access to customer repos to the SIEM.

---

## Sources

- [GitHub repository security best practices](https://docs.github.com/en/code-security/getting-started/securing-your-repository)
- [Diátaxis](https://diataxis.fr/) — applied to customer-specific knowledge.
- See also: [`practice-repo/CLAUDE.md`](../../../practice-repo/CLAUDE.md), [`enterprise-github-consultant-kit/.github/skills/customer-advisory/SKILL.md`](../../.github/skills/customer-advisory/SKILL.md)
