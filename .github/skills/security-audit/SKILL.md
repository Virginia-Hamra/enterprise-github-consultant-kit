# Security Audit Skill

## Purpose
Comprehensive security assessment and compliance validation for enterprise GitHub implementations. This skill supports **Security Engineers**, **Compliance Officers**, and **Auditors** in establishing and validating security controls.

## Role Context
Use this skill when conducting security assessments, preparing for compliance audits, or validating security posture of GitHub Enterprise platforms.

## Capabilities

### Platform Security Assessment
- **Identity & Access Management**: SSO/SAML configuration, SCIM provisioning, MFA enforcement
- **Enterprise Policies**: Repository creation restrictions, forking policies, GitHub Pages controls
- **Audit Logging**: Enterprise audit log configuration, retention, and SIEM integration
- **IP Allow Lists**: Network access controls and VPN requirements
- **API Security**: Personal access token usage, OAuth app restrictions, API rate limiting

### Repository Security Audit
- **Branch Protection**: Required reviews, status checks, signed commits, force push prevention
- **Code Scanning**: CodeQL coverage, custom queries, alert management
- **Secret Scanning**: Push protection, custom patterns, alert resolution
- **Dependency Management**: Dependabot configuration, vulnerability alert resolution
- **CODEOWNERS**: Proper configuration and enforcement

### Workflow Security Review
- **Permissions**: Minimal `permissions:` blocks, token scoping
- **Action Pinning**: SHA pinning vs tag usage, third-party Action allowlists
- **Secret Handling**: GitHub Secrets usage, OIDC configuration, no hardcoded secrets
- **Untrusted Input**: `pull_request_target` safety, user input sanitization
- **Environment Protection**: Environment-specific protections, required approvers

### Compliance Mapping
- **SOC 2**: Access controls, change management, monitoring, incident response
- **ISO 27001**: Information security controls, risk management
- **NIST Cybersecurity Framework**: Identify, Protect, Detect, Respond, Recover
- **GDPR**: Data protection, privacy, right to be forgotten
- **HIPAA**: Data encryption, access controls, audit logging
- **PCI DSS**: Network security, access control, monitoring

### Vulnerability Management
- **Dependency Vulnerabilities**: Track and remediate Dependabot alerts
- **Code Vulnerabilities**: CodeQL findings review and remediation tracking
- **Container Security**: Image scanning, base image vulnerabilities
- **Infrastructure Vulnerabilities**: IaC security scanning results
- **Third-Party Risks**: External Action and package assessment

## Usage Scenarios

### Scenario 1: Enterprise Platform Security Audit
**Objective**: Comprehensive security assessment of GitHub Enterprise Cloud configuration

```bash
# Run platform security audit
./scripts/audit-enterprise-platform.sh <enterprise-slug> --output report.json

# Generate executive summary
./scripts/generate-security-summary.sh --audit-report report.json --format pdf
```

**Audit Areas**:
- Enterprise-level security settings
- Organization security configurations
- SSO/SCIM configuration validation
- IP allow list verification
- Audit log configuration
- Enterprise billing and usage

**Output**: Security findings report with risk ratings and remediation recommendations

### Scenario 2: Repository Security Assessment
**Objective**: Audit individual repository security configurations

```bash
# Audit single repository
./scripts/audit-repository.sh owner/repo --comprehensive

# Bulk audit organization repositories
./scripts/audit-org-repositories.sh <org-name> --output audit-results/
```

**Checks**:
- Branch protection completeness
- Security feature enablement
- Secret scanning coverage
- Dependency security
- Workflow security posture
- CODEOWNERS validation

### Scenario 3: Compliance Evidence Generation
**Objective**: Generate compliance evidence for SOC 2 audit

```bash
# Generate SOC 2 compliance evidence
./scripts/generate-compliance-evidence.sh --framework soc2 --org <org-name> --output evidence/

# Create compliance mapping report
./scripts/map-controls-to-github.sh --framework soc2 --evidence-dir evidence/
```

**Evidence Generated**:
- Access control screenshots
- Audit log exports
- Change management evidence (PR reviews, approvals)
- Monitoring and alerting configurations
- Incident response procedures
- Security policy documentation

### Scenario 4: Vulnerability Remediation Tracking
**Objective**: Track and report on vulnerability remediation status

```bash
# Generate vulnerability report
./scripts/report-vulnerabilities.sh --org <org-name> --severity high,critical --output vulns.csv

# Track remediation SLAs
./scripts/track-remediation-slas.sh --vulns vulns.csv --sla-days 30
```

