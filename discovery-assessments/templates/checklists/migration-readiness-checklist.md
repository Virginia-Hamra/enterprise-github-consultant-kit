# Migration Readiness Checklist

Owner: Migration lead. Use before each migration wave.

## Inventory
- [ ] Source repos inventoried (CSV with size, activity, owners)
- [ ] Wave assignment confirmed
- [ ] Repos to archive vs. migrate decided

## Dependencies
- [ ] Webhooks / integrations cataloged
- [ ] CI/CD references to source URLs identified
- [ ] LFS / submodules / large files plan documented
- [ ] Service accounts & deploy keys cataloged

## Tooling
- [ ] Migration tool selected (GEI, BBS2GH, ADO2GH, gh-gei extension, custom)
- [ ] Tool tested on representative sample
- [ ] Rate-limit / throttling plan
- [ ] Secrets/tokens for migration provisioned

## User mapping
- [ ] User mapping CSV produced
- [ ] Email/username collisions resolved
- [ ] EMU mapping (if applicable)

## Cutover plan
- [ ] Cutover dates agreed with each repo owner
- [ ] Read-only / freeze procedure on source
- [ ] Validation script per repo
- [ ] Rollback procedure documented
- [ ] Communication plan & status page

## Post-migration
- [ ] Branch protection / rulesets applied
- [ ] CI/CD reconnected and verified
- [ ] Webhooks reconfigured
- [ ] Source repo archived / decommissioned
