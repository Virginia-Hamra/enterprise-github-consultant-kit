# DevOps Review Skill

## Purpose
Comprehensive analysis and review of DevOps practices, CI/CD pipelines, and infrastructure configurations for enterprise GitHub migrations and platform implementations.

## Role Context
Use this skill when working as a **DevOps Consultant** or **Platform Engineer** to assess current state, identify risks, and design migration strategies for enterprise customers.

## Capabilities

### CI/CD Pipeline Analysis
- **GitHub Actions Workflows**: Analyze workflow syntax, permissions, security, and performance
- **Legacy CI Migration**: Assess Jenkins, CircleCI, GitLab CI, Azure DevOps pipelines for GitHub Actions migration
- **Security Scanning**: Review secret handling, OIDC usage, and Actions pinning strategies
- **Runner Configuration**: Evaluate runner requirements (GitHub-hosted vs self-hosted)
- **Workflow Optimization**: Identify parallelization opportunities, caching strategies, and performance bottlenecks

### Infrastructure as Code Review
- **Terraform**: Analyze modules, state management, provider configurations
- **CloudFormation/ARM**: Review templates for AWS, Azure infrastructure
- **Kubernetes**: Assess deployment manifests, Helm charts, and operators
- **Policy as Code**: Review OPA, Sentinel, or custom policy implementations
- **GitOps Readiness**: Evaluate suitability for ArgoCD, Flux integration

### Deployment Strategy Assessment
- **Environment Management**: Review dev/staging/prod environment configurations
- **Protection Rules**: Assess GitHub Environment protection rules and approval workflows
- **Rollback Capabilities**: Evaluate deployment rollback and disaster recovery procedures
- **Blue/Green & Canary**: Analyze progressive delivery strategies
- **Deployment Frequency**: Identify opportunities to increase deployment velocity

### Security & Compliance Analysis
- **Secrets Management**: Review secret handling, rotation policies, and access patterns
- **OIDC Implementation**: Assess cloud provider authentication configurations
- **Branch Protection**: Validate required reviews, status checks, and merge restrictions
- **Supply Chain Security**: Review dependency management, Action allowlists, and SBOM generation
- **Audit Logging**: Evaluate audit trail completeness and SIEM integration

### Observability & Monitoring
- **Workflow Monitoring**: Assess GitHub Actions workflow run monitoring
- **Alerting**: Review failure notification and escalation procedures
- **Metrics Collection**: Evaluate DORA metrics tracking (deployment frequency, lead time, MTTR, change failure rate)
- **Cost Tracking**: Analyze runner usage costs and optimization opportunities
- **Log Aggregation**: Review log collection and retention strategies

## Usage Scenarios

### Scenario 1: Legacy CI Migration Assessment
**Objective**: Analyze existing Jenkins pipelines for GitHub Actions migration

```bash
# Run the migration assessment script
./scripts/analyze-jenkins-pipelines.sh /path/to/jenkinsfiles

# Output: Migration complexity report with recommendations
```

**Questions to Answer**:
- What are the equivalent GitHub Actions for Jenkins plugins?
- Which workflows can use GitHub-hosted runners?
- What secrets need to be migrated?
- Are there custom scripts that need refactoring?

### Scenario 2: Security Posture Review
**Objective**: Audit existing GitHub Actions workflows for security issues

```bash
# Run security audit on workflows
./scripts/audit-workflow-security.sh .github/workflows/

# Output: Security findings with severity ratings
```

**Review Checklist**:
- [ ] All Actions pinned to full commit SHA
- [ ] Minimal `permissions:` blocks used
- [ ] No hardcoded secrets in workflow files
- [ ] OIDC used instead of long-lived credentials
- [ ] Third-party Actions from approved allowlist only

### Scenario 3: Performance Optimization
**Objective**: Identify workflow performance bottlenecks

```bash
# Analyze workflow performance
./scripts/analyze-workflow-performance.sh owner/repo

# Output: Performance report with optimization recommendations
```