**Metrics Tracked**:
- Open vulnerabilities by severity
- Mean time to remediation
- SLA compliance rate
- Vulnerability trends over time
- Top vulnerable repositories

## Scripts

### `scripts/audit-enterprise-platform.sh`
Comprehensive security audit of GitHub Enterprise platform.

**Usage**:
```bash
./audit-enterprise-platform.sh <enterprise-slug> [--output json|html] [--compliance-framework soc2|iso27001]
```

**Checks**:
- Enterprise security policies
- Organization security settings
- SSO/SAML configuration
- SCIM provisioning status
- IP allow lists
- Audit log configuration
- GitHub Connect settings (GHES)

### `scripts/audit-repository.sh`
Detailed security assessment of a single repository.

**Usage**:
```bash
./audit-repository.sh <owner/repo> [--comprehensive] [--fix-issues]
```

**Options**:
- `--comprehensive`: Deep scan including workflow analysis
- `--fix-issues`: Automatically fix certain issues (e.g., enable Dependabot)

**Output**: Security scorecard with pass/fail for each control

### `scripts/audit-org-repositories.sh`
Bulk security audit across all organization repositories.

**Usage**:
```bash
./audit-org-repositories.sh <org-name> [--parallel 5] [--output dir]
```

**Features**:
- Parallel repository scanning
- Progress tracking
- Aggregated findings report
- Risk prioritization

### `scripts/generate-compliance-evidence.sh`
Generates compliance evidence packages for audits.

**Usage**:
```bash
./generate-compliance-evidence.sh --framework <soc2|iso27001|nist> --org <name> --output <dir> [--date-range 90d]
```

**Evidence Types**:
- Configuration exports
- Audit log samples
- Policy documentation
- Access control matrices
- Change management records
- Incident response logs

### `scripts/map-controls-to-github.sh`
Maps compliance framework controls to GitHub capabilities.

**Usage**:
```bash
./map-controls-to-github.sh --framework <name> [--evidence-dir <dir>]
```

**Output**: Control matrix showing GitHub implementation of each required control

### `scripts/audit-workflow-security.sh`
Security-focused audit of GitHub Actions workflows.

**Usage**:
```bash
./audit-workflow-security.sh <path-to-workflows> [--strict]
```

**Security Checks**:
- Unpinned Actions (should be SHA-pinned)
- Overly permissive `permissions` blocks
- Use of `pull_request_target` without safeguards
- Hardcoded secrets or tokens
- Missing timeout limits
- Untrusted input in dangerous contexts

### `scripts/report-vulnerabilities.sh`
Generates vulnerability reports from Dependabot and CodeQL alerts.

**Usage**:
```bash
./report-vulnerabilities.sh --org <name> --severity <high,critical> [--output csv|json]
```

**Requires**: `gh` CLI with security alert read permissions

**Metrics**:
- Total vulnerabilities by severity
- Vulnerabilities by repository
- Age of open vulnerabilities
- Remediation trends

### `scripts/track-remediation-slas.sh`
Tracks vulnerability remediation against SLA targets.

**Usage**:
```bash
./track-remediation-slas.sh --vulns <report> --sla-days <N> [--alert-overdue]
```

**SLA Tracking**:
- Critical: 7 days
- High: 30 days
- Medium: 90 days
- Low: No SLA

### `scripts/scan-for-secrets.sh`
Scans repositories for accidentally committed secrets.

**Usage**:
```bash
./scan-for-secrets.sh <repo-path> [--custom-patterns patterns.txt]
```

**Uses**: TruffleHog, GitLeaks, or similar secret scanning tools

**Output**: List of potential secrets with file locations

## Compliance Framework Mapping

### SOC 2 Type II Controls

| Control Area | GitHub Capability | Evidence |
|--------------|------------------|-----------|
| CC6.1 - Logical Access | SSO/SAML, SCIM, MFA | Identity provider config, user audit |
| CC6.2 - Authentication | SSO enforcement, MFA required | Enterprise policy settings |
| CC6.3 - Authorization | RBAC, teams, repository permissions | Access control matrix |
| CC7.2 - Change Management | Branch protection, PR reviews, CODEOWNERS | Pull request history, audit logs |
| CC7.3 - Quality Assurance | CI checks, CodeQL, Dependabot | Workflow runs, security alerts |
| CC8.1 - Logging | Enterprise audit logs, SIEM integration | Audit log exports |
| CC9.1 - Risk Mitigation | Security scanning, branch protection | Security dashboard |

### ISO 27001 Controls

