# 2.2.03 — Cloud Storage classes and access costs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Storage class describes an object's storage/access cost profile, not whether it can be retrieved only after an offline delay. Standard suits frequent access. Nearline, Coldline, and Archive trade lower storage pricing for retrieval charges and minimum storage durations. The typical minimum durations are 30, 90, and 365 days respectively; early deletion can incur charges.

Bucket location is a separate choice from class: regional, dual-region, and multi-region placement address geography and redundancy. Lifecycle rules can change object class or delete eligible objects. Autoclass can manage class transitions according to its supported behavior and fee model.

## Practical workflow

Estimate object size, retention time, access frequency, retrieval volume, operations, and data transfer. Choose a location and class separately. Use a scenario spreadsheet or handwritten estimate rather than selecting solely by per-GB storage price.

## Exam trap

Archive is still online object storage; it is not tape requiring hours to recall. Frequently reading Archive data may erase apparent storage savings.

## Check your understanding

**Scenario:** A backup is retained for years and almost never restored. Which class deserves consideration?

<details>
<summary>Answer and reasoning</summary>

Archive, provided retrieval and early-deletion economics fit. Standard can cost more for long idle retention, while a short-lived object may be unsuitable for Archive.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/storage/docs/storage-classes)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
