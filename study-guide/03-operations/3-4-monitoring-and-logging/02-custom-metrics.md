# 3.4.02 — Application metrics and log-based metrics

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Built-in metrics describe platform behavior. Custom/application metrics describe workload-specific facts such as queue age or completed orders. Log-based metrics derive counts or distributions from matching log entries. Metric kind, value type, units, labels, and monitored resource determine how data should be interpreted.

A counter and a gauge are different: cumulative totals often need rates, while current queue depth is directly meaningful as a point-in-time value. Excessively high-cardinality labels, such as a unique user ID per series, can increase cost and make monitoring unwieldy.

## Practical workflow

Choose direct instrumentation or a log-based metric, define a bounded label set, emit a known test value/event, inspect the resulting series, and create a useful chart or alert. Use a distribution for latency when percentiles matter.

## Exam trap

A newly created log-based metric should not be assumed to retroactively compute all historical values. Parsing application logs is not the same as instrumenting every request correctly.

## Check your understanding

**Scenario:** Application logs already identify failed payments, and the team wants an alert on their count. What is a reasonable approach?

<details>
<summary>Answer and reasoning</summary>

A log-based counter metric with a precise filter, then an alert. Check that retries do not inflate the business interpretation.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/logging/docs/logs-based-metrics)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
