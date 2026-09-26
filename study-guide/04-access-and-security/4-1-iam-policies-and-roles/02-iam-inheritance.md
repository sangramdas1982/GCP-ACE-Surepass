# 4.1.02 — Role attachment and policy inheritance

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Allow grants inherited from ancestors combine with direct grants. A role attached to a folder can authorize access across its descendant projects. Removing a narrower project-level grant does not remove a separate inherited grant. Resource-level scope should match the actual required resource set.

Deny policies can block supported permissions subject to their rules and exceptions; organization policies constrain resource configurations. These are separate controls from ordinary additive allow bindings. For effective access, inspect all applicable layers and conditions rather than reading one policy in isolation.

## Practical workflow

Draw the resource ancestry, locate direct and inherited bindings, inspect relevant denies/conditions, and use Policy Troubleshooter where supported to evaluate a specific principal-permission-resource question.

## Exam trap

Granting Viewer at a project does not downgrade someone who inherits a more powerful role. IAM allow policies are not a “most specific role wins” system.

## Check your understanding

**Scenario:** A user inherits Editor from a folder but is granted Viewer on one project. Is the user now read-only there?

<details>
<summary>Answer and reasoning</summary>

No. The inherited allow permissions still apply unless another applicable control blocks them. Remove or narrow the inappropriate ancestor grant to change that access pattern.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/resource-hierarchy-access-control)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
