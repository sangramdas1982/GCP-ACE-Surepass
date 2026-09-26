# 3.2.06 — Monitoring Dataflow and BigQuery jobs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A submitted job can be queued, running, completed, or failed; completion must be checked for errors and expected results. BigQuery jobs cover queries and data operations such as loads and exports. Inspect job metadata, error details, execution statistics, and location.

Dataflow batch jobs finish, whereas streaming jobs are intended to keep processing. A running streaming job can still be unhealthy if backlog, system lag, watermark progress, or worker failures worsen. For supported streaming updates, drain processes in-flight work differently from cancel, which stops more abruptly.

## Practical workflow

Identify job ID, project, region, and type. Inspect console job details or `bq show -j --location=LOCATION JOB_ID`; inspect Dataflow job graphs, logs, throughput, lag, and errors. Validate output counts or representative records.

## Exam trap

A running streaming pipeline is not necessarily keeping up. Retrying a load without understanding write disposition or idempotency can duplicate or overwrite data.

## Check your understanding

**Scenario:** A streaming job is RUNNING, but output is hours behind input. Is the status sufficient evidence of health?

<details>
<summary>Answer and reasoning</summary>

No. Investigate backlog, processing lag, worker capacity, hot keys, and sink performance. Lifecycle state alone does not measure timeliness.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/dataflow/docs/guides/using-monitoring-intf)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
