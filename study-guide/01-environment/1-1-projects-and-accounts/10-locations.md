# 1.1.10 — Regions, zones, and product availability

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A region is a geographic deployment area; zones are failure domains within it. Resource scope may be zonal, regional, multi-regional, or global. A global control-plane resource does not mean all workload data is replicated worldwide. Examine the specific service and configuration.

Choose location using residency requirements, user latency, dependent-service proximity, availability, and cost. A product may exist in a region without offering every machine family, accelerator, tier, or feature there. Cross-zone and cross-region designs also have different failure coverage and network charges.

## Practical workflow

List application dependencies, confirm each product and required feature in the candidate location, then choose a coherent deployment. For a zone-outage requirement, identify both compute and data components that would fail with the zone.

## Exam trap

Regional high availability does not necessarily survive loss of the entire region. A location restriction and a product availability check are separate checks.

## Check your understanding

**Scenario:** An application must survive a regional outage. Is placing two VMs in different zones of one region sufficient?

<details>
<summary>Answer and reasoning</summary>

No. Both zones belong to the same region. The design needs suitable cross-region application and data recovery/availability arrangements.

</details>

## Official reference

- [Product documentation](https://cloud.google.com/about/locations)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
