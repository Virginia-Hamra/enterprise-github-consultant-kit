# Capture Techniques

How to take notes in real time without falling behind, missing decisions, or producing unreadable walls of text.

## Setup before the meeting starts

- Open the right template (`../templates/<type>.md`) and pre-fill frontmatter.
- Pre-list **objectives** and **agenda** — they become headings.
- Pre-write your **probing questions** so you don't have to invent them mid-flow.
- Have the **engagement README**, **risk register**, and prior meeting note open in tabs for cross-reference.
- Mute notifications. Close Slack/Teams/email.

## Roles: scribe vs. lead

Don't lead and scribe in the same head. If you must, narrate it: "Let me capture that — give me ten seconds."

- **Lead consultant** — drives conversation, listens deeply, watches body language.
- **Scribe** — captures, asks for clarification on actions, owns the recap.
- For a critical meeting (kickoff, steering, architecture review): always have a dedicated scribe.

## The 4-column live capture

Inside the **Discussion** section, the fastest readable structure is a running log with 4 implicit columns of meaning:

```
HH:MM  [topic-tag]  speaker?: terse summary  → [decision]/[action]/[risk]/[open]
```

Example:
```
10:14  [identity]   CISO: pushed back on standard accounts; firm requirement for EMU  → [decision] EMU
10:18  [identity]   PlatLead: ~400 devs already onboarded; re-onboarding required     → [risk] R-007
10:22  [identity]   action: Platform team to produce re-onboarding plan by 2026-05-30  → [action]
```

You can drop the time and speaker when not needed. The **bracketed tags** are what make it scannable later.

## Shorthand vocabulary

Adopt a small, consistent shorthand:

| Mark | Meaning |
|---|---|
| `→` | leads to / therefore |
| `?` | open question (capture, don't answer) |
| `!` | surprising or important |
| `??` | I didn't understand — ask for clarification before close |
| `~` | approximate / TBC |
| `vs.` | trade-off being weighed |
| `cf.` | compare to (link prior topic / decision) |
| `[…]` | section omitted (you skipped capture deliberately) |

These are for **your** scratch capture — clean them up before sharing the recap.

## Capturing decisions in real time

The moment you hear consensus forming, *interrupt your own typing* and capture:

```markdown
**[decision]** <one-sentence statement of what was decided>
- Decided by: <name(s)>
- Rationale: <one line>
- Consequence / trade-off: <one line>
```

If rationale or consequence aren't clear, **ask out loud**:
> "To make sure I capture this — we're going with EMU because of identity isolation requirements, and we're accepting the re-onboarding cost. Anything I'm missing?"

That single sentence often catches misalignment **before** it goes into a doc as a "decision."

## Capturing actions in real time

Every action needs three things or it's not an action:

1. **Verb-led description** — "Produce re-onboarding plan", not "Re-onboarding"
2. **Single named owner** — "Customer platform team" is not an owner; *one human* is
3. **Due date** — relative ("by next Tuesday") is fine in flow; convert to absolute date in cleanup

If any of the three is missing, **say so out loud**:
> "Who owns producing the re-onboarding plan, and by when?"

If the room can't answer that, capture as `[open]` not `[action]`.

## Capturing what's *not* said

- Silence after a proposal → uncertainty, not agreement
- A topic deferred twice → political, not technical
- Same person answering questions intended for someone else → real owner is absent
- A name that gets dropped repeatedly → escalation point

Capture these as observations:
```
[obs] Security lead didn't push back on OIDC plan — likely already aligned, confirm in 1:1.
[obs] CFO name keeps coming up on cost questions — book her into the next steering.
```

## Demo / live walkthrough capture

When the customer is sharing screen and walking through their setup:

- **Screenshots > prose** for configuration. Save to `01-discovery/artifacts/` (gitignored).
- Capture the **path** they navigated (Settings → Org → Authentication → SAML), not what each screen looks like.
- Note the version / date of what you saw — configurations change.
- Capture **versions, counts, sizes** verbatim (e.g. "GHES 3.12.4, ~12k repos, ~8TB total").

## Catching up after a stretch you couldn't capture

You will fall behind. When you do:

1. Don't try to backfill prose — you'll get the words wrong.
2. Capture **the outcome** of the stretch you missed: "[summary] 10:15–10:32 — extended discussion on runner strategy; landed on ARC for self-hosted, GitHub-hosted for default."
3. Mark it `[needs-review]` and verify with the lead consultant before sending recap.

## Closing the meeting

Reserve **the last 5 minutes** for note-taking hygiene. This is non-negotiable in critical meetings.

1. Read aloud each **decision** captured. Ask: "Anyone disagree with how I've stated this?"
2. Read aloud each **action**. Confirm owner + due date verbally.
3. Read aloud **open questions**. Assign each one to a follow-up channel.
4. Confirm the next meeting (date, attendees, objective).

This 5 minutes is where notes go from "draft" to "evidence."

## Tools

- Plain Markdown is the lowest-common-denominator format. It survives every tool transition.
- A second screen / monitor helps — note doc on one, customer screen-share on the other.
- Voice recording (with consent) as **backup**, not primary capture. You won't go back and listen unless something blew up.
- Avoid AI auto-transcripts as the primary record — they over-capture and mislabel speakers; useful only for verbatim quote retrieval after the fact.
