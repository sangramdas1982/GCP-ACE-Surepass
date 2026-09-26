# 1.1.03 — Granting project IAM roles

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An IAM allow policy binds principals to roles on a resource. A principal can be a user, group, service account, or federated identity. A role is a named collection of permissions. The scope of the binding matters as much as the role: granting a storage role on one bucket is narrower than granting it on the entire project.

Use groups for people with the same job responsibilities. Choose a predefined role that matches the task; consider a custom role when predefined roles are materially broader than required. Conditions can restrict a binding when the service and condition attributes support the use case.

## Practical workflow

Write the required action first, identify the permission and suitable role, choose the narrowest supported resource, add the binding, and verify with the intended identity. Inspect a project policy using `gcloud projects get-iam-policy PROJECT_ID`.

## Exam trap

Basic Editor is usually excessively broad for a single-service task. A project policy listing does not by itself show every effective inherited permission.

## Check your understanding

**Scenario:** A team needs to view VM configuration without changing it. Should you grant project Editor?

<details>
<summary>Answer and reasoning</summary>

No. A suitable compute viewer role meets the read-only requirement with less privilege. Editor introduces unrelated write capabilities.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/granting-changing-revoking-access)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
