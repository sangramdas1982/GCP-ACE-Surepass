# 3.3.01 — Expanding a subnet IPv4 range

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An IPv4 CIDR prefix describes an address range. A smaller prefix number means a larger range: a /23 contains more addresses than a /24. Google reserves some addresses in primary subnet ranges, so not every numerical address is available to a VM.

Supported primary range expansion adds address space while preserving existing assigned addresses. It must avoid prohibited overlap and respect product constraints. Shrinking a primary range is not simply the reverse operation. GKE secondary ranges and Pod/service addressing have additional rules and should be analyzed separately.

## Practical workflow

Inspect current allocation, required growth, neighboring ranges and connected networks. Choose a containing larger range, confirm no overlap, then use the supported subnet expansion operation. Update external allowlists/routes only where needed by the broader design.

## Exam trap

Changing /24 to /25 makes a range smaller. Do not promise that every primary/secondary range can be resized identically.

## Check your understanding

**Scenario:** A /24 subnet is exhausted and a nonoverlapping containing /23 is available. What action should you evaluate?

<details>
<summary>Answer and reasoning</summary>

Expand the primary subnet range to the supported /23. Creating individual firewall rules does not add assignable IP addresses.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/vpc/docs/using-vpc)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
