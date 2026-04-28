# Security & Compliance Questionnaire

> **Audience:** Security / GRC &nbsp; | &nbsp; **Time:** ~60 min

## Compliance scope
1. Frameworks in scope (SOC 2, ISO 27001, PCI-DSS, HIPAA, FedRAMP, NIST 800-53)?
2. Audit cadence and upcoming audit dates?
3. Evidence collection process today?

## GHAS posture
1. GHAS licensed? Coverage (% of repos)?
2. Secret scanning + push protection enabled? Custom patterns?
3. Code scanning (CodeQL) — default setup or advanced? Languages covered?
4. Dependabot — alerts, security updates, version updates?
5. Triage SLAs by severity?

## Branch & repo protection
1. Required reviews, status checks, signed commits?
2. CODEOWNERS enforcement?
3. Rulesets vs classic protection?
4. Repository creation — open or restricted?

## Audit & logging
1. Audit log streaming destination (Splunk, Sentinel, S3, Datadog)?
2. Retention period?
3. Alerting on critical events?

## Incident response
1. IR playbook for leaked secrets / compromised account / malicious Action?
2. Contacts (security@, on-call)?

## Artifacts requested
- [ ] Current GHAS coverage report
- [ ] Branch protection / ruleset export
- [ ] Audit log streaming config
