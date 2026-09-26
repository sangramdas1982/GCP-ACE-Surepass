# 2.1.11 — Cloud Run functions, Pub/Sub, and Eventarc

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Event-driven processing separates an event producer from its handler. Pub/Sub distributes messages through topics and subscriptions. Eventarc routes supported events, such as object changes, to destinations such as Cloud Run. Cloud Run functions provide a function-focused handler model; a Cloud Run service can also handle events in a container.

Plan for retries and duplicate deliveries unless the exact feature/configuration guarantees otherwise. Idempotency means processing the same event twice does not incorrectly duplicate its business effect. The trigger identity, runtime identity, and resource permissions may be different.

## Practical workflow

Choose the event source and filter, deploy the handler, configure Eventarc or the appropriate subscription, grant invocation/access permissions, and generate a test event. Log an event identifier and observe retry behavior.

## Exam trap

Writing output into the same watched bucket/prefix can create an event loop. Deploying code alone does not create every required trigger or grant permission to invoke it.

## Check your understanding

**Scenario:** Each uploaded image must produce one thumbnail even if delivery retries occur. What should the design include?

<details>
<summary>Answer and reasoning</summary>

An object-change event handler with idempotent output logic, such as a deterministic destination and duplicate handling. Assuming every event arrives exactly once risks repeated side effects.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/eventarc/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
