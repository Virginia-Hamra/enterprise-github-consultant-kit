# Difficult Conversations

How to ask questions when the answers are politically loaded, when stakeholders disagree in front of you, when you need to challenge a customer's stated belief, or when you're delivering uncomfortable findings.

## The disposition

Three things must be true for a difficult conversation to land well:

1. **You are visibly on their side.** Your goal is the engagement's success, not winning the conversation.
2. **You are willing to be wrong.** If you only ask leading questions to confirm your view, you'll lose the room.
3. **You separate the person from the position.** Critique the architecture, not the architect.

Without these, even the best questions feel like attacks.

## Challenging a stated belief

When a customer says something you think is wrong (e.g. "Self-hosted runners are cheaper than GitHub-hosted"), don't lecture. Question into it.

### Patterns

**Curious, not corrective:**
> *"Help me understand the cost model behind that — how did you get to that number?"*

**Surface the assumption:**
> *"What's that based on — current self-hosted utilization, or list-price comparison?"*

**Offer an alternate frame:**
> *"What about the operational cost of patching, scaling, and on-call? Is that included?"*

**Pose the test:**
> *"If we ran a 30-day side-by-side, what would you want to measure to feel confident either way?"*

The goal is for the customer to **change their own mind** with new information, not for you to convict them of being wrong.

## Surfacing disagreement in the room

When two stakeholders disagree (visibly or quietly), you have two bad options:

1. Ignore it and proceed (decision will be re-litigated later).
2. Pick a side (you've now made an enemy).

…and one good option: **make the disagreement explicit, neutrally.**

### Patterns

> *"I'm hearing two different framings — Jane is leaning toward EMU, Mike is leaning toward standard accounts with strict policy. Can we spend five minutes on what's driving the difference?"*

> *"Before we move on — does anyone in the room hold a different view? Now's the cheap time to surface it."*

> *"What would have to be true for option B to be right? And for option A?"*

This:
- Validates both views as legitimate.
- Forces the constraint behind each view to surface.
- Puts the decision on a foundation that survives later challenge.

## When the customer is anxious about being judged

Some customers — especially when their environment is in poor shape — get defensive. They'll:

- Justify decisions you didn't ask about.
- Pre-empt criticism with self-deprecation ("yeah, I know it's bad…").
- Withhold information they think will look incriminating.

### Patterns

**Lead with strengths:**
> *"Before we dig in — what's working well that you'd be reluctant to change?"*

**Normalize the gap:**
> *"It's pretty common for the original design to drift — most environments we see have something like this. Walk me through how it ended up here."*

**Make it not their fault:**
> *"This kind of legacy is almost always a constraint or a deadline that was real at the time. What was the original driver?"*

You're not lying — most environments **do** have legacy. But framing it this way moves them from defending to explaining, which is what you actually need.

## Handling pushback on your recommendation

The customer says "I disagree" or "that won't work here." Three patterns:

### 1. Take it as a gift

> *"That's useful — help me understand what makes it not workable here?"*

You almost certainly missed a constraint. Find it. Adjust.

### 2. Distinguish capability from will

> *"Is the issue that it's technically not feasible, or that the org won't accept it?"*

These are completely different problems. The first you redesign for; the second you don't pretend doesn't exist.

### 3. Offer to be wrong cheaply

> *"Want to test the assumption with a small pilot before we commit?"*

Reduces stakes, surfaces real-world facts, and demonstrates you're not married to your recommendation.

## Delivering uncomfortable findings

You've discovered something the customer won't want to hear (e.g. their security posture is materially weaker than they believe, a key person is the bottleneck, a prior decision is going to need to be reversed).

### Patterns

**Pre-frame:**
> *"I want to share something uncomfortable. My job is to surface this, not to assign blame. Are you good if I'm direct?"*

(They'll say yes. Now you have permission.)

**Lead with the fact, not the interpretation:**
> *"Audit log retention is set to 90 days. Your SOC 2 requirement is 12 months."*
> Then pause.

**Frame the consequence in their vocabulary:**
> *"That likely means a finding in your next audit unless we close it before."*

**Move to action quickly:**
> *"Here's what closing it looks like. About a day's work. Want to fix this week?"*

Don't dwell. Customers respect crisp, action-oriented delivery. Don't soften so much that the seriousness is lost.

## Asking about people

Sometimes you need to surface that a specific person is the issue (bottleneck, knowledge silo, blocker, retiring next quarter). This is high-risk and must be done carefully.

### Patterns

**Functional, not personal:**
> *"Where do decisions on the platform tend to bottleneck?"*
> (Customer answers with a name.)
> *"Got it. What would have to be true for that not to be a single point of failure?"*

**The bus factor frame:**
> *"If <person> were unavailable for two weeks, what wouldn't get done?"*

**Never** ask "is X the problem?" or anything that sounds like building a case against an individual. Stay functional.

When the customer themselves names someone as the issue, **capture the functional concern**, not the personal one (`[risk] R-NNN: knowledge concentration on platform decisions; mitigations: pair / docs / cross-train`).

## Handling silence after a difficult question

After a hard question, **wait**. Customers often need time to formulate an honest answer. Resist the urge to:

- Rephrase ("…or, I mean, like…")
- Soften ("It's fine if you don't know")
- Fill with your own theory ("I imagine it's because…")

A 5–10 second silence after a difficult question is normal and productive.

## Recovering from a question that landed badly

If a question lands wrong (visible defensiveness, sudden shutdown, escalation):

### Pattern

1. **Acknowledge:** *"That came out sharper than I meant — let me re-ask."*
2. **Reframe:** Same question, gentler shape.
3. **Move on:** Don't dwell on the recovery.

Trying to win the original framing after it failed is rarely worth it. Adjust and continue.

## When to escalate vs. stay in the room

Some difficult conversations belong with **executive sponsors**, not with the team you're talking to. Signs:

- The disagreement is about scope, budget, or executive direction.
- The risk has cross-org implications the team can't decide on.
- The team is being asked to disclose things their leadership should hear in the room.

Pattern:
> *"This feels like something the steering committee should weigh in on, rather than us deciding here. Want me to bring it forward at next week's meeting?"*

This protects the team you're talking to (no awkward decisions made under pressure) and puts the topic at the right level.

## Capturing difficult conversations

In notes (see [`../how-to-take-notes/`](../how-to-take-notes/README.md)):

- Mark `[CONFIDENTIAL]` for content involving named individuals or political tension.
- Capture the **functional concern**, not the personal narrative.
- Note **what was decided** (or explicitly deferred) so it doesn't get re-litigated.
- For escalations to steering: stage the message you'll bring forward, not the raw transcript.

## The principle behind all of these

You are not the customer's adversary. You are not their judge. You are not their savior. You are a competent professional, hired to surface reality and help them act on it.

Difficult conversations land well when the customer experiences you as **someone clearly working alongside them, who isn't afraid to say what's true.**
