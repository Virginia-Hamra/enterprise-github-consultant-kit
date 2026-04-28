# 01 — Platform Options: Enterprise Cloud vs Enterprise Server

> The foundational deployment-model decision: **GHEC**, **GHEC with Data Residency**, **GHES**, or **EMU on GHEC**.

---

## Overview

| Option | What it is |
|--------|-----------|
| **GHEC** | GitHub-managed SaaS on github.com infrastructure |
| **GHEC + Data Residency** | GHEC hosted in a dedicated region (EU, AU as of 2026) |
| **GHES** | Self-hosted appliance (VM / cloud VM / on-prem) |
| **EMU** | Identity model on GHEC: enterprise-owned, fully isolated user accounts |

These are not mutually exclusive — GHES + GHEC hybrids via **GitHub Connect** are common.

---

## Configuration

### GHEC

- One enterprise account → multiple organizations (BU / product / boundary).
- Identity via SSO (SAML or OIDC) at the enterprise (EMU) or org level (non-EMU).
- Optional **EMU**: every user is provisioned via SCIM, no personal account mixing.
- Region selection only via **Data Residency** (EU / AU); standard GHEC is multi-region US-primary.

### GHES

- Single VM or HA pair on supported hypervisors / clouds.
- External load balancer in front of the HA pair.
- Built-in stack — do **not** modify the underlying OS.
- **GitHub Connect** bridges GHES → GHEC for license sync, advisory DB, marketplace Actions.

### Choosing

- Default to **GHEC**.
- Use **GHEC + Data Residency** when EU / AU sovereignty is required.
- Use **GHES** only when air-gap, on-prem mandate, or specific regulation forces it.
- Adopt **EMU** when complete identity isolation is required and you can accept the constraints (no public OSS contribution from EMU identity).

---

## Usage

- GHEC: GitHub runs the platform; you run policy + integrations.
- GHES: you also run patching, backups, HA, capacity.
- Hybrid: GHES is the dev environment; GHEC features (advisory DB, Actions marketplace) consumed via Connect.

---

## Best Practices

- **One enterprise account per legal entity / governance boundary**. Multiple = fragmented billing + audit.
- ≥ 2 enterprise owners with documented succession.
- For GHES: pin a **quarterly upgrade window**; never run > 2 minor versions behind.
- For EMU: map **IdP groups → GitHub teams via SCIM** before enabling SSO.
- For GHEC non-EMU: enforce SSO at org level **before** granting repo access.
- Document the GitHub architecture (org structure, SSO config, runner topology, branching).

---

## Common Pitfalls

- GHES chosen for control, then unpatched → security exposure.
- EMU adopted without realizing it blocks contribution to public repos under managed identity.
- Hybrid (GHES + GHEC via Connect) without documenting what data flows.
- Multiple enterprise accounts for the same org with no consolidation plan.
- GHEC IP allowlist enabled without including GitHub-hosted runner IP ranges → all Actions break.

---

## Implementation Notes

- Run a **load test** on GHES before committing — vendor specs ≠ real-developer load.
- GHEC SLA covers platform availability, not per-feature performance — write your own internal SLA.
- GHES HA replica is **passive** (no traffic until failover); failover is **manual**, rehearse it.
- Plan a **dedicated platform team of 2–5 engineers** at 10k+ scale.

---

## Sources (official GitHub docs)

- [About GitHub Enterprise Cloud](https://docs.github.com/en/enterprise-cloud@latest/admin/overview/about-github-enterprise-cloud)
- [About GitHub Enterprise Server](https://docs.github.com/en/enterprise-server/admin/overview/about-github-enterprise-server)
- [About Enterprise Managed Users](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/about-enterprise-managed-users)
- [GitHub Enterprise Cloud with data residency](https://docs.github.com/en/enterprise-cloud@latest/admin/data-residency)
- [About GitHub Connect](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-github-connect/about-github-connect)
- [GHES system overview & sizing](https://docs.github.com/en/enterprise-server/admin/installation/setting-up-a-github-enterprise-server-instance)
- [Configuring high availability replication for GHES](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-high-availability/about-high-availability-configuration)
- [GHES release notes (upgrade cadence)](https://docs.github.com/en/enterprise-server/admin/release-notes)
- [GitHub Trust Center](https://github.com/trust-center)
