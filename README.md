# GitHub Enterprise Toolkit for New Enterprise Customers

**Audience:** Enterprise & Regulated Organizations  
**Platform:** GitHub Enterprise Cloud / Server  
**Maturity:** New or Early-Stage Adoption

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

```js
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

- Protected environments with deployment rules
- Required manual approvals for production
- OIDC-based cloud authentication (no static credentials)
- No long-lived secrets (short-lived, federated access only)

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

Opinionated, safe defaults that show developers **how to succeed** on the platform while remaining compliant and secure.

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

## 11. Outcome

This toolkit provides a **repeatable, audit-ready GitHub Enterprise foundation** that balances:

- Security  
- Governance  
- Developer velocity  
- Long-term scalability  

Designed for **enterprise consulting delivery and regulated environments**.
