# Copilot Knowledge Bases

> Indexed, searchable corpora that ground Copilot Chat in your internal documentation. **Copilot Enterprise** only.

---

## 1. What a Knowledge Base Is

A Knowledge Base (KB) is a set of one or more **GitHub repositories** indexed for retrieval-augmented generation (RAG). When Chat queries a KB, it:

1. Retrieves relevant chunks via semantic + keyword search
2. Cites them inline with file links
3. Constrains its answer to grounded content (when configured)

KBs are the **source of truth** layer for Copilot. Spaces are working contexts; KBs are the encyclopedia.

---

## 2. Good Candidates for a KB

| Content | Why |
|---------|-----|
| Internal architecture docs (Markdown) | High-signal, authoritative |
| ADRs / RFCs | Decisions that shape "how we build" |
| Runbooks & SOPs | Operations grounding |
| API references (OpenAPI, generated MD) | Reduces hallucination on internal APIs |
| Onboarding guides | Faster ramp |
| Security policies & standards | Compliance grounding |

### Avoid

- Source code repos (use Spaces / `@workspace` instead)
- Auto-generated noisy outputs (build logs, screenshots dumps)
- Files larger than the chunker's window without restructuring
- Out-of-date docs — rot poisons the well

---

## 3. Structuring Repos for Retrieval

```text
docs-knowledge/
├── README.md              # KB index & ownership
├── architecture/
│   ├── 01-overview.md
│   └── adr/
│       └── 0001-event-bus.md
├── runbooks/
│   └── incident-payments.md
├── policies/
│   └── data-classification.md
└── glossary.md
```

Best practices:

- One topic per file, ≤ 1500 lines
- Descriptive H1; H2/H3 used as retrieval anchors
- Front-matter for metadata (`owner`, `last-reviewed`, `audience`)
- Link liberally between docs — improves context expansion
- Include examples and counter-examples

---

## 4. Configuration

`Org → Knowledge bases → New`:

- Name & description (used in `@kb-name` mentions in chat)
- Repos to include (one or many)
- Optional: scope to teams

Users invoke via:

```
@payments-kb how does the outbox work?
```

---

## 5. Lifecycle & Governance

| Phase | Owner | Action |
|-------|-------|--------|
| Create | Domain lead | Curate seed repo, write index |
| Maintain | Doc CODEOWNERS | PR-based updates, link checking |
| Review | Quarterly | Stale-doc audit, retirement |
| Retire | Domain lead | Archive repo, remove KB |

Add a CI check that fails when:

- `last-reviewed` front-matter is older than 6 months
- Internal links break
- Files exceed size threshold

---

## 6. Measurement

Use the [Copilot Metrics API](./metrics-and-measurement.md):

- Chats grounded in this KB
- Citations clicked (accuracy proxy)
- Top queries → identifies content gaps

Combine with developer survey: "Did the KB answer your question?"

---

## 7. Security & Privacy

- KB content is only visible to users with read access to the source repos
- Apply [content exclusions](./content-exclusions.md) to skip sensitive paths
- Audit events: `copilot.knowledge_base.created`, `copilot.knowledge_base.search`
- KB chunks are **not** used to train base models

---

## 8. Pairing Pattern

```
Knowledge Base       → broad authoritative grounding
└── Copilot Space    → curated working context for a team
    └── Custom Agent → role-specific behavior on top
```

A mature org has all three layers active.

---

## 9. References

- [Copilot Spaces](./copilot-spaces.md)
- [Custom Agents](./custom-agents.md)
- [Content Exclusions](./content-exclusions.md)
