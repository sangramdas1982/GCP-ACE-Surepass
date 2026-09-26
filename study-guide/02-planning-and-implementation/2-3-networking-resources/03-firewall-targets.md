# 2.3.03 — Network tags, secure tags, and service accounts

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Network tags are VM metadata labels used by supported VPC firewall rules and routes. Secure tags are Resource Manager tags with IAM-governed attachment and firewall-specific support. These are different mechanisms with different administration and policy capabilities. Ordinary resource labels used for cost/inventory are not automatically firewall selectors.

Service accounts can identify firewall targets or sources where supported. This links network policy to workload identity, but users who can attach that identity may affect which resources match. Always consider who can change tags or assign service accounts, not merely the rule text.

## Practical workflow

Choose a selector supported by the firewall rule/policy type. Establish who can attach it, apply it to the intended workloads, and inspect effective policy. For service-account targeting, verify the VM actually uses the expected account.

## Exam trap

An IAM role granted to a service account controls API access; a firewall selector controls network traffic. One does not replace the other.

## Check your understanding

**Scenario:** A team needs governed application grouping for centralized firewall policies. Are freely managed network tags interchangeable with secure tags?

<details>
<summary>Answer and reasoning</summary>

No. Secure tags have their own IAM-controlled lifecycle and supported firewall use. Choose based on policy type and governance requirements.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/firewall/docs/tags-firewalls-overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
