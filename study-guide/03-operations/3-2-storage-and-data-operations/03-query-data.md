# 3.2.03 — Querying Cloud SQL, BigQuery, Bigtable, Spanner, Firestore, and AlloyDB

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Use the interface appropriate to the data service. Cloud SQL and AlloyDB use their compatible relational engines and SQL clients. BigQuery runs analytical SQL jobs. Spanner supports its documented SQL dialects and client libraries. Firestore queries documents and collections with indexes and transaction capabilities. Bigtable supports key/range-oriented access and supported SQL querying; it is not interchangeable with an OLTP relational engine.

Connection authentication, database authorization, and network reachability are separate layers. Cloud IAM permission to view a database instance does not automatically provide a valid database login or every data permission.

## Practical workflow

Confirm endpoint/project/database, choose the supported client, authenticate, issue a small bounded query, and inspect returned rows and job/query diagnostics. In BigQuery, preview estimated bytes and select only required columns/partitions.

## Exam trap

Running SELECT * on a huge analytical table can scan unnecessary data. Firestore index requirements and Bigtable row-key design affect query feasibility and performance.

## Check your understanding

**Scenario:** A user can view a Cloud SQL instance in the console but cannot query a table. What should you check?

<details>
<summary>Answer and reasoning</summary>

The connection path, database authentication, and table/database privileges. Resource visibility alone does not establish data access.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/bigquery/docs/running-queries)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
