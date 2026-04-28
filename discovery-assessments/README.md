# Discovery & Assessments

Reusable assets and per-engagement workspace for **discovery, current-state assessment, gap analysis, and recommendations** on GitHub Enterprise consulting engagements.

## Structure

```
discovery-assessments/
├── templates/        # Reusable kit assets (versioned in this repo)
│   ├── questionnaires/    # Structured intake by domain (identity, security, CI/CD, ...)
│   ├── checklists/        # Phase-based checklists (kickoff, onboarding, handoff, ...)
│   ├── interview-guides/  # Audience-specific interview scripts
│   ├── scorecards/        # Maturity / posture scoring rubrics
│   └── reports/           # Deliverable templates (assessment, gap, roadmap, risk)
├── frameworks/       # Reference rubrics and compliance mappings
│   ├── maturity-model.md
│   └── compliance-mappings/   # SOC2, ISO 27001, NIST 800-53
└── engagements/      # Per-customer outputs (gitignored — sanitized only)
    └── _TEMPLATE/    # Copy this to start a new engagement
```

## How to use

### Starting a new engagement
1. Copy `engagements/_TEMPLATE/` to `engagements/<customer-slug>/` (e.g. `acme-corp`).
2. The customer folder is **gitignored by default** — confirm `.gitignore` excludes it before adding any customer data.
3. Work through `01-discovery/` → `02-assessment/` → `03-recommendations/` in order.
4. Pull blank questionnaires, checklists, and report templates from `templates/` as needed.

### Updating the kit
- Improvements discovered during an engagement → sanitize and contribute back to `templates/` or `frameworks/`.
- Treat `templates/` as **versioned product** — review changes via PR.

## Phase mapping

| Engagement phase | Primary assets |
|---|---|
| Pre-kickoff | `templates/checklists/pre-engagement-checklist.md` |
| Kickoff | `templates/checklists/kickoff-checklist.md`, `templates/interview-guides/` |
| Discovery | `templates/questionnaires/`, `templates/interview-guides/` |
| Assessment | `templates/scorecards/`, `frameworks/maturity-model.md` |
| Recommendations | `templates/reports/gap-analysis-template.md`, `recommendations-roadmap-template.md` |
| Handoff | `templates/checklists/handoff-checklist.md` |

## Data handling

> **Critical:** Customer data must never land in `templates/` or `frameworks/`. Only sanitized, generalized patterns belong in the reusable kit. See root `copilot-instructions.md` § *Customer Data Handling*.
