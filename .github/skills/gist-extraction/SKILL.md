# Gist Extraction Skill

## Purpose
Compress dense, complex material (long docs, RFCs, policies, source code, audit reports, vendor whitepapers, transcripts, customer artifacts) into the minimum viable understanding — fast. Optimized for **decision support**, not summarization-for-summarization's-sake. The output exists to let a senior consultant or stakeholder act, brief, or decide within minutes.

## Role Context
Use this skill when: you need to read a 60-page customer architecture doc before a 30-min call; a regulator publishes a new circular; a vendor sends a 40-page whitepaper; a customer dumps a year of meeting notes; you're catching up on a long-running issue thread or RFC; you must brief a leader who has 5 minutes.

Pair with [`customer-advisory/SKILL.md`](../customer-advisory/SKILL.md) for downstream deliverable production.

---

## Operating Principles

1. **Decision-first.** Every gist is built for someone who must decide, brief, or act — not for someone curious.
2. **Depth on demand.** Layered output: 30-second skim → 3-minute read → 10-minute deep-dive. The reader picks the depth.
3. **Quote, don't paraphrase, when wording matters.** Regulatory text, contracts, and policy thresholds are extracted verbatim with location.
4. **Surface the unsaid.** What the source omits is often more important than what it says. Flag gaps explicitly.
5. **Source the claim.** Every extracted point is traceable to a section, page, line, or URL.
6. **Date and version.** Capture the version / date of the source; topics change.

---

## Capabilities

### 1. Layered output by default

Produce three nested layers from the same source:

- **TL;DR (≤ 50 words / 30 seconds)** — the one thing the reader must remember.
- **Brief (≤ 300 words / 3 minutes)** — what it says, who it affects, what changes, what to do.
- **Deep notes (10 minutes)** — structured extraction with quotes, edges, open questions.

The reader chooses the depth; you always produce all three.

### 2. The 7-question scan

For any dense source, answer in order:

1. **What is it?** (type, scope, status — draft / final / GA / preview)
2. **Why does it exist?** (driver, problem it solves)
3. **Who is bound by it?** (audience, jurisdiction, scope of application)
4. **What does it require / propose / change?** (the actual asks, quoted if normative)
5. **What are the thresholds, dates, numbers?** (verbatim — these are the most-misread parts)
6. **What's new vs prior?** (delta — only meaningful if there *was* a prior)
7. **What does it omit / leave ambiguous?** (the gap list)

Skip a question only with a one-line note ("not applicable: source has no prior version").

### 3. Structural patterns by source type

Different inputs need different extraction shapes.

| Source type | Extraction shape |
|-------------|------------------|
| Regulation / policy | Scope · obligations (verbatim) · thresholds · effective dates · penalties · exemptions · open interpretation |
| Architecture doc | Goals · non-goals · components · data flows · trust boundaries · constraints · open questions |
| Vendor whitepaper | Claim · evidence offered · benchmark conditions · what's omitted · cost / lock-in · differentiation vs alternatives |
| Long discussion / thread | Decisions made · positions held · unresolved · who needs what next |
| Source code / repo | Entry points · domain model · external dependencies · tests as spec · what isn't tested |
| Meeting transcripts | Decisions · disagreements · action items (owner + date) · parking lot |
| Audit / assessment | Top findings by risk · evidence · root cause clusters · remediation themes |
| Long incident thread | Trigger · timeline · contributing factors · current state · open follow-ups |

### 4. Pattern recognition

While reading, actively extract:

- **Numbers** (thresholds, dates, percentages, counts) — quoted with units.
- **Modal verbs** in normative text (must / shall / should / may) — preserve exactly.
- **Defined terms** — when a doc capitalises a term, capture its definition.
- **Cross-references** — what does this depend on or contradict?
- **Hidden assumptions** — what must be true for the claims to hold?
- **Bait and switch** — common pattern where the headline differs from the fine print.

### 5. Gap & edge surfacing

Always produce a "**What this doesn't tell you**" section:

- Missing definitions for terms used.
- Unstated assumptions.
- Edge cases the source side-steps.
- Numbers without units, dates without timezones, percentages without denominators.
- Claims without evidence.
- "Up to" / "as much as" / "in some cases" — flag every weasel word.

