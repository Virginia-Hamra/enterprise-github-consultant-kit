# Migration & Modernization

> Move legacy estates to modern platforms without breaking the business. The senior-consultant playbook.

---

## 1. Frame the Portfolio: 7 Rs

For each application:

1. **Retire** — decommission
2. **Retain** — keep as-is, monitor
3. **Rehost** — lift-and-shift (VMs → cloud VMs / managed compute)
4. **Relocate** — move the platform (e.g., VMware on cloud)
5. **Replatform** — minor optimizations (managed DB, container)
6. **Repurchase** — switch to SaaS
7. **Refactor / Re-architect** — substantial redesign

A typical large estate: ~10% retire, ~20% rehost, ~30% replatform, ~20% repurchase, ~15% refactor, ~5% retain.

---

## 2. Strangler Fig (Refactor's Workhorse)

Pattern: incrementally route traffic from legacy to new behind a façade until legacy is gone.

```
User → Façade / Proxy
            ├── New service (growing)
            └── Legacy (shrinking)
```

Steps:

1. Identify a **seam** (URL pattern, function boundary, message topic)
2. Insert the façade
3. Build the replacement
4. Cut over per seam, observe, expand
5. Decommission legacy

Senior tip: do **not** boil the ocean. Pick a seam that delivers business value early.

---

## 3. Branch-by-Abstraction (In-Repo Refactor)

When the code itself is the legacy:

1. Introduce an abstraction over the old behavior
2. Add a new implementation behind the abstraction
3. Run both behind a feature flag
4. Migrate traffic
5. Remove old implementation
6. Remove abstraction (optional)

Keeps trunk shippable throughout.

---

## 4. Data Migration Patterns

| Pattern | Use |
|---------|-----|
| Bulk + cutover | Small data, allowed downtime |
| Dual-write | App writes both; consistency burden |
| Outbox + CDC | Capture events, replay; safer |
| Backfill + tail | Bulk historic + live CDC, then switch |
| Read-from-new, write-to-both | Pre-cutover validation |

Always:

- Reconciliation report before cutover
- Reversible cutover plan
- Lock window with owner sign-off

---

## 5. SCM / DevOps Migration to GitHub

Sources → tooling:

| Source | Tool |
|--------|------|
| Azure DevOps Repos + Pipelines | GitHub Enterprise Importer (`gh ado2gh`) |
| GitLab | `gh gl2gh` |
| Bitbucket Server / DC | `gh bbs2gh` |
| Bitbucket Cloud | GEI (preview / partner) |
| GHES → GHEC | GEI org migration |
| SVN / TFVC / Perforce | `git svn`, `git-p4`, partner tools |

Phases:

1. **Discovery** — inventory, sizing, exclusions
2. **Pilot** — 5–10 reps, full lifecycle
3. **Pipeline modernization** — Jenkins / Azure Pipelines / GitLab CI → Actions
4. **Identity reconciliation** — mannequins, IdP mapping
5. **Cutover** — freeze, migrate, validate, redirect
6. **Decommission** — after all traffic confirmed

See: [github-migration-framework](../../github-migration-framework/README.md), [migrations-github-scripts](../../migrations-github-scripts/README.md).

---

## 6. Pipeline Modernization

| From | To (GitHub Actions) |
|------|---------------------|
| Jenkins Groovy | Reusable workflows + composite actions |
| Azure Pipelines YAML | Direct port; jobs → jobs |
| GitLab CI YAML | Translate stages → jobs |
| TeamCity / Bamboo | Rebuild on Actions |
| CircleCI | Direct port |

Senior tips:

- Don't 1:1 port — refactor for reusable workflows + required workflows
- Move secrets to OIDC + environments
- Replace self-hosted shared runners with Actions Runner Controller (ARC) or larger runners
- Standardize required checks via rulesets

---

## 7. Mainframe & Legacy Considerations

- Don't refactor the mainframe in one swing — encapsulate, then strangle
- Use APIs in front (z/OS Connect, IBM API Connect, MQ)
- Separate **batch modernization** from **transaction modernization**
- Treat COBOL knowledge transfer as a critical risk (people > code)

---

## 8. Identity & Mannequin Reconciliation

- Pre-migration: export source identities + map to IdP IDs
- During migration: GEI creates mannequin users for missing mappings
- Post-migration: reconcile mannequins → real users (script via GraphQL)
- Audit: export reconciliation evidence

---

## 9. Cutover Playbook

T-0 readiness:

- [ ] Dry run completed in non-prod
- [ ] Rollback plan + owner
- [ ] Comms plan (internal + customer)
- [ ] Freeze window agreed and announced
- [ ] On-call rotation extended
- [ ] Validation tests automated
- [ ] Sign-off from app owner + security + ops

Run book includes minute-by-minute steps with owners.

---

## 10. Modernization Anti-Patterns

- "Re-platform everything to Kubernetes" without app-by-app analysis
- Ignoring data gravity in cloud moves
- Big-bang cutover for non-trivial systems
- Migrating CI/CD as 1:1 ports (carries the legacy debt)
- No retirement of source systems → double cost forever
- Forgetting people: training, redeployment, change comms

---

## 11. Measurement

- % of portfolio migrated by Phase
- Cost-to-serve before vs after
- DORA metrics on migrated services
- Incident rate during migration window
- Decommission completion rate

---

## 12. References

- [Cloud Strategy](./cloud-strategy.md)
- [Platform Engineering](./platform-engineering.md)
- [Integration Patterns](./integration-patterns.md)
- Sister repos: `github-migration-framework`, `migrations-github-scripts`
