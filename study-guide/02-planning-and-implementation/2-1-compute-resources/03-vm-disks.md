# 2.1.03 — Persistent Disk, Hyperdisk, and Local SSD

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Block storage presents disk-like devices to VMs. Persistent Disk and Hyperdisk provide durable network-attached block storage, with performance characteristics and machine compatibility depending on the disk type. Hyperdisk types offer different performance provisioning and workload fits; avoid assuming every type supports every machine series.

Zonal Persistent Disk protects data within its supported zonal design; regional Persistent Disk synchronously replicates across two zones in one region. Regional replication can support recovery from a zone failure, but the application still needs a failover procedure. Local SSD offers fast local scratch space with a different persistence model and should not be the only copy of durable application data.

## Practical workflow

Classify the workload as boot, durable data, shared-file requirement, or disposable scratch. Compare capacity, IOPS, throughput, attachment modes, machine compatibility, and location. Initialize and mount a new data disk inside the OS after attaching it.

## Exam trap

Disk replication is not historical backup. A replicated accidental deletion can affect both copies. Cloud Storage is object storage, not a drop-in VM block device.

## Check your understanding

**Scenario:** A batch job can recreate its intermediate files and needs very fast scratch I/O. Which option fits?

<details>
<summary>Answer and reasoning</summary>

Local SSD can fit disposable scratch data. Keep source inputs and final results in durable storage so loss of local state does not lose the job output permanently.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/disks)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