### 6. Comparison gist (optional layer)

When the source is one of N options the customer is choosing between, add a comparison row:

- **Differentiator** — the one thing this source does that others don't (or vice versa).
- **Where it fits** — the customer-shaped slot it best fills.
- **Where it doesn't** — disqualifying conditions.

### 7. Quote management

- Use **block quotes** for normative text (regulations, contracts, policy).
- Cite location: `§4.2.1`, `p. 17`, `lines 412–418`, `commit a1b2c3d:src/auth.py:45`, or URL with anchor.
- Never paraphrase a quoted obligation — re-state in plain language *alongside* the quote.

---

## Workflow

1. **Identify intent.** Why is this gist needed? (Decision / brief / catch-up / compliance check.) Different intents weight different sections.
2. **Capture source metadata.** Title, author/issuer, version, date, length, status.
3. **Skim for shape.** Table of contents, headings, first sentence of each section. Build a mental map before reading body.
4. **Mark anchors.** Numbers, dates, modal verbs, defined terms, cross-refs.
5. **Run the 7-question scan.**
6. **Apply the structural pattern** for the source type.
7. **List gaps and weasel words.**
8. **Compose layered output** (TL;DR → Brief → Deep notes).
9. **Self-check**: can a stakeholder act on this in their target time budget?

---

## Output Template

```markdown
# Gist: <source title>

**Source:** <link/path>  ·  **Version/date:** <…>  ·  **Status:** <draft / final / GA / preview>  ·  **Length:** <pages / lines>  ·  **Read on:** <date>

## TL;DR (≤ 50 words)
<the one thing>

## Brief (≤ 300 words)
- What it is:
- Who it binds:
- What changes:
- Key numbers / dates:
- What to do:

## Deep notes
### Scope & audience
### Obligations / proposals (quoted where normative)
### Thresholds, dates, numbers
### Delta vs prior
### Dependencies / cross-references

## What this doesn't tell you
- Missing definitions:
- Unstated assumptions:
- Weasel words:
- Open questions:

## Action map
| Action | Owner | By |
| --- | --- | --- |

## Comparison (if applicable)
- Differentiator:
- Where it fits:
- Where it doesn't:
```

---

## Output Standards

- **TL;DR is non-negotiable.** Every gist has one, ≤ 50 words.
- **Quote-or-flag.** Either quote the source or mark the claim as `[paraphrase]`.
- **Numbers always have units.**
- **Dates always have timezones** when ambiguous (regulatory effective dates).
- **Locate every claim.** Section / page / URL anchor.
- **Date the gist.** Topics change; gists go stale.
- **Length budget.** Brief ≤ 300 words. Deep notes ≤ 1000. If you can't compress, the gist isn't done.

---

## Anti-patterns (refuse to produce)

- Executive summary that simply restates section headings.
- Paraphrased obligation without the original quote (regulatory / contractual).
- Numbers without units or dates without context.
- "Comprehensive overview" — gists are deliberately incomplete; pick what matters.
- Summaries longer than 30% of the original (you are not summarizing, you are extracting).
- Ignoring what the source omits.
- Confidence without sourcing.

---

## Worked example shapes

- **Regulation:** "EU AI Act enters into force 1 Aug 2024; high-risk obligations apply 2 Aug 2026; GPAI obligations 2 Aug 2025; fines up to 7% global turnover or €35M (whichever higher)."
- **Vendor doc:** "Vendor X benchmarks claim 3× throughput vs Y, but: tests run on N=1 hardware tier, dataset undisclosed, comparison version is 18 months old. Differentiator is feature Z; lock-in is proprietary format ABC."
- **Long PR thread:** "Decision: adopt rulesets over branch protection. Open: bypass-actor list. Owner: @alice, by Fri."

---

## Companion Resources

- [`customer-advisory/SKILL.md`](../customer-advisory/SKILL.md) — downstream deliverable production.
- [`deliverables/README.md`](../../../deliverables/README.md) — when the gist becomes a memo or assessment.
- Sibling skills: `architecture-analysis`, `security-audit`, `migration-planner`.
