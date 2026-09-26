# 3.4.05 — Log buckets, retention, views, and Log Analytics

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud Logging log buckets store log entries with configured location and retention. They are not Cloud Storage buckets. The Log Router evaluates routing rules, while log views can restrict which stored entries authorized readers see. Log Analytics supports SQL-oriented analysis for appropriately configured log buckets.

Understand required/default routing behavior before changing exclusions or retention. Excluding logs from one storage destination does not necessarily prevent every other sink or downstream system from receiving them. Longer retention and high ingestion volume can affect cost.

## Practical workflow

Identify which logs need which retention/location, configure appropriate buckets and sinks, restrict reader views, and validate where a test entry lands. Enable supported analytics features when the query need justifies them.

## Exam trap

Deleting or excluding useful telemetry to reduce cost can remove future investigation evidence. A Logging bucket is not addressed with `gs://` like Cloud Storage.

## Check your understanding

**Scenario:** A team needs different retention for application logs and a restricted view for one support group. Which features are relevant?

<details>
<summary>Answer and reasoning</summary>

Log buckets with suitable retention and log views/access control. A single broad project role does not express that narrower visibility requirement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/logging/docs/buckets)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
