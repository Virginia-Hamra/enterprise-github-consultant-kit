# Copilot Licensing & Enterprise Policies

> Senior-consultant reference for licensing tiers, policy surface, and rollout governance for **GitHub Copilot Business** and **GitHub Copilot Enterprise**.

---

## 1. SKUs at a Glance (2026)

| SKU | Audience | Key Capabilities |
|-----|----------|------------------|
| Copilot Free | Individuals | Limited completions & chat |
| Copilot Pro | Individuals | Unlimited completions, chat, CLI |
| Copilot Pro+ | Power users | Premium models (Opus, GPT-5.1), more premium requests |
| **Copilot Business** | Orgs | Org policy, audit, IP indemnity, no training on code |
| **Copilot Enterprise** | Enterprises | Everything in Business + Knowledge Bases, GitHub.com chat, Copilot Code Review on the platform, custom models (where available), Copilot Coding Agent, Spaces |

Senior consultants should recommend **Copilot Enterprise** when the customer needs:

- Knowledge Bases grounded on internal repos / docs
- Copilot Code Review across the org
- Copilot Coding Agent (autonomous PR-based agent)
- Copilot Spaces / Workspaces
- Pull-request summaries on github.com

---

## 2. Policy Surface (Enterprise → Org → Repo)

Configure under **Enterprise / Organization → Settings → Copilot → Policies**.

### Identity & Access

- [ ] Restrict Copilot to specific teams / cost centers
- [ ] Use SCIM groups to provision seats
- [ ] Disable seat self-assignment where required
- [ ] Block personal accounts from using enterprise seats (EMU)

### Content & Suggestions

- [ ] **Suggestions matching public code** — `Block` (recommended for regulated)
- [ ] **Content exclusions** — see [content-exclusions.md](./content-exclusions.md)
- [ ] **Editor preview features** — opt-in per org
- [ ] **MCP servers** — allow / deny (Enterprise)

### Chat & Knowledge

- [ ] Copilot Chat in IDE — enabled
- [ ] Copilot Chat in github.com — enabled (Enterprise)
- [ ] Copilot Chat in mobile — enabled
- [ ] Knowledge Bases — enabled, scoped to teams
- [ ] Bing / web search grounding — allow / deny

### Code Review & Agents

- [ ] Copilot Code Review on PRs — enabled
- [ ] Copilot Coding Agent — enabled, scoped repos
- [ ] Custom instructions for code review — repo-level
- [ ] Pull request summaries — enabled

### Data & Privacy

- [ ] Confirm "no training on customer code" (default for Business/Enterprise)
- [ ] Telemetry / usage data retention reviewed with security
- [ ] Audit log streaming includes Copilot events

---

## 3. Seat Management

- Provision seats via **SCIM group** (preferred) or assign manually
- Use **cost center** tagging for chargeback
- Run a quarterly review of inactive seats (≥30 days) — reclaim or reassign
- Use the [Copilot Metrics API](./metrics-and-measurement.md) to validate ROI

```bash
# List seats
gh api /orgs/$ORG/copilot/billing/seats --paginate

# Remove inactive
gh api -X DELETE /orgs/$ORG/copilot/billing/selected_users \
  -f selected_usernames[]=user1 -f selected_usernames[]=user2
```

---

## 4. Legal & Compliance Posture

- **IP indemnity** — included with Business/Enterprise when public-code matching is set to *Block*
- **Data handling** — prompts and suggestions are **not** retained for model training under Business/Enterprise
- **Regional processing** — verify with customer's DPA (EU Data Boundary considerations)
- **Sub-processors** — list available in GitHub Trust Center
- **DORA / EU AI Act readiness** — document Copilot as a high-productivity assistive tool, not autonomous decisioning

---

## 5. Rollout Decision Matrix

| Customer Profile | Recommended SKU | Notes |
|------------------|-----------------|-------|
| SMB, low-regulation | Business | Fast time-to-value |
| Regulated, EU-only | Enterprise + EU Data Residency | Required for sovereignty |
| Public sector / FedRAMP | Enterprise on FedRAMP-authorized GHEC | Validate FedRAMP boundary |
| Innovation lab / R&D | Enterprise | Knowledge Bases + Spaces |

---

## 6. Common Pitfalls

- Enabling Copilot org-wide before policy review → public-code matches in regulated repos
- Not configuring content exclusions for `infra/`, `secrets/`, `*.env`
- Forgetting to stream Copilot audit events to SIEM
- Buying Business when the customer's value driver is Knowledge Bases (needs Enterprise)
- Not aligning seat assignment with SCIM groups → drift

---

## 7. References

- [GitHub Copilot Trust Center](https://resources.github.com/copilot-trust-center/)
- [Copilot policies docs](https://docs.github.com/en/copilot/managing-copilot)
- [Content exclusions](./content-exclusions.md)
- [Metrics & measurement](./metrics-and-measurement.md)
