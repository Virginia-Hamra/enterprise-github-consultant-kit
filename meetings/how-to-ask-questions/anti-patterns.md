# Asking Anti-patterns

Question shapes that consistently produce poor answers. Catch yourself in the act and fix.

## "The stack"

**Symptom:**
> "How is your branching set up — like, do you do trunk, or feature branches, and how do hotfixes work, and is there a release branch model?"

**Why it fails:** Customer answers the part they remember (usually the last bit, often badly). The other 2–3 questions are silently discarded.

**Fix:** One question, one question mark. Then stop. Ask the next one based on the answer.

---

## "The leading question"

**Symptom:**
> "You're using rulesets and not classic branch protection, right?"

**Why it fails:** Customer agrees to avoid looking misaligned, even when they aren't using rulesets. You record a false fact.

**Fix:** Ask neutrally:
> "Are you using rulesets, classic protection, both, or neither?"

Or, if you have a real hypothesis, **declare it** and invite correction:
> "I'd guess rulesets — am I right?"

---

## "The yes-or-no for an open question"

**Symptom:**
> "Is your CI/CD in good shape?"

**Why it fails:** Yes/no answers contain almost no information. Worst case: you record "yes" and walk away with nothing useful.

**Fix:** Reshape as open:
> "How would you rate your CI/CD setup today, and where's it weakest?"

Reserve yes/no for **confirming** specific facts you've already inferred.

---

## "The multi-stem"

**Symptom:**
> "How is access managed, and how does that compare to how it should be, and what's the gap?"

**Why it fails:** Three questions in one. The customer either picks one or produces a confused answer that addresses none of them well.

**Fix:** Sequential. Ask the diagnostic first ("How is access managed?"). After that's clear, ask the directional ("How should it be?"). The gap question writes itself.

---

## "The jargon trap"

**Symptom:**
> "Are your policies enforced via OPA, or do you use Rego rules embedded in CI for guardrail validation?"

**Why it fails:** Either the customer doesn't know what you mean and pretends to (pollutes data), or they assume you're testing them (defensive). Either way you don't learn what's actually true.

**Fix:** Plain language first; technical precision second.
> "How are policy rules enforced today — built into pipelines, a separate engine, or manually?"

If they answer with the right vocabulary, switch into it. Let them lead.

---

## "The false binary"

**Symptom:**
> "Is your CI Jenkins or GitHub Actions?"

**Why it fails:** Most enterprise environments have **three or more** CI tools, plus shadow tools, plus dead pipelines. The binary forces them to misrepresent.

**Fix:** Open it up:
> "What CI tools are in use, and roughly what percentage of pipelines run on each?"

---

## "The compound assumption"

**Symptom:**
> "How is your IdP-driven team sync working?"

**Why it fails:** You've assumed three things (they have an IdP, it drives team sync, sync is working). If any is wrong, the customer either contests the premise (awkward) or answers around it (you don't notice the assumption was wrong).

**Fix:** Decompose.
1. *"Is there an IdP of record for GitHub team membership?"*
2. *"Is it driving team sync, or just SSO?"*
3. *"Where is the sync inconsistent?"*

---

## "The closed for politeness"

**Symptom:**
> "Quick question — is there anything wrong with the current setup?"

**Why it fails:** Sounds like an open question, but the framing invites "no, it's fine." You miss everything.

**Fix:** Assume problems exist; ask which:
> "What's the worst part of the current setup?"

Or:
> "If you had to point at the most painful thing, what would it be?"

---

## "The premature solution"

**Symptom:**
> "Have you considered using Terraform for org config?"

