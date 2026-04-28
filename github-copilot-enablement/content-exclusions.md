# Copilot Content Exclusions

> Configure paths and repositories that Copilot should **not** see. Critical control for regulated, IP-sensitive, or secret-bearing code.

---

## 1. What Content Exclusions Do

When a path or repository is excluded:

- Copilot **does not** generate completions in those files
- Copilot Chat **does not** use that content as context
- The exclusion applies to **all users** in the org / repo
- Files are still visible to humans; this is an AI-assist guardrail, not access control

Exclusions are configured at:

- **Organization level** — applies to all repos in the org
- **Repository level** — only that repo

---

## 2. What to Exclude (Baseline)

### High-Risk Paths

```text
**/secrets/**
**/.env
**/.env.*
**/*.pem
**/*.key
**/*.pfx
**/credentials/**
**/private/**
**/*.kdbx
```

### Infrastructure with Embedded Identifiers

```text
infra/prod/**
terraform/**/*.tfvars
helm/**/values-prod.yaml
ansible/inventories/prod/**
```

### Regulated / Audit-Critical

```text
compliance/**
audit/**
legal/**
contracts/**
```

### Customer / PII Data Fixtures

```text
**/test-data/pii/**
**/fixtures/customers/**
**/seed/**
```

---

## 3. Configuration

### Organization-level (UI)

`Org → Settings → Copilot → Content exclusions`

```yaml
# Example org-level exclusion entry
"*":
  - "**/secrets/**"
  - "**/.env*"
  - "**/*.pem"
"infra-*":          # any repo matching glob
  - "terraform/**/*.tfvars"
```

### Repository-level

```yaml
# .github/copilot-content-exclusions.yml (or via repo settings)
paths:
  - "src/legal/**"
  - "data/customers/**"
```

---

## 4. Validation Workflow

1. Apply exclusions in a **non-prod** org first
2. Open an excluded file in VS Code → confirm completions are silent and a banner indicates exclusion
3. Ask Copilot Chat about the excluded path → it must refuse / not surface content
4. Audit-log filter: search for `copilot.content_exclusion` events

---

## 5. Limits & Caveats

- Maximum entries per org / repo (consult current GitHub docs — limits change)
- Excluded files are still **indexed for code search** — exclusion is for Copilot only
- Forks may inherit / not inherit exclusions depending on visibility — verify
- Glob patterns are evaluated relative to repo root
- Symlinks are followed — exclude the target as well

---

## 6. Operational Runbook

| Event | Action |
|-------|--------|
| New regulated repo | Apply org-default + repo-specific entries before first commit |
| Secret accidentally committed | Add path to exclusions, rotate secret, run `git filter-repo` |
| Audit finding | Pull `copilot_content_exclusion.*` audit events for evidence |
| Quarterly review | Reconcile exclusions with current repo inventory |

---

## 7. Pairs Well With

- [Secret scanning + push protection](../README.md#23-security--advanced-security-ghas)
- [Repository rulesets](../README.md#24-governance-risk--compliance-grc)
- [Custom CodeQL queries](../README.md#23-security--advanced-security-ghas)