| Control | GitHub Implementation |
|---------|---------------------|
| A.9.2.1 User Registration | SCIM provisioning |
| A.9.2.2 User Access Provisioning | Team-based RBAC |
| A.9.2.4 Secret Authentication | MFA enforcement, SSH keys |
| A.9.4.1 Information Access Restriction | Repository visibility, branch protection |
| A.12.1.2 Change Management | Pull requests, approvals, audit trail |
| A.12.6.1 Technical Vulnerabilities | Dependabot, CodeQL |
| A.18.1.1 Statutory Requirements | Compliance evidence generation |

### NIST Cybersecurity Framework

| Function | Category | GitHub Capability |
|----------|----------|------------------|
| Identify | Asset Management | Repository inventory, dependency tracking |
| Protect | Access Control | SSO, SCIM, MFA, RBAC |
| Protect | Data Security | Secret scanning, push protection |
| Detect | Security Monitoring | Audit logs, SIEM integration |
| Detect | Vulnerability Detection | CodeQL, Dependabot, secret scanning |
| Respond | Incident Response | Alert notifications, repository lockdown |
| Recover | Recovery Planning | Backup procedures, repository restoration |

## Best Practices

### When Conducting Audits
- **Start with risk assessment**: Focus on high-risk areas first
- **Use automation**: Leverage scripts for repeatable audits
- **Document everything**: Screenshots, exports, configurations
- **Validate fixes**: Re-run audits after remediation
- **Track trends**: Monitor security posture over time

### When Generating Compliance Evidence
- **Collect contemporaneously**: Don't wait until audit time
- **Organize systematically**: Map evidence to specific controls
- **Include context**: Explain how GitHub implements each control
- **Version evidence**: Date and version all artifacts
- **Automate collection**: Use scripts to generate evidence regularly

### When Reporting Findings
- **Risk-based prioritization**: Focus on critical and high-severity issues
- **Provide remediation guidance**: Don't just report problems, suggest solutions
- **Include business context**: Explain security impact in business terms
- **Track remediation**: Follow up on fix implementation
- **Celebrate improvements**: Recognize security posture gains

## Deliverables Template

When completing a security audit, provide:

1. **Executive Summary**
   - Overall security posture rating
   - Critical findings requiring immediate attention
   - Compliance status overview
   - Remediation effort estimate

2. **Detailed Findings Report**
   - Findings by category and severity
   - Evidence and reproduction steps
   - Risk assessment and impact analysis
   - Remediation recommendations
   - Acceptance criteria for fixes

3. **Compliance Matrix**
   - Controls mapped to GitHub capabilities
   - Implementation status
   - Evidence references
   - Gaps and remediation plans

4. **Remediation Roadmap**
   - Prioritized action items
   - Effort estimates
   - Dependencies and prerequisites
   - Target completion timeline

5. **Security Baseline Configuration**
   - Terraform/API scripts for secure configuration
   - Repository policy templates
   - Workflow security templates
   - Monitoring and alerting setup

## Integration with Other Skills

- **DevOps Review**: Security findings inform CI/CD security requirements
- **Architecture Analysis**: Security architecture shapes security controls
- **Governance Enforcer**: Security policies become automated governance rules
- **Test Generator**: Security tests validate security control effectiveness

## Customer Engagement Tips

### Discovery Questions
- "What compliance frameworks are you subject to?"
- "Have you had any security incidents or breaches?"
- "What are your biggest security concerns with GitHub?"
- "Do you have existing security policies we need to comply with?"
- "What is your risk tolerance for different types of issues?"

### Handling Sensitive Findings
- **Report privately first**: Give customer time to remediate before wider distribution
- **Provide context**: Not all findings are created equal
- **Offer solutions**: Don't just identify problems
- **Be constructive**: Frame as opportunities for improvement
- **Follow up**: Verify fixes and provide support

### Red Flags
- No branch protection on production repositories
- Admin access widely granted
- Secrets in repository code
- No audit logging enabled
- Public repositories with sensitive code
- Disabled security features (Dependabot, secret scanning)

## Reference Materials

- [GitHub Security Best Practices](https://docs.github.com/en/code-security/getting-started/securing-your-organization)
- [Actions Security Hardening](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [Enterprise Audit Log Events](https://docs.github.com/en/enterprise-cloud@latest/admin/monitoring-activity-in-your-enterprise/reviewing-audit-logs-for-your-enterprise/audit-log-events-for-your-enterprise)
- [GitHub Compliance Resources](https://resources.github.com/security/compliance/)
