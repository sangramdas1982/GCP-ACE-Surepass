# 3.2.05 — Database backups and restoration

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Backups preserve recoverable history; replication preserves availability/current copies. Cloud SQL and AlloyDB have managed backup and recovery capabilities, including point-in-time features subject to configuration and engine support. Spanner, Firestore, and Bigtable have their own backup/restore and retention models. Do not assume every service restores in place or supports identical recovery granularity.

Choose a recovery point before the damaging event, determine the destination resource and dependencies, and validate integrity. Restoring infrastructure without switching applications to the recovered database does not complete recovery. RPO and RTO should guide backup frequency and restoration design.

## Practical workflow

Enable the appropriate backup/recovery settings, confirm successful backups and retention, record a restoration procedure, restore into an approved test destination, and verify data and application access. Measure the actual duration.

## Exam trap

A read replica can reproduce corrupted writes. An export may serve portability but may not provide the same recovery features as managed backups.

## Check your understanding

**Scenario:** A user overwrote important records at 14:05. The database is otherwise healthy. Which recovery feature should you evaluate?

<details>
<summary>Answer and reasoning</summary>

Point-in-time recovery to before the overwrite, if configured and supported, or an appropriate earlier backup. Failing over to a synchronized replica may preserve the same bad change.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/sql/docs/postgres/backup-recovery/backups)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
