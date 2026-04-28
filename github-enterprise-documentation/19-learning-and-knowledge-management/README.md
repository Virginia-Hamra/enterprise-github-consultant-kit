# 19 — Learning & Knowledge Management

> The personal + team practice that keeps a senior consultant sharp across many customers, fast-moving features, and recurring patterns.

---

## Advisory Gist

**TL;DR.** Treat knowledge as **engineering**: versioned in Git, layered (gist → deep), cross-linked, dated. Maintain a personal advisory library + per-customer space + a reusable framework library. Update on every engagement; prune what's stale; never trust memory on rate-limits, thresholds, or feature parity.

**Decisions you will be asked to make**

- Knowledge home: GitHub repo (this kit) / Obsidian / Notion / Confluence.
- Granularity: one-domain-one-doc vs many-small.
- Personal vs customer-specific separation (and confidentiality handling).
- Update cadence (weekly / monthly / per-engagement).
- Tooling for retrieval (search / RAG / Copilot Knowledge Bases / Spaces).

**Top edges**

- Customer-specific notes drifting into reusable docs → confidentiality leak.
- Stale docs cited as current → reputational damage.
- Single private repo of notes → bus factor 1; backup it up.
- "I'll remember" → you won't, and the customer asks 6 months later.

**Connects to**

- [.github/skills/gist-extraction/](../../.github/skills/gist-extraction/SKILL.md) → compression workflow.
- [.github/skills/customer-advisory/](../../.github/skills/customer-advisory/SKILL.md) → application.
- [09 Copilot — Knowledge Bases / Spaces](../09-copilot-in-the-enterprise/README.md) → tooling for retrieval.

**Customer-fit questions** *(applied to yourself as a consultant)*

- When did I last update this domain?
- Which customer-specific note has leaked into the reusable doc?
- What did I learn this week that I didn't write down?

---

## Overview

A senior advisor's edge is **compounded learning**: the same regulation, the same migration shape, the same Actions cost trap appears across customers. Capturing it once, retrievably, makes every subsequent engagement faster and more accurate.

Three layers:

| Layer | Audience | Examples |
|-------|----------|----------|
| **Reference** (REF) | Reusable across customers | This kit's domain folders; deliverable templates |
| **Investigation** (INV) | Deep-dive on a topic | "GHAS active-committer counting at scale", "GHES backup-restore RTO" |
| **Customer** (CUST) | Engagement-scoped | One folder per customer, isolated, retention-policied |

---

## Configuration

### Repo layout

```
enterprise-github-consultant-kit/        ← REF library (this repo)
├── github-enterprise-documentation/     ← 18 domains
├── github-copilot-enablement/           ← Copilot deep-dive
├── solution-architecture-knowledge/     ← architecture context
├── deliverables/                        ← templates + standards
└── .github/skills/                      ← agent skills

advisory-investigations/  (separate repo) ← INV deep-dives
customer-{slug}/         (separate repo) ← per-customer (private, retention-scheduled)
```

### Document conventions

- **Front-load** with the Advisory Gist (TL;DR, decisions, edges, connects-to).
- **Source every claim** with a `docs.github.com` link; mark version where relevant.
- **Date** each doc; review cadence in the front-matter.
- **Cross-link**, don't duplicate. If two docs say the same thing, one is wrong.
- **Glossary**: define each capitalised term on first use, then link.

### Tooling

- **Editor**: VS Code or Obsidian over Markdown in Git.
- **Search**: ripgrep / GitHub search / Obsidian global search; layer on a Copilot Knowledge Base for semantic retrieval.
- **Diagrams**: Mermaid (text-based, lives in repo, diffable).
- **Templates**: under `deliverables/templates/`.

---

## Usage

### Daily

- 5-minute capture: anything new about features, rate-limits, customer pattern.
- 5-minute prune: kill or update one stale page.

### Per-engagement

- New customer folder from template.
- Pull domain READMEs into the engagement as anchors.
- After each milestone: distil one INV note from what was learned.

### Quarterly

- Sweep REF docs for staleness; bump dates.
- Promote recurring INV notes into REF.
- Retire customer folders past retention.

---

## Best Practices

- **Engineer the knowledge.** PRs, reviews, automation, link-checking.
- **One source of truth per fact.** If a number lives in two places, automate the second from the first.
- **Layer, always.** Gist ≤ 50 words → Brief ≤ 300 → Deep ≤ 1000. Same shape everywhere.
- **Capture gaps explicitly.** "What this doesn't tell you" sections.
- **Review on triggers**: GitHub release notes, Copilot changelog, regulatory update, customer pushback.

---

## Common Pitfalls

- Notes-as-prose with no structure → unsearchable.
- Customer-confidential drift into reusable repo → DLP failure.
- Updating one place not the other → contradictions in 6 months.
- "Living document" with no review trigger → stale by default.
- Too-large docs → no one reads to the end.

---

## Implementation Notes

- Use **GitHub Actions** for link-checking + spell-checking + lint of front-matter.
- A `last-reviewed` field per doc + a workflow that flags > 90 days.
- For customer repos: enforce repo-level retention via ruleset + scheduled archive.
- Keep a `CHANGELOG.md` per major area to track what changed when.

---

## Sources

- [Diátaxis documentation framework](https://diataxis.fr/)
- [Architecture Decision Records (Michael Nygard)](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions)
- [GitHub Copilot Knowledge Bases](https://docs.github.com/en/copilot/customizing-copilot/managing-copilot-knowledge-bases)
- [GitHub Copilot Spaces](https://docs.github.com/en/copilot/using-github-copilot/copilot-chat/copilot-spaces)
