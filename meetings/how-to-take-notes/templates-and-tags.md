# Templates and Tags

The mechanical conventions that make notes searchable, sortable, and shareable across engagements.

## Filenames

```
YYYY-MM-DD-<short-title>.md
```

- Date first → natural chronological sort in any tool.
- Short kebab-case title (3–6 words).
- One file per meeting; **never** append a later meeting to the same file.

Examples:

- `2026-05-12-kickoff.md`
- `2026-05-19-discovery-identity.md`
- `2026-06-02-architecture-review-runners.md`

For multi-day events (e.g. a 2-day workshop), one file per day:

- `2026-05-19-workshop-day1-discovery.md`
- `2026-05-20-workshop-day2-design.md`

## Frontmatter

Every note opens with the same frontmatter block:

```markdown
> **Meeting:** <title>
> **Date:** YYYY-MM-DD &nbsp; | &nbsp; **Time:** HH:MM–HH:MM <TZ>
> **Type:** Kickoff / Discovery / Working / Architecture review / Demo / Steering / Status / Retro / Handoff
> **Customer:** <name> &nbsp; | &nbsp; **Engagement:** <slug>
> **Attendees:** <name (role, org)>, ...
> **Note-taker:** <name>
> **Recording:** <link or "no recording">
> **Companion:** <link to interview guide / playbook / demo script if applicable>
```

## Required sections

Every note has these, in this order. Empty sections stay (with `_None._`) — their absence is itself information.

1. **Objectives** — 1–3 bullets, stated up front.
2. **Discussion** — flowing notes, organized by agenda or topic-tag.
3. **Decisions** — explicit, with decision-maker.
4. **Action items** — table (see [`decisions-and-actions.md`](decisions-and-actions.md)).
5. **Risks / blockers** — surfaced for the engagement risk register.
6. **Open questions** — for follow-up.
7. **Next steps & next meeting**.

## Inline tags

Use these consistently. They make grep / cross-engagement search trivial.

| Tag | Use for |
|---|---|
| `[decision]` | A formal decision (also captured in Decisions section) |
| `[action]` | A captured action item |
| `[risk]` | A risk to add to the register |
| `[blocker]` | An immediate impediment |
| `[open]` | Open question / unresolved |
| `[obs]` | Observation about dynamics, tone, what's not said |
| `[ADR-needed]` | Decision worth promoting to an ADR |
| `[CONFIDENTIAL]` | Sensitive content (named individuals, security weaknesses, contracts) |
| `[needs-review]` | Note-taker uncertain — verify with lead before sending recap |
| `[customer-quote]` | Verbatim customer language worth preserving |
| `[scope-change]` | Scope creep / scope reduction signal |
| `[follow-up]` | Generic follow-up (lighter than action) |

Tags appear **inline** in flowing notes and may also be the leading marker on a section item.

## Topic tags

Inside `Discussion`, prefix flowing notes with a short topic tag in brackets to enable scanning:

```
[identity]   CISO confirmed EMU mandate
[ci/cd]      Jenkins → Actions migration scoped to top-30 pipelines
[ghas]       Coverage today: ~40%; target ~95% by Q3
[copilot]    Legal review still open; ETA 2026-06-15
[migration]  BBS source has ~3.2k repos; 800 archive candidates
[cost]       FinOps team flagged Actions minute spend trajectory
```

These tags should match the workstreams in the engagement README so notes route automatically.

## Cross-linking

Notes are **link-rich, content-light**. Don't duplicate; link.

From a meeting note:

- Risks → engagement risk register: `[risk] R-007 — see ../../discovery-assessments/engagements/<slug>/02-assessment/risk-register.md`
- Decisions → ADRs: `[ADR-needed] ADR-0007: identity model`
- Actions → engagement README open-actions list
- Cross-meeting: `cf. 2026-05-12-kickoff.md § Decisions`

From the engagement README:

- "Latest status: [2026-05-26-status-update.md](../../meetings/meeting-notes/<slug>/2026-05-26-status-update.md)"

## Customer terminology glossary

For any engagement that runs > 4 weeks, maintain a `glossary.md` in the engagement folder:

```markdown
| Customer term | Means | Notes |
|---|---|---|
| "Code review board" | The required reviewers list per repo | Don't use "approving reviewer" |
| "Production cluster" | Their AKS prod environment | Internal codename: ATLAS |
| "Tier-1 system" | Revenue-impacting | Their classification, not ours |
```

Add to it as you learn. Reference from notes (`see glossary § "Tier-1 system"`).

## Recording references

When recordings exist:

- Link them in frontmatter.
- Reference timestamps for important moments: `(recording @ 23:14)`.
- Don't rely on the recording — your notes are still the source of truth.

When no recording:

- State `Recording: none` in frontmatter.
- Be more thorough in capture; recording isn't a fallback.

## Confidentiality discipline

- The whole `meeting-notes/<customer>/` folder is gitignored. That's a guardrail, not a license to be careless.
- `[CONFIDENTIAL]` marks content you wouldn't share **even within your own consultancy** without the customer's permission.
- Sanitize quotes that name individuals critically before reusing in retros, kit improvements, or case studies.
