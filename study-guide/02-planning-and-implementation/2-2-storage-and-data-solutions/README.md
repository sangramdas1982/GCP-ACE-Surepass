# 2.2 — Storage and data solutions

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Separate data model from access pattern. Files, objects, blocks, relational transactions, document access, analytics, messaging, and stream processing require different services.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Choosing databases, analytics, messaging, and pipelines](01-data-service-selection.md)
- [ ] [02. Object, file, and parallel filesystem storage](02-storage-selection.md)
- [ ] [03. Cloud Storage classes and access costs](03-storage-classes.md)
- [ ] [04. Loading and transferring data](04-load-transfer-data.md)
- [ ] [05. Multi-region redundancy and disaster recovery](05-multi-region-data.md)
- [ ] [06. Cloud SQL: relational applications, HA, and replicas](06-cloud-sql.md)
- [ ] [07. AlloyDB for PostgreSQL](07-alloydb.md)
- [ ] [08. Spanner: distributed relational transactions](08-spanner.md)
- [ ] [09. Firestore: document data and indexes](09-firestore.md)
- [ ] [10. Bigtable: wide-column and row-key design](10-bigtable.md)
- [ ] [11. BigQuery: analytical jobs and cost controls](11-bigquery.md)
- [ ] [12. Pub/Sub: topics, subscriptions, acknowledgments](12-pubsub.md)
- [ ] [13. Dataflow: batch and streaming pipelines](13-dataflow.md)
- [ ] [14. Managed Service for Apache Kafka](14-managed-kafka.md)
- [ ] [15. Memorystore: caching and in-memory access](15-memorystore.md)
- [ ] [16. NetApp Volumes: enterprise file workloads](16-netapp-volumes.md)
- [ ] [17. Managed Lustre: parallel filesystem workloads](17-managed-lustre.md)

## Worked study exercise: Select and exercise data services

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Use the storage lab in [Hands-on exercises](../../HANDS-ON-LABS.md) to create a private bucket, upload/read a small object, and inspect its metadata.
2. Compare storage classes using a one-year retention example and two access patterns: daily reads versus rare recovery.
3. Classify these requirements: PostgreSQL transactions, document records, sensor time-series lookup, large analytical SQL, shared NFS, and event delivery.
4. Use a tiny sample query in BigQuery with a strict bytes-billed limit, or a temporary training lab; inspect job details.
5. Draw Pub/Sub → Dataflow → BigQuery and explain why each step exists.

**Success evidence:** choose by interface and workload constraints rather than remembering product names alone.

**Cleanup:** delete lab objects/buckets and any query destination datasets. Review soft-deleted/versioned data and retention settings because visible deletion need not remove every billable copy immediately.

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
