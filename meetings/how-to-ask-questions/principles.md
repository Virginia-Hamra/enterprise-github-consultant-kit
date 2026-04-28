# Principles of Asking

## What questions are *for*

A consultant's questions serve **four purposes**, often simultaneously:

1. **Discover** — surface facts, constraints, history you don't know.
2. **Validate** — confirm your hypothesis is correct (or kill it cheaply).
3. **Align** — get the room to share an explicit understanding.
4. **Catalyze** — make the customer think differently about a problem.

Most junior consultants only ask discovery questions. Senior consultants weave all four. Knowing which mode you're in shapes the question.

## The bias against asking

Customers expect consultants to *know things*. Many consultants over-correct by avoiding questions that might "look ignorant." This is exactly backwards:

- The customer hired you because **you can ask the questions they can't ask themselves**.
- Confident "ignorance" — "Help me understand…" — is high-trust, not low-trust.
- The expensive failure mode is *assuming* and being wrong, not *asking* and looking new.

If you're not asking the question because you fear looking unprepared, ask it anyway.

## One question, one question mark

The single most common asking mistake is **stacking**:

> "So how is your branching set up — like, do you do trunk, or feature branches, or something else, and how do hotfixes work, and is there a release branch model?"

The customer answers the part they remember (usually the last bit, often badly), and three pieces of information are lost.

Rule: **one question. Then stop. Then wait.**

If you have three things to ask, ask one, listen, and then ask the next based on the answer.

## Silence is a question

After you ask, **stop talking**. Don't fill the gap.

Silence:
- Lets the customer think (most people need 2–4 seconds).
- Signals you actually want a real answer, not a polite one.
- Often produces a *second*, more honest answer after the first surface answer.

Counting "one Mississippi, two Mississippi" before re-prompting is a useful habit until silence becomes natural.

## Listen for what's not said

A complete picture comes from three signals:

| Signal | What it tells you |
|---|---|
| What they say | The official answer |
| How they say it | Confidence, hesitation, deflection |
| What they don't say | The political / unowned / painful parts |

Examples:
- A platform lead who answers "we do CI/CD" without naming a tool → likely fragmented, no standard.
- A security lead who pivots from "audit posture" to "we have GHAS" → audit prep is weak.
- Repeated "the team handles that" with no name attached → no owner exists.

Capture these as `[obs]` in your notes (see [`../how-to-take-notes/capture-techniques.md`](../how-to-take-notes/capture-techniques.md)).

## Confirm before you act

Before drawing a conclusion or making a recommendation, **repeat the answer back**:

> "Let me play that back — you have ~3,200 repos in BBS, of which ~800 are migration candidates, and the rest are archive. The 800 split roughly 60/40 between active and dormant. Is that right?"

This:
- Catches mishearings before they become misrecommendations.
- Demonstrates active listening, which builds trust.
- Forces the customer to confirm or correct on the record (great for notes).

## Hypothesis-driven asking

Once you've done some discovery, switch from open to **hypothesis-driven** questions:

- Open: "How is access managed?"
- Hypothesis-driven: "It looks like access is mostly managed manually with some SCIM in Okta. Is that fair?"

Hypothesis-driven questions are **faster** (closed-form answer), **higher signal** (you're testing something specific), and **demonstrate competence** (you've been listening).

But: if your hypothesis is wrong, you must be *visibly* willing to abandon it. Anchoring on a wrong hypothesis is worse than asking open.

## Cheap is better than thorough

A good question is cheap to answer. If a question requires the customer to:

- Pull data they don't have at hand,
- Speak for someone not in the room,
- Or commit to something without context,

…then it's the **wrong** question, asked at the wrong time. Either reframe ("In your view, roughly…?"), or capture as `[open]` and route to the right person/forum.

## The customer is also asking

Every customer interview is bidirectional. They're evaluating:

- Are you competent? (Do you ask sharp questions?)
- Are you safe? (Will you embarrass them with a question in front of leadership?)
- Are you listening? (Did you remember what I said 30 minutes ago?)
- Are you advocating for them? (Are you asking *for* them, or *at* them?)

Your questions are how they form those judgments. Ask accordingly.

## When in doubt, ask the dumb question

The "dumb question" — the one you're sure everyone else knows the answer to — is often the question that exposes the unstated assumption that's been quietly distorting the conversation for the whole meeting.

> "Sorry — quick check — when you say 'production', do you mean the customer-facing AKS cluster, or the regulated environment?"

Lifesaving. Ask it.
