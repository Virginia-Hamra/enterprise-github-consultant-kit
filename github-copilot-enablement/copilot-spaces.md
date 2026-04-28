# Copilot Spaces (and Workspace patterns)

> Curated, persistent context bundles for Copilot Chat — the "shared brain" for a team or topic. Copilot Enterprise.

---

## 1. What a Space Is

A **Copilot Space** is a named context window that bundles:

- Selected repositories, files, and folders
- Free-text instructions
- Optional MCP server connections
- Optional knowledge base attachment

Anyone with access to the space gets the same grounded context when chatting — eliminates the "did you `@workspace`?" coordination problem.

---

## 2. When to Create a Space

| Scenario | Space Composition |
|----------|-------------------|
| Onboarding a new team | All key repos + onboarding docs + glossary |
| Migration project | Source-platform exporter + target repos + runbook |
| Incident response | Service repos + runbooks + recent PRs |
| Architecture review | ADR repo + key services + design docs |
| Customer engagement | Engagement plan + customer-tagged docs |

A space is **not** a substitute for [Knowledge Bases](./knowledge-bases.md). Spaces are working contexts; knowledge bases are searchable corpora.

---

## 3. Designing a High-Signal Space

### Do

- Pin only the **canonical** files (architecture docs, top-level READMEs, key entrypoints)
- Add a **System instruction** describing the space's purpose and tone
- Link the relevant Knowledge Base for breadth
- Keep total context under ~300K tokens (model-dependent)

### Don't

- Pin every repo "just in case" — context dilution kills quality
- Pin generated files, lockfiles, vendored code
- Mix unrelated topics in one space — make multiple

---

## 4. Example System Instruction

```markdown
# Space: Payments Platform

You are assisting the Payments Platform team.

Stack: Java 21, Spring Boot 3, PostgreSQL, Kafka.
Compliance: PCI-DSS Level 1.

When answering:
- Always reference the canonical service in `payments-core`
- Flag any change touching `pci/**` paths for security review
- Prefer idempotent designs and outbox pattern for Kafka events
- Cite ADRs in `adr/` when relevant

Never:
- Suggest storing PAN/CVV in app DB
- Recommend disabling TLS or signature verification
```

---

## 5. Lifecycle

| Stage | Action |
|-------|--------|
| Create | Owner sets scope, instructions, members |
| Curate | Weekly: prune stale files, add new canonical sources |
| Measure | Track chat usage and acceptance from this space |
| Retire | Archive when project ends — preserves audit trail |

---

## 6. Governance

- Space ownership maps to a **CODEOWNERS-equivalent** team
- All members must already have read access to underlying repos
- Audit events: `copilot.space.created`, `copilot.space.updated`, `copilot.space.shared`
- Sensitive repos respect [content exclusions](./content-exclusions.md)

---

## 7. Spaces vs Knowledge Bases vs Custom Agents

| Capability | Space | Knowledge Base | Custom Agent |
|------------|-------|----------------|--------------|
| Curated repo context | ✅ | ❌ (corpus-wide) | partial |
| Persistent across sessions | ✅ | ✅ | ✅ |
| Free-text instructions | ✅ | ❌ | ✅ |
| Defined behavior / role | partial | ❌ | ✅ |
| Best for | team workspace | searching docs | repeatable workflow |

Combine all three for mature teams.

---

## 8. References

- [Knowledge Bases](./knowledge-bases.md)
- [Custom Agents](./custom-agents.md)
- [Context Windows](./context-windows.md)
