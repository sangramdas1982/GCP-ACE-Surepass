# 2.2.02 — Object, file, and parallel filesystem storage

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud Storage holds objects addressed by bucket and object name. It fits media, backups, artifacts, and data-lake inputs. It does not provide the same semantics as an ordinary local block device or shared POSIX filesystem. Filestore provides managed NFS file shares for applications needing a shared filesystem. NetApp Volumes supports enterprise file-service requirements, including protocol and data-management capabilities that depend on the selected service configuration.

Managed Lustre provides a parallel filesystem for demanding HPC and AI workloads with high aggregate throughput. It solves a different requirement from storing infrequently accessed backup objects. For VM block storage, compare Persistent Disk and Hyperdisk instead.

## Practical workflow

Determine the interface the application requires: object API, NFS/SMB as supported, parallel filesystem, or block device. Check region, performance tier, capacity, access network, and protection features before provisioning.

## Exam trap

Do not choose Cloud Storage only because it is a familiar storage product when the application explicitly requires filesystem semantics. A file mount adapter may not reproduce every filesystem behavior.

## Check your understanding

**Scenario:** An existing application requires a shared NFS volume with minimal rewriting. What is the natural starting service?

<details>
<summary>Answer and reasoning</summary>

Filestore, subject to capacity/performance requirements. Rewriting it to use object APIs adds work that the question did not require.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/filestore/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
