# Identity & Access Questionnaire

> **Audience:** IAM / Security &nbsp; | &nbsp; **Time:** ~45 min

## Identity provider
1. Primary IdP (Entra ID / Okta / Ping / other)?
2. SAML SSO enabled at enterprise or org level? Enforced?
3. SCIM provisioning configured? Source of truth for membership?
4. Authentication factors required (MFA, FIDO2, conditional access)?

## Account model
1. Standard accounts vs Enterprise Managed Users (EMU)?
2. Outside collaborator policy? Approval workflow?
3. Bot / service accounts — how are they managed?
4. Emergency access ("break-glass") procedure?

## Tokens & credentials
1. Personal Access Token policy (classic vs fine-grained, expiration, allowed scopes)?
2. SSH key policy (rotation, hardware-backed)?
3. GitHub App vs OAuth App preference for automation?
4. OIDC trust relationships (which clouds, which orgs)?

## Permissions & teams
1. Team structure — IdP-synced or manually managed?
2. Base permissions on orgs?
3. CODEOWNERS adoption?
4. Repository role customization?

## Artifacts requested
- [ ] SAML/SCIM configuration screenshots
- [ ] PAT/SSH policy docs
- [ ] Team-to-IdP-group mapping (if any)
