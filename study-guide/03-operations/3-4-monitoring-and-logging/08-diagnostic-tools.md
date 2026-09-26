# 3.4.08 — Trace, Profiler, Query Insights, and index advice

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud Trace helps analyze request latency across instrumented components using traces and spans. Cloud Profiler identifies where application code consumes resources such as CPU or memory in supported environments. Query Insights helps investigate database query performance; index advice can suggest database indexing changes where supported.

Choose the tool that matches the level of the problem. High request latency can come from a slow dependency, CPU-heavy code, an unindexed query, or network behavior. Metrics show a symptom; targeted diagnostic tools help explain it. Suggestions require validation against write overhead and workload behavior.

## Practical workflow

Identify the slow request or time window, inspect trace spans to locate delay, use profiling for code hotspots, and examine query execution/index information for database bottlenecks. Compare performance after one controlled change.

## Exam trap

An index can improve reads while adding write/storage cost. Increasing CPU everywhere may not help a single slow downstream query.

## Check your understanding

**Scenario:** An API is slow and you need to see which downstream call consumed most of the request time. Which tool fits?

<details>
<summary>Answer and reasoning</summary>

Cloud Trace, assuming instrumentation is available. Profiler answers a different question about code resource consumption.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/trace/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
