# 2.3.06 — Premium and Standard Network Service Tiers

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Network Service Tiers affect how supported external traffic uses Google's network versus public internet paths. Premium generally uses Google's backbone for a greater part of the path. Standard is a lower-cost option for supported regional use cases, with more internet transit.

Tier availability depends on the resource and load-balancer mode. Global external load-balancing capabilities can require Premium. Select using performance, geographic reach, feature compatibility, and price rather than assuming both tiers are interchangeable on every product.

## Practical workflow

Identify the public IP/load balancer and required scope, verify supported tiers, compare latency expectations and charges, then configure compatible frontend resources. Review existing IP tier when troubleshooting configuration mismatches.

## Exam trap

A cheaper tier that cannot support the required global design is not a valid exam answer. Service tier is separate from VM machine family or storage class.

## Check your understanding

**Scenario:** A design explicitly requires a supported global external load-balancer mode that uses Premium. Can you switch only its frontend to Standard for savings?

<details>
<summary>Answer and reasoning</summary>

No. The design must meet the product compatibility rules. Cost optimization cannot violate a stated functional requirement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/network-tiers/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
