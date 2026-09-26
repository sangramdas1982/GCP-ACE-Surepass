# 2.2.10 — Bigtable: wide-column and row-key design

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Bigtable is a wide-column database for large-scale, high-throughput, low-latency workloads such as telemetry and time-series access. Data access is strongly influenced by row keys and ranges. It is not a conventional relational database with arbitrary joins and foreign-key constraints.

Row-key design determines how work distributes. Sequential keys that concentrate writes can create hotspots; a design should balance distribution with required range reads. Clusters and replication provide availability/placement options, while app profiles influence routing behavior and supported consistency characteristics.

## Practical workflow

List dominant queries, design row keys and column families, load representative data, inspect distribution/latency, and select cluster/routing settings. Plan backups separately from replication.

## Exam trap

“Very large database” is insufficient to choose Bigtable. Large analytical SQL scans suggest BigQuery; relational transactional requirements may suggest Spanner or another relational service.

## Check your understanding

**Scenario:** Billions of sensor readings must be retrieved with low latency by device and time range. Which service deserves consideration?

<details>
<summary>Answer and reasoning</summary>

Bigtable with an appropriate row-key design. The design must distribute writes while preserving useful access paths.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/bigtable/docs/schema-design)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
