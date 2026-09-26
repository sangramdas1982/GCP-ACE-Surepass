# 2.2.06 — Cloud SQL: relational applications, HA, and replicas

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud SQL manages supported MySQL, PostgreSQL, and SQL Server engines. It is a strong starting point when an existing application expects one of those engines and does not require a different distributed database model. Google manages infrastructure operations, but you still own schema design, indexes, query efficiency, users, and application connection behavior.

Regional HA uses a supported primary/standby arrangement across zones and automated failover. A read replica serves supported read-scaling/replication purposes and is not simply the same as the HA standby. Cross-region DR has separate capabilities and edition/engine constraints. Backups and point-in-time recovery address historical recovery.

## Practical workflow

Choose engine/version, region, capacity, HA, backup/PITR, network access and authentication. Use supported connectors/Auth Proxy where appropriate, while remembering that private IP still needs a private network path. Configure connection pooling and test failover/reconnect behavior.

## Exam trap

The Auth Proxy handles authentication/encryption concerns but does not magically create connectivity into a private VPC. More replicas do not automatically scale primary writes.

## Check your understanding

**Scenario:** A conventional PostgreSQL application needs managed operation and zonal failover. Should you immediately redesign it for Bigtable?

<details>
<summary>Answer and reasoning</summary>

Cloud SQL with an appropriate HA configuration is a closer fit. Bigtable changes the data model and query interface, adding migration work not required by the scenario.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/sql/docs/postgres/high-availability)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
