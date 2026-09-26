# 2.2.13 — Dataflow: batch and streaming pipelines

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Dataflow executes Apache Beam pipelines for batch and streaming data processing. A pipeline reads from sources, applies transformations, and writes to sinks. Windows, event time, processing time, and late data matter in streaming analytics because events can arrive out of order.

Managed execution and autoscaling reduce infrastructure work, but the pipeline still needs correct transforms, permissions, worker networking, staging/temp locations, and sink capacity. A hot key can concentrate processing and limit effective scaling. Exactly-once processing claims must be interpreted with connector and external side-effect behavior.

## Practical workflow

Use a suitable template or develop a Beam pipeline, configure worker service account and network, run a small sample, verify output, then monitor throughput/lag and job logs. Use supported update/drain workflows for streaming operations.

## Exam trap

Dataflow transforms data; Pub/Sub transports messages. Adding workers does not necessarily fix a serial hot-key bottleneck or a saturated destination database.

## Check your understanding

**Scenario:** A streaming pipeline must enrich events and write aggregates. Which service handles the processing layer?

<details>
<summary>Answer and reasoning</summary>

Dataflow. Pub/Sub can supply events and BigQuery can store analytics, but neither alone describes the required Beam processing pipeline.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/dataflow/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
