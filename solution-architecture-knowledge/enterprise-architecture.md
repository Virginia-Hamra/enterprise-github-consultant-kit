# Enterprise Architecture for Senior Consultants

> Pragmatic EA — frameworks you actually use on engagements, not the 800-page versions.

---

## 1. EA Frameworks (When Each Helps)

| Framework | Best For | Avoid When |
|-----------|----------|------------|
| **TOGAF ADM** | Large transformations, regulated orgs | Small product teams |
| **Zachman** | Comprehensive inventory, governance | Need-for-speed delivery |
| **C4 Model** | Communicating software architecture | Business capability mapping |
| **ArchiMate** | Modeling business + app + tech layers | Lightweight contexts |
| **DDD Strategic Design** | Service boundaries, bounded contexts | CRUD-shaped systems |
| **Wardley Mapping** | Strategic positioning, build-vs-buy | Pure tactical decisions |

A senior consultant typically combines: **C4 + DDD + Wardley** for the engineering conversation, **TOGAF capability map** for the executive conversation.

---

## 2. The C4 Model (Practical)

Four levels of zoom, drawn with the same notation:

1. **System Context** — actors and the system as a black box
2. **Container** — deployable units (services, DBs, queues, web apps)
3. **Component** — major modules within a container
4. **Code** — class-level (rarely drawn, generated when needed)

Senior tip: **always draw L1 and L2**; draw L3 only for the 1–2 critical containers. Skip L4.

```text
[Actor] → [System] → [External System]
              ↓
   ┌──────────────────────┐
   │ Web App │ API │ DB   │ ← L2 containers
   └──────────────────────┘
```

Tooling: Structurizr, Mermaid, Excalidraw, draw.io.

---

## 3. Capability vs Service vs System

| Term | Definition | Example |
|------|------------|---------|
| Capability | What the business does (verb-noun, stable) | "Process Payments" |
| Service | A bounded autonomous implementation | `payments-core` |
| System | A coherent collection of services | "Payments Platform" |

A capability map is **outcome-stable**; service & system maps change with each architectural cycle.

---

## 4. Wardley Mapping in 5 Lines

1. Anchor on a **user need**
2. Map **components** in a value chain
3. Place each on the **evolution axis**: Genesis → Custom → Product → Commodity
4. Identify **inertia points** and **gameplay** (build, buy, outsource, retire)
5. Use to defend "why now" and "why this"

Excellent for telling a transformation story to executives.

---

## 5. Architecture Decision Records (ADRs)

Adopt ADRs day-1. Template:

```markdown
# ADR-0007: Use Postgres for Payments

## Status
Accepted — 2026-04-12

## Context
[Forces: throughput, consistency, ops experience, license cost]

## Decision
We adopt PostgreSQL 16 with logical replication.

## Consequences
+ Strong consistency, mature ecosystem
- Vertical scaling ceiling; revisit at >50k tps
```

Store in `docs/adr/` of the relevant repo. Number sequentially, never delete — supersede.

---

## 6. Reference Architectures You Should Carry

A senior consultant carries 5–8 customer-agnostic reference diagrams in their toolkit:

- Multi-region active-active SaaS
- Event-driven core with outbox
- Strangler-fig modernization
- Zero-trust API platform
- Data lakehouse for analytics
- Internal Developer Platform on Backstage
- AI / LLM application reference
- Migration cutover topology

---

## 7. Architecture Review Cadence

| Cadence | Forum | Output |
|---------|-------|--------|
| Weekly | Architecture office hours | Lightweight guidance |
| Monthly | Architecture review board | ADR approvals |
| Quarterly | Technical strategy review | Roadmap adjustments |
| Annually | Reference architecture refresh | Updated standards |

Anti-pattern: a single quarterly board that becomes a bottleneck. Push decisions down with a clear *guardrails* doc.

---

## 8. Common Anti-Patterns

- Top-down EA disconnected from delivery teams
- Frameworks worshipped over outcomes
- Diagrams without ownership
- Standards without enforcement (rulesets, CI checks)
- "Future state" decks with no transition architecture

---

## 9. References

- [Knowledge Base — SOLID, GoF, Architectural Styles](./knowledge-base.md)
- [Platform Engineering](./platform-engineering.md)
- [Cloud Strategy & Landing Zones](./cloud-strategy.md)
