# 1.2 — Billing configuration

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Billing accounts pay for project consumption. Financial visibility, permission to link a project, and permission to operate its resources are separate responsibilities.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Creating and administering billing accounts](01-billing-accounts.md)
- [ ] [02. Linking projects to billing accounts](02-link-billing.md)
- [ ] [03. Budgets and alerts](03-budgets-alerts.md)
- [ ] [04. Billing exports and cost analysis](04-billing-export.md)

## Worked study exercise: Design cost visibility

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Inspect accessible billing accounts and the training project's billing link.
2. In the billing budget workflow, select only the lab project, a small meaningful amount, and actual/forecast thresholds. Save only if you control that billing scope.
3. Identify recipients and explain why an alert is not a spending cap.
4. Walk through the export-to-BigQuery configuration. You can stop before creation if avoiding costs or lacking billing administration permissions.
5. Sketch a report grouped by project, service, and month, including credits.

**Success evidence:** identify which roles belong on the project versus the billing account and explain the difference between reporting, alerting, and enforcement.

**Cleanup:** remove test-only budgets/channels if created. A BigQuery dataset or export can retain data and incur storage/query charges; review it explicitly.

## How to answer this subsection's scenarios

1. Identify the concrete objective and every hard constraint.
2. State the resource and identity involved; separate configuration, permission, and connectivity failures.
3. Eliminate options that violate a requirement before comparing cost or convenience.
4. Prefer the simplest supported solution that satisfies all stated requirements; a managed service is not automatically correct if it lacks a required capability.
5. Explain why the most tempting alternative fails. Record uncertain answers in the [mistake log](../../MISTAKE-LOG.md).

## Completion check

- [ ] I can explain every linked topic in my own words.
- [ ] I can complete the exercise or explain its expected observations.
- [ ] I can distinguish the services/controls commonly confused here.
- [ ] I can answer the topic scenarios without relying on remembered wording.
