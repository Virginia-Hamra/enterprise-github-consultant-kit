# Decisions and Actions That Hold Up

Decisions and actions are the only parts of a meeting note that **must** survive scrutiny weeks or months later. This is how to write them so they do.

## What qualifies as a decision

A decision exists when **all three** are true:

1. A choice was made between **named alternatives** (or the alternative was "do nothing").
2. The decision-maker is **identifiable** and had **authority**.
3. Both parties (consultant + customer) **acknowledged** the choice.

If any leg is missing, it's a *direction*, a *preference*, or *consensus-leaning* — capture it as such, not as a decision.

## Decision template

```markdown
**[decision] <DEC-NNN>** <one-sentence statement>
- **Decided by:** <name, role>
- **Date:** YYYY-MM-DD
- **Rationale:** <one or two sentences>
- **Alternatives considered:** <briefly>
- **Trade-offs accepted:** <what we're giving up>
- **Reversibility:** Easy / Hard / One-way door
- **ADR:** [ADR-NNNN](../../discovery-assessments/engagements/<slug>/03-recommendations/architecture-decisions/) (for significant decisions)
```

`DEC-NNN` is a per-engagement counter that lets you reference decisions across notes.

## When to escalate a decision into an ADR

Promote a decision to an Architecture Decision Record (using `../../discovery-assessments/templates/reports/architecture-decision-record-template.md`) when **any** of these are true:

- It's a **one-way door** (hard to reverse)
- It affects **architecture, security, or compliance** posture
- It will outlast the engagement
- It will likely be **questioned later** ("why did we choose X?")
- It changes the **scope or budget** of the engagement

ADRs are the only artifact that's worth defending in front of an auditor. Everything else is just notes.

## Capturing rationale that survives

The rationale you write must answer the question a future skeptic will ask. Test your rationale with these:

- "Could a reasonable person, reading this in a year, understand why we chose this?"
- "Have I named the **constraint** that drove the choice (regulatory, technical, organizational, budget, time)?"
- "Have I named what we **declined** and why?"

Bad: *"We chose EMU because it's more secure."*
Better: *"Customer's data residency policy mandates full identity isolation from public GitHub. EMU is the only option that satisfies this without operating GHES."*

## When the customer is the decision-maker

Make this **explicit** in the note. The pattern of consulting failure: consultant proposes, customer nods, six months later the customer says "you decided that, not us."

Mitigation:
- Capture decisions in the customer's voice when they made the call:
  > **[decision]** Customer (CISO Jane Doe) elected to enforce SSO at enterprise level by 2026-06-30.
- Send the recap with decisions called out, and explicitly invite correction:
  > "If any decision below was misstated, please reply by EOD Thursday."
- For high-stakes decisions, request explicit sign-off in the recap email.

## What qualifies as an action item

An action item is **work that someone agreed to do**. It is not:

- A topic to revisit (that's an `[open]`)
- A risk to monitor (that's a `[risk]`)
- A vague intention ("we should look into runner costs")

## Action item template

Every action must have:

| Field | Required | Notes |
|---|---|---|
| ID | Yes | `A-NNN` per engagement |
| Description | Yes | Verb-led, specific, single outcome |
| Owner | Yes | One named person |
| Due date | Yes | Absolute (`YYYY-MM-DD`) |
| Status | Yes | Open / In progress / Done / Cancelled / Blocked |
| Dependency | Optional | Other action(s) this blocks on |
| Acceptance | Optional | How we'll know it's done |

```markdown
| ID | Action | Owner | Due | Status |
|---|---|---|---|---|
| A-014 | Produce re-onboarding plan for ~400 devs (EMU migration) covering identity sweep, communications, cutover schedule | Jane Doe (Platform Lead) | 2026-05-30 | Open |
```

## The verbs that make actions real

Actions start with a strong, **outcome-shaped** verb:

- ✅ *Produce*, *Decide*, *Approve*, *Configure*, *Validate*, *Send*, *Schedule*, *Confirm*, *Document*, *Migrate*
- ❌ *Look at*, *Think about*, *Consider*, *Discuss*, *Touch base*, *Sync*, *Explore*

If you find yourself writing a soft verb, the action isn't ready. Push back in the meeting:
> "What does 'look at runner costs' produce? A recommendation? A number? By whom, by when?"

## Owners

- **One person.** Joint ownership = no ownership.
- A team is not an owner. The team's lead is.
- The customer **must** own customer-side actions. Resist taking them on yourself out of politeness — you'll be doing the work without authority.

## Due dates

- Always **absolute** in the cleaned-up note (`2026-05-30`), even if captured as relative ("next Tuesday") in flow.
- If the owner can't commit to a date, capture it as `Due: TBC by <date>` — that itself becomes a mini-action.
- Default to dates **before** the next status meeting, so progress is visible at the cadence the customer is already attending.

## Verifying actions before close

In the last 5 minutes of the meeting:

1. Read each action **out loud**, including owner and date.
2. Listen for hesitation. "Yeah, sure" with a pause is *not* commitment.
3. If anything is fuzzy, ask: "What's the first concrete step? What would block you?"
4. Confirm the channel for status updates (status meeting, async update, ticket).

## Tracking across meetings

Action items live in **two** places:

1. The meeting note where they were created (immutable record)
2. The engagement-level **open actions list** in the engagement README (rolling source of truth)

Reconcile after each meeting. The engagement README is what you screenshare in the next status meeting.

## Closing actions

When an action is done:

- Update status in the engagement README.
- Don't edit the original meeting note (preserve the record). Reference: "[A-014 closed in 2026-05-28-status-update.md]"
- For high-stakes actions, capture the **evidence** (link, screenshot, artifact) so it's auditable.

## Cancelled / deferred actions

Failure mode: actions silently age into irrelevance. Counter:

- At each status meeting, review aging open actions.
- If something has been open for 3+ status cycles with no progress, **decide explicitly**:
  - Cancel (with reason)
  - Re-scope (re-create as a new action with a new owner / definition)
  - Escalate (block list goes to steering)
- Don't let actions die quietly. The customer notices.

## Decisions and actions in the recap email

Lead the recap with these. They're what executives read. See [`recap-and-followup.md`](recap-and-followup.md).
