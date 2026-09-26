# 1.1.02 — Organization policies and constraints

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

IAM answers who may perform an action. Organization Policy constrains which resource configurations are allowed, even for a user who otherwise has the necessary IAM permissions. Examples include restricting resource locations or preventing service-account key creation. Policies can be attached at organization, folder, or project scope.

Read the effective policy, including ancestors. Inheritance and override behavior depend on the constraint and policy configuration; do not assume every child can freely loosen an ancestor policy. Some constraints affect only new or updated resources, so enforcement does not necessarily repair existing resources.

## Practical workflow

Identify the prohibited configuration, locate its supported constraint, evaluate existing resources, apply the policy at the required scope, and test both an allowed and disallowed operation. Use dry-run capabilities where the specific policy supports them.

## Exam trap

Granting Owner does not generally bypass an enforced organization constraint. Restricting locations also does not automatically migrate existing data.

## Check your understanding

**Scenario:** A project administrator cannot create a resource outside approved regions despite having its creation permission. What should you inspect?

<details>
<summary>Answer and reasoning</summary>

The effective resource-location organization policy and product support for that constraint. More IAM privileges do not solve this configuration restriction.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/resource-manager/docs/organization-policy/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
