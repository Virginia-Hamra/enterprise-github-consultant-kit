# Meetings

Reusable meeting playbooks **and** customer-organized meeting notes for ongoing engagements.

## Structure

```
meetings/
├── how-to-take-notes/    # Note-taking craft guide (read first)
├── how-to-ask-questions/ # Asking craft guide (read alongside)
├── templates/            # Generic meeting templates by meeting type
├── prep/                 # Reusable prep checklists & agendas
├── demo/                 # Demo scripts & scenarios (reusable)
├── working-session/      # Hands-on working session playbooks
└── meeting-notes/        # Customer-organized notes (gitignored)
    └── <customer-slug>/
```

## Craft guides (read first, refer often)

- [`how-to-take-notes/`](how-to-take-notes/README.md) — principles, capture techniques, decisions & actions, recap discipline, anti-patterns
- [`how-to-ask-questions/`](how-to-ask-questions/README.md) — question types, probing, audience tuning, difficult conversations, question bank by domain

## How to use

### Before any meeting
1. Pick the right **template** from `templates/` for the meeting type.
2. Run the matching **prep** checklist from `prep/`.
3. If demoing, pull a script from `demo/`.
4. If running a working session, pull a playbook from `working-session/`.
5. Create the notes file in `meeting-notes/<customer-slug>/YYYY-MM-DD-<title>.md` using `meeting-notes/_TEMPLATE.md`.

### After every meeting
- Capture decisions, action items (owner + due date), and risks.
- Cross-link from the engagement folder (`discovery-assessments/engagements/<slug>/`).
- Send recap email within 24h.

## Meeting type → template mapping

| Meeting type | Template | Prep | Notes location |
|---|---|---|---|
| Kickoff | `templates/kickoff-meeting.md` | `prep/kickoff-prep.md` | `meeting-notes/<slug>/` |
| Discovery interview | `templates/discovery-interview.md` | `prep/discovery-prep.md` | `meeting-notes/<slug>/` |
| Working session | `templates/working-session.md` | `prep/working-session-prep.md` | `meeting-notes/<slug>/` |
| Architecture review | `templates/architecture-review.md` | `prep/architecture-review-prep.md` | `meeting-notes/<slug>/` |
| Demo | `templates/demo-session.md` | `prep/demo-prep.md` | `meeting-notes/<slug>/` |
| Steering committee | `templates/steering-committee.md` | `prep/steering-prep.md` | `meeting-notes/<slug>/` |
| Status update | `templates/status-update.md` | — | `meeting-notes/<slug>/` |
| Retrospective | `templates/retrospective.md` | — | `meeting-notes/<slug>/` |
| Handoff | `templates/handoff-meeting.md` | `prep/handoff-prep.md` | `meeting-notes/<slug>/` |

## Data handling
`meeting-notes/<customer-slug>/` is **gitignored**. Templates and playbooks are sanitized & shareable; customer notes are not.
