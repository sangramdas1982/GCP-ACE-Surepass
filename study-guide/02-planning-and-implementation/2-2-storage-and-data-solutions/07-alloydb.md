# 2.2.07 — AlloyDB for PostgreSQL

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

AlloyDB is a PostgreSQL-compatible managed database designed for demanding transactional and analytical database workloads. A cluster has a primary instance and can use read pools for read scaling. Its architecture differs from simply running open-source PostgreSQL on a VM.

Use compatibility, performance requirements, availability, and cost to compare it with Cloud SQL for PostgreSQL. PostgreSQL compatibility does not mean every extension or administrative operation is identical across managed services. Review migration requirements and test representative queries rather than assuming automatic performance gains.

## Practical workflow

Assess engine/version and extension compatibility, choose the cluster region/network, create primary/read-pool configuration as needed, set backup/recovery and identity, then benchmark representative traffic. Direct appropriate read-only traffic to a read pool.

## Exam trap

AlloyDB is not the same product as BigQuery or Spanner. Read pools do not make arbitrary application writes distribute as independent primary writers.

## Check your understanding

**Scenario:** A team needs PostgreSQL compatibility and a managed platform for a demanding database workload. Which two services should it compare first?

<details>
<summary>Answer and reasoning</summary>

Cloud SQL for PostgreSQL and AlloyDB, using concrete performance/feature/cost requirements. BigQuery addresses large-scale analytics with different application semantics.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/alloydb/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
