# 2.2.12 — Pub/Sub: topics, subscriptions, acknowledgments

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Publishers send messages to a topic; consumers receive them through subscriptions. Multiple subscriptions can each receive their own stream from the same topic. Multiple subscribers on one subscription typically share processing of that subscription’s messages.

Acknowledgment tells the service that processing is complete. Failure to acknowledge in the relevant period can cause redelivery. Design handlers to tolerate duplicates and use supported retry/dead-letter behavior for poison messages. Ordering and exactly-once features have specific configuration and scope; they are not universal assumptions.

## Practical workflow

Create a topic/subscription, grant publisher/subscriber permissions separately, publish a small message, process it, and acknowledge after successful work. Monitor backlog and oldest-unacknowledged-message age.

## Exam trap

A topic alone does not define every consumer’s durable processing state. Acknowledging before persisting results can lose the business effect if the worker fails afterward.

## Check your understanding

**Scenario:** Two independent systems must each process every event. One subscription or two?

<details>
<summary>Answer and reasoning</summary>

Two subscriptions, one per independent consumption flow. Two workers sharing one subscription generally divide its work rather than each receiving every message.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/pubsub/docs/pubsub-basics)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
