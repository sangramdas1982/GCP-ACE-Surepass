# 3.4 — Monitoring and logging

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Metrics quantify behavior, logs record events, traces follow requests, and profiles identify code-level resource consumption. Select evidence based on the question being investigated.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Metric-based alerts and notification channels](01-metric-alerts.md)
- [ ] [02. Application metrics and log-based metrics](02-custom-metrics.md)
- [ ] [03. Audit logs, VPC Flow Logs, and firewall logs](03-audit-flow-firewall-logs.md)
- [ ] [04. Routing logs to BigQuery, Cloud Storage, Pub/Sub, and external systems](04-log-export.md)
- [ ] [05. Log buckets, retention, views, and Log Analytics](05-log-buckets-router.md)
- [ ] [06. Viewing and filtering logs](06-logs-explorer.md)
- [ ] [07. Reading individual log entries and correlating requests](07-log-details.md)
- [ ] [08. Trace, Profiler, Query Insights, and index advice](08-diagnostic-tools.md)
- [ ] [09. Personalized Service Health](09-personalized-service-health.md)
- [ ] [10. Installing and configuring Ops Agent](10-ops-agent.md)
- [ ] [11. Managed Service for Prometheus](11-managed-prometheus.md)
- [ ] [12. Gemini Cloud Assist for monitoring](12-gemini-monitoring.md)
- [ ] [13. Active Assist and resource optimization](13-active-assist.md)
- [ ] [14. Cloud Hub events and application health](14-cloud-hub.md)

## Worked study exercise: Produce and find telemetry

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Generate a harmless test log in your lab project using the logging example in the command reference.
2. Find it in Logs Explorer with log name, severity, and time filters; expand its fields.
3. Inspect a metric chart and design an alert with a sustained threshold and recipient.
4. Create a test sink only if you have an approved destination; identify the sink writer identity and its destination access.
5. Explain which evidence you would use for a deleted VM, slow API, CPU hotspot, failed database query, and suspected provider incident.

**Success evidence:** use the correct telemetry tool and distinguish ingestion, storage, query, alert, and notification failures.

**Cleanup:** remove test sinks/alerts/channels and destinations if created. Keep any records needed for your notes before cleanup.

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
