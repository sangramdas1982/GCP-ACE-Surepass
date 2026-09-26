# 4.2.04 — Permissions held by versus permissions on a service account

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A service account is both a principal and an IAM-managed resource. As a principal, it can hold roles on buckets, projects, and other resources. As a resource, it has a policy controlling who may use, impersonate, or administer it. This distinction is essential in exam scenarios.

For example, granting Storage Object Viewer to an account on a bucket lets it read objects. Granting Service Account Token Creator to a user on that account lets the user obtain supported short-lived credentials as it. Those are two different bindings on two different resources.

## Practical workflow

Write the sentence “WHO needs to do WHAT on WHICH resource?” Read the service account’s own policy when investigating impersonation or use; read the target resource’s policy when investigating the account’s data access.

## Exam trap

Adding a storage role to the wrong policy resource may not grant the intended bucket access. Service Account Admin is not synonymous with every impersonation permission.

## Check your understanding

**Scenario:** A service account reads a bucket successfully, but a user cannot impersonate it. Should you add more bucket roles?

<details>
<summary>Answer and reasoning</summary>

No. Inspect the user’s impersonation permission on the service account. The account’s existing data access is already sufficient.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/manage-access-service-accounts)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
