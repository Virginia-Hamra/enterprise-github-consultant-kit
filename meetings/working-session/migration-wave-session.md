# Migration Wave Working Session

> **Duration:** 3–4h

## Objective
Execute one migration wave end-to-end: pre-flight, dry-run, cutover, validation, post-migration config.

## Definition of Done
- [ ] All wave repos migrated to target org(s)
- [ ] Validation passed on every repo
- [ ] Branch protection / rulesets applied
- [ ] CI/CD reconnected and green
- [ ] Source repos archived / read-only
- [ ] Comms sent to repo owners

## Pre-requisites
- [ ] Wave inventory final (CSV)
- [ ] Tool installed (GEI / BBS2GH / ADO2GH)
- [ ] User mapping CSV finalized
- [ ] Target org provisioned with rulesets ready
- [ ] Cutover window approved
- [ ] Rollback procedure documented

## Step-by-step
1. Read-only / freeze source repos in the wave
2. Run dry-run migration on the wave; review manifest
3. Execute live migration
4. Per-repo validation (script):
   - Code (commit hash on main)
   - Branch list
   - PR / issue counts
   - Releases / tags
   - LFS objects
5. Apply rulesets / repo settings via IaC
6. Reconnect webhooks / CI / integrations
7. Smoke-test CI on each repo
8. Archive source repo
9. Notify owners

## Validation
- [ ] All validation checks pass
- [ ] CI green on all repos
- [ ] No webhook errors
- [ ] Audit log captures migration events

## Rollback
- Restore source from read-only freeze
- Delete or unarchive target repos per agreed procedure
- Communicate rollback to owners

## Risks
- Rate limits → split wave or use larger windows
- LFS issues → pre-validate LFS object counts in dry-run
- Webhook secrets → re-issue, do not migrate as-is
