# Solution Architecture Knowledge Base

> Practitioner reference for senior consultants. Covers the architectural domains you advise on across an enterprise engagement.

---

## 📚 Index

### Foundations

- **[Knowledge Base](./knowledge-base.md)** — SOLID, GoF patterns, architectural styles, distributed systems, REST API design, UI/UX architecture, DDD, cross-cutting concerns
- **[Enterprise Architecture](./enterprise-architecture.md)** — TOGAF, C4, Wardley, ADRs, capability mapping

### Build & Operate

- **[Platform Engineering](./platform-engineering.md)** — IDPs, Backstage, golden paths, Team Topologies
- **[DevOps & SRE](./devops-and-sre.md)** — DORA, SPACE, SLOs, incidents, progressive delivery
- **[Resilience Patterns](./resilience-patterns.md)** — Circuit breaker, bulkhead, retry, idempotency
- **[Observability](./observability.md)** — OpenTelemetry, RED/USE, SLO alerting

### Data & AI

- **[Data Architecture](./data-architecture.md)** — OLTP/OLAP, lakehouse, mesh, governance
- **[AI / LLM Architecture](./ai-and-llm-architecture.md)** — RAG, agents, evals, guardrails

### Cloud & Integration

- **[Cloud Strategy](./cloud-strategy.md)** — Landing zones, multi-cloud, FinOps, DR
- **[Integration Patterns](./integration-patterns.md)** — EIP, REST/gRPC/GraphQL, sagas, idempotency

### Governance & Change

- **[Security Architecture](./security-architecture.md)** — STRIDE, zero-trust, SSDLC, supply chain
- **[Compliance & Governance](./compliance-and-governance.md)** — SOC2, ISO, DORA, GDPR, AI Act, evidence automation
- **[Migration & Modernization](./migration-and-modernization.md)** — 7 Rs, strangler fig, GitHub migrations

---

## 🎯 How to Use This Knowledge Base

| Engagement Phase | Read First |
|------------------|------------|
| Discovery | Enterprise Architecture, Cloud Strategy, Compliance & Governance |
| Pilot design | Platform Engineering, Security Architecture, Knowledge Base |
| Build phase | Integration Patterns, Resilience, Observability, Data Architecture |
| Modernization | Migration & Modernization, Cloud Strategy |
| AI workstream | AI / LLM Architecture, Responsible AI (Copilot enablement) |
| Run / scale | DevOps & SRE, Observability, Resilience |

Cross-link to the [GitHub Copilot Enablement](../github-copilot-enablement/README.md) folder for the AI / Copilot pillar of every engagement.

---

## 🧭 Mapping to Customer Advisory Domains

This folder backs the engineering depth of the [Customer Advisory Domains](../README.md#2-customer-advisory-domains) defined in the kit's main README. Each architecture file maps to one or more domains there.
