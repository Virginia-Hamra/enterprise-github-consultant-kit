# 14 — Migration & Adoption

> Moving repos, history, metadata, and workflows from a source SCM to GitHub. Adoption = the change-management work that makes it stick.

---

## Advisory Gist

**TL;DR.** Use the **official tools** (`gh gei`, `ado2gh`, `bbs2gh`, `gl2gh`). Migrate in **waves**, not big-bang. Modernise pipelines architecturally (reusable workflows, OIDC, ARC), not 1:1. Pre-resolve LFS + identity mapping. Set a hard cutover date for source CI/CD or pay the dual-platform tax forever.

**Decisions you will be asked to make**

- Wave criteria (size, complexity, risk).
- Pipeline modernisation depth (lift-and-shift vs re-architect).
- Identity mapping strategy (mannequin reclamation, SCIM-first).
- Cutover model (per-wave hard cut vs parallel observation window).
- Decommission criteria for the source SCM.

**Top edges**

- Bitbucket Server: SSH (port 7999) reachability is the most common blocker.
- GitLab: API export does not cover all metadata — gap inventory upfront.
- SVN: missing `authors.txt` → history attributed to `localhost`.
- Devs continuing to push to old remote post-cutover → split history.
- External integrations (Jira / Slack) silently broken post-cutover.

**Connects to**

- [02 Identity & Access](../02-identity-and-access-management/README.md) — mannequin reclamation, SCIM.
- [07 GitHub Actions](../07-github-actions/README.md) — pipeline modernisation.
- [github-migration-framework/](../../../github-migration-framework/README.md) — wave playbooks.
- [migrations-github-scripts/](../../../migrations-github-scripts/README.md) — helper scripts.

**Customer-fit questions**

- What is the source SCM, and what is *not* covered by the migration tool?
- How many waves, and what's the criteria to graduate to the next?
- What's the agreed sunset date for the source SCM CI/CD?

---

## Overview

| Source | Tool |
|--------|------|
| GitHub.com / GHEC / GHES → GHEC | **GitHub Enterprise Importer (GEI)** — `gh gei` |
| Azure DevOps Repos → GitHub | **`gh ado2gh`** |
| Bitbucket Server / DC → GitHub | **`gh bbs2gh`** |
| GitLab → GitHub | **`gh gl2gh`** + `gl-exporter` |
| Bitbucket Cloud → GitHub | GEI + partner tools |
| SVN / TFVC / Perforce / ClearCase | `git svn`, `git-p4`, partner tools → then GEI |
| Jenkins / Azure Pipelines / GitLab CI → Actions | Manual modernization (no 1:1 port) |

See companion repos: [github-migration-framework](../../../github-migration-framework/README.md), [migrations-github-scripts](../../../migrations-github-scripts/README.md).

---

## Configuration

### Phases

1. **Discovery & inventory** — repo count, sizes, LFS, ownership, integrations.
2. **Pilot** — 5–10 representative repos through the full lifecycle.
3. **Wave plan** — group by complexity / risk.
4. **Cutover plan** — freeze window, validation, rollback.
5. **Pipeline modernization** — recreate CI as Actions reusable workflows.
6. **Identity reconciliation** — mannequin → real-user via SCIM / GraphQL.
7. **Decommission** — only after parallel observation period.

### Tooling prerequisites

- PATs / GitHub Apps with the right permissions on **source + target**.
- Network reachability from the migration host to source SSH / HTTPS.
- Storage for migration archives.
- Staging org for dry runs.

---

## Usage

- **Always dry-run** to a staging org before production.
- Migrate in waves; never big-bang.
- Validation per repo: history integrity, open PRs, branch protection, CODEOWNERS, Actions, team access.
- Cutover freeze: short, clearly communicated, with on-call coverage.

---

## Best Practices

- Use the official tooling — don't build custom unless source is unsupported.
- Convert pipelines to Actions **architecturally** (reusable workflows, OIDC, ARC) — not 1:1.
- Pre-migrate large binary files to **LFS**.
- Map source identities → IdP IDs **before** migration.
- Define a **hard cutover date** for source-side CI/CD; otherwise parallel cost is permanent.

---

## Common Pitfalls

- No inventory → mid-migration surprises.
- LFS not addressed → bloated / failed migrations.
- Pipelines deferred indefinitely → permanent dual platforms.
- External integrations (Jira, Slack) silently broken post-cutover.
- Devs continue pushing to old remote → split history.

---

## Implementation Notes

- For Bitbucket Server: SSH access (default port 7999) required from migration host.
- For GitLab: API export does not cover all metadata — gap inventory upfront.
- For SVN: provide an `authors.txt` mapping or commits get attributed to `svn-author@localhost`.
- Validate per-repo: `git log --oneline | wc -l` matches source.

---

## Sources

- [About GitHub Enterprise Importer](https://docs.github.com/en/migrations/using-github-enterprise-importer/understanding-github-enterprise-importer)
- [`gh gei` extension (GitHub repo)](https://github.com/github/gh-gei)
- [Migrating from Azure DevOps to GitHub (`ado2gh`)](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-from-azure-devops-to-github-enterprise-cloud)
- [`gh ado2gh` (GitHub repo)](https://github.com/github/gh-ado2gh)
- [Migrating from Bitbucket Server to GitHub (`bbs2gh`)](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-from-bitbucket-server-to-github-enterprise-cloud)
- [`gh bbs2gh` (GitHub repo)](https://github.com/github/gh-bbs2gh)
- [Migrating from GitLab to GitHub (`gl2gh`)](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-from-gitlab-to-github-enterprise-cloud)
- [`gh gl2gh` (GitHub repo)](https://github.com/github/gh-gl2gh)
- [About reclaiming mannequins](https://docs.github.com/en/migrations/using-github-enterprise-importer/completing-your-migration-with-github-enterprise-importer/reclaiming-mannequins-for-github-enterprise-importer)
- [Migrating from Jenkins to GitHub Actions](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/migrating-from-jenkins-with-github-actions-importer)
