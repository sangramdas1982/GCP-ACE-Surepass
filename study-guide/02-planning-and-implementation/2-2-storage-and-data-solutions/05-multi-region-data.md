# 2.2.05 — Multi-region redundancy and disaster recovery

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

High availability keeps a service operating through certain failures; disaster recovery restores it after a larger disruption. RPO is acceptable data loss measured in time; RTO is acceptable recovery time. Replication, backups, and failover procedures jointly determine those outcomes.

Cloud Storage location choices provide different geographic placement. Cloud SQL regional HA primarily addresses zonal failures; cross-region replica/DR arrangements address a different failure scope. Spanner and Firestore offer location configurations with their own replication properties. Bigtable replication also requires understanding app profiles and routing behavior. Verify the service-specific design rather than assuming all multi-region labels are equivalent.

## Practical workflow

State the failure to survive, choose compatible compute/data locations, define replication and backup strategy, and test how the application reconnects after failover. Measure restoration rather than relying on the existence of a backup alone.

## Exam trap

A live replica can copy bad writes or deletions. It does not replace historical recovery. Multi-zone and multi-region are different resilience levels.

## Check your understanding

**Scenario:** A database replica contains the same accidental table deletion as the primary. Why did replication not protect the data?

<details>
<summary>Answer and reasoning</summary>

Replication preserves ongoing state, including unwanted changes. Recovery requires a suitable backup or point-in-time recovery mechanism.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/architecture/dr-scenarios-planning-guide)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
