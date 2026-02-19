# Migration Planner Skill

## Purpose
Strategic planning and execution of repository and platform migrations to GitHub Enterprise. This skill supports **Migration Specialists**, **Solutions Architects**, and **Platform Engineers** in designing and implementing successful migrations.

## Role Context
Use this skill when planning migrations from legacy VCS platforms (SVN, TFS, Bitbucket, GitLab) or between GitHub instances (GHES to GHEC, org-to-org migrations).

## Capabilities

### Migration Strategy Design
- **Source Platform Assessment**: Analyze current VCS platform (SVN, TFS, Perforce, Bitbucket, GitLab)
- **Scope Definition**: Identify repositories, users, permissions, workflows to migrate
- **Approach Selection**: Big bang vs phased vs parallel run strategies
- **Risk Assessment**: Identify migration risks and mitigation strategies
- **Rollback Planning**: Design fallback procedures if migration fails

### Repository Analysis & Planning
- **Repository Inventory**: Catalog all repositories with metadata (size, activity, ownership)
- **Dependency Mapping**: Identify inter-repository dependencies
- **Large File Detection**: Find repositories with LFS or large binary requirements
- **History Preservation**: Determine history depth to preserve
- **Branching Model**: Assess source branching strategy and target model

### User & Access Migration
- **User Mapping**: Map source platform users to GitHub identities
- **Team Structure**: Design GitHub teams based on current access patterns
- **Permission Translation**: Convert source permissions to GitHub RBAC model
- **SSO Integration**: Plan identity provider integration
- **SCIM Provisioning**: Automate user lifecycle management

### CI/CD Pipeline Migration
- **Pipeline Inventory**: Catalog all build/deployment pipelines
- **Tooling Assessment**: Jenkins, CircleCI, Travis, GitLab CI, Azure DevOps
- **Actions Translation**: Convert pipelines to GitHub Actions workflows
- **Secret Migration**: Plan secure secret transfer
- **Runner Strategy**: GitHub-hosted vs self-hosted runners

### Integration Migration
- **Webhook Migration**: Map existing webhooks to GitHub webhooks
- **API Integration**: Update integrations using source platform APIs
- **Tooling Integration**: Migrate JIRA, ServiceNow, Slack integrations
- **Reporting & Analytics**: Preserve or enhance metrics dashboards
- **Backup Systems**: Update backup and disaster recovery systems

## Usage Scenarios

### Scenario 1: Enterprise Migration Discovery
**Objective**: Comprehensive assessment of current VCS platform for GitHub migration planning

```bash
# Discover repositories from source platform
./scripts/discover-source-repositories.sh --platform gitlab --url https://gitlab.company.com --output inventory.json

# Analyze migration complexity
./scripts/analyze-migration-complexity.sh --inventory inventory.json --output complexity-report.html
```

**Discovery Outputs**:
- Repository count and total size
- Active vs inactive repository identification
- Large repository identification (>5GB)
- User and team structure analysis
- CI/CD pipeline inventory
- Integration touchpoint mapping

### Scenario 2: Repository Migration Planning
**Objective**: Create detailed migration plan for repository portfolio

```bash
# Prioritize repositories for migration
./scripts/prioritize-repositories.sh --inventory inventory.json --criteria business-critical,active,small

# Generate migration waves
./scripts/plan-migration-waves.sh --prioritized repos-prioritized.json --wave-size 50 --output waves.json
```

**Migration Wave Planning**:
- Wave 1: Pilot (low-risk, high-visibility)
- Wave 2: Early adopters (willing teams, moderate complexity)
- Wave 3: Standard repositories (bulk migration)
- Wave 4: Complex cases (large repos, special requirements)
- Wave 5: Archival (inactive, read-only)

### Scenario 3: CI/CD Pipeline Migration
**Objective**: Migrate Jenkins pipelines to GitHub Actions

```bash
# Analyze Jenkins jobs
./scripts/analyze-jenkins-jobs.sh --jenkins-url https://jenkins.company.com --output jenkins-analysis.json

# Generate GitHub Actions workflows
./scripts/convert-jenkins-to-actions.sh --analysis jenkins-analysis.json --output workflows/ --validate
```

**Migration Approach**:
- Map Jenkinsfile syntax to Actions YAML
- Convert plugins to Actions equivalents
- Migrate secrets to GitHub Secrets
- Configure self-hosted runners if needed
- Test workflows before cutover

### Scenario 4: User & Permission Migration
**Objective**: Migrate users and access controls to GitHub Enterprise

```bash
# Export users from source platform
./scripts/export-source-users.sh --platform gitlab --output users.csv

# Map to GitHub identities
./scripts/map-users-to-github.sh --source-users users.csv --identity-provider okta --output user-mapping.json

# Create GitHub teams and permissions
./scripts/provision-github-access.sh --mapping user-mapping.json --org target-org --dry-run
```

