# Governance Enforcer Skill

## Purpose
Automated enforcement of GitHub Enterprise governance policies, compliance requirements, and organizational standards. This skill supports **Platform Engineers**, **Compliance Officers**, and **Security Teams** in maintaining consistent, compliant GitHub configurations at scale.

## Role Context
Use this skill when implementing policy-as-code, automating governance enforcement, or maintaining compliance across large GitHub Enterprise deployments.

## Capabilities

### Policy-as-Code Implementation
- **Repository Policies**: Automated branch protection, required workflows, naming standards
- **Organization Policies**: Creation restrictions, forking policies, default permissions
- **Enterprise Policies**: Global settings, SSO enforcement, audit log retention
- **Custom Policies**: Business-specific rules and constraints
- **Policy Versioning**: Track policy changes over time

### Automated Compliance Enforcement
- **Continuous Compliance**: Monitor and remediate drift from policies
- **Compliance Scanning**: Regular audits of org/repo configurations
- **Remediation Automation**: Auto-fix non-compliant configurations
- **Exception Management**: Approved exceptions and waivers
- **Compliance Reporting**: Evidence generation for auditors

### Configuration Management
- **Desired State Configuration**: Define and maintain target state
- **Drift Detection**: Identify configuration changes
- **Automated Remediation**: Restore compliant configuration
- **Change Tracking**: Audit trail of all configuration changes
- **Bulk Operations**: Apply settings across multiple repos/orgs

### Quality Gates & Controls
- **PR Review Requirements**: Enforce code review policies
- **CI Check Requirements**: Mandate security scans and tests
- **Deployment Approvals**: Environment-specific approval workflows
- **Merge Restrictions**: Control who can merge to protected branches
- **Signed Commits**: Require GPG/SSH signature verification

### Monitoring & Alerting
- **Policy Violations**: Real-time detection and alerting
- **Configuration Drift**: Alert on unauthorized changes
- **Compliance Metrics**: Track adherence over time
- **Audit Log Monitoring**: Suspicious activity detection
- **Remediation Tracking**: Monitor fix implementation

## Usage Scenarios

### Scenario 1: Repository Policy Enforcement
**Objective**: Ensure all repositories meet organization security standards

```bash
# Define repository policies
./scripts/define-repo-policies.sh --template policies/secure-repo-baseline.yaml --org <org-name>

# Apply policies to existing repositories
./scripts/apply-repo-policies.sh --policy policies/secure-repo-baseline.yaml --org <org-name> --dry-run

# Continuous enforcement
./scripts/enforce-policies.sh --policy policies/secure-repo-baseline.yaml --org <org-name> --schedule "0 */6 * * *"
```

**Policy Example** (`secure-repo-baseline.yaml`):
```yaml
repository_policies:
  branch_protection:
    pattern: "main"
    required_reviews: 2
    dismiss_stale_reviews: true
    require_code_owner_reviews: true
    required_status_checks:
      strict: true
      contexts:
        - "ci/build"
        - "security/codeql"
    enforce_admins: true

  security_features:
    secret_scanning: enabled
    push_protection: enabled
    dependabot_alerts: enabled
    dependabot_security_updates: enabled
    code_scanning_default_setup: enabled

  repository_settings:
    has_issues: true
    has_wiki: false
    has_projects: false
    allow_squash_merge: true
    allow_merge_commit: false
    allow_rebase_merge: false
    delete_branch_on_merge: true

  required_files:
    - "README.md"
    - "SECURITY.md"
    - "CODEOWNERS"
    - ".github/workflows/ci.yml"
```

### Scenario 2: Compliance Drift Detection
**Objective**: Detect and remediate policy violations automatically

```bash
# Scan for compliance drift
./scripts/detect-compliance-drift.sh --org <org-name> --policies policies/ --output drift-report.json

# Auto-remediate drift
./scripts/remediate-drift.sh --drift-report drift-report.json --auto-fix --notify
```

**Drift Detection**:
- Branch protection disabled or weakened
- Security features turned off
- Unauthorized admin access grants
- Policy exceptions expired
- Required files missing

