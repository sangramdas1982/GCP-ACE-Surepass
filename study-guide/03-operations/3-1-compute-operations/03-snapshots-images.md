# 3.1.03 — Snapshots, images, and recovery

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A disk snapshot captures a recoverable point in a disk's history. Snapshot schedules automate creation and retention. Incremental storage behavior reduces repeated data storage, but a snapshot remains a logical restore point. An image is commonly used as a reusable boot-disk template for creating VMs. A machine image captures more VM configuration and disk information for supported machine-level use cases.

Application consistency matters: copying blocks during active writes may produce a crash-consistent state without coordinating application transactions. Quiescing or application-aware backup procedures can be required for reliable recovery.

## Practical workflow

Choose the artifact for the goal, create a snapshot or image, inspect completion and location, then restore into a test disk/VM. For schedules, attach the policy and verify retention behavior. Confirm restored application data, not only resource creation.

## Exam trap

An instance template defines configuration; it is not a backup of current disk contents. Snapshot existence alone does not prove the restore procedure works.

## Check your understanding

**Scenario:** A team needs yesterday’s data after accidental corruption. Should it use an instance template?

<details>
<summary>Answer and reasoning</summary>

Use an appropriate snapshot or application backup from before corruption. A template recreates configuration and may reference a base image with none of yesterday’s data.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/disks/snapshots)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
