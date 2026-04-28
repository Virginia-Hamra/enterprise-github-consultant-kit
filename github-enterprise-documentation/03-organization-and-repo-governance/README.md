# 03 — Organization & Repository Governance

> Policies, rulesets, and structural conventions that keep GitHub auditable and consistent.

---

## Overview

Governance defines:

- **Org topology** — who owns what
- **Rulesets** — branch / push protections enforced at scale
- **Templates & defaults** — every new repo starts safe
- **Repository lifecycle** — creation → archive → retire
- **Webhooks** — cross-cutting integration plane

Ungoverned environments accumulate orphaned repos, inconsistent branch protection, and undocumented access.

---

## Configuration

### Org structure

- 5–30 orgs typical for a 10k-dev enterprise (not hundreds).
- Naming convention: `{company}-{division}` or `{company}-{product-area}`.
- **Base permission** at org level = `Read` or `None`. Never `Write`.
- Restrict repo creation to defined approvers; block public repos by default.

### Rulesets (preferred over legacy branch protection)

- Org- or enterprise-scoped, applied via repo glob pattern.
- Required reviewers, status checks, signed commits, linear history, push restrictions.
- **Bypass actors** for automation only.
- Use `evaluate` mode before `active` to assess impact.

### Repository templates

- One template per language/platform: includes `CODEOWNERS`, `SECURITY.md`, `.github/PULL_REQUEST_TEMPLATE.md`, default Actions workflow, `.gitignore`, `README` scaffolding.
- Auto-apply `.github` org-level repo for default community health files.

### Webhooks

- Enterprise- or org-level for cross-cutting concerns.
- All webhooks **must use payload secret** verification.

---

## Usage

- New repos created from templates — never from blank.
- `CODEOWNERS` enforces required reviewers via branch protection / rulesets.
- Teams own repos; user-to-repo grants are exceptions.
- Archive at end-of-life; delete only if there is no historical value.

---

## Best Practices

- Prefer **rulesets** over legacy branch protection rules.
- Consistent repo naming: `{team}-{service}-{type}`.
- Limit public repo creation to a documented approval flow.
- Automate repo provisioning (team assignment, labels, board) via webhook.
- Quarterly **repo health report**: last commit, CODEOWNERS presence, branch protection, Dependabot alerts.

---

## Common Pitfalls

- One org per team → unmanageable sprawl.
- Branch protection inconsistent across repos.
- Orphaned repos with stale secrets or vulnerable code.
- Webhooks without secret validation = spoofable.
- Org base permission `Write` = enterprise-wide overpermissioning.

---

## Implementation Notes

- Cap org owners at 2–4; rotate via PAM where possible.
- Use the GraphQL API or `gh` CLI for monthly health reports.
- Use rulesets `evaluate` mode for staged enforcement.
- Webhook handlers must be **idempotent** — GitHub retries.

---

## Sources

- [About repository rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [Creating rulesets for repositories in your organization](https://docs.github.com/en/organizations/managing-organization-settings/creating-rulesets-for-repositories-in-your-organization)
- [About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)
- [About code owners](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Creating a template repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository)
- [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file)
- [About webhooks](https://docs.github.com/en/webhooks)
- [Setting permissions for adding outside collaborators](https://docs.github.com/en/organizations/managing-organization-settings/restricting-collaboration-in-your-organization)
- [Managing the publication of GitHub Pages sites](https://docs.github.com/en/organizations/managing-organization-settings/managing-the-publication-of-github-pages-sites-for-your-organization)
