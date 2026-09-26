# 3.4.06 — Viewing and filtering logs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Logs Explorer searches structured log entries. Narrow queries by time, resource type, severity, log name, and structured payload fields. A VM application error and a Cloud Run request log have different resource fields and payload structures.

Start with a small known time window and broaden gradually. A wrong project, log scope, resource filter, timezone, or retention period can make valid events appear missing. A severity filter only works as expected when producers set severity correctly.

## Practical workflow

Try a filter such as `resource.type="gce_instance"` with `severity>=ERROR`, then add the specific instance identifier and time window. For JSON logs, filter relevant `jsonPayload` fields. Remove filters one at a time when expected entries do not appear.

## Exam trap

Searching only free text can miss structured fields. No results is not immediate proof that no failure occurred.

## Check your understanding

**Scenario:** An engineer cannot find an error that occurred yesterday, but the query is restricted to the last hour. What is the first fix?

<details>
<summary>Answer and reasoning</summary>

Correct the time range and project/resource scope. Reinstalling the logging agent before checking the query would be premature.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/logging/docs/view/logs-explorer-interface)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
