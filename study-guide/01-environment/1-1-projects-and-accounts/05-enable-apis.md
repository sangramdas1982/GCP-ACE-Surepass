# 1.1.05 — Enabling service APIs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Google Cloud products expose APIs. Enabling an API in a project makes that service available for use, subject to billing, IAM, quota, and organization policy. API enablement is not the same as granting a caller permission or creating a resource. Some services provision Google-managed service identities as part of enablement or first use.

A deployment can fail because the API is disabled in the consumer project even when the resource name refers to another project. Read the error and identify which project needs enablement. Avoid disabling APIs casually: dependent workloads can stop working.

## Practical workflow

Check the active project with `gcloud config get-value project`. List enabled services with `gcloud services list --enabled --project=PROJECT_ID`. Enable the required service, for example `gcloud services enable compute.googleapis.com --project=PROJECT_ID`, using an identity with Service Usage permissions.

## Exam trap

An API-disabled error is not necessarily an IAM-role problem. Enabling every available API is unnecessary and does not provision every product.

## Check your understanding

**Scenario:** A VM creation call says Compute Engine API has not been used or is disabled. What is the first targeted correction?

<details>
<summary>Answer and reasoning</summary>

Enable the Compute Engine API in the project identified by the error, then retry after propagation. Changing the VM machine type will not fix service enablement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/service-usage/docs/enable-disable)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
