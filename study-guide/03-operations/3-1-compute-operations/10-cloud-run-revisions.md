# 3.1.10 — Deploying Cloud Run revisions

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A Cloud Run service has immutable revisions created by relevant deployments/configuration changes. A revision associates a particular container and settings. Separating deployment from traffic assignment allows testing a new revision before it receives normal user traffic.

The container must satisfy the runtime contract, including listening correctly for service requests. Keep durable data outside disposable instances. Runtime service-account permissions, build permissions, and deployment permissions are separate concerns. A successful build does not prove the deployed revision can access its dependencies.

## Practical workflow

Deploy a new revision with no traffic when appropriate, inspect startup logs and readiness, test the tagged revision URL with suitable authentication, then move traffic gradually. Roll back traffic to a known-good revision if needed.

## Exam trap

A revision is not edited in place. Moving traffic back does not automatically reverse a database schema migration performed by the application.

## Check your understanding

**Scenario:** A new release has errors immediately after traffic switches. What is a fast application rollback option?

<details>
<summary>Answer and reasoning</summary>

Redirect traffic to the previous healthy revision, provided its dependency/schema compatibility remains valid. Rebuilding the old code is often slower and unnecessary.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/run/docs/deploying)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
