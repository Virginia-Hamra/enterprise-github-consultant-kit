# Compliance & Governance

> Map controls to regulations, automate evidence, prove it on demand.

---

## 1. The Operating Model

Three loops, all running:

1. **Design** — controls in policy, threat models, ADRs
2. **Run** — automated enforcement (rulesets, OPA, CI checks, audit streaming)
3. **Prove** — continuous evidence, dashboards, audit-ready reports

A senior consultant designs all three together — never compliance as a paperwork exercise after the fact.

---

## 2. Regulations You'll Meet

| Region / Sector | Frameworks |
|-----------------|------------|
| Global SaaS | SOC 2 (Type II), ISO 27001, ISO 27017/27018 |
| US Federal | FedRAMP, FISMA, NIST 800-53, NIST SSDF (800-218) |
| EU | GDPR, NIS2, **DORA** (financial), **EU AI Act** |
| UK | UK GDPR, FCA / PRA expectations |
| Finance | PCI-DSS v4, SOX (ITGCs), MAS TRM (SG) |
| Healthcare | HIPAA (US), MDR, HDS (FR) |
| AI-specific | ISO/IEC 42001, NIST AI RMF |

---

## 3. Control Catalog (Common Set)

| Domain | Common Controls |
|--------|-----------------|
| Access | SSO, MFA, JML, periodic access review |
| Change | PR-based change, ≥1 reviewer, audit log |
| SDLC | Threat model, code review, SAST/DAST/SCA |
| Crypto | TLS 1.2+, key rotation, HSM where required |
| Logging | Centralized, tamper-resistant, retention |
| Incident | Plan, tested, reportable timelines met |
| BCP/DR | Plan, tested, RTO/RPO met |
| Vendor | Risk-tiered, contractual security clauses |
| Data | Classification, retention, deletion |

Each maps to GitHub features (rulesets, GHAS, audit log streaming) — see [enterprise consultant kit README](../README.md).

---

## 4. Policy-as-Code

Sources of policy enforcement, in increasing rigor:

1. Documentation (necessary, insufficient)
2. CI checks (lint, test)
3. Repository / Org **rulesets**
4. Branch protection + required reviewers
5. **OPA / Conftest** for IaC and Kubernetes
6. **Cedar / IAM Access Analyzer** for cloud auth
7. Real-time admission controllers

Aim to push from #1/#2 to #3–#7 as engagement matures.

---

## 5. Evidence Automation

| Evidence | Source |
|----------|--------|
| Who can access prod | IdP report + GitHub team membership |
| Code change approvals | Branch protection + PR audit log |
| Vulnerability remediation | Dependabot + CodeQL alerts → SLA dashboard |
| Secret scanning | GHAS + push protection events |
| Deploy approvals | GitHub Environments approvals |
| Backups verified | Cloud backup reports |
| Access reviews | Quarterly export from IdP |

Stream all to SIEM / data warehouse; build a single **Compliance Dashboard** that auditors can see.

---

## 6. SOC 2 Mapping (Quick)

| Trust Services Criteria | Common GitHub Mapping |
|-------------------------|----------------------|
| CC1 — Control environment | Org policies, training |
| CC2 — Communication | Internal policy portal |
| CC3 — Risk assessment | Threat models, risk register |
| CC4 — Monitoring | Audit log streaming, dashboards |
| CC5 — Control activities | Rulesets, branch protection |
| CC6 — Logical access | SSO, SCIM, MFA, RBAC |
| CC7 — System operations | CI/CD, monitoring, incident |
| CC8 — Change management | PR, approval, audit |
| CC9 — Risk mitigation | Vendor mgmt, BCP/DR |

---

## 7. DORA (EU Financial Sector)

Senior-consultant focus areas:

- **ICT Risk management framework** documented and approved
- **ICT third-party register** including GitHub, cloud, SaaS
- **Incident reporting** within statutory timelines
- **Resilience testing** including TLPT (threat-led penetration testing)
- **Contractual clauses** with critical providers

GitHub posture: enterprise audit log streaming, GHAS, EMU, EU residency where required.

---

## 8. EU AI Act (Practical)

Risk classification first:

- Prohibited (unacceptable risk) — avoid
- High-risk — heavy obligations (logging, transparency, oversight)
- Limited — disclosure
- Minimal — voluntary

Most enterprise AI today (Copilot, internal RAG) is **limited or minimal** — but document the assessment.

For high-risk: maintain technical file, conformity assessment, post-market monitoring.

---

## 9. Privacy (GDPR + Friends)

- DPIA for new high-risk processing (incl. agentic AI)
- ROPA (records of processing activities)
- Sub-processor list public
- Lawful basis documented per processing
- Data subject rights process (access, deletion, portability)
- Cross-border transfer mechanism (SCCs, adequacy)

---

## 10. Audit Readiness Checklist

- [ ] Control catalog mapped to frameworks in scope
- [ ] Each control has an owner and an automated evidence source
- [ ] Quarterly internal audit run
- [ ] Policy library version-controlled
- [ ] Change-management trail complete (Git history is your friend)
- [ ] DR + IR tested in last 12 months
- [ ] Vendor risk register current
- [ ] Training records current

---

## 11. Anti-Patterns

- Compliance as a separate org from engineering
- "Compliance season" once a year
- Manual evidence collection in spreadsheets
- Policies that nobody reads
- Treating SOC 2 as the goal rather than the proof

---

## 12. References

- [Security Architecture](./security-architecture.md)
- [Cloud Strategy](./cloud-strategy.md)
- [DevOps & SRE](./devops-and-sre.md)
