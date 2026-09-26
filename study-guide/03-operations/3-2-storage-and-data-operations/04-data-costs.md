# 3.2.04 — Estimating storage and database costs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Total data cost can include capacity, provisioned compute, replicas, backups, operations, retrieval, and network transfer. Analytical systems can also charge for query processing or reserved capacity. A service with low raw storage pricing can still be expensive under frequent reads or cross-region traffic.

Compare the full workload over its retention period. For Cloud Storage, include minimum storage duration and class-specific retrieval/operation charges. For provisioned databases, include instance capacity and availability/replica choices. For BigQuery, understand the selected compute pricing model, partition pruning, and bytes processed.

## Practical workflow

Write a small estimate with stored GB, retention, operations, retrieval, compute hours, replicas, backup footprint, and transfer destinations. Use current pricing/calculator values when making a real purchase decision; this guide intentionally avoids memorizing changeable prices.

## Exam trap

High availability and backups are not free just because the database is managed. Budget alerts report cost trends but do not optimize queries.

## Check your understanding

**Scenario:** A daily query scans a year of data although only yesterday is needed. What can reduce work?

<details>
<summary>Answer and reasoning</summary>

Partition-aware filtering and selecting needed columns, with a suitable table design. Moving the same unnecessary scan to a different dashboard does not address the cause.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/bigquery/docs/best-practices-costs)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