**User Migration Steps**:
1. Export users from source platform
2. Map to corporate identity provider
3. Create GitHub Enterprise accounts via SCIM
4. Design team structure
5. Apply repository permissions
6. Validate access with test users

### Scenario 5: Migration Execution & Validation
**Objective**: Execute migration and validate success

```bash
# Execute migration for a wave
./scripts/execute-migration-wave.sh --wave waves/wave-1.json --parallel 5 --validate

# Validate migrated repositories
./scripts/validate-migration.sh --wave waves/wave-1.json --checks all --output validation-report.html
```

**Validation Checks**:
- All branches migrated
- Tags preserved
- Commit history intact
- LFS objects transferred
- Repository settings applied
- Webhooks configured
- CI/CD workflows functional
- Access controls applied

## Scripts

### `scripts/discover-source-repositories.sh`
Discovers and inventories repositories from source VCS platform.

**Usage**:
```bash
./discover-source-repositories.sh --platform <gitlab|bitbucket|azdo> --url <url> [--auth-token <token>] --output <file>
```

**Output Schema**:
```json
{
  "repositories": [
    {
      "name": "repo-name",
      "url": "https://source.com/repo",
      "size_mb": 150,
      "last_commit": "2024-01-15",
      "active": true,
      "branches": 5,
      "tags": 10,
      "has_lfs": false,
      "primary_language": "Java",
      "ci_pipeline": "Jenkins"
    }
  ]
}
```

### `scripts/analyze-migration-complexity.sh`
Analyzes migration complexity and generates risk assessment.

**Usage**:
```bash
./analyze-migration-complexity.sh --inventory <file> --output <report>
```

**Complexity Factors**:
- Repository size
- Binary file count
- History depth
- Branch count
- Active user count
- CI/CD complexity
- Integration count

**Risk Levels**: Low, Medium, High, Very High

### `scripts/prioritize-repositories.sh`
Prioritizes repositories for migration based on criteria.

**Usage**:
```bash
./prioritize-repositories.sh --inventory <file> --criteria <criteria> [--output <file>]
```

**Criteria Options**:
- `business-critical`: Mission-critical applications first
- `active`: Recently active repositories prioritized
- `small`: Small repositories are easier to migrate
- `pilot-candidates`: Low-risk, high-visibility for learning
- `inactive`: Archival candidates last

### `scripts/plan-migration-waves.sh`
Creates migration wave plan with batched repository groups.

**Usage**:
```bash
./plan-migration-waves.sh --prioritized <file> --wave-size <N> [--output <file>]
```

**Wave Planning**:
- Balances repository count and total size
- Groups related repositories
- Schedules based on team availability
- Includes buffer time between waves

### `scripts/convert-jenkins-to-actions.sh`
Converts Jenkins pipelines to GitHub Actions workflows.

**Usage**:
```bash
./convert-jenkins-to-actions.sh --jenkinsfile <path> --output <workflow-file> [--validate]
```

**Conversion Mapping**:
```
Jenkins                    GitHub Actions
---------                  ---------------
pipeline { }               workflow with jobs
stage { }                  job
steps { }                  steps
agent                      runs-on
environment { }            env:
credentials()              secrets.SECRET_NAME
sh 'command'               run: command
```

### `scripts/migrate-repository.sh`
Migrates a single repository with full fidelity.

**Usage**:
```bash
./migrate-repository.sh --source <url> --target <org/repo> [--mirror] [--lfs] [--validate]
```

**Options**:
- `--mirror`: Mirror all refs (branches, tags)
- `--lfs`: Migrate Git LFS objects
- `--validate`: Run post-migration validation
- `--preserve-dates`: Keep original commit dates

**Migration Process**:
1. Clone source repository
2. Clean up (if needed)
3. Push to target GitHub repository
4. Transfer LFS objects (if applicable)
5. Configure repository settings
6. Apply branch protection rules
7. Configure webhooks and integrations
8. Validate migration success

### `scripts/execute-migration-wave.sh`
Executes migration for a batch of repositories.

**Usage**:
```bash
./execute-migration-wave.sh --wave <wave-file> [--parallel <N>] [--validate] [--rollback-on-error]
```

**Features**:
- Parallel migration execution
- Progress tracking
- Error handling and logging
- Automatic validation
- Rollback capability

### `scripts/validate-migration.sh`
Validates migrated repositories for completeness and correctness.

**Usage**:
```bash
./validate-migration.sh --repo <org/repo> --checks <all|basic|comprehensive> [--fix-issues]
```

