# 1.1.08 — Standalone organization setup

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

The supplied syllabus explicitly includes standalone organizations. This is an alternative onboarding model that does not require a Cloud Identity directory or a verified corporate domain. Current documentation describes automatic organization creation for eligible new Free Trial accounts; do not assume existing accounts can all convert through the same flow.

Distinguish Organization Owner from Organization Administrator. Ownership includes recovery and owner-management responsibilities, while IAM administration manages cloud access policies. Standalone organization ownership has special management behavior; it is not simply the basic project Owner role.

## Practical workflow

Inspect the organization shown during onboarding, record its ID, review Organization details, and establish more than one appropriate human recovery/administration contact. Use `gcloud organizations list` to inspect visible organizations. Review eligibility before trying to reproduce this in an existing account.

## Exam trap

Older material claiming that every organization always requires a Cloud Identity domain misses this syllabus item. An organization administrator still needs additional roles for unrelated operations.

## Check your understanding

**Scenario:** A new eligible user wants an organization without operating a corporate identity domain. Which syllabus feature addresses this?

<details>
<summary>Answer and reasoning</summary>

A standalone organization. A folder cannot exist as an independent replacement for the organization root.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/resource-manager/docs/standalone-organization-overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
