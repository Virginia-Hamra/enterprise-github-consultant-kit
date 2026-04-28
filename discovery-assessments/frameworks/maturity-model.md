# GitHub Enterprise Maturity Model

A 5-level model (0–4) used across all scorecards. Each domain is scored independently.

| Level | Name | Definition | Indicators |
|---|---|---|---|
| 0 | Absent | Capability does not exist | No tooling / process; ad hoc only |
| 1 | Initial | Exists but ad hoc and undocumented | Tribal knowledge; results inconsistent |
| 2 | Repeatable | Documented but inconsistently applied | Some teams follow; gaps common |
| 3 | Defined | Standardized and broadly applied | Org-wide standard; enforced |
| 4 | Optimizing | Measured, automated, continuously improved | Metrics-driven; IaC; SLOs; quarterly review |

## Domain rubrics

### Identity & access
- **0**: No SSO, shared accounts.
- **1**: SSO exists, not enforced; manual onboarding.
- **2**: SSO enforced; SCIM partial.
- **3**: SSO + SCIM enforced; PAT policy; break-glass documented.
- **4**: IdP-driven team sync; phishing-resistant MFA; automated access reviews.

### Branch protection & rulesets
- **0**: None.
- **1**: Some repos protected manually.
- **2**: Documented standard; partial coverage.
- **3**: Org-wide rulesets; required reviews + checks + signed commits.
- **4**: Rulesets in IaC; bypass governed; drift detected & alerted.

### Secret scanning
- **0**: Disabled.
- **1**: Enabled on some repos; alerts unowned.
- **2**: Enabled org-wide; push protection on some repos; SLAs ad hoc.
- **3**: Push protection org-wide; custom patterns; SLAs enforced.
- **4**: Auto-remediation flows; metrics tracked; mean-time-to-remediate < SLO.

### Code scanning (CodeQL)
- **0**: Not used.
- **1**: Default setup on some repos.
- **2**: Default setup org-wide; advanced on key repos.
- **3**: Advanced configs in IaC; query suites tuned; triage SLAs.
- **4**: Custom queries; CI integration; FP rate measured & tuned.

### Actions adoption
- **0**: Not in use.
- **1**: Per-repo workflows; copy-paste sprawl.
- **2**: Some shared workflows; documented patterns.
- **3**: Reusable workflows + composite actions library; allowed-Actions policy enforced.
- **4**: Centralized library versioned; usage metrics; cost optimization; runner autoscaling.

### Audit & compliance
- **0**: No streaming.
- **1**: Logs accessible on demand only.
- **2**: Streaming to SIEM; basic retention.
- **3**: Streaming + alerting + retention per policy; periodic evidence collection.
- **4**: Evidence automated; control-to-framework mapping live; continuous compliance.

### Developer experience
- **0**: No standards; high friction.
- **1**: Some templates; informal patterns.
- **2**: Documented golden paths; patchy adoption.
- **3**: Self-service provisioning; templates + scaffolders; metrics tracked.
- **4**: DORA + DevEx surveys; quarterly improvements; toil < target.

### Copilot / AI tooling
- **0**: Not enabled.
- **1**: Pilot underway; minimal governance.
- **2**: Enabled with policy; content exclusions configured.
- **3**: Broad rollout; champion network; metrics dashboard.
- **4**: Custom instructions / prompt files / knowledge bases; coding agent governance; continuous measurement.
