# GitHub Copilot Skills for Enterprise Consulting

This directory contains specialized skills for GitHub Enterprise consulting engagements. Each skill provides focused capabilities, scripts, and deliverables templates for specific aspects of enterprise platform implementation.

## Available Skills

### 1. DevOps Review (`devops-review/`)
**Purpose**: CI/CD pipeline analysis and migration planning
**Use When**: Assessing existing pipelines, migrating to GitHub Actions, optimizing workflows
**Key Deliverables**: Migration assessment, security audit, performance optimization recommendations
**Primary Users**: DevOps Consultants, Platform Engineers

### 2. Architecture Analysis (`architecture-analysis/`)
**Purpose**: System design and organization structure analysis
**Use When**: Designing GitHub org structures, evaluating architecture patterns, planning modernization
**Key Deliverables**: Architecture diagrams, ADRs, org structure recommendations, migration strategies
**Primary Users**: Solutions Architects, Technical Leads

### 3. Test Generator (`test-generator/`)
**Purpose**: Comprehensive test suite generation
**Use When**: Building test automation, validating infrastructure, testing GitHub Actions
**Key Deliverables**: Unit tests, integration tests, infrastructure tests, security tests, test strategy
**Primary Users**: QA Engineers, Platform Engineers, Developers

### 4. Security Audit (`security-audit/`)
**Purpose**: Security assessment and compliance validation
**Use When**: Conducting security audits, preparing for compliance certification, validating security posture
**Key Deliverables**: Security findings report, compliance evidence, remediation roadmap
**Primary Users**: Security Engineers, Compliance Officers, Auditors

### 5. Migration Planner (`migration-planner/`)
**Purpose**: Repository and platform migration planning
**Use When**: Migrating from legacy VCS, planning org migrations, managing large-scale transitions
**Key Deliverables**: Migration strategy, wave plans, validation reports, user enablement materials
**Primary Users**: Migration Specialists, Solutions Architects, Platform Engineers

### 6. Governance Enforcer (`governance-enforcer/`)
**Purpose**: Policy-as-code implementation and enforcement
**Use When**: Implementing governance policies, automating compliance, managing at scale
**Key Deliverables**: Policy definitions, enforcement automation, compliance dashboard, operational runbooks
**Primary Users**: Platform Engineers, Compliance Officers, Security Teams

## Skill Structure

Each skill follows a consistent structure:

```
skill-name/
├── SKILL.md              # Detailed skill documentation
├── scripts/              # Automation scripts
│   ├── script1.sh
│   ├── script2.py
│   └── ...
├── templates/            # Reusable templates (optional)
│   ├── template1.yaml
│   └── ...
└── examples/             # Example outputs (optional)
    └── ...
```

## Using Skills

### With GitHub Copilot
Skills are automatically available to GitHub Copilot in this repository. Copilot will reference skill documentation when providing assistance.

**Example prompts**:
- "Review this workflow using the devops-review skill"
- "Generate an architecture analysis for this org structure"
- "Create a security audit report following the security-audit skill"

### Standalone Usage
Skills can be used independently via their scripts:

```bash
# Navigate to skill directory
cd .github/skills/devops-review

# Run skill script
./scripts/analyze-jenkins-pipelines.sh /path/to/jenkinsfiles
```

### Customization
Skills are designed to be customized per engagement:

1. **Fork or copy** the skill to customer repository
2. **Customize scripts** for customer-specific requirements
3. **Update templates** with customer branding and standards
4. **Add examples** from customer environment
5. **Document modifications** in skill SKILL.md

## Skill Integration

Skills are designed to work together:

| Primary Skill | Integrates With | Purpose |
|--------------|----------------|---------|
| Architecture Analysis | DevOps Review | Architecture informs CI/CD design |
| DevOps Review | Security Audit | Pipeline security validation |
| Security Audit | Governance Enforcer | Security policies become governance rules |
| Migration Planner | All skills | Migration requires architecture, security, CI/CD planning |
| Test Generator | All skills | Tests validate implementations from all skills |

## Creating New Skills

To create a new skill for your engagement:

### 1. Create Skill Directory Structure
```bash
mkdir -p .github/skills/new-skill/scripts
```

### 2. Create SKILL.md
Use existing skills as templates. Include:
- **Purpose**: What problem does this skill solve?
- **Role Context**: Who uses this skill and when?
- **Capabilities**: What can this skill do?
- **Usage Scenarios**: Real-world examples with scripts
- **Scripts**: Documentation for each script
- **Deliverables Template**: Expected outputs
- **Best Practices**: Lessons learned and guidance
- **Integration**: How this skill relates to others
- **Customer Engagement Tips**: Discovery questions, common challenges

### 3. Add Scripts
Create executable scripts with:
- Clear usage documentation (--help flag)
- Input validation and error handling
- Structured output (JSON, CSV, HTML reports)
- Logging for audit trails
- Dry-run modes for safety

### 4. Test Thoroughly
- Test scripts with realistic data
- Validate on pilot before production use
- Document known limitations
- Include example inputs and outputs

### 5. Update This README
Add your new skill to the "Available Skills" section.

## Skill Maintenance

### Version Control
- Skills should be versioned (track changes in git)
- Document breaking changes clearly
- Maintain backward compatibility when possible

### Documentation
- Keep SKILL.md in sync with script capabilities
- Update examples when scripts change
- Document customer-specific customizations

### Script Quality
- Use consistent style across scripts
- Add error handling and validation
- Log operations for audit trails
- Test scripts before customer use

### Continuous Improvement
- Collect feedback from field usage
- Share improvements across engagements
- Update based on GitHub platform changes
- Incorporate lessons learned

## Support & Contribution

### Getting Help
- **Documentation**: Read SKILL.md for each skill
- **Examples**: Check examples/ directories for usage patterns
- **Issues**: Document problems and resolutions
- **Team**: Consult with other consultants who've used skills

### Contributing Improvements
1. Test changes thoroughly on real engagement
2. Document new capabilities in SKILL.md
3. Update this README if adding new skills
4. Share learnings with the consulting team

## Best Practices

### Engagement Workflow
1. **Discovery**: Understand customer needs
2. **Skill Selection**: Choose relevant skills for engagement
3. **Customization**: Adapt to customer context
4. **Validation**: Test on pilot/sandbox first
5. **Execution**: Apply to production with monitoring
6. **Handoff**: Transfer knowledge to customer team

### Customer Deliverables
- **Always provide**: Comprehensive documentation
- **Never skip**: Validation and testing steps
- **Include**: Runbooks for ongoing operations
- **Customize**: Templates with customer branding
- **Archive**: Evidence and artifacts for reference

### Quality Standards
- ✅ All scripts tested on real data
- ✅ Documentation complete and accurate
- ✅ Error handling and logging present
- ✅ Customer-specific customizations documented
- ✅ Validation steps included
- ✅ Rollback procedures defined

## License

This skills framework is proprietary to [Your Company] and intended for customer consulting engagements. Do not share publicly without authorization.

## Revision History

- **2024-02**: Initial skill framework with 6 core skills
- Skills created: devops-review, architecture-analysis, test-generator, security-audit, migration-planner, governance-enforcer
