# GitHub Enterprise Toolkit for New Enterprise Customers

**Audience:** Enterprise & Regulated Organizations
**Platform:** GitHub Enterprise Cloud / Server
**Maturity:** New or Early-Stage Adoption

---

## Table of Contents

1. [Objective](#1-objective)
2. [Enterprise Risk Landscape](#2-enterprise-risk-landscape)
3. [Rollout Strategies](#3-rollout-strategies)
4. [Reference Architecture](#4-reference-architecture)
5. [Security & Governance Baseline](#5-security--governance-baseline)
6. [CI/CD Starter Pack](#6-cicd-starter-pack)
7. [Developer Enablement Toolkit](#7-developer-enablement-toolkit)
8. [Migration & Adoption Playbook](#8-migration--adoption-playbook)
9. [Operational Runbook](#9-operational-runbook)
10. [Customer Readiness Checklist](#10-customer-readiness-checklist)
11. [Outcome](#11-outcome)

---

## 1. Objective

Provide a **secure-by-default, enterprise-scalable GitHub Enterprise foundation** that accelerates adoption while meeting security, compliance, and governance requirements.

This toolkit is designed for **consulting delivery** and enables:

- Secure platform onboarding
- Strong governance and auditability
- CI/CD standardization
- Developer productivity at scale
- Long-term maintainability

---

## 2. Enterprise Risk Landscape

| Risk | Description | Mitigation |
|-----|------------|------------|
| Identity Sprawl | Unmanaged users & access | Enforced SSO + SCIM |
| Excessive Privileges | Overuse of admin rights | Least-privilege RBAC |
| Insecure Pipelines | Shadow CI/CD & secrets | Curated Actions + OIDC |
| Compliance Drift | Manual, undocumented controls | Policy-as-Code |
| Repo Inconsistency | No standards | Templates + guardrails |
| Supply Chain Risk | Untrusted Actions/dependencies | Allow-listing + Dependabot |
| Low Adoption | Overly restrictive platform | Golden paths + enablement |

---

## 3. Rollout Strategies

### Strategy A — Security-First Landing Zone (Recommended)

Establish a hardened GitHub Enterprise baseline **before** broad developer onboarding.

**Pros**

- Strong governance & audit readiness
- Minimal rework later

**Cons**

- Slightly slower initial onboarding

---

### Strategy B — Parallel Enablement

Build governance and developer enablement in parallel.

**Pros**

- Faster adoption

**Cons**

- Higher coordination overhead

---

### Strategy C — Pilot → Scale

Start with 1–2 teams, then standardize.

**Pros**

- Lower initial friction

**Cons**

- Risk of inconsistent patterns

---

**Selected Approach:**
➡️ **Strategy A with early developer golden paths**

---

## 4. Reference Architecture

### Organization Structure

```text
GitHub Enterprise
├── org-platform
├── org-shared-services
├── org-product-a
├── org-product-b
└── org-sandbox
```

### Repository Types

| Repo Type | Purpose |
|---------|--------|
| `.github` | Org-wide workflows & policies |
| `*-template` | Standardized repo templates |
| `*-infra` | Infrastructure & automation |
| `*-app` | Application code |
| `*-docs` | Documentation |

### Access Model

| Role | Responsibility |
|----|---------------|
| Enterprise Owner | Identity, policies, audit |
| Org Owner | Org configuration |
| Maintainer | Repo administration |
| Developer | Code contribution |
| Auditor | Read-only + audit logs |

---

## 5. Security & Governance Baseline

### Identity & Access

- Enforced SSO (SAML / OIDC)
- SCIM provisioning & deprovisioning
- Mandatory MFA
- No unmanaged personal accounts

### Repository Guardrails

- Branch protection on `main`
- Required pull request reviews (≥2)
- Required status checks
- No force pushes
- CODEOWNERS enforced

### Platform Security

- Dependabot alerts & updates
- Secret scanning with push protection
- CodeQL enabled by default
- Private vulnerability reporting

### Audit & Compliance

- Enterprise audit logs enabled
- Export to SIEM
- Evidence mapping for:
  - SOC 2
  - ISO 27001
  - NIST
  - GDPR

### Compliance Mapping (Example)

| Control Area | GitHub Capability |
|-------------|------------------|
| Access Control | SSO, SCIM, RBAC |
| Change Management | Pull requests, reviews, audit logs |
| Secure SDLC | Branch protection, CI checks |
| Vulnerability Management | Dependabot, CodeQL |
| Secrets Management | Secret scanning, OIDC |
| Audit Logging | Enterprise audit logs, SIEM export |

---

## 6. CI/CD Starter Pack

### Actions Governance

- Allow-listed Actions only:
  - `actions/*`
  - `github/*`
  - Internal Actions repository
- Third-party Actions must be SHA-pinned

### Secure CI Template

```yaml
name: ci
on: [pull_request]

permissions:
  contents: read
  security-events: write

jobs:
  build-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@<sha>
      - run: make test
```

### Secure CD Principles

- GitHub Environments used for all deployments (dev, staging, prod)
- Environment-level protection rules enforced for production
- Mandatory human approvals for production deployments
- OIDC-based cloud authentication (AWS/GCP/Azure) with short-lived tokens
- No long-lived secrets stored in GitHub (no PATs, no static cloud keys)
- Deployment permissions scoped per environment and per repository

---

## 7. Developer Enablement Toolkit

### Repository Templates

- Backend service
- Frontend application
- Shared library
- Terraform module

Each template includes:

- Standardized `README.md`
- `SECURITY.md`
- `CODEOWNERS`
- Preconfigured CI workflow
- Branch protection presets

### Best Practices

- Trunk-based development
- Small, reviewable pull requests
- Automated testing by default
- Docs-as-code approach

### Golden Paths

Opinionated, secure-by-default workflows that provide developers with a clear,
approved path to production. Golden paths minimize cognitive load, reduce risk,
and ensure teams remain compliant without slowing delivery.

---

## 8. Migration & Adoption Playbook

### Migration Phases

1. Discovery & risk classification
2. Repository migration
3. CI/CD translation
4. Security hardening
5. Cutover & validation

### Tooling

- GitHub Importer
- GitHub CLI (`gh`)
- API-based bulk automation

---

## 9. Operational Runbook

### Day-to-Day Operations

- Monitor enterprise audit logs
- Review Dependabot alerts
- Triage CodeQL findings

### Incident Response

- Token compromise procedure
- Repository lockdown steps
- Forensic audit trail preservation

### Change Management

- All governance changes via pull requests
- Peer review & approval required
- Versioned and traceable policy history

---

## 10. Customer Readiness Checklist

### Identity

- [ ] SSO enforced
- [ ] SCIM active
- [ ] MFA required

### Governance

- [ ] Branch protections applied
- [ ] Repository creation restricted
- [ ] Admin access minimized

### Security

- [ ] CodeQL enabled
- [ ] Secret scanning enabled
- [ ] Dependabot active

### CI/CD

- [ ] Approved Actions defined
- [ ] Environments protected
- [ ] No hard-coded secrets

### Enablement

- [ ] Templates published
- [ ] Documentation available
- [ ] Developer training delivered

---

## How to Use This Toolkit

This repository is intended to be used as a consulting starter kit:

1. Clone or fork for each customer engagement
2. Customize organization structure and policies per customer context
3. Automate enforcement using GitHub APIs and Terraform
4. Use this README as the authoritative customer reference

---

## 11. Outcome

This toolkit provides a **repeatable, audit-ready GitHub Enterprise foundation** that balances:

- Security
- Governance
- Developer velocity
- Long-term scalability

Designed for **enterprise consulting delivery and regulated environments**.
