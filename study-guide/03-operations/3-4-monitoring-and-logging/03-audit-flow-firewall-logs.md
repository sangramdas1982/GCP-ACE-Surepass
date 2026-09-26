# 3.4.03 — Audit logs, VPC Flow Logs, and firewall logs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud Audit Logs answer questions about administrative/data-access actions: who did what, to which resource, and when. Admin Activity and Data Access logs have different purposes and default/enablement behavior; many Data Access categories require deliberate configuration, while some services differ.

VPC Flow Logs describe sampled/aggregated network flows for supported resources and configuration. Firewall Rules Logging records supported matching rule events. These network telemetry sources are not interchangeable with API audit logs, despite being grouped together in the syllabus. Flow sampling means absence of a record is not conclusive proof that no packet existed.

## Practical workflow

Choose telemetry based on the question: resource change, data access, traffic flow, or firewall decision. Configure the relevant source, scope, retention and reader roles, then perform a known action to validate records.

## Exam trap

Admin Activity logs are not a complete record of every object read. VPC Flow Logs do not show the application payload or replace firewall rule evaluation.

## Check your understanding

**Scenario:** You need to identify who deleted a VM. Should you start with VPC Flow Logs?

<details>
<summary>Answer and reasoning</summary>

No. Inspect relevant Cloud Audit Logs. Flow logs describe network communication, not the IAM principal responsible for a resource-management API call.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/logging/docs/audit)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
