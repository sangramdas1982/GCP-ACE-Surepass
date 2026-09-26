# 2.2.01 — Choosing databases, analytics, messaging, and pipelines

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Start with the access pattern. Cloud SQL supplies managed MySQL, PostgreSQL, and SQL Server for conventional relational applications. AlloyDB is PostgreSQL-compatible and designed for demanding database workloads. Spanner supplies horizontally scalable relational transactions with strong consistency. Firestore stores documents for application access. Bigtable is a wide-column database for high-throughput, low-latency key-oriented workloads. BigQuery is an analytical warehouse for large scans and aggregation.

Memorystore provides managed in-memory caching services. Pub/Sub decouples producers and consumers. Managed Service for Apache Kafka fits Kafka ecosystem/protocol requirements. Dataflow executes batch and streaming data processing using Apache Beam. Messaging transports events; processing transforms them; databases and warehouses store/query resulting data.

## Practical workflow

Ask: transactional or analytical; relational or document/key-based; scale and consistency; engine compatibility; latency; and management effort. Then identify whether the problem actually requires storage, a queue, a cache, or a processing pipeline.

## Exam trap

BigQuery is not the default operational database for per-request order updates. Pub/Sub is not a relational query engine. A cache should not silently become the only durable system of record.

## Check your understanding

**Scenario:** A pipeline must ingest events, transform them continuously, and run historical aggregate SQL. What service combination fits?

<details>
<summary>Answer and reasoning</summary>

Pub/Sub for ingestion, Dataflow for transformation, and BigQuery for analytical storage/querying. Each fulfills a different responsibility.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/docs/product-list)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
