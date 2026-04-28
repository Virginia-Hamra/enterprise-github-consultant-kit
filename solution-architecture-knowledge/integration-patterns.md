# Integration Patterns

> Connecting systems without creating new monoliths. EAI / EIP, API styles, and event-driven design.

---

## 1. Choose the Style

| Style | Use When |
|-------|----------|
| **Synchronous request/response (REST/gRPC)** | Strong consistency, immediate result |
| **Asynchronous messaging (queues)** | Decoupling, load leveling, retries |
| **Event-driven (pub/sub, streaming)** | Many consumers, fan-out, audit trail |
| **Bulk / batch** | High volume, latency-tolerant |
| **File transfer (still real)** | Legacy partners, finance, EDI |
| **Shared database** | Almost never. |

A mature platform uses **all of the above**, mapped to the right interaction.

---

## 2. EIP — The Patterns You Reuse

From Hohpe & Woolf:

- **Message Channel** — typed pipe between producer and consumer
- **Message Router** — content-based routing
- **Message Translator** — schema mapping
- **Message Endpoint** — boundary adapter
- **Pipes & Filters** — composable transformations
- **Aggregator** — collect related messages, emit composite
- **Splitter** — break composite into items
- **Saga / Process Manager** — coordinate long-running flows
- **Dead Letter Channel** — quarantine poison messages

Implementations: Kafka + Streams / Connect, RabbitMQ + topology, Azure Service Bus, AWS EventBridge + SQS, NATS JetStream.

---

## 3. Sync API Styles

| Style | Strengths | Use For |
|-------|-----------|---------|
| **REST** | Ubiquity, caching, hypermedia | Public APIs, CRUDish |
| **GraphQL** | Client-shaped responses | Aggregating UIs, BFFs |
| **gRPC** | Performance, contracts | Service-to-service |
| **WebSockets / SSE** | Push to clients | Real-time UIs |
| **AsyncAPI** | Event-API contract | Producer/consumer governance |

Don't fight religious wars. Use REST for partners, gRPC for internal hot paths, GraphQL when client diversity demands it.

---

## 4. Eventual Consistency Realities

When you cross a service boundary asynchronously, you trade:

- Immediate consistency → eventual consistency
- Single transaction → saga / compensations
- Read-your-writes guarantees → explicit waits or read-from-write-store

Make this **visible to the product team**, not hidden.

---

## 5. Saga Pattern

Two flavors:

- **Choreography** — services emit events, others react. Simple, but emergent behavior.
- **Orchestration** — central process manager calls steps. Visible, but a new component.

Default to **orchestration** when ≥ 5 steps or strong audit needs.

Always design **compensating actions** for each step. Never assume rollback.

---

## 6. Idempotency

Every async consumer must be idempotent. Common mechanisms:

- Idempotency key (UUID per request)
- Natural keys (order-id, payment-id)
- Outbox + dedup table
- Conditional updates (compare-and-set)

API rule: support `Idempotency-Key` header on all POST endpoints that mutate.

---

## 7. Schema & Contract Governance

- **OpenAPI** for REST, **AsyncAPI** for events, **proto** for gRPC
- Store schemas in a dedicated repo
- CI: backwards-compat checks (Buf, Optic, openapi-diff)
- Publish to a registry (Confluent Schema Registry, Azure API Center)
- Version explicitly; deprecate with sunset headers

---

## 8. API Gateway vs Service Mesh

| Concern | Gateway | Mesh |
|---------|---------|------|
| North-south (user → service) | ✅ | partial |
| East-west (service → service) | partial | ✅ |
| Auth | ✅ | ✅ |
| Rate limiting | ✅ | partial |
| mTLS | partial | ✅ |
| Observability | ✅ | ✅ |

Don't conflate. Use both where complexity warrants.

---

## 9. Backwards Compatibility Rules

- Never remove a field; deprecate it
- Never change a type or semantics; add a new field
- Always default new server fields to safe values
- Use feature flags + per-tenant rollout for breaking shifts

---

## 10. Anti-Patterns

- Shared DB across services
- Distributed monolith (everything calls everything synchronously)
- Sagas without compensations
- Events used as commands (`OrderShouldBeCharged`) — name them facts (`OrderPlaced`)
- API gateway implementing business logic

---

## 11. References

- [Knowledge Base — REST API Design, Architectural Styles](./knowledge-base.md)
- [Data Architecture](./data-architecture.md)
- [Resilience Patterns](./resilience-patterns.md)
