# Data Architecture

> Patterns for OLTP, analytics, streaming, and modern lakehouse / data-mesh designs.

---

## 1. The Layered Reference

```
Sources →  Ingest  →  Storage  →  Process  →  Serve  →  Consume
(apps,    (CDC,      (lake,      (batch,     (mart,    (BI, ML,
 SaaS,     events,    lakehouse,   stream,    API,      apps)
 files)    ETL/ELT)   warehouse)   ML)        cache)
```

Map every project to this skeleton. Optimize the layer that hurts most.

---

## 2. OLTP vs OLAP vs HTAP

| Aspect | OLTP | OLAP | HTAP |
|--------|------|------|------|
| Workload | Many small tx | Few large scans | Mixed |
| Latency | ms | seconds | ms–seconds |
| Schema | Normalized | Star / Snowflake | Hybrid |
| Examples | Postgres, MySQL, DynamoDB | Snowflake, BigQuery, Redshift, Databricks | TiDB, SingleStore |

Choose based on dominant pattern; combine via CDC where both matter.

---

## 3. Lakehouse (Delta / Iceberg / Hudi)

The lakehouse pattern reduces the historical lake-vs-warehouse split:

- Object storage as the system of record (S3, ADLS, GCS)
- Open table format (Delta Lake, Apache Iceberg, Apache Hudi)
- Compute decoupled (Spark, Trino, DuckDB, Snowflake external tables)
- ACID, time travel, schema evolution
- Unified for BI + ML

Senior tip: pick **Iceberg** for max engine portability in 2026.

---

## 4. Data Warehouse Modeling

| Model | Use |
|-------|-----|
| **Star schema** | Default for BI; one fact + dimensions |
| **Snowflake schema** | Normalized dimensions; rarely worth it |
| **Data Vault** | Auditable, change-resistant raw layer |
| **One Big Table (OBT)** | When compute is cheap and joins are slow |

Use **dbt** to manage transformations; treat models as code with tests.

---

## 5. Streaming Patterns

- **Outbox pattern** — atomic write of business event with DB tx
- **CDC** — Debezium / Kafka Connect off the OLTP DB
- **Event sourcing** — events are the source of truth, snapshots derived
- **CQRS** — separate write and read models

Be honest about needs: most "we need streaming" is solved by **CDC + nightly batch**.

---

## 6. Data Mesh (When It Fits)

Four principles:

1. Domain ownership of data
2. Data as a product
3. Self-serve data platform
4. Federated computational governance

Fits when:

- > 5 product domains, each with their own data
- Central data team is the bottleneck
- Strong platform engineering culture exists

Doesn't fit when:

- Single product, single team
- Regulatory model needs strong central oversight

---

## 7. Data Governance

| Capability | Tooling |
|------------|---------|
| Catalog | Unity Catalog, Atlan, Collibra, OpenMetadata, DataHub |
| Lineage | OpenLineage, dbt docs, Marquez |
| Quality | Great Expectations, Soda, dbt tests |
| Access | Privacera, Immuta, native cloud IAM |
| Privacy | Tokenization, masking, k-anonymity |

A senior consultant always asks: **who owns this dataset, who is on-call, where's the SLA?**

---

## 8. Data Classification

| Class | Examples | Default Controls |
|-------|----------|------------------|
| Public | Marketing copy | None |
| Internal | Org charts | SSO |
| Confidential | Financial reports | SSO + audit |
| Restricted | PII, PHI, PCI | Encryption + DLP + minimization |

Tag at the column level where possible; enforce in views.

---

## 9. ML / Feature Stores

- **Feature store** (Feast, Tecton) for online + offline parity
- **Model registry** (MLflow, SageMaker, Vertex)
- **Inference patterns**: batch, real-time, embedded
- **Monitoring**: data drift, prediction drift, performance decay

Treat models as products with SLOs.

---

## 10. AI / Vector Patterns

For AI-application data layer:

- **Vector store** (pgvector, Pinecone, Weaviate, Vespa)
- **Hybrid search** = BM25 + vector + reranker
- **Chunking strategy** matters more than the embedding model
- **Eval set** is the most important data asset for an LLM app

See [ai-and-llm-architecture.md](./ai-and-llm-architecture.md).

---

## 11. Anti-Patterns

- "Single source of truth" written into every diagram, never enforced
- Streaming everything because Kafka exists
- Data mesh as an excuse to avoid governance
- Lakehouse without retention or partitioning strategy
- ML models in prod without monitoring

---

## 12. References

- [AI / LLM Architecture](./ai-and-llm-architecture.md)
- [Knowledge Base — Data Modeling & DDD](./knowledge-base.md)
