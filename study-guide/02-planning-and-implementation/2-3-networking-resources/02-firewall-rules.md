# 2.3.02 — VPC firewall rules and Cloud NGFW policies

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A firewall rule evaluates traffic using direction, action, priority, source/destination criteria, target, and protocol/port. Ingress is traffic entering a target workload; egress is traffic leaving it. VPC firewalls are stateful, so permitted connection response traffic is tracked. Lower numeric priority values represent higher priority within a rule set.

VPC firewall rules and hierarchical/network firewall policies have distinct attachment and evaluation models. Policies can centralize controls across broader scopes, and rules can delegate evaluation where supported. The familiar implied deny ingress and allow egress behavior must be understood alongside effective explicit policies.

## Practical workflow

Write the traffic tuple: source → destination, protocol, port. Inspect the effective rules, matching targets, and priority. Permit only the necessary source and port, enable logging where useful, and test from the actual source.

## Exam trap

A rule allowing TCP 443 does not start an HTTPS server. Do not apply a universal “deny always wins” rule without checking policy evaluation and equal-priority behavior.

## Check your understanding

**Scenario:** A VM has the correct IP and route, but TCP 8080 times out while SSH works. What should you inspect?

<details>
<summary>Answer and reasoning</summary>

The rule permitting 8080, its source range and target, and the application listener. Working SSH only verifies a different permitted traffic flow.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/firewall/docs/firewalls)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
