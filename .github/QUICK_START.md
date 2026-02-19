# Quick Start Guide - GitHub Copilot Skills

## For AI Agents (GitHub Copilot)

### Understanding Your Instructions
Your instructions are in `.github/copilot-instructions.md`. This provides:
- **Project context**: Enterprise GitHub consulting toolkit
- **Role definitions**: Architect, Engineer, Consultant, Security Advisor
- **Development standards**: Code quality, security, testing, documentation
- **Engagement lifecycle**: Discovery → Design → Build → Validate → Enable

### Using Skills
When a user asks for help, reference the appropriate skill:

| User Request Contains... | Use This Skill |
|-------------------------|----------------|
| "CI/CD", "pipeline", "Jenkins", "Actions" | `skills/devops-review/` |
| "org structure", "architecture", "design" | `skills/architecture-analysis/` |
| "test", "testing", "unit test", "integration test" | `skills/test-generator/` |
| "security", "audit", "compliance", "SOC 2", "ISO" | `skills/security-audit/` |
| "migration", "migrate", "GitLab", "Bitbucket" | `skills/migration-planner/` |
| "policy", "governance", "enforce", "compliance" | `skills/governance-enforcer/` |

### Example Interactions

**User**: "Help me migrate from Jenkins to GitHub Actions"
**You Should**:
1. Reference `skills/devops-review/SKILL.md`
2. Suggest running `scripts/analyze-jenkins-pipelines.sh`
3. Provide GitHub Actions conversion guidance
4. Reference security best practices (SHA pinning, OIDC)

**User**: "Design an org structure for 200 teams"
**You Should**:
1. Reference `skills/architecture-analysis/SKILL.md`
2. Ask discovery questions (compliance needs, business units, etc.)
3. Suggest running `scripts/recommend-org-structure.sh`
4. Provide ADR template for decision documentation

**User**: "Generate compliance evidence for SOC 2"
**You Should**:
1. Reference `skills/security-audit/SKILL.md`
2. Review SOC 2 control mappings in skill documentation
3. Suggest running `scripts/generate-compliance-evidence.sh`
4. Provide evidence checklist

## For Human Consultants

### Quick Navigation

```bash
# View main instructions
cat .github/copilot-instructions.md

# List all skills
ls .github/skills/

# Read a specific skill
cat .github/skills/devops-review/SKILL.md

# View skills overview
cat .github/skills/README.md
```

### Starting a New Engagement

**Week 1 - Discovery**
```bash
# Run discovery scripts from relevant skills
cd .github/skills/migration-planner/scripts
./discover-source-repositories.sh --platform gitlab --url <url>

cd ../../security-audit/scripts
./audit-enterprise-platform.sh <enterprise-slug>

cd ../../architecture-analysis/scripts
./analyze-repo-distribution.sh <enterprise-slug>
```

**Week 2-3 - Analysis & Planning**
- Review skill SKILL.md files for deliverable templates
- Generate architecture diagrams
- Create migration wave plans
- Develop security baseline

**Week 4+ - Implementation**
- Use governance-enforcer scripts to apply policies
- Execute migration waves
- Generate tests with test-generator
- Validate with security audits

### Script Usage Pattern

All scripts follow this pattern:
```bash
./script-name.sh <required-args> [--optional-flags] [--output <file>]

# Examples:
./audit-repository.sh owner/repo --comprehensive --output report.json
./migrate-repository.sh --source <url> --target org/repo --validate
./apply-repo-policies.sh --policy baseline.yaml --org myorg --dry-run
```

### Common Commands

```bash
# Run with help flag
./script-name.sh --help

# Dry-run mode (preview changes)
./script-name.sh --dry-run

# Generate report
./script-name.sh --output report.html

# Parallel execution
./script-name.sh --parallel 5
```

## Customization for Your Engagement

### 1. Copy Skills to Customer Repo
```bash
# Copy relevant skills to customer repository
cp -r .github/skills/devops-review customer-repo/.github/skills/
```

### 2. Customize for Customer Context
- Update script paths for customer environment
- Add customer-specific compliance requirements
- Include customer branding in templates
- Reference customer internal tools

### 3. Add Customer-Specific Skills
```bash
# Create new skill
mkdir -p .github/skills/custom-skill/scripts
# Copy SKILL.md from existing skill as template
# Customize for your needs
```

## Troubleshooting

### GitHub Copilot Not Using Skills
- Ensure `.github/copilot-instructions.md` exists
- Check that skill SKILL.md files are readable
- Try being more specific in your prompts
- Reference skills explicitly: "Using the devops-review skill, analyze..."

### Scripts Not Found
```bash
# Make scripts executable
chmod +x .github/skills/*/scripts/*.sh

# Check script location
ls -la .github/skills/devops-review/scripts/
```

### Need More Detail
Each skill has comprehensive documentation:
```bash
# Read full skill documentation
less .github/skills/security-audit/SKILL.md

# Search for specific topics
grep -r "branch protection" .github/skills/
```

## Getting Help

### Documentation Hierarchy
1. **Quick Start** (this file) - Overview and common patterns
2. **SUMMARY.md** - Implementation details and statistics
3. **skills/README.md** - Skills framework overview
4. **skills/*/SKILL.md** - Individual skill deep-dives
5. **copilot-instructions.md** - Complete AI agent instructions

### Example Prompts for Copilot

```
"Using the devops-review skill, analyze this Jenkinsfile for migration to GitHub Actions"

"Following the architecture-analysis skill, design an org structure for 50 teams with SOC 2 requirements"

"Using test-generator skill, create Terraform tests for these modules"

"Generate a security audit report using the security-audit skill for org:mycompany"

"Create a migration plan following the migration-planner skill for 500 repos from GitLab"

"Apply governance policies using the governance-enforcer skill to all repositories"
```

## Next Actions

### For Your First Engagement
1. ✅ Read this Quick Start
2. ✅ Review `skills/README.md`
3. ✅ Read SKILL.md for relevant skills
4. ✅ Run discovery scripts
5. ✅ Generate analysis reports
6. ✅ Customize templates
7. ✅ Execute implementation
8. ✅ Generate deliverables

### For Ongoing Use
- Share feedback on skills with team
- Add new scripts as you create them
- Document customer-specific patterns
- Update skills with lessons learned

---

**Quick Reference Created**: February 2024  
**For**: Enterprise GitHub Consulting Toolkit  
**Questions?**: Review `.github/SUMMARY.md` for detailed implementation guide
