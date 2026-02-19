# GitHub Enterprise Consultant Kit - Copilot Instructions

This document provides instructions for AI agents working on enterprise GitHub consulting engagements focused on secure platform onboarding, governance implementation, and developer enablement.

## Project Overview

This toolkit enables consultants, solutions architects, and platform engineers to deliver **secure-by-default, enterprise-scalable GitHub Enterprise foundations** for regulated organizations and large enterprises.

**Core Delivery Areas:**
- Secure platform onboarding & identity management
- Governance, compliance, and auditability frameworks
- CI/CD standardization & security automation
- Developer productivity and golden path engineering
- Migration & adoption strategies

## Roles & Responsibilities

### Enterprise Solutions Architect
- Design organization structures and access models
- Define security baselines and compliance mappings
- Create reference architectures for multi-org environments
- Establish governance frameworks and policy-as-code approaches
- Lead technical discovery and risk assessments

### Platform Engineer
- Implement infrastructure-as-code for GitHub platform configuration
- Build and maintain CI/CD pipelines and Actions workflows
- Configure security tooling (Dependabot, CodeQL, secret scanning)
- Automate repository provisioning and governance enforcement
- Integrate GitHub with enterprise identity providers (SAML, SCIM, OIDC)

### DevOps Consultant
- Assess and migrate existing CI/CD pipelines to GitHub Actions
- Design deployment strategies and environment protection rules
- Implement OIDC-based cloud authentication patterns
- Optimize workflow performance and runner configurations
- Establish observability and audit logging practices

### Security & Compliance Advisor
- Map GitHub capabilities to compliance frameworks (SOC 2, ISO 27001, NIST)
- Configure branch protection rules and CODEOWNERS enforcement
- Implement secret scanning and push protection
- Design incident response procedures
- Establish audit trail preservation and SIEM integration

## Development Guidelines

### Code Quality Standards

**Infrastructure-as-Code (Terraform, ARM, CloudFormation)**
- Use modules for reusable components
- Pin provider versions explicitly
- Include comprehensive variable descriptions
- Add validation rules for inputs
- Document outputs with examples

**GitHub Actions Workflows**
- Use minimal permissions with explicit `permissions:` blocks
- Pin all Actions to full commit SHA, not tags
- Avoid storing secrets in GitHub (prefer OIDC)
- Add timeout limits to all jobs and steps
- Include descriptive names for all jobs and steps

**Scripts (Bash, PowerShell, Python)**
- Add error handling and validation for all inputs
- Use exit codes appropriately (0=success, non-zero=failure)
- Include usage documentation in comments
- Make scripts idempotent where possible
- Log operations for audit trails

**APIs & Automation**
- Use GitHub CLI (`gh`) when possible for simplicity
- Handle rate limiting with retry logic
- Validate inputs before making API calls
- Log all mutations (create, update, delete operations)
- Use fine-grained PATs with minimal scopes

### Security Baseline

**Secrets Management**
- Never commit credentials, tokens, or API keys
- Use environment variables or secret managers
- Rotate credentials following engagement
- Document secret requirements clearly
- Prefer OIDC over long-lived credentials

**Access Control**
- Follow least-privilege principle
- Document required permissions explicitly
- Time-box elevated access where needed
- Use service accounts for automation
- Review and revoke unused access regularly

**Supply Chain Security**
- Verify checksums for downloaded binaries
- Use trusted sources for dependencies
- Pin dependency versions explicitly
- Scan third-party Actions before approval
- Maintain allowlists for approved Actions and packages

### Testing & Validation

**Infrastructure Testing**
- Validate Terraform plans before applying
- Test workflows in non-production environments first
- Verify branch protections are enforced correctly
- Confirm OIDC token exchange works before production use
- Dry-run migrations before actual execution

**Security Validation**
- Verify CodeQL finds known vulnerabilities
- Confirm secret scanning detects test secrets
- Test branch protection prevents unauthorized merges
- Validate SSO and SCIM provisioning flows
- Audit generated policies against requirements

**Integration Testing**
- Test CI/CD workflows end-to-end
- Verify deployment protection rules work as expected
- Confirm audit logs capture required events
- Test incident response procedures
- Validate backup and recovery processes

### Documentation Standards

**For Customers**
- Write for non-technical stakeholders as well as engineers
- Include architecture diagrams for complex systems
- Provide runbooks for operational procedures
- Document compliance mappings explicitly
- Create quick-start guides for common tasks

