# GHAS Demo Script

> **Audience:** Security / AppSec / Eng leadership &nbsp; | &nbsp; **Run time:** 30–45 min

## Narrative arc
"Shift left without slowing developers down — by surfacing the right signal at the right moment in the SDLC."

## Setup
- Demo org with one repo per scenario:
  - `demo-vuln-app` — known vulnerable JS/Python app
  - `demo-secret-leak` — sample with planted (fake) secret patterns
  - `demo-supply-chain` — mixed dependency tree
- All GHAS features enabled
- Sample alerts pre-populated; one fresh demo flow live

## Flow

### 1. Secret scanning + push protection (8 min)
- Show partner alert in `demo-secret-leak`
- **Live:** attempt push of fake AWS key → blocked at push
- Show bypass workflow & audit log entry
- Mention custom patterns

### 2. CodeQL (10 min)
- Open default-setup config
- Walk a real alert with data flow viz
- Show triage in PR (security tab + check)
- Mention advanced setup, custom queries, query suites

### 3. Dependabot (8 min)
- Alert list view, severity filtering
- Open auto-generated security update PR
- Show grouped updates & version-update config

### 4. Dependency review (5 min)
- Open a PR introducing a new vulnerable dep
- Show inline review comment & blocking check

### 5. Org-level dashboards (5 min)
- Security overview by repo
- Trend by severity over time

## Key talking points
- Default setup vs. advanced — pick by language coverage
- Push protection eliminates "secret in history" debt at source
- Triage SLAs + CODEOWNERS = ownership, not orphan alerts
- Integrates with SIEM / ticketing via webhooks + audit streaming

## FAQ pocket
- "False positives?" → custom patterns + suppressions + tuned suites
- "Self-hosted runners for CodeQL?" → yes, larger runners often needed
- "Cost?" → seat-based on active committers in GHAS-enabled repos

## Fallback
- Pre-recorded 4-min walkthrough video in case live env breaks
