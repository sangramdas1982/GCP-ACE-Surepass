# 2.2.04 — Loading and transferring data

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Use direct uploads for small or straightforward object transfers, such as `gcloud storage cp`. Storage Transfer Service manages larger or recurring transfers from supported sources and can provide scheduling and operational visibility. Choose the tool based on source, scale, transfer window, connectivity, and recurrence.

Loading data into a database or warehouse is a separate operation from copying a file into a bucket. For example, a BigQuery load job needs a destination table, supported format/schema, permissions, and compatible data locations. Large offline migration requirements may call for specialized transfer options beyond a simple CLI copy.

## Practical workflow

Inventory source data, choose destination location, estimate transfer duration and charges, grant source/destination permissions, transfer a sample, validate counts/checksums as supported, then run the full transfer. Inspect job status and individual errors.

## Exam trap

A file in Cloud Storage is not automatically a native BigQuery table. A successful job submission is not proof that every record loaded correctly.

## Check your understanding

**Scenario:** An organization needs a nightly managed transfer from a supported external object store. Is a manually run laptop copy the best fit?

<details>
<summary>Answer and reasoning</summary>

Storage Transfer Service is more suitable for scheduled managed transfer. A laptop command creates an unnecessary operational dependency.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/storage-transfer/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
