# Question Types

The toolkit. Knowing which type to use when is the core skill.

## Open vs. closed

| Type | Shape | Use for | Example |
|---|---|---|---|
| **Open** | "How / What / Why / Tell me about…" | Discovery, exploring the unknown | *"How does access provisioning work today?"* |
| **Closed** | "Yes/no, single fact, choose one" | Confirming, narrowing, deciding | *"Is SSO enforced at the enterprise level?"* |

Default to **open** in early discovery. Switch to **closed** as you build hypotheses and need to confirm.

A common failure: asking closed questions during discovery (gets you a thin set of facts but misses the actual landscape).

## The funnel

Move from broad to narrow within a topic:

1. **Broad open**: *"Tell me about how identity is managed today."*
2. **Narrowing open**: *"Where do you see the most friction in onboarding?"*
3. **Hypothesis-test**: *"It sounds like SCIM is in place but inconsistently scoped — is that fair?"*
4. **Closed confirmation**: *"And the IdP of record is Entra, not Okta?"*

The funnel respects the customer's expertise (they get to frame first) and ends with you holding precise facts.

## The reverse funnel (rarely)

Used when the customer is anxious or unsure where to start. Begin with a **closed** orienting question to give them traction, then open up:

1. *"Quick orienting question — are you using GHEC, GHES, or both?"* (closed, easy)
2. *"Got it. Walk me through how teams are organized today."* (open, expansive)

## Layering: 5 whys / laddering up

When you hit a "because that's how we do it," **layer**:

> Customer: "We restrict repo creation to admins."
> You: "What drove that?"
> Customer: "We had a sprawl issue."
> You: "What kind of sprawl?"
> Customer: "Repos created with no owner, then orphaned."
> You: "Got it — so the underlying problem was ownership, not repo count?"
> Customer: "Yeah, exactly."

Now you know the **real** requirement is *ownership enforcement*, not *creation restriction*. The recommendation can solve the actual problem (CODEOWNERS, repo metadata) instead of the surface symptom (gatekept creation).

3–5 layers usually surfaces the underlying constraint. Beyond 5 you're being annoying.

## Hypothesis-driven asking

State your current model, invite correction:

> "Here's what I think I'm hearing: identity is well-controlled at the IdP, but team-to-repo mapping is manual and drifts. The biggest pain is during reorgs. Am I close?"

Three outcomes:
- **Yes** → you're aligned, advance.
- **Mostly, but…** → the "but" is the gold; capture it precisely.
- **No** → reset. *"Help me understand where I went wrong?"*

This style is fast, demonstrates listening, and forces the customer to be explicit about disagreement.

## Diagnostic vs. directional

A diagnostic question asks **what is** — a directional question asks **what should be**. Don't mix them in early discovery; you'll get a cocktail of facts and aspirations and won't be able to tell them apart.

| Diagnostic | Directional |
|---|---|
| "How is GHAS used today?" | "Where do you want GHAS to land in 12 months?" |
| "Who has org-owner access?" | "Who *should* have org-owner access?" |
| "What's your branch protection setup?" | "What ruleset would you ideally enforce?" |

Start diagnostic. Once current state is clear, switch to directional.

## The contrast question

Useful for surfacing implicit choices:

> "If you had decided **not** to enforce SSO, what would have to be true?"

Or:

> "What would have to be different for self-hosted runners to be the right answer?"

These reveal:
- Hidden constraints (legal, regulatory, political)
- The customer's actual decision criteria
- The strength of conviction behind a stated preference

## The pre-mortem question

Sometimes called the "newspaper question":

> "Imagine it's a year from now and this initiative has failed. The post-mortem doc is open on your screen. What's the first sentence?"

This pulls out:
- Risks the customer hasn't articulated
- Political dynamics they're worried about
- Capability gaps they're hiding
- Skepticism about the plan that hasn't been voiced

## The reframe question

Use when the customer is stuck in a too-narrow framing:

- *"Is this fundamentally a tooling problem, or a process problem?"*
- *"Is the goal to reduce risk, or to demonstrate that risk is reduced?"*
- *"Is the constraint actually budget, or is it stakeholder capacity?"*

Good reframes make customers say "huh." That's the moment of value.

## The numbers question

Quantitative questions cut through hand-waving:

- "How many?" / "What percent?" / "How often?" / "How long?" / "Since when?"
- *"What percent of repos have CODEOWNERS today?"*
- *"How many seats of GHAS are licensed vs. active?"*
- *"How long does a typical PR take from open to merge?"*

If the customer can't answer, that's data: **what they don't measure is what they don't manage**. Capture the gap.

## The "show me" question

Better than asking *about* something — ask to **see** it:

- *"Can you screen-share the org settings page?"*
- *"Walk me through a typical PR — pick a recent one."*
- *"Show me the runbook you'd use if a secret leaked."*

Live walkthroughs surface the gap between *intent* and *reality* faster than any verbal description.

## The "what's missing" question

End every interview block with:

> "What haven't I asked about that I should have?"

Or:

> "If you were running this engagement, what would you want to know that we haven't covered?"

This:
- Surfaces topics outside your script.
- Gives the customer agency.
- Often produces the meeting's most valuable insight.

## When silence is the question

The most underused tool. After a fragile or politically charged answer, do not fill the space. Wait.

> Customer: "Yeah, the security team and the platform team have… history."
> You: *(does not respond, waits 4 seconds)*
> Customer: "Okay, look, here's what really happened…"

The silence is the question. Use sparingly; it's powerful.
