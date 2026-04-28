# Security Baseline Checklist

Owner: Security Advisor. Validates secure-by-default posture.

## Authentication
- [ ] SSO enforced at enterprise level
- [ ] 2FA enforced (or EMU equivalent)
- [ ] Conditional access policies aligned with corp standard
- [ ] PATs restricted (fine-grained, expiration max ≤ 90 days)
- [ ] SSH certificate authority configured (if required)

## Authorization
- [ ] Base permissions minimized
- [ ] Org owner count minimized & reviewed
- [ ] Custom roles defined where built-ins insufficient
- [ ] CODEOWNERS required on protected branches

## Repository protection
- [ ] Rulesets deployed: required reviews, status checks, signed commits, linear history
- [ ] Force-push and branch deletion restricted
- [ ] Bypass list reviewed
- [ ] Tag protection for releases

## Code security
- [ ] Secret scanning + push protection enabled org-wide
- [ ] Dependabot alerts + security updates enabled
- [ ] CodeQL default setup or advanced configured
- [ ] Custom secret patterns added (if applicable)

## Supply chain
- [ ] Allowed Actions policy: verified creators or curated allowlist
- [ ] Default `GITHUB_TOKEN` permissions = read
- [ ] OIDC trust policies reviewed (least privilege)
- [ ] Third-party Apps reviewed

## Audit & response
- [ ] Audit log streaming validated end-to-end
- [ ] Alert rules for: SSO disable, owner add, secret found, ruleset bypass
- [ ] Incident response runbook exists & tested
