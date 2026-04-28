# Migration Demo Script

> **Audience:** Migration owner / platform &nbsp; | &nbsp; **Run time:** 30–45 min

## Narrative arc
"Wave-based, validated migration with full chain-of-custody — from inventory to cutover."

## Setup
- Source platform sandbox (BBS / GitLab / ADO)
- Target GHEC org
- GEI / BBS2GH / ADO2GH tool installed
- One sample repo with PRs, issues, releases, CI history

## Flow

### 1. Inventory (5 min)
- Show inventory CSV
- Wave assignment logic

### 2. Dry-run (10 min)
- Run migration tool in dry-run mode
- Walk the manifest output

### 3. Live migration (15 min)
- Execute the migration on the sample repo
- Walk validation: code, history, PRs, issues, wikis, releases
- User mapping outcomes

### 4. Post-migration steps (5 min)
- Apply org rulesets
- Reconnect CI / webhooks
- Archive source

### 5. Cutover comms & rollback (5 min)
- Status page snippet
- Rollback procedure walkthrough

## Key talking points
- Always dry-run first
- Validate per repo against documented criteria
- Freeze source, then cutover, then archive
- Rate limits → sequence wave size accordingly

## Fallback
- Recorded migration run for the live segment
