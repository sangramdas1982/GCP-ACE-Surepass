# 2.1.07 — Spot VMs and custom machine types

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Spot VMs use spare capacity at lower pricing but can be preempted when capacity is needed elsewhere. They fit fault-tolerant batch workers, checkpointed jobs, and other interruption-tolerant work. They are not a reliable single-instance foundation for an availability-critical application. Current Spot behavior should not be confused with the older preemptible VM 24-hour lifetime rule.

Custom machine types, where supported, let you choose a compatible vCPU/memory combination. They can reduce waste when predefined shapes have the wrong ratio. CPU platform, machine family, permitted ratios, quotas, and pricing still constrain the choice.

## Practical workflow

For Spot, persist checkpoints and outputs externally, design retries to be safe, and test worker interruption. For a custom machine type, measure the workload and choose a supported size rather than guessing from peak averages.

## Exam trap

A low VM price is not necessarily a low completed-job cost if interruptions repeatedly restart expensive work. Spot availability is not guaranteed.

## Check your understanding

**Scenario:** Overnight processing can retry each independent task and saves results externally. Is Spot appropriate?

<details>
<summary>Answer and reasoning</summary>

Yes, it can be a good fit. Make task handling idempotent and allow for interrupted workers and delayed capacity; use a more reliable option if a strict deadline cannot tolerate that uncertainty.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/instances/spot)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