**Optimization Areas**:
- Caching dependencies (npm, pip, Maven, etc.)
- Parallelizing independent jobs
- Using matrix strategies effectively
- Optimizing Docker builds with layer caching
- Reducing checkout depth for faster clones

## Scripts

### `scripts/analyze-jenkins-pipelines.sh`
Scans Jenkins pipeline files and generates GitHub Actions migration assessment.

**Usage**:
```bash
./analyze-jenkins-pipelines.sh <path-to-jenkinsfiles> [output-dir]
```

**Output**: JSON report with migration complexity scores and recommendations.

### `scripts/audit-workflow-security.sh`
Performs security analysis on GitHub Actions workflow files.

**Usage**:
```bash
./audit-workflow-security.sh <workflows-dir> [--fix]
```

**Checks**:
- Unpinned Actions usage
- Overly permissive `permissions` blocks
- Hardcoded secrets detection
- Untrusted Action sources

### `scripts/analyze-workflow-performance.sh`
Analyzes workflow execution times and suggests optimizations.

**Usage**:
```bash
./analyze-workflow-performance.sh <owner/repo> [--days 30]
```

**Requires**: `gh` CLI authenticated with workflow read permissions

### `scripts/migration-cost-estimator.sh`
Estimates GitHub Actions runner costs based on current CI usage.

**Usage**:
```bash
./migration-cost-estimator.sh <jenkins-metrics.json>
```

**Output**: Monthly cost estimates for GitHub-hosted vs self-hosted runners

## Deliverables Template

When completing a DevOps review, provide:

1. **Executive Summary**: High-level findings and recommendations
2. **Current State Assessment**:
   - CI/CD tooling inventory
   - Security posture baseline
   - Performance metrics
3. **Migration Roadmap**:
   - Phased migration plan
   - Risk assessment
   - Resource requirements
4. **Technical Design**:
   - GitHub Actions workflow examples
   - Repository structure recommendations
   - Runner architecture (if self-hosted needed)
5. **Cost Analysis**:
   - Current CI costs
   - Projected GitHub Actions costs
   - ROI justification
6. **Implementation Guide**:
   - Step-by-step migration instructions
   - Testing and validation procedures
   - Rollback procedures

## Best Practices

### When Reviewing Workflows
- Start with security analysis first
- Look for anti-patterns (e.g., `pull_request_target` with untrusted code)
- Validate environment variable handling
- Check for race conditions in concurrent workflows
- Verify artifact retention policies

### When Planning Migrations
- Begin with non-critical workflows for learning
- Maintain parallel CI systems during transition
- Test thoroughly in non-production first
- Plan for runner capacity (self-hosted scenarios)
- Document workflow ownership and support procedures

### When Optimizing Performance
- Profile slow jobs to identify bottlenecks
- Use build caching aggressively
- Consider workflow run time limits
- Optimize for fastest feedback to developers
- Monitor and alert on workflow duration trends

## Integration with Other Skills

- **Architecture Analysis**: Review overall system design before CI/CD migration
- **Security Audit**: Deep-dive on secrets management and compliance controls
- **Test Generator**: Generate workflow tests using Actions' testing frameworks
- **Migration Planner**: Coordinate repository migrations with CI/CD cutover

## Customer Engagement Tips

- **Discovery Questions**:
  - "What's your current deployment frequency?"
  - "How long does a typical CI run take?"
  - "What compliance requirements affect your CI/CD?"
  - "Who currently maintains pipeline configurations?"

- **Show Quick Wins**: Demonstrate workflow improvements in pilot before broad rollout
- **Address Concerns**: Many teams fear losing Jenkins flexibility - show GitHub Actions equivalents
- **Measure Success**: Define metrics before migration (build times, failure rates, developer satisfaction)

## Reference Materials

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Actions Security Hardening Guide](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions)
- [Migrating from Jenkins to GitHub Actions](https://docs.github.com/en/actions/migrating-to-github-actions/migrating-from-jenkins-to-github-actions)
- [Self-hosted Runners](https://docs.github.com/en/actions/hosting-your-own-runners)
