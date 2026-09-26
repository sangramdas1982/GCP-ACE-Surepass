# 3.4.04 — Routing logs to BigQuery, Cloud Storage, Pub/Sub, and external systems

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A log sink selects matching entries and routes them to a supported destination. BigQuery supports analytical querying, Cloud Storage suits archival objects, and Pub/Sub can feed streaming consumers or external systems. Export to an on-premises tool commonly uses a supported integration or subscriber rather than inventing an arbitrary sink URL.

The sink writer identity needs permission at the destination. Filters and scope determine which logs are routed. New routing configuration generally processes newly received matching entries; do not assume creating a sink automatically backfills all existing history.

## Practical workflow

Choose destination based on analysis/archive/streaming needs, create the sink and filter, grant its writer identity access, generate a test log, and confirm destination delivery. For organization-wide collection, evaluate supported aggregated sink behavior.

## Exam trap

A sink existing in the console is not proof of successful delivery. Destination IAM or an overly narrow filter can prevent useful output.

## Check your understanding

**Scenario:** A company wants a third-party event processor to consume new security logs continuously. Which destination commonly fits?

<details>
<summary>Answer and reasoning</summary>

Pub/Sub, with a secured consuming integration. An archive bucket alone does not provide the same event-consumption workflow.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/logging/docs/export/configure_export_v2)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
