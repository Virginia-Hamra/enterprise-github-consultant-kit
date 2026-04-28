# Identity Onboarding Working Session

> **Duration:** 2–3h &nbsp; | &nbsp; **Driver:** customer IAM engineer &nbsp; | &nbsp; **Co-driver:** consultant

## Objective
Configure and enforce SAML SSO + SCIM provisioning between the customer IdP and the GitHub enterprise.

## Definition of Done
- [ ] SAML SSO enabled & enforced at enterprise level
- [ ] SCIM provisioning configured & validated with at least one test user
- [ ] IdP groups mapped to GitHub teams (≥ 1 group)
- [ ] Break-glass account documented & secured
- [ ] PAT / SSH policy applied

## Pre-requisites
- [ ] Enterprise owner access available
- [ ] IdP admin access available (Entra / Okta / Ping)
- [ ] Test user account in IdP
- [ ] Change ticket approved
- [ ] Rollback plan: documented break-glass + ability to revert SAML config

## Step-by-step
1. Configure SAML in IdP (entityID, SSO URL, certificate)
2. Configure SAML on GitHub enterprise; test with single user (do **not** enforce yet)
3. Verify successful sign-in + assertion claims
4. Configure SCIM endpoint + token in IdP
5. Provision test user → confirm in GitHub
6. Map IdP group → GitHub team; verify membership sync
7. Stage break-glass: create local owner, store credential in vault, document procedure
8. Enforce SSO at enterprise level
9. Apply PAT / SSH policy

## Validation
- [ ] Test user can sign in via SSO
- [ ] Test user provisioned via SCIM
- [ ] Test user assigned to correct team via group sync
- [ ] Break-glass account works with SSO bypass
- [ ] PAT creation respects new policy

## Rollback
1. Disable SSO enforcement at enterprise
2. (If needed) remove SAML config
3. Disable SCIM token in IdP

## Risks
- Lockout from misconfigured SAML → mitigated by break-glass + non-enforcement testing first
- Group mapping error → only map one group in this session; expand later
