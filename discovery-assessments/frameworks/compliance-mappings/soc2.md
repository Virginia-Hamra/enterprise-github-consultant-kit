# SOC 2 → GitHub Enterprise Control Mapping

> **Disclaimer:** Guidance only. Validate with customer auditors.

| TSC | Criterion (abbrev.) | GitHub control(s) | Evidence artifact |
|---|---|---|---|
| CC6.1 | Logical access — restrict | SAML SSO, SCIM, base permissions, CODEOWNERS | SSO config, SCIM logs, ruleset export |
| CC6.2 | New user access | SCIM provisioning, role assignment via IdP groups | IdP → team mapping, audit log |
| CC6.3 | Access modification / removal | SCIM deprovisioning, audit log | Audit log queries, access review reports |
| CC6.6 | External authentication | SSO enforcement, EMU | SSO settings, EMU config |
| CC6.7 | Restrict transmission | TLS enforced, signed commits | Repo settings, ruleset export |
| CC6.8 | Prevent / detect malicious software | Push protection, GHAS, allowed Actions policy | GHAS reports, policy config |
| CC7.1 | Detection of vulnerabilities | CodeQL, Dependabot, secret scanning | Scan reports, alert dashboards |
| CC7.2 | Monitoring & analysis | Audit log streaming → SIEM | Streaming config, SIEM dashboards |
| CC7.3 | Incident response | IR runbook, alert rules | Runbook, tabletop notes |
| CC7.4 | Mitigation | Branch protection, rulesets, push protection | Ruleset export, blocked-push samples |
| CC8.1 | Change management | PR + required reviews + CODEOWNERS + status checks | Ruleset, sample PR audit |

## Notes
- CODEOWNERS without `require_code_owner_review` does not satisfy CC8.1 review controls.
- Audit log retention should match SOC 2 retention period (typically ≥ 1 year).
- Bypass list on rulesets must be reviewed for CC8.1 effectiveness.
