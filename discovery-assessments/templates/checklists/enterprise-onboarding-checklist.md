# Enterprise Onboarding Checklist

Foundation configuration for a new GitHub Enterprise tenant or new orgs within an existing tenant. Owner: Platform Engineer.

## Enterprise account
- [ ] Enterprise slug / display name confirmed
- [ ] Billing & licensing assigned
- [ ] Enterprise owners list reviewed (least privilege)
- [ ] Notification email & verified domains configured

## Identity
- [ ] SAML SSO configured & enforced (or EMU activated)
- [ ] SCIM provisioning configured
- [ ] IdP group → team mapping defined
- [ ] Break-glass account documented & secured
- [ ] PAT / SSH policy set
- [ ] OAuth/GitHub App approval policy set

## Organizations
- [ ] Org naming convention agreed
- [ ] Base permissions = `none` or `read` (justified)
- [ ] Repository creation restricted to admins or roles
- [ ] Default repo visibility set
- [ ] Outside collaborator policy set
- [ ] Member privilege policies set

## Repository defaults
- [ ] Default branch name standard
- [ ] Required ruleset(s) deployed
- [ ] CODEOWNERS template available
- [ ] SECURITY.md / README.md / LICENSE templates available

## Audit & telemetry
- [ ] Audit log streaming enabled to SIEM
- [ ] Retention policy confirmed
- [ ] Critical-event alerting configured

## Documentation
- [ ] Architecture diagram produced
- [ ] Runbook for common admin tasks
- [ ] Onboarding runbook for new orgs / teams
