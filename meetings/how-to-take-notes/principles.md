# Principles of Good Notes

## What notes are *for*

Notes serve **five audiences**, in this order of importance:

1. **Future you** — re-entering the engagement in two weeks
2. **Your delivery team** — the colleague who joins the project mid-flight
3. **The customer** — to confirm shared understanding (via recap)
4. **The next consultant** — picking up after handoff
5. **The auditor** — proving a decision was made deliberately

If your notes don't serve all five, they're incomplete.

## The capture hierarchy

In every meeting, prioritize capture in this order. Drop lower priorities before higher ones if you fall behind:

1. **Decisions** — what was agreed, who agreed, why
2. **Action items** — owner, action, due date
3. **Risks & blockers** — surfaced concerns to feed the register
4. **Customer terminology** — the words *they* use for systems, teams, processes
5. **Open questions** — what we still don't know
6. **Verbatim quotes** — when stakes are high, when wording matters
7. **Discussion flow** — only enough context to make the above make sense
8. **Demo / live observations** — what the customer actually showed you
9. **Your interpretation** — last, and clearly marked

Notice: literal transcription is **last**. Your job isn't to be a court reporter.

## The "decision-rationale-consequence" rule

Every decision should be captured in three parts:

> **[decision]** Use Enterprise Managed Users instead of standard accounts.
> **Rationale:** Regulatory requirement to fully isolate identity from personal GitHub accounts.
> **Consequence:** Re-onboarding for ~400 existing developers; loss of contribution history attribution.

Without rationale, you'll re-litigate the decision in three weeks. Without consequence, you'll look surprised when the trade-off shows up.

## Truth, not transcript

You are paid for **judgment**, not stenography. That means:

- Filter noise: side conversations, scheduling chatter, unrelated tangents.
- Synthesize: "Customer expressed concern about runner cost three times" beats three separate quote captures.
- Surface what's *not* said: silence on a topic is data ("Security team didn't push back on the OIDC plan").
- Note tone shifts: when an exec leans in or a tech lead goes quiet, that's a signal.

## Customer's words, not yours

Use the customer's terminology even when it's "wrong":

- They say "code review board"? Don't translate to "approving reviewer."
- They say "production cluster"? Don't substitute "prod environment."
- They name systems with internal codenames? Use those. Add a glossary file if needed.

Translating to your jargon erodes recognition when you read the notes back to them.

## Write for re-entry, not for the meeting

The test isn't "did I capture this meeting?" It's "can I, three weeks from now, after a vacation, jump back in by reading this note in 90 seconds?"

That means:
- Lead with **objectives** and **outcomes** before discussion.
- Make decisions and actions visually distinct (tables, bold tags).
- Cross-link to ADRs, risk register, engagement README — don't duplicate.
- Avoid pronouns without antecedents ("they said" — who is *they*?).

## Confidentiality is a discipline, not a label

- Mark `[CONFIDENTIAL]` only when content actually warrants it (named individuals, specific contracts, security weaknesses, in-flight legal matters).
- Marking everything confidential equals marking nothing confidential.
- Customer engagement folders are gitignored — that's a guardrail, not an excuse to be sloppy with what you write.

## The asymmetry of effort

Spending **5 extra minutes** at the end of a meeting to clean up notes saves **30+ minutes** later (re-reading recordings, asking the customer to repeat themselves, reconstructing decisions from chat threads).

Treat the cleanup as part of the meeting, not optional homework.
