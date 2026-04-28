# 15 — Cost & Licensing

> Seat-based licensing + consumption add-ons (Actions, Packages, Codespaces, Copilot, GHAS).

---

## Advisory Gist

**TL;DR.** Licensing = seats (Enterprise, Copilot) + active committers (GHAS, post-2024 unbundle) + consumption (Actions / Packages / Codespaces). Track $ / dev / month as the headline KPI. Spending limits at $0 default — raise deliberately with alerts. Reclaim inactive Copilot + GHAS seats quarterly.

**Decisions you will be asked to make**

- Cost-allocation model (chargeback / showback / shared).
- Spending limits + alert thresholds per consumption SKU.
- Copilot Business vs Enterprise (per-team).
- GHAS scope (which orgs / repos enabled).
- Runner topology economics (hosted vs ARC break-even).

**Top edges**

- GHAS active-committer = unique enterprise-wide, *not* per-repo — finance miscalculates.
- macOS multiplier 10×, Windows 2× — silent driver of Actions cost.
- Codespaces idle → leak; enforce 30-min timeout + 30-day retention.
- Packages egress to external Kubernetes / dev workstations is billable.
- "Unlimited" spending limit without alerts → budget overrun.

**Connects to**

- [04 GHAS](../04-security-ghas/README.md) — committer counting.
- [07 Actions](../07-github-actions/README.md) / [08 CI/CD](../08-cicd-and-infrastructure/README.md) — minutes economics.
- [09 Copilot](../09-copilot-in-the-enterprise/README.md) — seat reclamation.
- [17 Additional Services](../17-additional-services/README.md) — Codespaces, Packages.

**Customer-fit questions**

- What's $ / dev / month today, and what's the target?
- Who reclaims inactive seats, and how often?
- Which workloads should leave macOS for Linux, and what's the ROM saving?

---

## Overview

| Component | Billing model |
|-----------|---------------|
| Enterprise seat | Per active user / month |
| **GHAS** (Code Security + Secret Protection — unbundled 2024+) | Per unique active committer / month, per add-on |
| **Copilot Business / Enterprise** | Per seat / month |
| **Actions minutes** | Per minute, multiplier by OS (Linux 1×, Windows 2×, macOS 10×) |
| **Larger runners** | Per minute, separate SKU |
| **Actions storage** | Per GB-month (artifacts + logs) |
| **Packages** | Storage / GB-month + egress (egress within Actions to GHCR is free) |
| **Codespaces** | Compute / hour + storage / GB-month |

Verify **current multipliers and definitions** in GitHub billing docs — they change.

---

## Configuration

- One **enterprise account** = billing entity. Orgs are children.
- **Spending limits** per enterprise for Actions / Packages / Codespaces — default is $0 (no overage). Raise deliberately, set alert thresholds.
- **Cost allocation**: build chargeback via GitHub billing API (no native cost-center reports).
- **Spend alerts** at the enterprise / org level.

---

## Usage

- Finance reconciles seats vs active users monthly.
- Engineering reviews top-N Actions workflows by minutes / cost.
- FinOps reclaims Copilot / GHAS seats inactive ≥ 60 days.

---

## Best Practices

- Monthly cost report by component + by org with trend.
- Migrate macOS / Windows CI to Linux where possible — multipliers are 10× / 2×.
- **Cache dependencies** in workflows aggressively.
- Tighten artifact retention: 7–30 days for CI artifacts (default 90).
- Self-hosted (ARC) for high-volume Linux CI — break-even point is calculable.
- Reclaim inactive Copilot + GHAS seats quarterly.

---

## Common Pitfalls

- GHAS active-committer count miscalculated — every committer to every GHAS-enabled repo counts (unique enterprise-wide, not per repo).
- Copilot seats abandoned → quiet license bloat.
- No Actions cost visibility → silent budget overruns.
- Packages egress costs from external Kubernetes / dev workstations.
- Spending limits raised to "unlimited" without alerts.

---

## Implementation Notes

- Pull billing data via the **REST API** monthly to a warehouse.
- Compute **$ / dev / month** as the headline KPI.
- Surface cost per service in your IDP / Backstage Cost Insights plugin.
- For Codespaces: enforce **idle timeout** (30 min) and **retention** (30 days inactive → delete).
- Audit `actions/cache` hit rate — low hit rate = wasted minutes.

---

## Sources

- [About billing for your enterprise](https://docs.github.com/en/billing/managing-billing-for-your-products/about-billing-for-your-enterprise)
- [About billing for GitHub Actions](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions)
- [About billing for GitHub Packages](https://docs.github.com/en/billing/managing-billing-for-github-packages/about-billing-for-github-packages)
- [About billing for GitHub Codespaces](https://docs.github.com/en/billing/managing-billing-for-github-codespaces/about-billing-for-github-codespaces)
- [About billing for GitHub Copilot](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-copilot/about-billing-for-github-copilot)
- [About billing for GitHub Advanced Security](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-advanced-security/about-billing-for-github-advanced-security)
- [Setting a spending limit](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/managing-your-spending-limit-for-github-actions)
- [Billing API (REST)](https://docs.github.com/en/rest/billing)
- [GitHub usage report (enhanced billing platform)](https://docs.github.com/en/billing/managing-the-plan-for-your-github-account/about-the-enhanced-billing-platform)
