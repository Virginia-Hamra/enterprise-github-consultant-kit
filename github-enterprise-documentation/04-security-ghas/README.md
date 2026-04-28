# 04 — Security: GitHub Advanced Security (GHAS)

> Detection layer for secrets, vulnerable code, and vulnerable dependencies. **In 2024 GHAS was unbundled** into two add-ons: **Code Security** and **Secret Protection**. Verify current SKU before contracting.

---

## Overview

| Capability | What it does |
|------------|--------------|
| **Secret scanning** + **push protection** | Detect / block secret commits |
| **Code scanning** (CodeQL or third-party) | SAST in PRs and on push |
| **Dependabot alerts / updates** | SCA + auto-PRs |
| **Dependency review** | Block PRs introducing vulnerable deps |
| **Security overview** | Alert dashboards, security campaigns |
| **Copilot Autofix** | AI-suggested fixes for code-scanning alerts |
| **Custom patterns** | Org-specific secret formats |
| **Private vulnerability reporting** | Coordinated disclosure |

GHAS is **detection + alerting**. Triage and remediation are owned by repo / engineering teams; platform team owns policy.

---

## Configuration

### Secret Protection

- Enable at **org / enterprise** level.
- Enable **push protection** alongside scanning — they are independent toggles.
- Add **custom patterns** for internal token formats.
- Enable **non-provider patterns** for general high-entropy / generic credentials.
- Stream `secret_scanning.*` audit events to SIEM.

### Code Security

- **Default setup** for most repos (auto-detected language, CodeQL).
- **Advanced setup** for monorepos / custom build steps (Actions-based workflow).
- Choose `security-and-quality` or `security-extended` query suite based on triage capacity.
- Pin the CodeQL action to a SHA in advanced setup.

### Dependabot

- Org-level alerts on.
- **Security updates** (auto-PRs) only on repos with reviewer capacity.
- **Version updates** opt-in per repo via `.github/dependabot.yml`.
- Group PRs to reduce noise.

### Dependency Review

- Add `dependency-review-action` to PR workflows; configure severity threshold.

### Security overview

- Enterprise + org dashboards: alert volume, MTTR, coverage.

---

## Usage

- Secret alerts treated as **incidents** → rotate the secret, then close.
- Code-scanning alerts triaged by owning team to documented SLA.
- Push-protection bypass events reviewed daily by security.
- Dismissals require a reason; audited monthly.

---

## Best Practices

- GHAS on **all** code repos, not just prod.
- Push protection bypass = tier-2 security alert.
- Pin CodeQL Action to commit SHA.
- Automate alert metrics via API; manual review doesn't scale.
- Suggested SLA: critical 7d, high 30d, medium 90d.

---

## Common Pitfalls

- Secret scanning enabled without push protection → already-leaked secrets.
- Code-scanning alerts ignored → developer trust collapses.
- Dependabot auto-PRs without merge capacity → unmerged backlog.
- Custom secret patterns set once, never updated.
- CodeQL on unsupported languages assumed to be coverage.

---

## Implementation Notes

- GHAS billing per **active committer / month** to GHAS-enabled repos. Audit committer counts before enablement.
- **Pilot** in a high-risk cohort first; measure triage capacity; roll out in waves.
- Integrate alerts into Jira / ServiceNow for SLA tracking.
- Review GitHub-supplied secret pattern catalog regularly — it expands frequently.

---

## Sources

- [GitHub Advanced Security — overview](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [About secret scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning)
- [Push protection for repositories and organizations](https://docs.github.com/en/code-security/secret-scanning/working-with-secret-scanning-and-push-protection/about-push-protection)
- [Defining custom patterns for secret scanning](https://docs.github.com/en/code-security/secret-scanning/using-advanced-secret-scanning-and-push-protection-features/custom-patterns/defining-custom-patterns-for-secret-scanning)
- [About code scanning with CodeQL](https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql)
- [Setting up code scanning at scale](https://docs.github.com/en/code-security/code-scanning/enabling-code-scanning/configuring-default-setup-for-code-scanning)
- [About Copilot Autofix](https://docs.github.com/en/code-security/code-scanning/managing-code-scanning-alerts/about-autofix-for-codeql-code-scanning)
- [About Dependabot alerts](https://docs.github.com/en/code-security/dependabot/dependabot-alerts/about-dependabot-alerts)
- [Configuring Dependabot security updates](https://docs.github.com/en/code-security/dependabot/dependabot-security-updates/configuring-dependabot-security-updates)
- [Dependency review action](https://github.com/actions/dependency-review-action)
- [About security campaigns](https://docs.github.com/en/code-security/security-campaigns/about-security-campaigns)
- [Private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
