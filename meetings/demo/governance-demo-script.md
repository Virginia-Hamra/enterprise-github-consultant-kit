# Governance Demo Script

> **Audience:** Platform / Security / GRC &nbsp; | &nbsp; **Run time:** 30 min

## Narrative arc
"Policy-as-code for GitHub: rulesets, CODEOWNERS, and audit streaming as the three legs of governance."

## Setup
- Org with rulesets defined as IaC (Terraform)
- Sample repo with CODEOWNERS, SECURITY.md, branch protection
- Audit log streaming → SIEM (or local viewer)

## Flow

### 1. Rulesets vs. classic protection (8 min)
- Show ruleset hierarchy (enterprise → org → repo)
- Demonstrate enforcement: PR + reviews + status checks + signed commits + linear history
- Bypass list governance & audit trail

### 2. CODEOWNERS (5 min)
- Walk a CODEOWNERS file
- Show required-review-from-code-owners enforcement
- IdP-group → team mapping for ownership

### 3. IaC for platform config (8 min)
- Walk Terraform repo: rulesets, teams, repos
- Show plan → PR → apply flow
- Drift detection job

### 4. Audit log streaming (5 min)
- Live event ingestion
- Sample queries: SSO disable, owner add, ruleset bypass, secret-found
- Alert rule examples

### 5. Compliance evidence (4 min)
- Map a SOC 2 control to evidence artifact
- Show the relevant export / dashboard

## Key talking points
- Rulesets > classic for hierarchy + bypass control
- IaC eliminates console drift; PR review is your change record
- Audit streaming + retention = audit-ready by default

## Fallback
- Pre-rendered Terraform plan output
- Static SIEM screenshots
