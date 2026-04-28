# Copilot Extensions

> Third-party and custom integrations exposed inside Copilot Chat as `@extension` mentions. Built on the GitHub Apps platform.

---

## 1. Two Flavors

| Type | Built On | Use For |
|------|----------|---------|
| **Marketplace extensions** | GitHub App + Copilot Extension SDK | Vendor capabilities (Datadog, Sentry, Stripe, MongoDB, etc.) |
| **Custom extensions** | Same SDK, private to your org | Internal systems (ticketing, CMDB, deploy tools) |

For agent-to-tool protocols, see also [MCP Servers](./mcp-servers.md). MCP and Extensions are complementary:

- **Extensions** live in Chat surfaces (IDE + github.com), called by `@name`.
- **MCP servers** are tool servers consumed by agents (Coding Agent, IDE agent mode).

---

## 2. When to Build a Custom Extension

Build when:

- An internal system is queried frequently in chat ("What's the status of incident #X?")
- The capability is broader than a single repo (org-wide)
- You want a branded entry point (`@acme-deploy`)
- The action requires authenticated calls to your APIs

Use **MCP** instead when the integration is for the autonomous agent or IDE agent loops, not chat-driven.

---

## 3. Architecture

```text
User in Chat
  → Copilot platform
    → GitHub App webhook (POST /agent)
      → Your service
        → Internal APIs / DB
        ← SSE response stream
      ← Confirmation
    ← Final assistant message
```

Implement with the official SDK (TypeScript or Go). The service must:

- Verify webhook signatures
- Honor the streaming SSE contract
- Respect timeouts (typical 30s)
- Handle conversation history passed in the request

---

## 4. Security Posture

- [ ] GitHub App scoped to the **minimum** permissions needed
- [ ] Run on private infrastructure (no public secrets ingest)
- [ ] mTLS or strict allow-listing from GitHub egress IPs
- [ ] Authenticated calls to backend use **OBO** (on-behalf-of) when possible
- [ ] Audit every action in your own log + GitHub audit log
- [ ] Block prompt-injection: validate and sandbox tool inputs

---

## 5. Org Governance

`Org → Settings → Copilot → Extensions`:

- Allow / block specific marketplace extensions
- Require admin approval before users install
- Enforce that only **verified** extensions are installable
- Disable extensions in regulated repos via repo-level policy

---

## 6. Recommended Internal Extensions (Common Patterns)

| Extension | Purpose |
|-----------|---------|
| `@servicenow` | Incident & change context |
| `@jira` | Ticket lookup, status, transitions |
| `@deploy` | Trigger and inspect deployments |
| `@cmdb` | Service ownership and dependencies |
| `@costs` | Cloud spend per service |
| `@compliance` | Control evidence lookup |

---

## 7. Lifecycle

| Phase | Owner |
|-------|-------|
| Idea & scope | Platform / DevEx team |
| Build | Owning domain team |
| Review | Security + Platform |
| Publish | Org admin enables |
| Operate | SRE on-call rotation |
| Deprecate | Sunset window, redirect users |

---

## 8. Measurement

- Invocations per extension (Metrics API + your service logs)
- Latency p50 / p95
- Error rate
- User satisfaction (in-chat thumbs)
- Coverage: % of intended use cases handled

---

## 9. References

- [Building Copilot Extensions docs](https://docs.github.com/en/copilot/building-copilot-extensions)
- [MCP Servers](./mcp-servers.md)
- [Custom Agents](./custom-agents.md)
