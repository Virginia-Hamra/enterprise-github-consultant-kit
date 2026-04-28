# Recap and Follow-up

The 24 hours after a meeting determines whether the meeting actually mattered.

## The 5/30/24 rule

- **5 minutes** at end of meeting → confirm decisions and actions verbally.
- **30 minutes** within 2 hours of meeting → clean up notes while context is fresh.
- **24 hours** maximum → recap email sent.

If you skip the 30-min cleanup, the recap takes 90 minutes the next day and is worse.

## Cleanup pass (within 2 hours)

While the meeting is fresh:

1. **Resolve `[needs-review]` markers** — verify with lead consultant.
2. **Convert relative dates** ("next Tuesday") to absolute (`2026-05-12`).
3. **Expand shorthand** (your `→`, `??`, `~` marks) into prose.
4. **De-duplicate** — same point captured twice? Merge.
5. **Cross-link** — wire up references to risk register, ADRs, prior notes.
6. **Promote risks** — copy each `[risk]` into the engagement risk register with an ID.
7. **Promote actions** — copy each `[action]` into the engagement README open-actions list.
8. **Flag ADR candidates** — list `[ADR-needed]` items and assign authors.
9. **Save and commit** — to the engagement folder (gitignored, so this is local-only history).

## The recap email

Send within 24 hours. To: meeting attendees + their managers if appropriate. CC: delivery team.

### Recap email structure

```
Subject: <Customer> — Recap: <Meeting title> — YYYY-MM-DD

Hi all,

Thanks for the time today. Quick recap below — please reply by <date>
if anything is misstated.

DECISIONS
1. <decision in plain language> — decided by <name>.
   Rationale: <one line>.
2. ...

ACTIONS
| # | Action                                  | Owner          | Due        |
|---|-----------------------------------------|----------------|------------|
| 1 | <verb-led action>                       | <name>         | YYYY-MM-DD |

OPEN ITEMS
- <open question> — to be answered by <name> in <forum>.

RISKS RAISED
- <risk> — adding to the engagement risk register as R-NNN.

NEXT
- Next session: <type>, <date>, <objective>.

Full notes: <link if shared> | <or: "available on request">

<sign-off>
```

### Recap principles

- **Decisions first.** Executives stop reading after the first screen.
- **Plain language.** Not jargon, not customer-internal codenames the email recipients won't recognize.
- **Invite correction explicitly.** "Reply if misstated by <date>." Silence then = consent.
- **No surprises.** If something in the recap will surprise an attendee, you've miscaptured a decision — fix it before sending.
- **Don't dump raw notes.** The full notes are working artifacts; the recap is a polished record.

## When to request explicit sign-off

For decisions that are **high-stakes, irreversible, or scope-changing**, the standard recap isn't enough. Request explicit acknowledgment:

> "This recap captures a scope change agreed today. To proceed, please reply with 'approved' by EOD Thursday."

Track these acknowledgments — store the email reply in the engagement folder under `01-discovery/artifacts/decisions/`.

## Updating engagement artifacts

Within the same 24h window:

| Artifact | What to update |
|---|---|
| Engagement README | Open actions list, status updates, decision log entries |
| Risk register | New risks promoted from `[risk]` tags |
| ADR folder | Stub files for `[ADR-needed]` items, authors assigned |
| Stakeholder map | New names that appeared, role / influence updates |
| Glossary | New customer terminology |
| Status section | Latest status with link back to today's note |

If you don't update these, the meeting note becomes orphaned and the engagement loses its memory.

## Handling corrections

When the customer replies with corrections:

1. **Update the recap** as a follow-up message ("Recap v2 — corrected per <name>'s reply").
2. **Update the meeting note** with a `> Correction (YYYY-MM-DD): <what changed and why>` block at the bottom. **Do not silently edit history** — it erodes trust if discovered later.
3. **Update the engagement artifacts** that were affected.

## When you receive *no* response

No response within the deadline = implicit acceptance, **as long as you stated the deadline clearly in the recap**.

For high-stakes items, follow up at the deadline:
> "Just confirming — silence by today's deadline = acknowledgment of the recap below. Will proceed accordingly."

## Async meeting alternatives

For routine status updates, an async written update may be better than a meeting:

- Pro: written record by default, no scheduling tax, attendees read on their own time.
- Con: hard to surface implicit signals (tone, hesitation, body language).

Use async for:
- Weekly status updates with no decisions needed
- Information broadcasts
- Routine check-ins

Use synchronous + recap for:
- Anything with a decision
- First conversation with a new stakeholder
- Architecture or design reviews
- Any session likely to surface conflict or escalation

## Preserve recaps

- Save recap emails to the engagement folder under `01-discovery/artifacts/recaps/` — they're the auditable record.
- For long engagements, generate a **decision log** quarterly by extracting all `[decision]` items across notes. This becomes a closeout artifact.
