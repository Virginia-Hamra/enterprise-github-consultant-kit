# 17 — Additional GitHub Services & Features

> Capabilities that don't sit in the primary 16 domains but matter at enterprise scale. One subsection per feature.

---

## Index

| # | Feature | Status |
|---|---------|--------|
| 17.1 | [GitHub Coding Agents](#171-github-coding-agents) | GA (Pro+ / Enterprise) |
| 17.2 | [AI Models in GitHub](#172-ai-models-in-github) | Evolving |
| 17.3 | [GitHub Codespaces](#173-github-codespaces) | GA (GHEC) |
| 17.4 | [GitHub Packages](#174-github-packages) | GA (GHEC + GHES) |
| 17.5 | [GitHub Pages](#175-github-pages) | GA |
| 17.6 | [Gists](#176-gists) | GA |
| 17.7 | [GitHub Discussions](#177-github-discussions) | GA |
| 17.8 | [GitHub Apps](#178-github-apps) | GA |
| 17.9 | [Scheduled Reminders](#179-scheduled-reminders) | GA (Slack / Teams) |
| 17.10 | [GitHub Projects](#1710-github-projects) | GA |
| 17.11 | [GitHub Insights / Metrics](#1711-github-insights--metrics) | GA |
| 17.12 | [GitHub Sponsors](#1712-github-sponsors) | GA |

---

## 17.1 GitHub Coding Agents

**What:** AI agents (Copilot Coding Agent) that take an issue and produce a PR autonomously, running in an ephemeral cloud environment. See [09-copilot-in-the-enterprise](../09-copilot-in-the-enterprise/README.md) and [github-copilot-enablement/copilot-coding-agent.md](../../github-copilot-enablement/copilot-coding-agent.md).

**Best practices:**

- Required human CODEOWNER on every agent PR.
- Scope agent secrets and network egress; never grant prod-deploy permissions.
- Configure `.github/workflows/copilot-setup-steps.yml` for environment bootstrap.

**Pitfalls:** Bypass rules for "automated fixes" → no human gate; over-broad agent permissions.

**Sources:**

- [About Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/about-copilot-coding-agent)
- [Customizing the development environment for Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/customizing-the-development-environment-for-copilot-coding-agent)

---

## 17.2 AI Models in GitHub

**What:** **GitHub Models** is a model playground / catalog for inference (Llama, Mistral, OpenAI, Cohere, DeepSeek, Phi, etc.) plus the underlying models that power Copilot. Enterprise tier may allow model selection in chat.

**Best practices:**

- Confirm data handling per model — provider varies.
- Approve specific models for sensitive code via policy.
- Monitor the Copilot changelog for model updates.

**Pitfalls:** Assuming model selection implies data isolation.

**Sources:**

- [GitHub Models docs](https://docs.github.com/en/github-models)
- [Choosing an AI model for Copilot](https://docs.github.com/en/copilot/using-github-copilot/ai-models/changing-the-ai-model-for-copilot-chat)
- [Copilot changelog](https://github.blog/changelog/label/copilot/)

---

## 17.3 GitHub Codespaces

**What:** Cloud dev environments (VS Code in a container) defined by `.devcontainer/devcontainer.json`.

**Configuration:**

- Org-level machine type policy, idle timeout, retention.
- Codespaces secrets (user / org level).
- **Prebuilds** for repos with long bootstrap.

**Best practices:**

- `devcontainer.json` for every actively developed repo.
- Idle timeout ≤ 30 min.
- Retention 30 days inactive → delete.
- Never inject production credentials.

**Pitfalls:** Cost runaway; production secrets in dev environments; stale devcontainers.

**Sources:**

- [About GitHub Codespaces](https://docs.github.com/en/codespaces/overview)
- [Managing Codespaces in your organization](https://docs.github.com/en/codespaces/managing-codespaces-for-your-organization/managing-the-cost-of-github-codespaces-in-your-organization)
- [Configuring dev containers](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/introduction-to-dev-containers)
- [Codespaces prebuilds](https://docs.github.com/en/codespaces/prebuilding-your-codespaces)

---

## 17.4 GitHub Packages

**What:** Hosted artifact registry: GHCR (containers), npm, Maven, Gradle, NuGet, RubyGems.

**Configuration:**

- Visibility inherits from repo by default; override per package.
- **OIDC trusted publishing** preferred over PATs.
- Retention via `actions/delete-package-versions` scheduled workflow.

**Best practices:** SHA / digest pulls in production, not mutable tags. Scan images before promoting.

**Pitfalls:** Untagged image accumulation, public packages from private repos, external pull egress costs.

**Sources:**

- [GitHub Packages docs](https://docs.github.com/en/packages)
- [Working with GHCR](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [actions/delete-package-versions](https://github.com/actions/delete-package-versions)

---

## 17.5 GitHub Pages

**What:** Static site hosting from a branch or Actions workflow. Public + private (Enterprise).

**Configuration:** Source = branch or Actions; custom domain + DNS; HTTPS via Let's Encrypt.

**Best practices:** Custom domains + HTTPS; disable Pages on orgs that don't need it; verify private Pages behavior for sensitive content.

**Pitfalls:** Internal docs published publicly; entire build directory copied to Pages.

**Sources:**

- [About GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)
- [Configuring a custom domain for GitHub Pages](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site)
- [Publishing with a custom workflow](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow)

---

## 17.6 Gists

**What:** Single-/multi-file mini-repos. **Public** or **secret** (URL-only, not access-controlled).

**Best practices:** Acceptable-use policy; secret scanning where supported; quarterly audit for high-sensitivity orgs.

**Pitfalls:** "Secret" gists treated as private; internal config / URLs leaked.

**Sources:**

- [About gists](https://docs.github.com/en/get-started/writing-on-github/editing-and-sharing-content-with-gists/about-gists)
- [Secret scanning for gists](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)

---

## 17.7 GitHub Discussions

**What:** Forum-style threaded conversations per repo or org.

**Best practices:** Enable only where there's a real Q&A or community. Moderate. Archive resolved threads. Don't substitute for Issues or ADRs.

**Pitfalls:** Architectural decisions buried in Discussions instead of ADRs.

**Sources:**

- [About discussions](https://docs.github.com/en/discussions/collaborating-with-your-community-using-discussions/about-discussions)
- [Managing discussions for your community](https://docs.github.com/en/discussions/managing-discussions-for-your-community)

---

## 17.8 GitHub Apps

**What:** The default integration mechanism. Authenticates as itself; short-lived installation tokens; granular permissions.

**Best practices:** Minimum permissions; private key in secrets manager; multi-key rotation; one app per integration.

**Pitfalls:** Private key in repo / unencrypted storage; "general purpose" app with broad scope; no rotation.

**Sources:**

- [About GitHub Apps](https://docs.github.com/en/apps/overview)
- [Creating a GitHub App](https://docs.github.com/en/apps/creating-github-apps/registering-a-github-app/registering-a-github-app)
- [Authenticating as a GitHub App installation](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/authenticating-as-a-github-app-installation)
- [Managing private keys](https://docs.github.com/en/apps/creating-github-apps/authenticating-with-a-github-app/managing-private-keys-for-github-apps)

---

## 17.9 Scheduled Reminders

**What:** Slack / Teams notifications for pending PR reviews, configured per team.

**Best practices:** Filter drafts; min PR-age threshold; ≤ 2 reminders / day.

**Pitfalls:** Reminders ignored; OAuth connection silently expires.

**Sources:**

- [Managing scheduled reminders](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-your-membership-in-organizations/managing-your-scheduled-reminders)
- [Enabling scheduled reminders for an organization](https://docs.github.com/en/organizations/managing-organization-settings/managing-scheduled-reminders-for-your-organization)

---

## 17.10 GitHub Projects

**What:** Spreadsheet-and-board work tracking (Projects v2). Aggregates issues / PRs across repos.

**Best practices:** One project per team / product, not per repo. Custom fields + iteration. Archive completed projects.

**Pitfalls:** Work tracked only in Projects (no Issues); public org project leaks roadmap.

**Sources:**

- [About Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)
- [Projects GraphQL API](https://docs.github.com/en/graphql/reference/objects#projectv2)

---

## 17.11 GitHub Insights / Metrics

**What:** Engineering metrics from repo activity (cycle time, review time, deploy frequency). DORA-adjacent.

**Best practices:** Use for process improvement, never individual performance reviews. Combine with qualitative input.

**Pitfalls:** Individual-level metrics; acting on numbers without context.

**Sources:**

- [About engineering metrics in GitHub Enterprise (community / DORA framework)](https://github.blog/2023-08-22-introducing-github-actions-importer/)
- [GraphQL for custom metrics (PR / deploy events)](https://docs.github.com/en/graphql)
- [DORA metrics framework](https://dora.dev)

---

## 17.12 GitHub Sponsors

**What:** Financial support of open-source maintainers / orgs.

**Best practices:** Route enterprise sponsorship through procurement, not personal cards. Track sponsored OSS dependencies as supply-chain investments.

**Pitfalls:** Individuals using personal cards for what should be enterprise procurement.

**Sources:**

- [About GitHub Sponsors](https://docs.github.com/en/sponsors/getting-started-with-github-sponsors/about-github-sponsors)
- [Sponsoring a developer or organization](https://docs.github.com/en/sponsors/sponsoring-open-source-contributors/sponsoring-an-open-source-contributor)
