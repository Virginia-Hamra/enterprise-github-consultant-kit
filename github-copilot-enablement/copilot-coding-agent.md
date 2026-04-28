# Copilot Coding Agent

> The autonomous, asynchronous agent that takes an issue and produces a pull request. Available with **Copilot Enterprise** (and Pro+ for individuals).

---

## 1. Mental Model

| Sync Copilot | Coding Agent |
|--------------|--------------|
| You drive in IDE / chat | You delegate via issue assignment |
| Lives in editor session | Runs in cloud-isolated environment |
| Output: completions, edits | Output: a PR with commits, tests, description |
| Latency: seconds | Latency: minutes |

The coding agent is appropriate for **well-scoped, low-to-medium risk** tasks — not greenfield architecture.

---

## 2. How to Invoke

### Assign an issue

```
Assignees: @copilot
```

The agent picks up the issue, opens a draft PR, iterates until it self-reports ready for review, then requests review from CODEOWNERS.

### From a PR comment

```
@copilot fix the failing tests and address the lint warnings
```

### From the IDE / chat

`@github` agent → "open a coding agent session for issue #123"

---

## 3. Environment

The agent runs in an **ephemeral, isolated** runner with:

- The repo checked out at the latest default branch
- Network egress per org policy (firewall rules apply)
- Access to org secrets explicitly granted to the agent
- A devcontainer / Codespaces-like environment when configured

Configure with:

```yaml
# .github/workflows/copilot-setup-steps.yml
name: Copilot setup steps
on: workflow_dispatch
jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-node@v5
        with: { node-version: '22' }
      - run: npm ci
      - run: npm run build
```

This file tells the agent how to bootstrap the project before it starts editing.

---

## 4. Security Boundary

- Agent commits are **signed** as Copilot, attributed to the assigning user
- Branch protection rules **still apply** — required checks must pass before merge
- The agent **cannot merge its own PR** unless explicitly allowed
- Secrets are scoped via `Settings → Copilot → Coding agent`
- All agent actions are in the audit log (`copilot.agent.*` events)

Recommended baseline:

- [ ] Agent runs only on repos where it is explicitly enabled
- [ ] Required reviewers: ≥1 human CODEOWNER on every agent PR
- [ ] No production deployment workflows triggered by agent PRs
- [ ] Agent secret scope = minimum needed to build & test
- [ ] MCP servers used by the agent are allow-listed

---

## 5. Good Tasks for the Agent

- Dependency upgrades with test coverage
- Bug fixes with reproduction in the issue
- Adding tests to existing modules
- Refactors with clear scope ("rename X to Y", "extract module Z")
- Documentation updates from a spec
- Migrations following an established pattern

## 6. Bad Tasks

- Ambiguous "make this better" requests
- Cross-repo coordination
- Production migrations
- Anything requiring human stakeholder input
- Security-critical changes without strong tests

---

## 7. Quality Gates for Agent PRs

| Gate | Tool |
|------|------|
| Build & test pass | Required Actions check |
| Code coverage non-regressive | Codecov / coverage check |
| CodeQL clean | GHAS |
| Secret scan clean | GHAS |
| Style / lint | Required check |
| Human CODEOWNER approval | Branch protection |
| Optional: Copilot Code Review pass | [Copilot Code Review](./copilot-code-review.md) |

---

## 8. Measurement

- PRs opened by `@copilot` per week
- Merge rate of agent PRs
- Mean review iterations before merge
- Mean human edit ratio on agent PRs (lower = more autonomous)

---

## 9. References

- [Background Agents](./background-agents.md)
- [Custom Agents](./custom-agents.md)
- [MCP Servers](./mcp-servers.md)
