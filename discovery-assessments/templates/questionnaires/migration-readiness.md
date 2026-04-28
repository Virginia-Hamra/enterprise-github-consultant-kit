# Migration Readiness Questionnaire

> **Audience:** Migration owner / source platform admins &nbsp; | &nbsp; **Time:** ~60 min

## Source inventory
1. Source platform(s) (Bitbucket Server/DC, GitLab self-managed/SaaS, Azure DevOps, GHES, ...)?
2. Repository count, total size, largest repo?
3. LFS usage? Submodules? Monorepos?
4. Active vs. archived repos?

## Metadata to migrate
- [ ] Code history (full vs. shallow)
- [ ] PRs / MRs (open + closed)
- [ ] Issues
- [ ] Wikis
- [ ] Releases / tags
- [ ] Pipelines / CI history
- [ ] User mappings
- [ ] Webhook configs
- [ ] Deploy keys / service accounts

## Constraints
1. Downtime tolerance per repo / per org?
2. Compliance / chain-of-custody requirements?
3. Cutover blackout windows?
4. Network constraints (private connectivity, egress restrictions)?

## Cutover plan
1. Wave strategy (by team, by criticality, by risk)?
2. Validation criteria per repo?
3. Rollback plan?
4. Communication plan?

## Artifacts requested
- [ ] Source inventory CSV (org/project, repo, size, last activity)
- [ ] Active integrations list
- [ ] Existing migration attempts / lessons learned
