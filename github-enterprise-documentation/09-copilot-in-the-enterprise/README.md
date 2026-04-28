# 09 — Copilot in the Enterprise

> AI-assisted coding under organizational controls. New risk surface for IP, data handling, and developer behavior.

---

## Overview

| SKU (2026) | Audience |
|------------|----------|
| Copilot Free / Pro / Pro+ | Individuals |
| **Copilot Business** | Orgs — IP indemnity, no training on code |
| **Copilot Enterprise** | + Knowledge Bases, Copilot Code Review, Copilot Coding Agent, Copilot Spaces, github.com chat |

Surfaces:

- IDE inline + chat (VS Code, JetBrains, Visual Studio, Xcode, Neovim, Eclipse)
- github.com chat + PR summaries
- **Copilot CLI** (`gh copilot`)
- **Copilot Code Review** (PR reviewer)
- **Copilot Coding Agent** (autonomous PRs)
- **Copilot Spaces / Knowledge Bases** (curated context, RAG grounding)
- **MCP servers** + **Copilot Extensions**

See companion folder: [github-copilot-enablement](../../github-copilot-enablement/README.md) for hands-on configuration.

---

## Configuration

### Policies (Enterprise / Org → Settings → Copilot)

- **Suggestions matching public code** = `Block` for regulated customers (preserves IP indemnity).
- **Content exclusions** for sensitive paths / repos.
- **Editor preview features** opt-in per org.
- **MCP servers** allow / deny.
- **Copilot Chat in github.com / IDE / mobile** toggles.
- **Code Review on PRs** + **Coding Agent** scoped repos.
- **Knowledge Bases** scoped per team.

### Seat management

- Provision via SCIM groups.
- Assign by cost center.
- Quarterly inactive-seat reclaim.

### Audit

- Stream `copilot.*` events to SIEM.

---

## Usage

- Developer is responsible for review of every accepted suggestion.
- No secrets / PII / regulated data pasted into Chat.
- Coding Agent PRs treated like any other PR — required reviewers, required checks.

---

## Best Practices

- Publish an **AI Acceptable Use Policy** before enabling seats.
- Enable duplication detection.
- Configure **content exclusions** for `infra/`, secrets, regulated paths.
- Use **Knowledge Bases** for grounded answers; don't rely on the model alone.
- Measure adoption via Metrics API; do not use individual metrics for performance.

---

## Common Pitfalls

- Copilot enabled without training → uncritical acceptance of hallucinated code.
- Sensitive data pasted into Chat without policy.
- Content exclusions missing on regulated repos.
- Agent PRs auto-merged without human review.
- Treating Copilot as a code-review substitute.

---

## Implementation Notes

- Verify current data-handling and IP-indemnity terms in the **Trust Center**.
- Integrate seat management with IdP offboarding.
- Engage Legal for AI-generated-code IP review before broad rollout (jurisdiction-dependent).
- Cohort + control pilot methodology to attribute uplift; track DORA + survey.

---

## Sources

- [About GitHub Copilot for Business](https://docs.github.com/en/copilot/about-github-copilot/subscription-plans-for-github-copilot)
- [About GitHub Copilot Enterprise](https://docs.github.com/en/copilot/about-github-copilot/subscription-plans-for-github-copilot)
- [Managing policies for Copilot in your organization](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/setting-policies-for-github-copilot-in-your-organization/managing-policies-for-copilot-in-your-organization)
- [Configuring content exclusions](https://docs.github.com/en/copilot/managing-copilot/managing-github-copilot-in-your-organization/setting-policies-for-github-copilot-in-your-organization/excluding-content-from-github-copilot)
- [About Copilot Code Review](https://docs.github.com/en/copilot/using-github-copilot/code-review/using-copilot-code-review)
- [About Copilot Coding Agent](https://docs.github.com/en/copilot/using-github-copilot/coding-agent/about-copilot-coding-agent)
- [About Copilot Spaces](https://docs.github.com/en/copilot/using-github-copilot/copilot-spaces/about-copilot-spaces)
- [About Copilot Knowledge Bases](https://docs.github.com/en/enterprise-cloud@latest/copilot/customizing-copilot/managing-copilot-knowledge-bases)
- [Copilot Extensions](https://docs.github.com/en/copilot/building-copilot-extensions/about-building-copilot-extensions)
- [Copilot Metrics API](https://docs.github.com/en/rest/copilot/copilot-metrics)
- [GitHub Copilot Trust Center](https://resources.github.com/copilot-trust-center/)
