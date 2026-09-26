# 4.2.01 — User-managed accounts and Google-managed service agents

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A service account is an identity for software rather than an individual person. A user-managed account can be created for an application and assigned appropriate roles. Google-managed service agents are provisioned for supported products to perform service operations in your environment; they have product-specific responsibilities.

Do not treat every address ending in a service-account domain as the same kind of account. Default service accounts, application identities, and service agents can have different creation paths and permissions. Prefer one identity per meaningful workload responsibility rather than a shared all-powerful identity.

## Practical workflow

Identify the runtime responsibility, create a user-managed account when needed, document its owner/purpose, and grant required access. For service agents, follow the product provisioning process and preserve required service-agent roles.

## Exam trap

Creating a service account does not require creating a downloadable key. Manually recreating a service agent as a normal account is not an equivalent replacement.

## Check your understanding

**Scenario:** A new application needs to read one bucket. Should it run as a human engineer’s account?

<details>
<summary>Answer and reasoning</summary>

Use a dedicated workload service account or supported federated workload identity with scoped access. A personal identity couples application operation to a human account lifecycle.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/service-account-types)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
