# Current-State Platform Questionnaire

> **Audience:** Platform / GitHub admins &nbsp; | &nbsp; **Time:** ~45 min

## Section 1 — GitHub footprint
1. Which GitHub product(s) are in use? (GHEC / GHEC w/ Managed Users / GHES / GHAE)
   - Versions / tenant URLs:
2. Number of enterprises, organizations, repositories, members, outside collaborators?
3. License SKUs and seat counts (Enterprise, GHAS, Copilot Business/Enterprise)?
4. Are there multiple tenants? Why?

## Section 2 — Ownership & operations
1. Who owns the GitHub platform? (team, RACI)
2. Support model (tier 1/2/3, SLAs)?
3. Change management process for org-level settings?
4. Runbooks / on-call coverage?

## Section 3 — Integrations
1. Connected tools (Jira, ServiceNow, Snyk, Artifactory, etc.)?
2. GitHub Apps / OAuth Apps installed at enterprise/org level?
3. Webhooks consumers?

## Section 4 — Constraints
1. Network: IP allowlists, private connectivity, proxies?
2. Data residency requirements?
3. Regulatory frameworks in scope?

## Artifacts requested
- [ ] Org list export (`gh api /enterprises/{slug}/organizations`)
- [ ] Installed Apps inventory
- [ ] Current architecture diagram (if any)
