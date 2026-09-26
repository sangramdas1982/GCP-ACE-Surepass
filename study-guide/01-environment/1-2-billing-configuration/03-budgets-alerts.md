# 1.2.03 — Budgets and alerts

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A budget compares selected spending against a target over a period. You can scope it to relevant projects/services and set actual-spend or forecast thresholds. Notification channels help responsible people react. A budget is an observation and alerting mechanism, not a hard limit that guarantees resource shutdown.

Billing data and alerts have processing delay. Programmatic notifications can trigger automation, but that automation must be designed explicitly and can disrupt workloads. For exam questions, separate a request to be notified from a request to enforce a resource limit or prevent particular deployments.

## Practical workflow

Choose a meaningful budget scope, amount, period, thresholds, and recipients. Verify who receives notifications. If automation is requested, connect the supported notification mechanism to a reviewed response workflow instead of assuming a checkbox caps costs.

## Exam trap

Crossing 100% of a budget does not automatically stop charges. Quotas are resource controls, not a universal currency-based spending limit.

## Check your understanding

**Scenario:** A team sets a $100 budget and spends $110. Why are its VMs still running?

<details>
<summary>Answer and reasoning</summary>

The budget produces alerts; it does not automatically stop resources. Any shutdown response would require separate automation and may occur after further costs accrue.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/billing/docs/how-to/budgets)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
