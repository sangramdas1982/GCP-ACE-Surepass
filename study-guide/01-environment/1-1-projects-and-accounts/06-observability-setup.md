# 1.1.06 — Initial observability setup

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Establish telemetry before an incident. Cloud Monitoring stores and evaluates metrics, and Cloud Logging collects searchable events. Many managed products emit platform telemetry automatically, but guest operating-system and application details can require agents or instrumentation.

Monitoring can view multiple projects through a metrics scope. This changes visibility, not resource ownership or billing linkage. Logging has its own storage, routing, retention, and access model. Decide which team should view application logs and which people should receive alerts; broad project access is not a prerequisite for every observer.

## Practical workflow

Confirm the monitored projects, relevant APIs, telemetry-writing identities, dashboard access, log retention, and notification channels. Generate a known test event and verify that it appears. On a VM, determine whether guest memory or application logs require Ops Agent.

## Exam trap

A dashboard with no errors does not prove every component is instrumented. Missing guest metrics should not immediately be interpreted as zero usage.

## Check your understanding

**Scenario:** VM CPU metrics appear, but memory usage is absent. Is Cloud Monitoring necessarily broken?

<details>
<summary>Answer and reasoning</summary>

No. Platform CPU metrics and guest memory metrics have different collection paths. Verify a supported guest agent and its permissions/configuration.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/monitoring/docs/monitoring-overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