### Scenario 3: Organization-Wide Policy Rollout
**Objective**: Deploy new governance policies across all organizations in enterprise

```bash
# Test policy on pilot org
./scripts/test-policy-rollout.sh --policy policies/new-policy.yaml --org pilot-org --validate

# Staged rollout to production
./scripts/rollout-policy.sh --policy policies/new-policy.yaml --enterprise <slug> --stage-by-org --delay 24h
```

**Rollout Strategy**:
1. Validate policy syntax
2. Test on pilot organization
3. Review impact analysis
4. Staged rollout to production orgs
5. Monitor for issues
6. Generate compliance report

### Scenario 4: Exception Management
**Objective**: Track and manage approved policy exceptions

```bash
# Request policy exception
./scripts/request-exception.sh --repo owner/repo --policy branch_protection --justification "Legacy system migration" --duration 90d

# Review and approve exception
./scripts/approve-exception.sh --exception-id 12345 --approver john.doe@company.com

# Monitor exception expiry
./scripts/monitor-exceptions.sh --org <org-name> --alert-days-before-expiry 7
```

**Exception Workflow**:
1. Developer requests exception with justification
2. Security/compliance team reviews
3. Approval or denial with comments
4. Temporary exception granted with expiration
5. Automated alerts before expiration
6. Auto-remediation when exception expires

### Scenario 5: Compliance Reporting
**Objective**: Generate compliance evidence for audits

```bash
# Generate compliance report
./scripts/generate-compliance-report.sh --org <org-name> --framework soc2 --output compliance-report.html

# Export compliance metrics
./scripts/export-compliance-metrics.sh --enterprise <slug> --format csv --output metrics.csv
```

**Report Contents**:
- Policy compliance rate by org/repo
- Open violations and remediation status
- Exception tracking and approvals
- Configuration change audit trail
- Security posture trends

## Scripts

### `scripts/define-repo-policies.sh`
Creates repository policy definitions from templates.

**Usage**:
```bash
./define-repo-policies.sh --template <file> --org <name> [--customize]
```

**Templates Available**:
- `secure-repo-baseline.yaml`: Standard security controls
- `compliance-soc2.yaml`: SOC 2 requirements
- `compliance-iso27001.yaml`: ISO 27001 requirements
- `inner-source.yaml`: Internal collaboration policies
- `open-source.yaml`: Public repository policies

### `scripts/apply-repo-policies.sh`
Applies policies to repositories.

**Usage**:
```bash
./apply-repo-policies.sh --policy <file> --org <name> [--repos pattern] [--dry-run] [--force]
```

**Options**:
- `--repos pattern`: Apply to specific repos (glob pattern)
- `--dry-run`: Show changes without applying
- `--force`: Override existing conflicting settings
- `--exclude`: Exclude repos from policy application

### `scripts/enforce-policies.sh`
Continuous policy enforcement with scheduled runs.

**Usage**:
```bash
./enforce-policies.sh --policy <file> --org <name> --schedule <cron> [--remediate] [--alert]
```

**Enforcement Actions**:
- Detect violations
- Log findings
- Send alerts (Slack, email, PagerDuty)
- Auto-remediate (if enabled)
- Generate reports

### `scripts/detect-compliance-drift.sh`
Scans for configuration drift from defined policies.

**Usage**:
```bash
./detect-compliance-drift.sh --org <name> --policies <dir> --output <file> [--severity high|medium|low]
```

**Drift Categories**:
- Branch protection modifications
- Security feature disabled
- Access control changes
- Repository settings altered
- Required files missing or modified

### `scripts/remediate-drift.sh`
Remediates detected policy violations.

**Usage**:
```bash
./remediate-drift.sh --drift-report <file> [--auto-fix] [--approve-required] [--notify]
```

**Remediation Process**:
1. Review drift report
2. Categorize by severity
3. Request approval (if required)
4. Apply remediations
5. Verify fixes
6. Notify stakeholders

