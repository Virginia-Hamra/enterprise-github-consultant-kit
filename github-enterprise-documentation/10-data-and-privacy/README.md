# 10 — Data & Privacy

> What data lives in GitHub, how it's handled, and how to keep regulated data out.

---

## Advisory Gist

**TL;DR.** Map every data class the customer holds (PII / PHI / cardholder / regulated source). Default = no regulated data in repos, issues, or wikis. Use **GHEC + Data Residency** for EU/AU sovereignty; **GHES** for hard locality; secret scanning + content exclusions for ongoing hygiene. Document retention against the regulator's clock.

**Decisions you will be asked to make**

- Data classification taxonomy that GitHub will respect.
- Residency posture (GHEC / GHEC-DR / GHES).
- Retention windows per data class.
- DSAR / right-to-erasure response process.
- Cross-border transfer mechanism (SCCs / DPF).

**Top edges**

- Issues + wikis + discussions are not always covered by the same controls as repos.
- DSAR on git history is hard — history rewrite is destructive.
- Logs may be retained longer than data — not always intentional.
- Copilot prompts can leak regulated content into telemetry if not gated.

**Connects to**

- [01 Platform Options](../01-platform-options/README.md) — residency by platform.
- [05 Compliance & Audit](../05-compliance-and-audit/README.md) — retention.
- [04 GHAS](../04-security-ghas/README.md) — secret + sensitive-string scanning.
- [09 Copilot](../09-copilot-in-the-enterprise/README.md) — prompt data flow.

**Customer-fit questions**

- What's the worst-case data class that *could* land in a repo today?
- Which regulator dictates location, and what does "location" mean to them?
- Who handles a DSAR on commits authored 5 years ago?

---

## Overview

GitHub stores: code, commit metadata, issues, PRs, comments, Actions logs, Packages, Pages content, telemetry, audit logs.

Each surface has different sensitivity. Some controls are technical (push protection, content exclusions); some are procedural (acceptable-use policy).

---

## Configuration

### Data classification policy

- Document tiers permitted in GitHub: Public / Internal / Confidential / Restricted.
- Map each tier to required controls (visibility, GHAS, exclusions, audit).

### Repository visibility

- Block public repo creation by default; allow via approval workflow.
- Verified org domains + email policy to limit notification leakage.

### Copilot data handling

- Verify Business / Enterprise terms — code is **not** used for training.
- Apply content exclusions for sensitive paths.
- Document GitHub as a sub-processor in your DPA.

### Commit metadata

- Enforce corporate-email commits via git client config + IdP.
- For privacy: GitHub provides a **`noreply` email** option; document policy.

### Actions logs

- Mask secrets with `add-mask`; do not `echo $SECRET`.
- Default retention 90d; align with data-handling policy.
- Restrict log access via repo permissions.

### OAuth & GitHub Apps

- Org owners restrict OAuth App authorizations.
- Quarterly review of installed Apps + scopes.

---

## Usage

- GitHub stores **development artifacts**. Never: production PII, secrets vault content, customer data dumps, document management.
- Test fixtures must be anonymized.
- Issue / PR comments must not contain PII or regulated payloads.

---

## Best Practices

- Maintain a developer-facing "What belongs in GitHub and what doesn't" guide — concrete examples.
- Map GitHub in your **ROPA** (records of processing) under GDPR.
- Review the **GitHub DPA** at enterprise onboarding.
- Audit OAuth Apps quarterly.
- For regulated industries: review every external-data feature (Copilot, Dependabot, Connect) for compliance.

---

## Common Pitfalls

- Production credentials printed in Actions logs (verbose flags, `env`).
- Personal email addresses in commit history → permanent.
- Customer PII in issue comments / PR descriptions.
- Long-lived debug logs containing sensitive output.
- "Secret" gists assumed private (they are URL-only — see [17-additional-services](../17-additional-services)).

---

## Implementation Notes

- Enforce a pre-commit hook in templates that verifies committer email domain.
- Build a redaction lambda for SIEM ingestion if audit-streamed payloads contain regulated identifiers.
- Educate developers on Copilot Chat boundaries before enabling.
- Document data residency: GHEC standard vs **GHEC + Data Residency (EU / AU)** vs GHES.

---

## Sources

- [GitHub Privacy Statement](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement)
- [GitHub Data Protection Agreement](https://docs.github.com/en/site-policy/privacy-policies/github-data-protection-agreement)
- [GitHub Subprocessor List](https://docs.github.com/en/site-policy/privacy-policies/github-subprocessors-and-cookies)
- [About Copilot privacy](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features)
- [Excluding content from GitHub Copilot](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/setting-policies-for-github-copilot-in-your-organization/excluding-content-from-github-copilot)
- [Setting email preferences (noreply)](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address)
- [Masking a value in workflow logs](https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#masking-a-value-in-a-log)
- [Configuring artifact and log retention](https://docs.github.com/en/organizations/managing-organization-settings/configuring-the-retention-period-for-github-actions-artifacts-and-logs-in-your-organization)
- [GitHub Trust Center](https://github.com/trust-center)
- [GHEC with data residency](https://docs.github.com/en/enterprise-cloud@latest/admin/data-residency)
