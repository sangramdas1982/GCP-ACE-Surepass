# 1.2.02 — Linking projects to billing accounts

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A project can be linked to one billing account at a time, while an account can pay for many projects. Linking requires authorization on both sides: permission to associate the project and permission to use the billing account. Common role combinations use Project Billing Manager on the project and Billing Account User on the destination account.

Moving a project between folders or organizations and changing its billing account are separate operations. Disabling project billing can interrupt chargeable services; it is not equivalent to pausing every resource safely while preserving normal availability.

## Practical workflow

Inspect `gcloud billing projects describe PROJECT_ID`. Verify the intended destination account and both permissions before linking. After a change, confirm billing is enabled and review any product-specific commitments or commercial implications.

## Exam trap

A user who can administer a project may still lack permission to associate it with the selected billing account. Investigate both resources in a permission failure.

## Check your understanding

**Scenario:** A developer can create projects but cannot attach the company billing account. Which missing access is plausible?

<details>
<summary>Answer and reasoning</summary>

Permission to use that billing account. Creating a project does not establish authority over a separate financial resource.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/billing/docs/how-to/modify-project)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
