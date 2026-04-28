# Engagements

Per-customer assessment outputs. **This entire folder is gitignored** to prevent accidental commit of customer data — see root `.gitignore`.

## Starting a new engagement

```bash
cp -R discovery-assessments/engagements/_TEMPLATE \
      discovery-assessments/engagements/<customer-slug>
```

Recommended slug format: `<short-customer-name>` (lowercase, hyphenated). Optionally suffix with engagement code, e.g. `acme-corp-2026q2`.

## Folder shape (per engagement)

```
<customer-slug>/
├── README.md                     # Engagement charter & status
├── 01-discovery/
│   ├── stakeholder-map.md
│   ├── current-state.md
│   ├── completed-questionnaires/
│   └── interview-notes/          # ← link out to /meetings/meeting-notes/<slug>/
├── 02-assessment/
│   ├── gap-analysis.md
│   ├── risk-register.md
│   └── scorecards/
└── 03-recommendations/
    ├── current-state-assessment.md
    ├── recommendations-roadmap.md
    ├── architecture-decisions/
    └── executive-summary.md
```

## Conventions
- Date filenames `YYYY-MM-DD-...` for time-series notes.
- Cross-link to `/meetings/meeting-notes/<customer-slug>/` rather than duplicate notes.
- Keep customer-uploaded artifacts under `01-discovery/artifacts/` (also gitignored).
- At engagement close: produce a sanitized retrospective and archive the entire folder per data-handling policy.
