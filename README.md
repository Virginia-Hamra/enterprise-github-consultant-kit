# GitHub Enterprise Toolkit for New Enterprise Customers

**Audience:** Enterprise & Regulated Organizations
**Platform:** GitHub Enterprise Cloud / Server
**Maturity:** New or Early-Stage Adoption

---

## Table of Contents

1. [Objective](#1-objective)
2. [Customer Advisory Domains](#2-customer-advisory-domains)
3. [Enterprise Risk Landscape](#3-enterprise-risk-landscape)
4. [Rollout Strategies](#4-rollout-strategies)
5. [Reference Architecture](#5-reference-architecture)
6. [Security & Governance Baseline](#6-security--governance-baseline)
7. [CI/CD Starter Pack](#7-cicd-starter-pack)
8. [Developer Enablement Toolkit](#8-developer-enablement-toolkit)
9. [Migration & Adoption Playbook](#9-migration--adoption-playbook)
10. [Operational Runbook](#10-operational-runbook)
11. [Customer Readiness Checklist](#11-customer-readiness-checklist)
12. [Outcome](#12-outcome)

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

## 2. Customer Advisory Domains

The following are the **canonical delivery domains** an advisory / senior delivery
engineer covers when onboarding and maturing an enterprise customer on
**GitHub Enterprise Cloud (GHEC)**, **GitHub Enterprise Cloud with Data Residency
(EU / AU)**, and **GitHub Enterprise Server (GHES)**. Every customer engagement
should be mapped against these domains to ensure full coverage.

### 2.1 Platform & Tenancy

| Domain | Scope |
|--------|-------|
| Enterprise Account | Enterprise-level setup, ownership, billing, policies |
| Tenancy Model | GHEC, GHEC + Data Residency (EU, AU), GHES, hybrid |
| Organization Topology | Org structure, naming, ownership, lifecycle |
| Verified & Approved Domains | Email domain verification, restrict notifications, member email policies |
| Enterprise Managed Users (EMU) | Centralized identity, isolated user namespace |
| Region & Data Residency | Hosting region selection, sovereignty, compliance posture |

### 2.2 Identity & Access Management (IAM)

| Domain | Scope |
|--------|-------|
| Authentication | SSO (SAML / OIDC), Entra ID, Okta, Ping, Google Workspace |
| Provisioning | SCIM user/group lifecycle, JIT, deprovisioning |
| Authorization | RBAC, custom org roles, custom repository roles |
| Team Modeling | IdP-group-mapped teams, nested teams, CODEOWNERS |
| MFA & Passkeys | Enforced 2FA, passkeys, WebAuthn, FIDO2 |
| Personal Access Tokens | Fine-grained PATs, PAT policies, restrictions |
| Service Identity | GitHub Apps, OIDC federation, machine accounts |

### 2.3 Security & Advanced Security (GHAS)

| Domain | Scope |
|--------|-------|
| Code Security | CodeQL, third-party SAST integration, custom queries |
| Secret Protection | Secret scanning, push protection, custom patterns, partner program |
| Software Composition Analysis | Dependabot alerts, updates, version updates, grouped PRs |
| Supply Chain | Dependency review, SBOM export (SPDX), provenance, attestations |
| Security Campaigns & Overview | Risk dashboards, campaigns, security configurations |
| AI Security | Copilot Autofix, AI-assisted triage |
| Vulnerability Disclosure | Private vulnerability reporting, advisories, CVE workflow |

### 2.4 Governance, Risk & Compliance (GRC)

| Domain | Scope |
|--------|-------|
| Policy-as-Code | Repository rulesets, org rulesets, push rules |
| Repository Standards | Templates, required files, naming, visibility policies |
| Audit & Logging | Enterprise audit log, Git events, streaming to SIEM (Splunk, Sentinel, S3, Azure Event Hubs, Datadog) |
| Compliance Mapping | SOC 2, ISO 27001, FedRAMP, NIST 800-53, PCI-DSS, HIPAA, GDPR, DORA |
| Data Protection | IP allow lists, data residency, retention, legal hold |
| License Compliance | Seat management, license usage reports, true-up |

### 2.5 CI/CD & Automation (GitHub Actions)

| Domain | Scope |
|--------|-------|
| Actions Governance | Allow-listing, SHA pinning, reusable workflows, required workflows |
| Runners | GitHub-hosted, larger runners, ARM runners, GPU runners, self-hosted, ARC (Actions Runner Controller) |
| Network Egress | Private networking, Azure VNET injection, private runners |
| Secrets & OIDC | Environments, OIDC federation to AWS / Azure / GCP / HashiCorp Vault |
| Deployments | Environments, protection rules, deployment approvals, deployment branch policies |
| Artifacts & Registries | GitHub Packages (npm, Maven, NuGet, RubyGems, Container Registry) |
| Release Engineering | Releases, immutable artifacts, attestations, SLSA |

### 2.6 Developer Experience & Productivity

| Domain | Scope |
|--------|-------|
| Codespaces | Cloud dev environments, prebuilds, policies, cost controls |
| Repository Templates | Backend, frontend, library, IaC, mobile, data |
| Golden Paths | Opinionated, paved-road delivery patterns |
| Inner Source | Internal visibility, contribution model, fork strategy |
| Documentation | Pages, Wikis, docs-as-code, ADRs |
| Search & Discovery | Code search, knowledge sharing |

### 2.7 GitHub Copilot & AI

| Domain | Scope |
|--------|-------|
| Copilot Business / Enterprise | Licensing, enablement, policy, content exclusions |
| Copilot Chat | IDE chat, GitHub.com chat, knowledge bases |
| Copilot in the CLI | `gh copilot` adoption |
| Copilot Code Review | Automated PR review, custom instructions |
| Copilot Workspace / Agents | Agentic workflows, custom agents, MCP servers |
| Copilot Extensions | Marketplace extensions, custom extensions |
| Measurement | Copilot Metrics API, adoption, acceptance, impact |
| Responsible AI | Data handling, IP indemnity, exclusions, transparency |

### 2.8 Migration & Modernization

| Domain | Scope |
|--------|-------|
| Source Platform Discovery | Azure DevOps, GitLab, Bitbucket Server/Cloud, GHES, SVN, Perforce, TFVC, CC |
| Migration Tooling | GitHub Enterprise Importer (GEI), `gh ado2gh`, `gh bbs2gh`, `gh gl2gh`, ECI |
| Pipeline Modernization | Jenkins → Actions, Azure Pipelines → Actions, GitLab CI → Actions |
| Identity Migration | Mannequin reconciliation, history preservation |
| Artifact Migration | Nexus, Artifactory → GitHub Packages |
| Cutover & Validation | Freeze windows, parallel run, rollback, signoff |

### 2.9 Integrations & Extensibility

| Domain | Scope |
|--------|-------|
| Issue Tracking | Jira, Azure Boards, ServiceNow, Linear |
| Communication | Slack, Microsoft Teams |
| Observability | Datadog, New Relic, Grafana, Splunk |
| Cloud Providers | AWS, Azure, GCP via OIDC |
| GitHub Apps | Custom apps, webhooks, GraphQL/REST APIs |
| Marketplace | Verified apps, third-party Actions vetting |

### 2.10 Operations & Reliability

| Domain | Scope |
|--------|-------|
| Service Health | GitHub Status, incident response, RTO/RPO |
| Backup & DR | Repository backups, GHES backup utilities, BYOK |
| Capacity & Cost | Actions minutes, storage, Codespaces, Copilot seats, larger runners |
| Support Model | Premium / Premium Plus, escalation paths, TAM engagement |
| Change Management | Release pipelines, GHES upgrade cadence, hotpatches |
| Observability | Audit log streaming, metrics APIs, webhook event pipelines |

### 2.11 Adoption, Enablement & Change Management

| Domain | Scope |
|--------|-------|
| Executive Alignment | Vision, KPIs, business outcomes |
| Persona Enablement | Developers, maintainers, security, platform, leadership |
| Training & Certification | GitHub Certifications, internal academy, office hours |
| Champions Program | Internal advocates, communities of practice |
| Communications | Rollout plans, FAQs, runbooks, internal portal |
| Measurement | DORA metrics, SPACE, developer satisfaction, adoption funnels |

---

## 3. Enterprise Risk Landscape

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

## 4. Rollout Strategies

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

## 5. Reference Architecture

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

## 6. Security & Governance Baseline

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

## 7. CI/CD Starter Pack

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

## 8. Developer Enablement Toolkit

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

## 9. Migration & Adoption Playbook

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

## 10. Operational Runbook

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

## 11. Customer Readiness Checklist

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

## 12. Outcome

This toolkit provides a **repeatable, audit-ready GitHub Enterprise foundation** that balances:

- Security
- Governance
- Developer velocity
- Long-term scalability

Designed for **enterprise consulting delivery and regulated environments**.