**For Code**
- Document WHY, not just WHAT (context matters)
- Include examples for complex operations
- Explain security decisions and trade-offs
- Link to relevant GitHub documentation
- Note any enterprise-specific customizations

**Repository Standards**
- Every repository must have README.md, SECURITY.md, and CODEOWNERS
- Include compliance evidence when required
- Document repository purpose and ownership
- Maintain up-to-date dependency documentation
- Version policy changes with explanations

### Version Control Practices

**Commit Guidelines**
- Use conventional commit format: `type(scope): description`
- Types: `feat`, `fix`, `docs`, `refactor`, `security`, `chore`
- Keep commits atomic and focused
- Reference Jira/ADO issues where applicable
- Sign commits when required by customer policy

**Branch Strategy**
- Use `main` as the default production branch
- Feature branches: `feature/<description>`
- Hotfix branches: `hotfix/<description>`
- Never commit directly to `main` in production repos
- Delete branches after merge

**Pull Request Standards**
- Include description of changes and rationale
- Link to related issues or design documents
- Ensure all CI checks pass before requesting review
- Request review from appropriate subject matter experts
- Address review comments promptly

## Enterprise Consulting Conventions

### Discovery Phase
- Use structured questionnaires for requirements gathering
- Document current state and desired future state
- Identify compliance requirements early
- Map existing tools and workflows
- Assess team skills and training needs

### Design Phase
- Create architecture decision records (ADRs) for major decisions
- Validate designs with customer stakeholders
- Consider both technical and organizational constraints
- Plan for phased rollout where appropriate
- Document risks and mitigation strategies

### Implementation Phase
- Start with security and governance foundation
- Implement in small, verifiable increments
- Test thoroughly before customer handoff
- Document all configuration changes
- Maintain audit trail of decisions

### Enablement Phase
- Provide hands-on training for customer teams
- Create golden path examples for common scenarios
- Document operational procedures
- Establish support escalation paths
- Plan for knowledge transfer

## Working with Skills

This toolkit includes specialized skills (see `.github/skills/`) for common consulting tasks. When using these skills:

- **Read the SKILL.md** documentation before invoking
- **Use provided scripts** for repeatable operations
- **Customize for customer context** as needed
- **Document any modifications** made to skills
- **Share improvements** back to the baseline toolkit

Available skills:
- `devops-review/` - CI/CD pipeline analysis and migration planning
- `architecture-analysis/` - System design review and optimization
- `test-generator/` - Security and integration test generation
- `security-audit/` - Compliance and security posture assessment
- `migration-planner/` - Repository and workflow migration strategies
- `governance-enforcer/` - Policy-as-code implementation and validation

## Customer Data Handling

**CRITICAL**: This toolkit may be used with sensitive customer data.

- Do not commit customer-specific information to public repositories
- Use `.gitignore` to exclude customer configurations
- Sanitize examples before sharing externally
- Follow customer data handling policies strictly
- Remove customer data before archiving engagements

## Automation Best Practices

- **Idempotency**: Scripts should be safe to run multiple times
- **Validation**: Check prerequisites before execution
- **Logging**: Record all actions for audit purposes
- **Error Handling**: Fail gracefully with clear error messages
- **Rollback**: Plan for reverting changes when needed

## Compliance & Audit Readiness

- Maintain traceability from requirements to implementation
- Document all security controls and their purpose
- Generate evidence artifacts for audits
- Version control all policies and configurations
- Preserve audit logs per customer retention policies

## Engagement Lifecycle

1. **Kickoff**: Review requirements, access, and constraints
2. **Discovery**: Assess current state and identify gaps
3. **Design**: Create architecture and implementation plan
4. **Build**: Implement foundation and automation
5. **Validate**: Test, verify, and remediate issues
6. **Enable**: Train customer teams and transfer knowledge
7. **Handoff**: Document, archive, and transition support
8. **Retrospective**: Capture lessons learned for toolkit improvement

## Quality Checklist

Before completing any deliverable:

- [ ] All code is tested and functional
- [ ] Security controls are properly configured
- [ ] Documentation is complete and accurate
- [ ] Customer review and approval obtained
- [ ] Compliance evidence generated and stored
- [ ] Knowledge transfer completed
- [ ] Operational runbooks provided
- [ ] Support escalation paths documented
- [ ] Engagement artifacts archived securely

## Continuous Improvement

- Share reusable patterns back to this toolkit
- Document lessons learned after each engagement
- Update skills based on field experience
- Maintain pricing and capability data currency
- Collaborate with peers on challenging scenarios
