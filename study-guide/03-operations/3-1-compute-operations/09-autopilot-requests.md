# 3.1.09 — Autopilot Pod resource requests

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Resource requests describe the CPU, memory, and other resources a Pod asks the scheduler to reserve. Limits constrain supported runtime usage. In Autopilot, requests influence scheduling and can influence billing according to the selected compute model; the platform can apply defaults and enforce supported ranges/ratios.

Requests that are too low can produce poor performance or placement assumptions; overly large requests waste resources. Memory exhaustion and CPU throttling have different symptoms. Check the admitted Pod configuration because platform defaults or adjustments can differ from the original manifest.

## Practical workflow

Inspect requested and actual usage, review the Autopilot compute class and permitted resource shapes, set explicit suitable requests, and examine the resulting Pod spec. Load-test the service before treating low idle usage as a sizing target.

## Exam trap

Autopilot manages infrastructure but cannot infer every application performance requirement. Do not assume all workloads use identical billing or minimum-resource rules.

## Check your understanding

**Scenario:** A workload is slow after migration and its resource configuration was left implicit. What should you inspect?

<details>
<summary>Answer and reasoning</summary>

The admitted CPU/memory requests and limits, actual utilization, and throttling/OOM evidence. Adding arbitrary replicas may not fix a per-Pod resource constraint.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/autopilot-resource-requests)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
