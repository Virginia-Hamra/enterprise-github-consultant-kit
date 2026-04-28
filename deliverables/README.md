# Deliverables — Advisory & Consultancy

> The deliverable is the engagement. Everything else is process. This folder defines the canonical deliverable types a senior GitHub Enterprise consultant produces, when each applies, what "done" looks like, and the templates to use.

---

## The five core deliverable types

| Icon | Type | Use when… | Output format |
|------|------|-----------|---------------|
| 📄 | **Written recommendation** | The customer needs a defensible answer to a specific question | Markdown / PDF memo |
| 🌳 | **Decision framework / tree** | The answer is "it depends" and the customer must choose | Matrix or flowchart + criteria |
| 📅 | **Meeting (agenda → notes → actions)** | Alignment, discovery, or decisions need multiple stakeholders in a room | Agenda doc → notes doc → action register |
| 🔍 | **Technical assessment** | Evidence-based diagnosis of a current state | Findings report with risk-ranked observations |
| 📐 | **Spec / architecture diagram** | A future-state design needs to be built or reviewed | Diagram + accompanying spec |

Every engagement output should map to one of the five. If it doesn't fit, question whether it should exist.

---

## 📄 Written Recommendation

A short, opinionated memo that answers a specific question with a defensible position.

**When to use**

- Customer asks "should we do X?" and expects a take, not options.
- A decision needs to be recorded for governance / audit.
- An async stakeholder needs to be brought up to speed without a meeting.

**Structure (1–4 pages)**

1. **Question** — the exact decision being made, in one sentence.
2. **Recommendation** — the answer, in one sentence, up front.
3. **Context** — the constraints that drove it (max 5 bullets).
4. **Rationale** — why this and not the alternatives.
5. **Risks & mitigations** — top 3.
6. **Next steps & owners** — who does what by when.
7. **Sources** — links to docs, ADRs, prior decisions.

**Quality bar**

- Recommendation in the first 100 words.
- No section longer than the rationale.
- Every claim is sourced or marked as judgment.
- Reviewable in under 10 minutes.

**Anti-patterns**

- Restating the brief instead of answering.
- Listing options without picking one.
- Hiding the recommendation in the conclusion.

---

## 🌳 Decision Framework / Decision Tree

A reusable artifact that lets the customer (or future-you) make the same class of decision repeatedly without re-doing the analysis.

**When to use**

- The decision recurs (per-repo, per-team, per-app).
- Tradeoffs are real and context-dependent.
- The customer needs to operate the framework after you leave.

**Forms**

- **Matrix** — criteria vs options, scored or qualitative. Best for ≤ 6 options across ≤ 8 criteria. (See [`../github-enterprise-documentation/16-risks-and-tradeoffs/README.md`](../github-enterprise-documentation/16-risks-and-tradeoffs/README.md).)
- **Decision tree / flowchart** — sequential yes/no questions leading to an outcome. Best when one or two factors dominate.
- **Scoring rubric** — weighted criteria with a numeric threshold. Best when decisions must be auditable.

**Required elements**

- Inputs (what you need to know to use it).
- Criteria — with measurable definitions ("complexity" must be quantifiable).
- Outcomes — specific, named.
- A worked example.
- An owner (who maintains it).

**Quality bar**

- A new platform engineer can apply it without you in the room.
- The output is reproducible — two people get the same answer.
- It has been validated against ≥ 3 historical decisions.

**Anti-patterns**

- Criteria that are unmeasurable ("strategic fit").
- Outcomes that say "consider X" — pick.
- Frameworks that require the consultant to interpret them.

---

## 📅 Meeting (agenda → notes → actions)

A meeting is a deliverable when it produces alignment, discovery, or a decision that is captured in writing. If nothing is captured, it wasn't a meeting — it was a conversation.

**Three artifacts, one engagement**

1. **Agenda** (sent ≥ 24h before)
   - Purpose (one sentence).
   - Required attendees with roles (RACI).
   - Pre-reads with time required.
   - Topics with timeboxes and desired outcome (decision / input / FYI).
   - Decision rights for each topic.

2. **Notes** (within 24h after)
   - Decisions made (with owner and date).
   - Open questions (with owner and target date).
   - Disagreements parked.
   - Verbatim only when a position must be quoted later.

3. **Actions** (in the same doc or in a tracker)
   - One owner per action. "The team" is not an owner.
   - A date. "Soon" is not a date.
   - Tracked to closure in the next session.

**Meeting types and patterns**

| Meeting | Cadence | Goal | Anti-pattern |
|---------|---------|------|--------------|
| Discovery workshop | Once, early | Build shared current-state | Solutioning |
| Decision review | As needed | Pick from prepared options | Re-opening prior decisions |
| Steering committee | Monthly / quarterly | Sponsor alignment, risk escalation | Status update only |
| Working session | Weekly | Build artifact together | Status with no artifact |
| Show-and-tell / demo | Milestone | Validate against intent | One-way presentation |
| Retrospective | End of phase | Capture lessons, adjust | Blame |

**Quality bar**

- Decisions are written and circulated within 24 hours.
- Action items have owner + date — always.
- Pre-reads exist for any decision meeting.

**Anti-patterns**

