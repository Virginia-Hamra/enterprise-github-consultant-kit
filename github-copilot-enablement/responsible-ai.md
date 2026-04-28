# Responsible AI for Copilot

> Senior-consultant guidance on safe, compliant, and trustworthy use of GitHub Copilot in regulated and enterprise settings.

---

## 1. Principles to Anchor To

- **Human accountability**: AI assists; humans decide and are responsible.
- **Transparency**: users know when AI is involved.
- **Privacy**: minimal data, scoped processing.
- **Fairness**: monitor for biased or harmful outputs.
- **Security**: AI surface is part of the threat model.
- **Auditability**: every AI-influenced change is traceable.

These map directly to the EU AI Act, NIST AI RMF, and ISO/IEC 42001.

---

## 2. Data Handling Posture

| Data | Treatment (Business / Enterprise) |
|------|------------------------------------|
| Prompts | Processed, not retained for training |
| Suggestions | Generated transiently |
| Code in your repos | Used only with your access; not training |
| Telemetry | Aggregate, anonymizable; opt-out per org |
| Audit logs | Retained per enterprise plan |

Document the above in the customer's DPIA and Data Processing Addendum.

---

## 3. Public-Code Matching

Set **Suggestions matching public code** to **Block** for regulated customers.

Trade-offs:

- Block → IP indemnity preserved, fewer license-attribution risks
- Allow → some suggestions may match public repos and require attribution

When Block is set, GitHub provides IP indemnification (verify current contract terms with the customer's legal).

---

## 4. Content Exclusions as a Safety Control

See [content-exclusions.md](./content-exclusions.md). Treat exclusions as part of the AI threat model, not as an afterthought.

---

## 5. Prompt-Injection Threat Model

The autonomous **Coding Agent** and any **MCP server** that ingests external content (issues, web pages, third-party APIs) is exposed to prompt-injection. Mitigations:

- [ ] Treat all external text as untrusted input
- [ ] Validate / strip content rendered by MCP tools
- [ ] Never auto-execute commands from agent output without policy gates
- [ ] Restrict the agent's secret scope and network egress
- [ ] Log and review the agent's tool calls
- [ ] Required human review on any PR opened by the agent

---

## 6. Hallucination Management

Reduce risk via:

- [Knowledge Bases](./knowledge-bases.md) for grounded answers
- [Spaces](./copilot-spaces.md) with curated context
- Custom instructions specifying "cite sources from KB only"
- Tests as truth — refuse to merge agent PRs with weak coverage

Make explicit in customer comms: Copilot can be confidently wrong. Always validate.

---

## 7. Bias & Harm Monitoring

- Spot-check for stereotypes in generated docs/comments
- Use diverse pilot cohorts to surface usability issues
- Provide an internal channel to report concerns
- Track and triage in a dedicated label (`copilot-concern`)

---

## 8. Human-in-the-Loop Controls

| Activity | HITL Requirement |
|----------|------------------|
| Code suggestions | Author reviews before commit |
| Coding Agent PRs | ≥1 human CODEOWNER, required checks |
| Code Review comments | Treated as advisory, not gating |
| Knowledge Base edits | PR-based, owner-reviewed |
| Policy changes | Reviewed via change management |

---

## 9. Documentation Set for the Customer

- **AI Acceptable Use** (internal policy)
- **Copilot Operating Model** (your responsibilities + GitHub's)
- **DPIA / TIA** (data protection & transfer impact assessments)
- **Threat model** for agentic surfaces
- **Audit & evidence map** for SOC 2 / ISO / DORA / EU AI Act

---

## 10. Regulatory Touchpoints (Quick Map)

| Regulation | Relevant Controls |
|------------|------------------|
| EU AI Act | Risk classification, transparency, human oversight |
| GDPR / UK GDPR | Lawful basis, DPIA, sub-processor disclosure |
| DORA (EU finance) | ICT risk register, third-party concentration |
| HIPAA | BAA in scope (where applicable), exclusions for PHI |
| PCI-DSS | Exclude card-data paths, restrict agent in CDE |
| FedRAMP | Use FedRAMP-authorized GHEC; map controls |
| ISO/IEC 42001 | AI management system documentation |

---

## 11. References

- [GitHub Trust Center](https://github.com/trust-center)
- [Licensing & Policies](./licensing-and-policies.md)
- [Adoption Rollout](./adoption-rollout.md)
