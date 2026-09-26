# 3.2 — Storage and data operations

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Keep data available, recoverable, secure, and affordable. Replication, backup, retention, encryption, and authorization solve distinct problems.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Managing and securing Cloud Storage objects](01-secure-storage-objects.md)
- [ ] [02. Object lifecycle management](02-object-lifecycle.md)
- [ ] [03. Querying Cloud SQL, BigQuery, Bigtable, Spanner, Firestore, and AlloyDB](03-query-data.md)
- [ ] [04. Estimating storage and database costs](04-data-costs.md)
- [ ] [05. Database backups and restoration](05-database-backup-restore.md)
- [ ] [06. Monitoring Dataflow and BigQuery jobs](06-job-status.md)
- [ ] [07. Database Center fleet management](07-database-center.md)
- [ ] [08. Customer-managed encryption keys](08-cmek.md)

## Worked study exercise: Test protection and recovery reasoning

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Inspect bucket IAM, uniform bucket-level access, soft delete, retention, and lifecycle settings.
2. Write a lifecycle rule on paper for older objects; explain any conflict with retention or minimum-duration charges.
3. Define RPO/RTO for an example database and choose backup/PITR versus replication for accidental deletion versus zonal outage.
4. Inspect a sample BigQuery job and identify completion status, errors, bytes processed, and destination.
5. Draw a CMEK-protected service → service identity → KMS key permission relationship.

**Success evidence:** explain which protection prevents deletion, which preserves history, and which supports availability. Identify the identity requiring key use.

**Cleanup:** do not lock a retention policy or disable a real encryption key for practice. Use a disposable sandbox if testing those features.

## How to answer this subsection's scenarios

1. Identify the concrete objective and every hard constraint.
2. State the resource and identity involved; separate configuration, permission, and connectivity failures.
3. Eliminate options that violate a requirement before comparing cost or convenience.
4. Prefer the simplest supported solution that satisfies all stated requirements; a managed service is not automatically correct if it lacks a required capability.
5. Explain why the most tempting alternative fails. Record uncertain answers in the [mistake log](../../MISTAKE-LOG.md).

## Completion check

- [ ] I can explain every linked topic in my own words.
- [ ] I can complete the exercise or explain its expected observations.
- [ ] I can distinguish the services/controls commonly confused here.
- [ ] I can answer the topic scenarios without relying on remembered wording.
