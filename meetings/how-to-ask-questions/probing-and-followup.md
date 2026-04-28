# Probing and Follow-up

When the first answer is shallow, surface, or evasive — how to go deeper without being adversarial.

## Recognize a shallow answer

Shallow answers sound complete but leave the *real* question unanswered. Watch for:

- **Slogans**: "We follow least privilege" (says nothing about actual permissions).
- **Tool names without practice**: "We have GHAS" (licensed ≠ used).
- **Passive voice**: "It's been managed for years" (managed how, by whom?).
- **Generalities**: "Mostly," "generally," "in most cases" (where are the exceptions?).
- **Deflections**: "That's a great question, the security team would know…"
- **Future tense for current state**: "We're working toward…" (what's true *now*?).

These are not lies — they are the path of least conversational resistance. Your job is to redirect to the substance.

## Probing techniques

### 1. The 5 whys (ladder down)

Repeatedly ask "why" or "what drove that" to find the root rationale or constraint.

> "Repo creation is restricted to admins."
> *"What drove that?"*
> "Sprawl."
> *"What kind of sprawl was the issue?"*
> "Orphan repos."
> *"What made orphans hard to manage?"*
> "We couldn't tell who to ask about them."
> → Real problem: **ownership metadata**, not creation gating.

Stop when you hit a stable constraint (regulation, budget, hard policy) or a clear root cause.

### 2. Laddering up

The opposite: starting from a tactical answer, climb to the strategic intent.

> "We need OIDC to AWS by Q3."
> *"What does OIDC unlock for you?"*
> "Removes long-lived secrets in GitHub."
> *"And what does removing long-lived secrets enable?"*
> "Passes our audit and lets us rotate accounts faster."
> → Connects the tactical ask to the **business outcome** (audit + agility), reframes priority.

Use to align technical discussion with executive value.

### 3. Concrete-ize

When answers are abstract, force a concrete example:

- *"Walk me through the last time that happened."*
- *"Pick a real repo — let's look at its access list."*
- *"What did that look like in the most recent PR?"*

Concrete examples expose discrepancies between policy and practice.

### 4. The numerical probe

Counter slogans with numbers:

> "We follow least privilege."
> *"What percent of users today have org-owner or admin role across your top-20 repos?"*

If they can't answer, the slogan is unverified.

### 5. The "and what else" probe

Customers often state the **first** problem, not the **biggest** one:

> "Onboarding is slow."
> *"And what else is slow?"*
> "PR review backlogs."
> *"And what else?"*
> "Honestly… runner queue waits — that's actually the worst right now."

Loop 2–3 times. The third item is often the real top problem.

### 6. The contrast probe

Counter a confident statement with the opposite:

> "Branch protection is enforced everywhere."
> *"Where is it weakest?"*

Forces a more honest answer than "anywhere it isn't?"

### 7. The "who would disagree" probe

Surface political tension:

> "We're aligned on the EMU plan."
> *"Who, if asked, would push back the hardest?"*

This both:
- Identifies stakeholders you need to talk to.
- Tells you whether the "alignment" is real.

## Handling deflection

When the customer punts a question to "the X team":

- **Capture the redirect**: `[open] route to <team> via <person>`
- **Ask their version anyway**: *"Understood — and from your seat, what's your read on it?"*
- **Probe ownership**: *"Should that be your team's responsibility, or do you think it's correctly with them?"*

Deflection is data — it tells you where ownership is unclear.

## When the customer doesn't know

Distinguish three flavors of "I don't know":

1. **"I'd have to check"** → factual gap, easy follow-up. Capture as `[open]` with owner.
2. **"Nobody knows"** → systemic gap. This is a finding, not a follow-up.
3. **"I don't know but I should"** → embarrassment signal. Don't press in the meeting; capture privately.

For (2) and (3), respond with neutrality: *"Got it — that's useful to know. Let's note it as something to figure out."* Never make the customer feel exposed.

## Pacing: when to probe, when to move on

Probe when:
- The answer affects a major decision.
- Something doesn't add up against earlier answers.
- You sense the surface answer hides a politically loaded reality.
- You're testing a specific hypothesis.

Move on when:
- You have enough for the decision at hand.
- The probe is producing diminishing returns.
- The customer is showing signs of fatigue.
- The topic belongs in a different forum / audience.

A good rule: **3 probes per topic in a single meeting.** More than that, schedule a deep-dive.

## Repeat-back as a probe

A summarizing repeat-back is a probe in disguise:

> "Let me play that back — your CI is mostly Jenkins, with maybe 20 percent of teams already on Actions, and the migration is blocked on runner strategy. Is that fair?"

Three good outcomes:
- **"Yes"** → confirmed.
- **"Mostly — but the blocker is actually licensing, not runners"** → a precise correction (gold).
- **"No, totally different…"** → resets the conversation, saving you from a wrong recommendation.

Use this every 10–15 minutes in a long discovery session.

## Capturing probe outcomes

In your notes (see [`../how-to-take-notes/capture-techniques.md`](../how-to-take-notes/capture-techniques.md)):

- Capture the **first answer** AND the **deeper answer** — both are data.
- Note the probe used (laddering, concrete-ize, etc.) — useful for retros and skill-building.
- Flag where probes were **deflected** — you'll want to revisit with the right person.

## Knowing when to stop probing

Probing is powerful and easy to overuse. Stop when:

- The customer's answers are getting shorter and more guarded.
- You see fatigue signals (looking at clock, less detail, defensive tone).
- You've hit a hard constraint that won't change with more probing.
- The probe has become more about your curiosity than the engagement's needs.

Probe to serve the customer's outcome, not your understanding.
