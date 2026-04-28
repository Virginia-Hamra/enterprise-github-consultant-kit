# Meeting Notes

Customer-organized meeting notes for ongoing engagements.

> **This entire folder (except this README and `_TEMPLATE.md`) is gitignored.** See root `.gitignore`.

## Layout

```
meeting-notes/
├── README.md
├── _TEMPLATE.md                      # Generic note skeleton
└── <customer-slug>/
    ├── 2026-05-12-kickoff.md
    ├── 2026-05-19-discovery-identity.md
    └── ...
```

## Conventions
- One folder per engagement (`<customer-slug>` matching `discovery-assessments/engagements/<slug>`).
- Filename: `YYYY-MM-DD-<short-title>.md`.
- Always start from `_TEMPLATE.md` or a meeting-type template under `../templates/`.
- See [`../how-to-take-notes/`](../how-to-take-notes/README.md) for style rules.

## Cross-linking
From an engagement folder, link to notes here rather than duplicating:

```
discovery-assessments/engagements/<slug>/01-discovery/interview-notes/
  → /meetings/meeting-notes/<slug>/2026-05-19-discovery-identity.md
```
