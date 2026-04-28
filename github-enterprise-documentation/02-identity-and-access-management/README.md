# 02 — Identity & Access Management

> Authentication (SSO), provisioning (SCIM), authorization (RBAC) — the operational baseline at enterprise scale.

---

## Overview

IAM in GitHub Enterprise has four layers, applied top-down:

1. **Authentication** — SAML / OIDC SSO (Entra ID, Okta, Ping, Google Workspace, etc.)
2. **Provisioning** — SCIM 2.0 from the IdP to GitHub
3. **Authorization** — RBAC from enterprise → org → team → repo
4. **Machine identity** — GitHub Apps, OIDC federation, fine-grained PATs

At 10k+ developers, manual account / access management is non-viable. SSO + SCIM are mandatory, not optional.

---

## Configuration

### SSO

- Configure at the **enterprise** level for EMU; at the **org** level for non-EMU.
- OIDC preferred for new EMU deployments; SAML well-established universally.
- Enforce **SSO-authorized sessions** for all API access — otherwise pre-SSO PATs bypass the IdP.

### SCIM

- Configure SCIM in the IdP, pointed at GitHub's SCIM endpoint.
- Handles user creation, attribute sync, **deprovisioning**.
- Use **SCIM team sync** (or IdP group → team mapping) to drive team membership.

### RBAC layers

```
Enterprise Owner
└── Organization Owner
    └── Security Manager (read-only sec alerts)
        └── Org Member
            └── Repo roles (Admin, Maintain, Write, Triage, Read)
                └── Team membership
```

Effective permission = most permissive grant across all layers.

### Tokens

- **Fine-grained PATs** preferred — repo-scoped, expirable.
- Org-level policy: require approval for fine-grained PATs with write access; max-expiry policy.
- **Classic PATs** — phase out; audit and revoke.
- Service identity: **GitHub Apps** with installation tokens, not user PATs.

### Keys

- Enforce SSH key expiration policies.
- Require **GPG / Sigstore commit signing** where compliance mandates it.

---

## Usage

- All human auth via SSO; no GitHub-managed passwords.
- SCIM is the sole provisioning mechanism — not GitHub email invites.
- Team membership flows from IdP groups; repo access is granted via teams, not user-to-repo.
- External collaborators by exception, with documented justification + expiry.

---

## Best Practices

- IdP-group → GitHub-team sync from day 1.
- Separate **Security Manager** from **Org Owner**.
- Quarterly access reviews automated via API / audit-log streaming.
- One **GitHub App or service account per system**, scoped to minimum repos / permissions.
- Document and test the **SSO break-glass / recovery code** procedure.

---

## Common Pitfalls

- SCIM provisions but never tested for **deprovisioning** — JML failure waiting for an audit.
- Classic PATs with no expiry, broad `repo` scope, lingering after employees leave.
- Every senior dev given Org Owner — should be a tiny set of platform engineers.
- External collaborators retained after project end.
- SSO enforced without documenting recovery → IdP outage = total lockout.

---

## Implementation Notes

- For GHES SAML: detect IdP signing-cert rotation **before** it causes an outage.
- GitHub SCIM is strict about attribute formats — test in a staging org.
- Large IdP group changes can lag SCIM team sync at scale; batch off-peak.
- Build an **internal access self-service portal** that drives IdP group changes.

---

## Sources

- [Authentication and identity management for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management)
- [Configuring SAML SSO for your enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-saml-for-enterprise-iam/about-saml-for-enterprise-iam)
- [About SCIM for organizations](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/provisioning-user-accounts-with-scim/about-scim-for-organizations)
- [Roles in an enterprise](https://docs.github.com/en/enterprise-cloud@latest/admin/managing-accounts-and-repositories/managing-users-in-your-enterprise/roles-in-an-enterprise)
- [Custom organization roles](https://docs.github.com/en/enterprise-cloud@latest/organizations/managing-peoples-access-to-your-organization-with-roles/about-custom-organization-roles)
- [Custom repository roles](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/managing-repository-settings/managing-custom-repository-roles)
- [Setting a personal access token policy](https://docs.github.com/en/enterprise-cloud@latest/organizations/managing-programmatic-access-to-your-organization/setting-a-personal-access-token-policy-for-your-organization)
- [About Enterprise Managed Users](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/managing-iam-with-enterprise-managed-users/about-enterprise-managed-users)
- [Security Managers role](https://docs.github.com/en/organizations/managing-peoples-access-to-your-organization-with-roles/managing-security-managers-in-your-organization)
- [Authenticating as a GitHub App installation](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/authenticating-as-a-github-app-installation)
