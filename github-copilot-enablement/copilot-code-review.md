# Copilot Code Review

> Automated, AI-assisted PR review on github.com and in IDEs. Available with **Copilot Enterprise** (and partially Business).

---

## 1. What It Is

Copilot Code Review acts as an automated reviewer that:

- Posts inline review comments on diffs
- Suggests concrete code fixes (one-click apply)
- Summarizes the PR's intent and risk
- Enforces **custom review instructions** stored in the repo

It is **assistive**, not gating — it does not satisfy `required reviewers` rules unless explicitly configured by policy.

---

## 2. Enablement

### Org / Enterprise

`Org → Settings → Copilot → Code review` → enable for selected repos or all.

### Repository

```yaml
# .github/copilot-instructions.md (general repo guidance)
# .github/instructions/*.instructions.md (path-scoped, recommended)
```

Path-scoped example:

```markdown
---
applyTo: "src/api/**/*.ts"
---
# API Review Rules

- All endpoints must validate input with zod
- 4xx errors must use the shared `ApiError` class
- No raw SQL — use the repository layer
```

### PR-level

- Request review from `@copilot-pull-request-reviewer` (or use the "Review changes" button if surfaced)
- Re-request after pushes — review is regenerated

---

## 3. Custom Instructions Strategy

| Layer | File | Scope |
|-------|------|-------|
| Repo-wide | `.github/copilot-instructions.md` | Every PR in repo |
| Path-scoped | `.github/instructions/*.instructions.md` with `applyTo` glob | Files matching pattern |
| Org-wide | Org-level instructions (Enterprise) | All repos |

Best practices:

- Keep each instruction file under ~200 lines (context budget)
- Use **imperative voice**: "Use X", "Reject Y"
- Include **anti-patterns** — what to flag
- Reference linters/standards, don't duplicate them
- Version-control like code — review changes via PR

---

## 4. What to Review With Copilot

Effective use cases:

- Style & convention drift
- Missing tests / docstrings
- Obvious null / error-handling gaps
- Repetitive boilerplate violations
- Cross-cutting policies (logging, auth, telemetry)

Do **not** rely on Copilot Code Review alone for:

- Security-critical changes (use [GHAS](../README.md#23-security--advanced-security-ghas) + human review)
- Architectural decisions
- Cryptography, auth/authz logic
- Compliance-bearing changes

---

## 5. Branch Protection Integration

Recommended ruleset:

- Required reviewers: ≥1 human CODEOWNER
- Required status checks: CodeQL, tests, lint
- Optional: require Copilot review acknowledgment
- Block merging until Copilot's blocking comments are resolved (manual policy)

---

## 6. Measurement

Track via [Copilot Metrics API](./metrics-and-measurement.md):

- Review comments produced per repo
- Acceptance rate of suggested fixes
- Time-to-first-review (Copilot vs. human)
- PRs reviewed by Copilot only vs. Copilot + human

KPI target (mature org): Copilot reviews ≥ 80% of PRs within 2 minutes of open.

---

## 7. Common Pitfalls

- Generic instructions ("write good code") → noisy, low-signal comments
- Too many instruction files → context window bloat, slow reviews
- Treating Copilot review as gating without human follow-up → false security
- Not retraining instructions when standards evolve

---

## 8. References

- [Custom Agents](./custom-agents.md)
- [Copilot Instructions](./copilot-instructions.md)
- [Strategic Workflows](./strategic-workflows.md)
