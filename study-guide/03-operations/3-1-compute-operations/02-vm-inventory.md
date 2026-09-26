# 3.1.02 — Viewing and inspecting running VMs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Inventory identifies resources and their current lifecycle state. A VM's RUNNING state indicates infrastructure state, not necessarily application health. CPU metrics, service health checks, logs, and guest processes provide additional evidence.

Project and zone mistakes are common operational errors. A command may return nothing because the active project is wrong or the caller cannot see the resource. Use filters and explicit scope to narrow a fleet; labels help identify ownership and environment but are not a substitute for IAM.

## Practical workflow

Run `gcloud compute instances list --project=PROJECT_ID`. Filter with `--filter='status=RUNNING'`. Inspect a selected VM with `gcloud compute instances describe VM_NAME --zone=ZONE --project=PROJECT_ID`, then compare network, identity, disks, and metadata to the expected configuration.

## Exam trap

No resources returned is not proof that the organization has no VMs. A project-scoped list does not enumerate every project automatically.

## Check your understanding

**Scenario:** The console shows a VM but a CLI list does not. What should you verify before recreating it?

<details>
<summary>Answer and reasoning</summary>

The CLI account, active project, explicit filters, and permissions. Recreating it risks a duplicate while leaving the actual visibility problem unresolved.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/sdk/gcloud/reference/compute/instances/list)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