- Meetings without agendas.
- Notes that record discussion but not decisions.
- Recurring meetings with no clear purpose ("standing sync").

---

## 🔍 Technical Assessment

An evidence-based snapshot of the current state, with risk-ranked findings and recommendations.

**When to use**

- Pre-engagement diagnosis (security posture, migration readiness, Copilot readiness, FinOps review).
- Validation that a target state has been reached.
- Independent review of a third-party design.

**Method**

1. **Scope** — what's in / out, on paper, signed.
2. **Evidence collection** — interviews, config exports, audit logs, tooling output (REST / GraphQL pulls, scanner outputs, runner data).
3. **Analysis against a rubric** — a published standard (CIS, OWASP, GitHub Well-Architected) or internal baseline.
4. **Findings** — each with: observation, evidence, risk, recommendation, effort.
5. **Risk ranking** — Critical / High / Medium / Low against agreed criteria.
6. **Roadmap** — sequenced remediation with owners.

**Structure**

1. Executive summary (1 page, scannable).
2. Scope & method.
3. Findings table (sortable, risk-ranked).
4. Findings detail (one per page max).
5. Roadmap.
6. Appendix: raw evidence, configs, queries.

**Quality bar**

- Every finding has evidence attached.
- Risk ranking uses a documented rubric, not gut feel.
- Recommendations are actionable — name the config setting, the doc page, the owner.
- The customer can hand the report to a different consultant who can execute it.

**Anti-patterns**

- Findings that are opinions without evidence.
- "Improve security posture" instead of "Enable required PR reviews on `main` in 14 production repos via org ruleset".
- Risk ranks that don't differentiate (everything is High).

---

## 📐 Spec / Architecture Diagram

A future-state design in enough detail that someone else can build it.

**When to use**

- Greenfield platform / org design.
- Major refactor (CI modernization, EMU adoption, runner topology change).
- Integration architecture (GitHub ↔ IdP, ITSM, observability).

**Required elements**

1. **Context diagram** — the system in its environment (C4 level 1).
2. **Container / component diagram** — major moving parts and their relationships (C4 level 2 / 3).
3. **Sequence diagrams** for the 3–5 most important flows (auth, deploy, incident).
4. **Spec document** covering:
   - Goals & non-goals.
   - Constraints (regulatory, technical, organizational).
   - Design decisions with ADR links.
   - Interfaces (APIs, events, identities).
   - Data flows + classification.
   - Security model (authN, authZ, secrets, audit).
   - Operational model (HA, DR, monitoring, on-call).
   - Cost model.
   - Rollout plan + rollback.
   - Open questions.

**Tooling**

- Diagrams: **Mermaid** (text-based, lives in repo, diffable) for flow / sequence; **draw.io / excalidraw** for richer architecture.
- Spec: Markdown in a versioned repo, reviewed via PR.
- ADRs: one per significant decision, linked from the spec.

**Quality bar**

- A capable engineer who wasn't in the design sessions can build from it.
- Every component has an owner.
- Non-goals are stated — explicitly out of scope.
- Diagrams and prose agree (no orphan components in either).

**Anti-patterns**

- Diagram with no spec ("draw it on a slide").
- Spec with no diagram (walls of prose).
- Implementation details in the context diagram, or vice versa.
- Decisions baked in without ADRs.

---

## Choosing the right deliverable

| If the customer needs to… | Use |
|---------------------------|-----|
| Decide one specific thing now | 📄 Written recommendation |
| Decide the same class of thing repeatedly | 🌳 Decision framework |
| Align stakeholders / surface unknowns | 📅 Meeting |
| Understand where they actually stand | 🔍 Technical assessment |
| Build or validate a future state | 📐 Spec / diagram |

Most engagements combine 2–4 of these. A typical migration engagement, for example:

- 🔍 Migration-readiness assessment →
- 📅 Findings review meeting →
- 📐 Target architecture spec →
- 🌳 Wave-planning decision framework →
- 📄 Cutover go / no-go recommendation.

---

## Cross-cutting standards

- **Source everything.** Every claim links to a `docs.github.com` page, an ADR, or attached evidence.
- **Date everything.** GitHub features change quickly; deliverables go stale.
- **Name an owner.** No artifact ships without a named maintainer.
- **Version in Git.** Deliverables live in a repo, not in email or Slack.
- **One decision per artifact.** Bundling decisions hides them.
- **Plain language.** A senior leader reads it without a glossary; an engineer can act on it without translation.

---

## Templates

Templates for each type live (or will live) under:

- `deliverables/templates/written-recommendation.md`
- `deliverables/templates/decision-framework.md`
- `deliverables/templates/meeting-pack.md`
- `deliverables/templates/technical-assessment.md`
- `deliverables/templates/architecture-spec.md`

Companion knowledge:

- [`../github-enterprise-documentation/`](../github-enterprise-documentation/README.md) — domain knowledge backing every deliverable.
- [`../github-enterprise-documentation/16-risks-and-tradeoffs/`](../github-enterprise-documentation/16-risks-and-tradeoffs/README.md) — worked decision matrices.
- [`../solution-architecture-knowledge/`](../solution-architecture-knowledge/) — broader architecture patterns.
