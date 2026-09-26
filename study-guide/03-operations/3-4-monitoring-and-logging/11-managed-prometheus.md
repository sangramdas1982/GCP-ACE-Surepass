# 3.4.11 — Managed Service for Prometheus

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Prometheus uses a metric model and query language widely adopted in Kubernetes environments. Google Cloud Managed Service for Prometheus supports managed metric storage/querying and supported collection approaches. Managed collection can scrape configured workloads without operating an entire self-managed Prometheus storage stack.

Applications still need to expose metrics, and collection resources must select the correct endpoints and labels. High-cardinality labels and excessive scrape volume remain operational and cost concerns. Prometheus metrics complement logs rather than replace them.

## Practical workflow

Enable the supported collection mode, expose a metrics endpoint, configure collection such as PodMonitoring where applicable, verify target discovery/scrapes, and query the resulting series. Add alerts for meaningful service symptoms.

## Exam trap

A deployed collector does not automatically discover every custom endpoint. A dashboard with an incorrect label filter can look empty despite successful ingestion.

## Check your understanding

**Scenario:** A GKE application already exposes Prometheus metrics and the team wants managed collection/storage. Which service fits?

<details>
<summary>Answer and reasoning</summary>

Managed Service for Prometheus. Cloud Audit Logs capture API actions, not the application’s Prometheus metric series.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/stackdriver/docs/managed-prometheus)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