**Why it fails:** You've recommended a solution before you understand the problem. Customer either:
- Says "we're not ready for that" (you've shown your hand prematurely),
- Or says "yes" politely and moves on (you've burned your one chance to ask).

**Fix:** Ask about the problem first.
> "How is org-level config managed today, and where does it drift?"

The Terraform conversation will write itself once the problem is on the table.

---

## "The audit question"

**Symptom:**
> "Why don't you have CODEOWNERS on these repos?"

**Why it fails:** Sounds prosecutorial. Customer becomes defensive; honest answers stop.

**Fix:** Assume good reason; ask for it.
> "What's driven the choice not to use CODEOWNERS broadly?"

This presumes a reason exists (which it almost always does — convention, tooling gap, prior bad experience), inviting the customer to share it without defensiveness.

---

## "The leading hypothesis"

**Symptom:**
> "It seems like the security team is the bottleneck here — would you agree?"

**Why it fails:** Asks the customer to agree with a critical statement about another team. They will deflect, deny, or agree to be polite — none of which is useful data.

**Fix:** Ask functionally, not personally.
> "Where do decisions on the platform tend to bottleneck?"

Let the customer name names if they choose.

---

## "The vague qualifier"

**Symptom:**
> "Are most repos protected?"
> "Generally is GHAS turned on?"
> "Mostly, do you enforce branch protection?"

**Why it fails:** "Most," "generally," "mostly" let the customer give vague answers that **feel** affirming but aren't measurable.

**Fix:** Ask for numbers or for the **exception**.
- *"What percent of active repos have branch protection?"*
- *"Where is GHAS not turned on, and why?"*

---

## "The unanswerable"

**Symptom:**
> "What's the cost of a GHAS seat in three years?"
> *(asked of a platform engineer)*

**Why it fails:** Wrong audience. Customer can't answer; either guesses (bad data) or feels stupid (relationship damage).

**Fix:** Either route to the right audience (FinOps), or reshape:
> "Within your seat, how do you forecast license cost trajectory? Who owns that conversation?"

---

## "The interrogator's pace"

**Symptom:** Question, answer, question, answer, question, answer — for 45 minutes.

**Why it fails:** Customer gets exhausted and starts giving short, surface answers. You pollute the rest of the data.

**Fix:** Mix in:
- **Repeat-backs:** *"Let me play that back…"*
- **Their turn:** *"What questions do you have for me at this point?"*
- **Pauses:** Just stop talking for 5 seconds after a meaty answer. They'll often add the most valuable bit then.

---

## "The decision before the data"

**Symptom:**
> "So we're going with EMU — sound good?"
> *(in the second meeting, with no real exploration)*

**Why it fails:** Customer either rubber-stamps (and resents it later) or pushes back hard (and you've lost trust by trying to rush).

**Fix:** Surface the criteria first.
> "Before we land a decision — what would have to be true to choose EMU? And what would have to be true to **not** choose EMU?"

Once both lists are explicit, the decision often becomes obvious to everyone in the room.

---

## "The exhaustive sweep"

**Symptom:** Going through every section of every questionnaire in every meeting.

**Why it fails:** Drains the customer's patience. Forces shallow answers across many topics rather than deep answers across the few that matter.

**Fix:** Triage in advance.
- 60% of meeting on the **2–3 highest-leverage topics.**
- 20% on **what's surprised you so far.**
- 20% on **closing questions** (what's missing, who else, what's working).

The rest of the questionnaire goes async (written intake) or to a follow-up session with a more targeted audience.

---

## "The rhetorical question"

**Symptom:**
> "I mean, you wouldn't really want to leave PATs without expiration, right?"

**Why it fails:** It's not a question; it's an opinion in question form. You've signaled the answer, learned nothing, and possibly embarrassed the customer if their reality doesn't match.

**Fix:** Either ask cleanly:
> "What's the current PAT expiration policy?"

…or state your view and own it:
> "I'd push toward enforced expiration — what's blocking that today?"

---

## "The over-specific"

**Symptom:**
> "Are you using `actions/checkout@v4` with `fetch-depth: 0` for your CodeQL workflows on macOS-latest runners?"

**Why it fails:** Reduces to a flat "I don't know." You learn nothing; the customer feels micro-checked.

**Fix:** Climb a level.
> "How are you running CodeQL — default setup or advanced? If advanced, what does the workflow look like?"

The specific details, if relevant, surface naturally from there.

---

## "The escape hatch"

**Symptom:** Phrasing every question with so much hedging that the customer gets a free pass:
> "If, you know, in some cases, hypothetically, you might consider — and feel free to skip this if it's not relevant — but how is access managed?"

**Why it fails:** Customer takes the escape ("not really relevant for us, let's move on"). You miss the answer.

**Fix:** Be respectfully direct.
> "How is access managed today?"

You can be polite **and** clear. Hedging isn't politeness; it's discomfort with the question.

---

## The meta-pattern

Most asking anti-patterns share one root cause: **the consultant is more focused on themselves (looking smart, looking polite, not looking ignorant) than on the customer's reality.**

Catch yourself there. Re-center on what the customer needs you to surface. Ask accordingly.
