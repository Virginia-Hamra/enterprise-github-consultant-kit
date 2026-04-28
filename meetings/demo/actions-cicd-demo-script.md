# Actions / CI/CD Demo Script

> **Audience:** Platform / DevOps &nbsp; | &nbsp; **Run time:** 30–60 min

## Narrative arc
"Standardize build & deploy with reusable workflows + OIDC + governed runners — secure by default, fast by design."

## Setup
- Demo org with reusable workflow repo (`actions-shared`)
- One consumer repo using the reusable workflow
- OIDC trust to a sandbox cloud account
- Larger runners + ARC environment configured (one of each)

## Flow

### 1. Reusable workflows / composite actions (10 min)
- Walk `actions-shared/.github/workflows/build.yml` (reusable)
- Show consumer `uses:` + inputs / secrets inheritance
- Versioning strategy (tag + SHA pin)

### 2. Allowed Actions policy (5 min)
- Show org-level policy: verified creators + allowlist
- Demonstrate denial of a non-allowed Action

### 3. OIDC to cloud (10 min)
- Show trust policy with `repo:org/*:ref:refs/heads/main` scoping
- Run a deploy job → no long-lived secret in GitHub
- Audit STS / cloud trail event

### 4. Environments + protection rules (5 min)
- Required reviewers, wait timers, deployment branches
- Show a deployment paused for approval

### 5. Runner strategy (10 min)
- GitHub-hosted vs. larger runners (perf comparison)
- ARC autoscaling demo (scale-from-zero)
- When to choose self-hosted

### 6. Observability (5 min)
- Workflow run insights
- Billing & usage dashboards
- Webhooks → Datadog/Splunk

## Key talking points
- SHA-pin third-party Actions
- Default `GITHUB_TOKEN` permissions = read
- OIDC > stored secrets, always
- Reusable workflows are the unit of standardization
- Costs: hosted minutes vs. self-hosted ops cost

## Fallback
- Recorded run of each scenario; use if network blocks runner pickup
