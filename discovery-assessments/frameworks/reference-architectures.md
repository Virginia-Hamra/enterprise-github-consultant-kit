# Reference Architectures

Reusable, customer-agnostic patterns to anchor design conversations. Diagram links may live alongside this file.

## Identity
- **Pattern: Entra ID + SAML SSO + SCIM (multi-tenant aware)**
- **Pattern: Enterprise Managed Users (EMU) for fully isolated identity**
- **Pattern: Federated identity for contractors / outside collaborators**

## Org topology
- **Pattern: Single org per business unit**
- **Pattern: Single enterprise / multi-org by data classification**
- **Pattern: Multi-enterprise (regulated separation)**

## Repository governance
- **Pattern: Rulesets-as-code via Terraform**
- **Pattern: CODEOWNERS-driven review**
- **Pattern: Repo template + scaffolder service**

## Actions runner topology
- **Pattern: GitHub-hosted only (lowest ops)**
- **Pattern: GitHub-hosted + larger runners**
- **Pattern: ARC on Kubernetes (per-org or per-enterprise)**
- **Pattern: Self-hosted VMs in private network**

## Cloud auth
- **Pattern: OIDC to AWS / Azure / GCP (no long-lived secrets)**
- **Pattern: Cross-account / cross-subscription deploy**

## Secret management
- **Pattern: Org-level secrets + environment protection**
- **Pattern: External vault (HashiCorp / Azure Key Vault / Secrets Manager) via OIDC**

## Audit pipeline
- **Pattern: Audit log streaming → SIEM (Splunk/Sentinel) → dashboards + alerts**
- **Pattern: Audit + GHAS → unified security data lake**

## Migration
- **Pattern: GEI wave-based migration (BBS / ADO / GitLab → GHEC)**
- **Pattern: Hybrid coexistence during multi-quarter migration**

## Copilot
- **Pattern: Pilot → broad enablement with content exclusions**
- **Pattern: Copilot Enterprise + knowledge bases + custom instructions**
- **Pattern: Coding agent with governed task scope**

> Each pattern should be expanded into its own doc with diagram, components, decisions, and trade-offs as it gets reused.
