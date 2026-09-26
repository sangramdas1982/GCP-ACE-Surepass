# 4.2.08 — Workload Identity Federation for external workloads

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Workload Identity Federation exchanges trusted external workload credentials for short-lived Google Cloud access. A workload identity pool groups identities; a provider defines the external issuer/trust configuration. Attribute mappings and conditions identify exactly which workloads may authenticate.

It suits CI/CD systems, workloads on other clouds, and supported external environments. Access can use direct resource grants where supported or service-account impersonation. Restrict trust to intended repository, branch, organization, or equivalent stable attributes; trusting an entire issuer without narrowing claims can admit unrelated workloads.

## Practical workflow

Create the pool/provider, map stable claims, apply restrictive conditions, grant resource access or narrowly scoped impersonation, configure the external credential flow, and test both an approved workload and an unapproved one.

## Exam trap

Workload federation is for software; workforce federation is for people. Neither removes the need for resource-level IAM authorization.

## Check your understanding

**Scenario:** A CI pipeline must deploy to Google Cloud without storing a service-account JSON key. What should you evaluate?

<details>
<summary>Answer and reasoning</summary>

Workload Identity Federation using the CI provider’s supported identity token, with restrictive trust and deployment permissions. Rotating a stored key is less effective than avoiding it where federation is available.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/workload-identity-federation)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
