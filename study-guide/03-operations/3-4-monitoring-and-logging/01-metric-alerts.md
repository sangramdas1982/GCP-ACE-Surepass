# 3.4.01 — Metric-based alerts and notification channels

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An alerting policy evaluates conditions on time-series data. Define the metric, resource filter, aggregation/alignment, threshold, duration, and notification channels. A sustained condition can distinguish a real issue from a brief harmless spike. Missing data requires an explicit interpretation where supported.

Select symptoms that matter: error rate, latency, saturation, or availability. CPU alone may not reflect user experience. An incident and a notification are related but distinct; channel configuration and verification determine whether a human receives the message.

## Practical workflow

Choose a measurable failure condition, create the policy, configure verified recipients, generate a controlled breach, and confirm incident creation and notification. Document the response action so the alert is actionable.

## Exam trap

A chart is not an alert policy. A policy with no usable notification channel may detect a problem without reaching the on-call engineer.

## Check your understanding

**Scenario:** The team wants notification when CPU stays high for several minutes, but not for a one-sample spike. What should it configure?

<details>
<summary>Answer and reasoning</summary>

A suitable threshold with a retest/duration window and verified notification channel. A dashboard screenshot does not provide ongoing evaluation.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/monitoring/alerts)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
