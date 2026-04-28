# Anti-patterns

Patterns that produce notes that look thorough but quietly fail. If you catch yourself doing one, stop and fix it.

## "The transcript"

**Symptom:** Wall of prose attempting to capture what everyone said in order.

**Why it fails:** Unreadable on re-entry. Buries decisions inside conversational filler. Wastes capture energy that should go to judgment.

**Fix:** Capture decisions, actions, and risks as discrete items. Discussion is for *just enough context* to make those make sense.

---

## "The vague verb"

**Symptom:** Actions like "Look at runner costs," "Sync on identity," "Discuss with the team."

**Why it fails:** Nobody can be done with a "discuss." There's no completion criterion. The action ages into the open list and dies of embarrassment.

**Fix:** Replace with outcome-shaped verbs (*Produce*, *Decide*, *Configure*, *Send*, *Validate*). If the customer can't articulate the outcome, the action isn't ready — capture as `[open]`.

---

## "The orphan owner"

**Symptom:** "Action: platform team to review the proposal."

**Why it fails:** The platform team isn't a person. There's no inbox the action lands in. Everyone assumes someone else.

**Fix:** Push for one named human. If they're not in the room, capture as `[open] who owns this?` and resolve before the recap.

---

## "The polite decision"

**Symptom:** Decision captured because heads nodded; no actual choice was articulated.

**Why it fails:** "Sounds good" is not a decision. Two weeks later, the customer remembers it differently and you have no defense.

**Fix:** Verbalize the decision yourself before capturing: *"To make sure I capture this — we're going with X because of Y, accepting Z. Yes?"* Wait for confirmation. If you don't get a clean yes, it's not a decision.

---

## "The phantom rationale"

**Symptom:** Decision captured without why.

**Why it fails:** In three weeks the customer asks "why did we choose this?" and your note says nothing. You re-litigate.

**Fix:** Every decision needs a one-line rationale **and** a one-line trade-off. If you didn't capture them, ask in the meeting (it's also a forcing function for clean thinking).

---

## "The translation"

**Symptom:** Customer says "code review board"; your notes say "approving reviewers."

**Why it fails:** The customer reads the recap and doesn't recognize their own meeting. Worse, you've now introduced terminology drift across artifacts.

**Fix:** Use customer terminology verbatim. Keep a glossary file for the engagement.

---

## "The grand summary"

**Symptom:** Three-paragraph overview at the top of the note explaining what the meeting was about, in your own words.

**Why it fails:** It's editorializing. Future readers can't tell what was *captured* vs. what's your *interpretation*. Auditors hate it.

**Fix:** Lead with bullet **objectives** stated upfront (in customer language), and let decisions/actions speak for themselves. Save your interpretation for the recap email's framing line.

---

## "The everything-confidential"

**Symptom:** Every other paragraph marked `[CONFIDENTIAL]`.

**Why it fails:** When everything is confidential, nothing is. The marking loses meaning, and reviewers stop respecting it.

**Fix:** Reserve the tag for genuinely sensitive content (named individuals, specific contract terms, security weaknesses, in-flight legal). The default for engagement notes is already non-public.

---

## "The silent edit"

**Symptom:** Customer pushes back on a decision; you quietly edit the note to match the new version.

**Why it fails:** When discovered (and it will be), it destroys trust irreversibly. Auditors interpret it as evidence of poor controls.

**Fix:** Append a `> Correction (YYYY-MM-DD): ...` block at the bottom. Notes are an immutable record; corrections are additive.

---

## "The orphan note"

**Symptom:** Meeting note sits in the meeting-notes folder, never referenced from the engagement README, never extracted into the risk register or decision log.

**Why it fails:** The engagement loses its memory. New team members can't find anything. The note might as well not exist.

**Fix:** Within 24h, propagate: actions → engagement README; risks → risk register; ADR-needed → ADR stubs; status → engagement README status section.

---

## "The lead-and-scribe"

**Symptom:** Same person driving the conversation and capturing notes for a 90-minute architecture review.

**Why it fails:** You'll either lead well and capture poorly, or capture well and lead poorly. Both at once is exhausting and produces mediocre output.

**Fix:** For any meeting with stakes, pair a **lead** with a **scribe**. The scribe owns the note, the recap, and the action follow-up.

---

## "The recording-as-fallback"

**Symptom:** "I'll just check the recording" instead of capturing in real time.

**Why it fails:** You won't go back to the recording (you don't have time). When you do, finding the right moment is slow. Customer-side attendees lose trust seeing you not engaged.

**Fix:** Capture live. Recording is **backup** for verbatim quotes and disputed moments only.

---

## "The 7-day recap"

**Symptom:** Recap sent a week after the meeting.

**Why it fails:** Memory decays. Attendees can no longer reliably correct what's misstated. The recap isn't evidence anymore — it's reconstruction.

**Fix:** 24-hour SLA, full stop. If you can't meet it, send a *placeholder* recap within 24h with decisions and actions only, and follow up with the long form.

---

## "The unflagged scope change"

**Symptom:** Customer mentions adding a new workstream; you capture it neutrally in flow; nobody notices it grew the engagement.

**Why it fails:** You discover the scope change on the budget review call. Painful conversation follows.

**Fix:** When a `[scope-change]` signal appears, **flag it explicitly** in the meeting and the recap. Make it a discrete decision, not a captured aside.

---

## "The hero scribe"

**Symptom:** One consultant takes all the notes for the entire engagement and becomes a single point of failure.

**Why it fails:** They burn out. They go on vacation. They leave the firm. The engagement loses continuity.

**Fix:** Rotate the scribe role. Hold each scribe to the same standard. Templates exist precisely so the role is interchangeable.
