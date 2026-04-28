# Cloud Strategy & Landing Zones

> Multi-cloud reality, FinOps discipline, and the landing-zone patterns that keep enterprises sane.

---

## 1. Cloud Strategy Choices

| Strategy | When | Trade-offs |
|----------|------|------------|
| Single cloud | Most enterprises | Lock-in vs. depth |
| Primary + DR | Regulation, BCP | 1.3–1.6× cost |
| Multi-cloud | Acquisitions, regulatory | High complexity, talent split |
| Hybrid (cloud + on-prem) | Data gravity, latency | Networking burden |
| Sovereign / regional | Public sector, EU finance | Limited service surface |

Senior consultant rule: **be multi-cloud at the architecture level (abstract where it matters), single-cloud at the implementation level (use deep services)**.

---

## 2. Landing Zone Pattern

A landing zone is the **secure, compliant, account/subscription topology** a workload lands into.

Reference structure:

```
Org / Tenant
├── Management / Audit / Log Archive    (security-owned)
├── Shared Services (DNS, identity)
├── Network (transit, firewalls)
├── Sandbox
├── Non-prod
│   ├── Dev / Test / Staging accounts
└── Prod
    ├── Workload account 1
    └── Workload account 2
```

Tooling:

- AWS: Control Tower, AFT
- Azure: Cloud Adoption Framework, Enterprise-Scale, Bicep / Terraform-AzureVerified
- GCP: Cloud Foundation Toolkit, Project Factory

Provision via Terraform / Crossplane / Pulumi with GitHub Actions + OIDC.

---

## 3. Account / Subscription Boundaries

Default to: **one app + one env = one account/subscription**.

Why:

- Blast radius isolation
- Clean cost attribution
- Independent quota management
- Simpler compliance scoping

Exception: very small orgs may collapse to per-env accounts only.

---

## 4. Networking Patterns

| Pattern | Use |
|---------|-----|
| Hub-and-spoke | Centralized egress / inspection |
| Mesh (Transit Gateway, Virtual WAN) | Many-to-many connectivity |
| Service mesh (Istio, Linkerd) | Inside-cluster mTLS, traffic policies |
| Private endpoints | Keep traffic off the internet |
| Egress firewall | Control external dependencies |

For GitHub Actions, prefer **private runners with VNet integration** (or larger runners with VNet) when egress control is required.

---

## 5. Identity in the Cloud

- **OIDC federation** from GitHub → cloud (no long-lived keys)
- Workload identity (IRSA, Azure Workload Identity, GKE WI)
- Centralized identity in IdP (Entra ID, Okta, Ping)
- Use **permission boundaries** / **management groups** to cap blast radius

---

## 6. FinOps

| Capability | Practice |
|------------|----------|
| Visibility | Tagging policy, cost dashboards, anomaly alerts |
| Optimization | Right-sizing, savings plans / RIs, spot |
| Operate | Showback / chargeback, budgets, FinOps champions |

Tagging baseline: `service`, `team`, `cost-center`, `env`, `compliance`, `owner`.

Run a **monthly FinOps review** with engineering + finance.

---

## 7. Compliance & Sovereignty

- Map workloads to data classification
- Use cloud regions consistent with residency law
- Document **EU data boundary** (or equivalent) for SaaS dependencies
- For DORA: maintain ICT third-party register including cloud + GitHub

GitHub options:

- GHEC standard (multi-region)
- GHEC with Data Residency (EU, AU)
- GHES on a customer-controlled environment

---

## 8. Disaster Recovery

| Strategy | RPO/RTO | Cost |
|----------|---------|------|
| Backup & restore | hours | $ |
| Pilot light | 10s of min | $$ |
| Warm standby | minutes | $$$ |
| Active/active | seconds | $$$$ |

Test DR at least annually. Untested DR is a liability.

---

## 9. Migration to Cloud — The 7 Rs

1. **Retire**
2. **Retain**
3. **Rehost** (lift-and-shift)
4. **Relocate** (e.g., VMware-on-cloud)
5. **Replatform** (lift-tinker-shift)
6. **Repurchase** (move to SaaS)
7. **Refactor / Re-architect**

Senior tip: most portfolios end up **40% rehost, 30% replatform, 20% repurchase, 10% refactor + retire**.

---

## 10. Anti-Patterns

- "Cloud-first" without economics
- Multi-cloud as the goal rather than an outcome
- Account sprawl with no naming convention
- Tagging without enforcement
- DR plan written, never tested

---

## 11. References

- [Platform Engineering](./platform-engineering.md)
- [Security Architecture](./security-architecture.md)
- [Migration & Modernization](./migration-and-modernization.md)
