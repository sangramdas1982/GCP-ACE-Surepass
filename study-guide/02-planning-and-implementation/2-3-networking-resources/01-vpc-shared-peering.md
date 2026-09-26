# 2.3.01 — Custom VPC, Shared VPC, and VPC Peering

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A VPC is global; its subnets are regional. Custom mode lets you select subnet CIDRs explicitly. Shared VPC places centrally administered networking in a host project, with authorized service projects deploying resources that use shared subnets. This separates network administration from application administration.

VPC Network Peering connects separate VPCs through private connectivity and supported route exchange. It does not merge IAM policies, eliminate firewall checks, or provide arbitrary transitive routing. If A peers with B and B peers with C, A does not automatically reach C through B. Overlapping subnet ranges can prevent peering arrangements.

## Practical workflow

Use Shared VPC for centralized network ownership across projects; use peering when separate VPCs need suitable private connectivity. Plan IP ranges first and verify required subnet-use permissions or both peering sides as appropriate.

## Exam trap

Shared VPC is not the same as VPC Peering. Neither option automatically makes all application ports reachable.

## Check your understanding

**Scenario:** A network team must own subnets while application teams own VMs in separate projects. Which model fits?

<details>
<summary>Answer and reasoning</summary>

Shared VPC. Peering leaves separate networks in place and does not provide the same host-project subnet-sharing administration model.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/vpc/docs/shared-vpc)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
