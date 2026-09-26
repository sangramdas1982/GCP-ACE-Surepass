# 4.1.03 — Basic, predefined, and custom IAM roles

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Basic roles such as Owner, Editor, and Viewer are broad and coarse-grained. Predefined roles are maintained for specific service responsibilities. Custom roles contain a chosen set of supported permissions and are created at a supported project or organization scope.

Prefer a predefined role when it closely matches the job. Use a custom role when the permissions truly need tailoring and the team can maintain it as APIs evolve. A custom role definition does not itself grant access: it must be bound to a principal on an appropriate resource.

## Practical workflow

List required actions, compare suitable predefined roles, and choose the smallest practical permission set. For custom roles, verify permission support and scope, test the role, document ownership, and review future changes.

## Exam trap

Custom roles are not automatically safer if they contain broad permissions. Service-agent roles are intended for specific service agents, not a shortcut for human administrators.

## Check your understanding

**Scenario:** No predefined role matches a stable job without significant extra permissions. What option should you consider?

<details>
<summary>Answer and reasoning</summary>

A custom role containing supported required permissions, followed by an appropriate binding. Creating the definition without a binding gives nobody the new access.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/roles-overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