**Validation Checks**:
- Branch count matches source
- Tag count matches source
- Commit history depth
- LFS objects present
- Repository settings applied
- CI/CD workflows functional
- Webhooks configured
- Team access correct

### `scripts/export-source-users.sh`
Exports users from source VCS platform.

**Usage**:
```bash
./export-source-users.sh --platform <gitlab|bitbucket|azdo> --output <file>
```

**Export Fields**: username, email, full name, access level, last active date

### `scripts/map-users-to-github.sh`
Maps source platform users to GitHub identities.

**Usage**:
```bash
./map-users-to-github.sh --source-users <file> --identity-provider <okta|aad> --output <mapping-file>
```

**Handles**:
- Username differences
- Email matching
- Manual overrides for exceptions
- Unmapped user reporting

### `scripts/rollback-migration.sh`
Rolls back a failed migration (use with extreme caution).

**Usage**:
```bash
./rollback-migration.sh --repo <org/repo> [--delete-target] [--restore-source]
```

**Rollback Actions**:
- Delete target GitHub repository (optional)
- Re-enable source repository (if disabled)
- Restore webhooks to source (if moved)
- Notify stakeholders

## Migration Best Practices

### Planning Phase
- **Start with discovery**: Understand scope before planning
- **Pilot first**: Migrate 1-2 non-critical repos for learning
- **Get executive buy-in**: Migrations require time and resources
- **Communicate timeline**: Set realistic expectations
- **Plan for contingency**: Allow buffer time for issues

### Execution Phase
- **Batch migrations**: Don't migrate everything at once
- **Validate immediately**: Check each migration before next batch
- **Monitor closely**: Watch for performance or capacity issues
- **Document issues**: Track problems and resolutions for future waves
- **Support users**: Provide training and help desk support

### Post-Migration Phase
- **Decommission source**: Archive or shut down old platform
- **Update integrations**: Ensure all tools point to GitHub
- **Collect feedback**: Learn from user experience
- **Measure success**: Track adoption and satisfaction metrics
- **Continuous improvement**: Apply lessons learned to future work

### Common Pitfalls to Avoid
- **Underestimating complexity**: Large repositories take longer
- **Ignoring CI/CD**: Don't forget to migrate pipelines
- **Poor communication**: Keep stakeholders informed
- **Insufficient testing**: Validate thoroughly before cutover
- **No rollback plan**: Always have a backup plan
- **Migrating everything**: Some repos should be archived, not migrated

## Deliverables Template

When completing a migration planning engagement, provide:

1. **Migration Strategy Document**
   - Current state assessment
   - Target state design
   - Migration approach and rationale
   - Risk assessment and mitigation
   - Success criteria and metrics

2. **Detailed Migration Plan**
   - Repository prioritization
   - Migration wave schedule
   - Resource requirements
   - Communication plan
   - Training plan

3. **Technical Implementation Guide**
   - Step-by-step migration procedures
   - Script usage documentation
   - Validation checklists
   - Troubleshooting guide
   - Rollback procedures

4. **User Enablement Materials**
   - GitHub training resources
   - Migration FAQ
   - Support contact information
   - Before/after workflow comparisons

5. **Project Tracking Dashboard**
   - Migration progress by wave
   - Issues and blockers
   - Validation status
   - User adoption metrics

## Integration with Other Skills

- **Architecture Analysis**: Org structure design informs migration approach
- **DevOps Review**: CI/CD migration complexity assessment
- **Security Audit**: Ensure security controls migrate properly
- **Governance Enforcer**: Apply governance policies to migrated repos

## Customer Engagement Tips

### Discovery Questions
- "Why are you migrating to GitHub now?"
- "What pain points do you hope to solve?"
- "What is your target timeline?"
- "How many users/repositories are in scope?"
- "What compliance or security requirements exist?"
- "Who are your stakeholders and decision makers?"

### Managing Expectations
- **Be realistic about timeline**: Migrations take longer than expected
- **Emphasize validation**: Don't rush, quality matters
- **Highlight benefits**: Focus on post-migration improvements
- **Prepare for disruption**: Some workflow changes are inevitable
- **Plan for support**: Users will need help adjusting

### Success Metrics
- Repositories migrated successfully
- CI/CD pipeline migration rate
- User adoption and satisfaction
- Time to first commit post-migration
- Reduction in incidents vs source platform

## Reference Materials

- [GitHub Migrations](https://docs.github.com/en/migrations)
- [GitHub Importer](https://docs.github.com/en/get-started/importing-your-projects-to-github/importing-source-code-to-github/about-github-importer)
- [Migrating from GitLab](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)
- [Migrating Jenkins to Actions](https://docs.github.com/en/actions/migrating-to-github-actions/migrating-from-jenkins-to-github-actions)
- [Enterprise Migration Best Practices](https://resources.github.com/migrations/)
