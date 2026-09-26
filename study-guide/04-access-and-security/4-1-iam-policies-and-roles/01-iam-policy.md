# 4.1.01 — Viewing and creating IAM policies

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An IAM allow policy contains bindings from principals to roles, optionally with supported conditions. A permission such as reading an object is checked against the caller and resource context. A role groups permissions; a policy assigns that role. These terms are related but not synonyms.

Inspect existing bindings before modifying a policy. Replacing an entire policy with an incomplete copy can remove other administrators or applications. Concurrency controls such as policy etags help avoid overwriting another change. Deny policies and organization constraints can still block an action even when an allow binding exists.

## Practical workflow

Read the policy, identify the precise principal/role/scope change, use a binding-level operation where appropriate, and verify both access and remaining bindings. For full-policy writes, preserve existing content and concurrency information.

## Exam trap

Authentication proves identity; authorization permits actions. Logging in with gcloud does not guarantee access to every project.

## Check your understanding

**Scenario:** A script overwrites a project policy with only its own binding and applications stop working. Why?

<details>
<summary>Answer and reasoning</summary>

The replacement removed other required bindings. Safely updating a policy means preserving unrelated access, not just writing the new desired entry.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/policies)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