### `scripts/test-policy-rollout.sh`
Tests policy changes before production rollout.

**Usage**:
```bash
./test-policy-rollout.sh --policy <file> --org <pilot-org> --validate [--impact-analysis]
```

**Validation Steps**:
- Syntax validation
- Logical consistency checks
- Impact analysis (affected repos)
- Test application to pilot org
- Generate rollback plan

### `scripts/rollout-policy.sh`
Executes staged policy rollout across enterprise.

**Usage**:
```bash
./rollout-policy.sh --policy <file> --enterprise <slug> [--stage-by-org] [--delay <duration>] [--rollback-on-error]
```

**Rollout Stages**:
1. Pre-rollout validation
2. Impact communication
3. Staged application
4. Monitoring and validation
5. Issue remediation
6. Post-rollout reporting

### `scripts/request-exception.sh`
Submits policy exception request.

**Usage**:
```bash
./request-exception.sh --repo <owner/repo> --policy <policy-id> --justification <text> --duration <days>
```

**Exception Request Fields**:
- Repository or organization
- Policy being excepted
- Business justification
- Requested duration
- Risk assessment
- Compensating controls

### `scripts/approve-exception.sh`
Approves or denies exception requests.

**Usage**:
```bash
./approve-exception.sh --exception-id <id> --approver <email> [--deny] [--comments <text>]
```

**Approval Workflow**:
- Security team review
- Risk assessment validation
- Approval or denial with comments
- Notification to requester
- Exception tracking in database

### `scripts/monitor-exceptions.sh`
Monitors active exceptions and alerts on expiry.

**Usage**:
```bash
./monitor-exceptions.sh --org <name> [--alert-days-before-expiry N] [--auto-remediate-expired]
```

**Monitoring**:
- List active exceptions
- Alert on upcoming expiration
- Auto-remediate expired exceptions
- Generate exception reports

### `scripts/generate-compliance-report.sh`
Generates comprehensive compliance reports.

**Usage**:
```bash
./generate-compliance-report.sh --org <name> --framework <soc2|iso27001> --output <file> [--evidence]
```

**Report Sections**:
- Executive summary
- Policy compliance dashboard
- Violation details
- Remediation status
- Exception tracking
- Compliance trends
- Evidence artifacts

### `scripts/export-compliance-metrics.sh`
Exports compliance metrics for analysis.

**Usage**:
```bash
./export-compliance-metrics.sh --enterprise <slug> --format <csv|json> --output <file> [--date-range 30d]
```

**Metrics**:
- Compliance rate by policy
- Violations by severity
- Mean time to remediation
- Exception approval rate
- Policy coverage percentage

## Policy Definition Examples

### Branch Protection Policy
```yaml
policy_id: branch-protection-main
description: Standard branch protection for main branch
applies_to:
  repositories: all
  branches: [main, master]

requirements:
  required_pull_request_reviews:
    required_approving_review_count: 2
    dismiss_stale_reviews: true
    require_code_owner_reviews: true
    require_last_push_approval: true

  required_status_checks:
    strict: true
    checks:
      - context: "ci/build"
        app_id: "github-actions"
      - context: "security/codeql"
        app_id: "github-code-scanning"

  restrictions:
    users: []
    teams: ["platform-team"]
    apps: []

  enforce_admins: true
  required_linear_history: false
  allow_force_pushes: false
  allow_deletions: false
  required_conversation_resolution: true
  lock_branch: false
  allow_fork_syncing: false
```

### Security Features Policy
```yaml
policy_id: security-features-standard
description: Mandatory security features for all repositories
applies_to:
  repositories: all
  visibility: [private, internal, public]

requirements:
  secret_scanning:
    enabled: true
    push_protection: true

  dependabot:
    alerts: enabled
    security_updates: enabled
    version_updates: optional

  code_scanning:
    default_setup: enabled
    tools: [codeql]
    query_suite: security-extended

  vulnerability_reporting:
    private_vulnerability_reporting: enabled
    security_policy_file: required
```

