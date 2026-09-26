# 1.1.01 — Resource hierarchy and project identifiers

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Think of a project as a workspace for a workload: it groups resources, API configuration, quotas, and billing attribution. Organizations sit above projects; optional folders group projects by department, environment, or policy boundary. Resources belong inside this hierarchy. A billing account is linked separately, not inserted between a folder and a project.

A project has a mutable display name, a globally unique project ID, and a generated project number. APIs and service-account addresses often use the ID or number; the display name is not a reliable identifier. Separating production and development into different projects makes permissions, quotas, and cost attribution easier to manage.

## Practical workflow

Draw Organization → Production folder → Payments project → VM. Give a finance group visibility at the appropriate billing scope and an operations group access to the production folder. Inspect `gcloud projects list` and `gcloud projects describe PROJECT_ID`; identify all three project identifiers.

## Exam trap

An inherited allow grant is not removed by simply deleting a project-level binding. Moving a project can change inherited policies; it does not automatically change its billing account.

## Check your understanding

**Scenario:** Two teams need separate quotas and billing attribution but shared corporate governance. One project with labels, or separate projects under an organization?

<details>
<summary>Answer and reasoning</summary>

Separate projects. Labels help attribute costs but do not create independent project quotas or IAM boundaries.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
