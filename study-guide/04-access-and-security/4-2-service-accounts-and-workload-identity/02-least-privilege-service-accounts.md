# 4.2.02 — Least privilege for workload identities

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A workload service account should receive only the permissions its code requires on the resources it uses. Grant object read access on a required bucket rather than broad project Editor. Different applications should use separate identities when their permissions or ownership differ.

Review indirect privilege escalation. Permission to alter code running as a powerful service account can effectively give access to that account’s capabilities. Access to attach or impersonate the account is therefore security-sensitive even if the caller does not directly hold every data role.

## Practical workflow

List the exact API actions, identify the target resources, grant narrow predefined/custom roles, run representative operations, and confirm prohibited operations fail. Review observed permissions carefully before removing infrequently used recovery access.

## Exam trap

API scopes on older VM access paths can also constrain access, but broad OAuth scopes do not replace IAM permissions. Avoid explaining every VM denial solely through one layer.

## Check your understanding

**Scenario:** A worker only reads one bucket. Is assigning project Owner defensible because it avoids permission errors?

<details>
<summary>Answer and reasoning</summary>

No. A bucket-scoped object-viewing role is much closer to the requirement. Troubleshoot missing specific access instead of granting unrelated administration rights.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/best-practices-service-accounts)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
