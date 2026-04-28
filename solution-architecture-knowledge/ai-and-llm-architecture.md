# AI / LLM Application Architecture

> Reference patterns for building production-grade LLM-powered systems. The 2026 baseline.

---

## 1. The Reference Stack

```
User → UI → Orchestrator → (Tools | Retrieval | Models) → Memory + Observability
                                ↓
                            Eval & Guardrails
```

| Layer | Choices |
|-------|---------|
| Models | OpenAI, Anthropic, Azure OpenAI, AWS Bedrock, Google Vertex, OSS via vLLM |
| Orchestration | LangGraph, Llama Index, Semantic Kernel, custom |
| Retrieval | pgvector, Pinecone, Weaviate, Vespa, Elastic |
| Tools | OpenAPI, MCP servers |
| Memory | Vector + key-value + summary memory |
| Eval | Promptfoo, Braintrust, Langfuse, Ragas |
| Guardrails | Llama Guard, Azure Content Safety, custom classifiers |
| Obs | OpenTelemetry GenAI conventions, Langfuse, Phoenix |

---

## 2. Core Patterns

### Retrieval-Augmented Generation (RAG)

```
Query → Retriever (BM25 + vector + reranker) → Context → LLM → Cited answer
```

The retriever quality, not the model, defines RAG quality.

### Tool / Function Calling

The model decides which tool to invoke; orchestrator executes; result feeds back. Use for structured actions, not knowledge.

### Agentic Loops

Plan → Act → Observe → Reflect, with bounded steps. Add:

- Max iterations
- Cost ceiling
- Human-in-the-loop checkpoints

### Multi-Agent

Coordinator + specialized agents. Reserve for genuinely heterogeneous problem spaces — most "multi-agent" wins are actually better single-agent designs.

---

## 3. RAG Done Right

- **Chunking**: 200–800 tokens, semantic-aware (heading boundaries, code blocks intact)
- **Embeddings**: domain-appropriate model; test multiple
- **Hybrid retrieval**: BM25 + vector + reranker (Cohere Rerank, BGE, in-house)
- **Metadata filters**: tenant, language, recency
- **Citations**: always; without them, RAG is theater
- **Re-indexing**: scheduled + event-driven on source change

The cost of poor chunking is invisible until users complain.

---

## 4. Evals Are the Architecture

Without evals, you cannot:

- Compare prompt versions
- Compare models
- Catch regressions
- Make data-driven changes

Build:

- **Unit-level**: deterministic input/output assertions
- **LLM-as-judge**: rubrics with calibrated judges (and human-aligned)
- **End-to-end**: conversational flows with metrics
- **Online evals**: sample prod traffic for review

Eval set is the most valuable artifact in an LLM project. Version it.

---

## 5. Guardrails

| Layer | Examples |
|-------|----------|
| Input | PII detection, jailbreak detection, topic filters |
| Tool use | Allow-listing, parameter validation, sandboxes |
| Output | Toxicity, factuality, schema validation |
| System | Rate limit, cost cap, abuse detection |

Apply in defense-in-depth — never single-layer.

---

## 6. Cost & Latency Patterns

- **Cascade** — small model first, escalate on uncertainty
- **Cache** — semantic cache + exact cache
- **Stream** — first-token latency dominates UX
- **Batching** — for offline jobs
- **Distillation** — fine-tune a small model on big-model traces

Track $/request and tokens/request like SLOs.

---

## 7. Memory Architecture

- **Short-term**: messages within a session
- **Working**: scratchpad / state during a task
- **Long-term**: summarized history per user
- **Semantic**: embeddings of past interactions
- **Episodic**: structured event log

Don't store more than you need; treat memory as PII.

---

## 8. Security & Privacy

- Treat **prompt content as user input** — sanitize, log carefully
- **Prompt injection**: assume hostile inputs; validate tool outputs; never auto-execute
- **Data minimization**: prompt and retrieval contexts are exfiltration vectors
- **PII redaction**: at ingestion and at log
- **Tenant isolation**: in vector store, in cache, in eval data
- **Model exfiltration risk**: rate-limit, watermark expensive outputs

See [responsible-ai.md](../github-copilot-enablement/responsible-ai.md) for governance.

---

## 9. Observability for LLMs

OpenTelemetry GenAI semantic conventions:

- `gen_ai.system`, `gen_ai.request.model`
- `gen_ai.usage.prompt_tokens`, `completion_tokens`, `total_tokens`
- `gen_ai.response.finish_reasons`
- Span per LLM call, tool call, retrieval call

Connect with cost and latency dashboards; alert on regression.

---

## 10. MLOps for LLM Apps

| Stage | Practice |
|-------|----------|
| Data | Versioned eval sets, prompt registry |
| Build | CI runs evals; PRs blocked on regression |
| Release | Canary by % traffic, by user cohort |
| Operate | Online evals, drift, cost alerts |
| Improve | Trace-driven prompt iteration |

Treat prompts and pipelines as code (Git).

---

## 11. Anti-Patterns

- Picking the model first, problem second
- Skipping evals → "it felt better"
- One giant prompt with 30 instructions
- Tool calling without input/output validation
- Multi-agent for a problem one agent solves
- RAG without recency / freshness strategy
- Treating LLM cost as opex without budget controls

---

## 12. References

- [Data Architecture](./data-architecture.md)
- [Security Architecture](./security-architecture.md)
- [Observability](./observability.md)
- [Copilot Responsible AI](../github-copilot-enablement/responsible-ai.md)
