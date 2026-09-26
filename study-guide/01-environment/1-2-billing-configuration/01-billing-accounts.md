# 1.2.01 — Creating and administering billing accounts

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A Cloud Billing account tracks charges and payment responsibility. It is distinct from a project and from the Google payments profile that manages payment details. One billing account can fund several projects. Billing-specific roles govern financial operations without inherently granting VM or database administration.

Separate billing accounts may be appropriate for different legal/payment arrangements. Merely separating teams or environments usually calls for projects and cost attribution first. Distinguish the ability to create billing accounts, administer an existing account, view costs, and link projects.

## Practical workflow

Identify the payment owner, currency and commercial arrangement, create the account through the supported billing workflow, and assign finance and project-linking responsibilities separately. Use `gcloud billing accounts list` to inspect accessible accounts.

## Exam trap

Billing Account Administrator is not equivalent to Project Owner. Financial access does not automatically confer access to application data.

## Check your understanding

**Scenario:** A finance analyst needs cost reports without changing infrastructure. Should the analyst receive project Editor?

<details>
<summary>Answer and reasoning</summary>

No. Grant an appropriate billing viewing role at the billing-account scope. Editor adds unnecessary resource-changing permissions.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/billing/docs/concepts)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