### Repository Settings Policy
```yaml
policy_id: repo-settings-standard
description: Standard repository configuration
applies_to:
  repositories: all

requirements:
  general:
    has_issues: true
    has_wiki: false
    has_projects: false
    has_discussions: optional

  merge_options:
    allow_squash_merge: true
    allow_merge_commit: false
    allow_rebase_merge: false
    squash_merge_commit_title: PR_TITLE
    squash_merge_commit_message: PR_BODY
    delete_branch_on_merge: true

  pull_request_settings:
    allow_auto_merge: false
    allow_update_branch: true

  required_files:
    - path: "README.md"
      required: true
    - path: "SECURITY.md"
      required: true
    - path: "CODEOWNERS"
      required: true
    - path: "LICENSE"
      required: optional
```

## Best Practices

### Policy Design
- **Start simple**: Begin with essential policies, expand over time
- **Version policies**: Track changes with semantic versioning
- **Test thoroughly**: Validate on pilot before production
- **Document exceptions**: Clear criteria for granting exceptions
- **Balance security and usability**: Overly restrictive policies reduce adoption

### Enforcement Strategy
- **Gradual rollout**: Phase in policies to minimize disruption
- **Clear communication**: Explain why policies exist and how to comply
- **Provide tools**: Give developers easy ways to check compliance
- **Automate remediation**: Fix common issues automatically
- **Monitor impact**: Track policy effectiveness and developer friction

### Exception Management
- **Clear approval process**: Define who can approve exceptions
- **Time-limited exceptions**: All exceptions should expire
- **Require justification**: Document business need for exception
- **Compensating controls**: Require alternative security measures
- **Regular review**: Audit exceptions quarterly

### Reporting & Metrics
- **Track trends**: Monitor compliance over time
- **Identify patterns**: Find common violation types
- **Measure efficiency**: Time to detect and remediate violations
- **Celebrate success**: Recognize teams with high compliance
- **Continuous improvement**: Use data to refine policies

## Deliverables Template

When implementing governance enforcement, provide:

1. **Governance Framework Document**
   - Policy hierarchy and structure
   - Roles and responsibilities
   - Exception process
   - Escalation procedures

2. **Policy Definitions**
   - Baseline security policies
   - Compliance-specific policies
   - Organization custom policies
   - Policy versioning and changelog

3. **Enforcement Automation**
   - Policy-as-code implementations
   - Drift detection scripts
   - Auto-remediation workflows
   - Monitoring and alerting setup

4. **Compliance Dashboard**
   - Real-time compliance metrics
   - Violation tracking
   - Exception management
   - Audit-ready reporting

5. **Operational Runbook**
   - Policy update procedures
   - Exception request process
   - Incident response for violations
   - Regular audit procedures

## Integration with Other Skills

- **Security Audit**: Policies enforce security controls identified in audits
- **Architecture Analysis**: Org structure informs policy boundaries
- **DevOps Review**: CI/CD policies integrated with workflow requirements
- **Migration Planner**: Apply governance during migration

## Customer Engagement Tips

### Discovery Questions
- "What governance policies currently exist?"
- "Who is responsible for compliance?"
- "What are your biggest compliance challenges?"
- "How do you currently detect policy violations?"
- "What is your risk tolerance for non-compliance?"

### Handling Resistance
- **Explain the why**: Connect policies to business risk
- **Show flexibility**: Demonstrate exception process
- **Provide support**: Help teams become compliant
- **Quick wins**: Start with easy, high-impact policies
- **Measure success**: Show reduction in security incidents

### Red Flags
- No documented policies
- Manual, inconsistent enforcement
- Excessive exceptions granted
- No monitoring or alerting
- Policies created but never enforced
- Developers unaware of policies

## Reference Materials

- [GitHub Enterprise Policies](https://docs.github.com/en/enterprise-cloud@latest/admin/policies)
- [Branch Protection Rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [Terraform GitHub Provider](https://registry.terraform.io/providers/integrations/github/latest/docs)
- [Governance Best Practices](https://resources.github.com/security/governance/)
