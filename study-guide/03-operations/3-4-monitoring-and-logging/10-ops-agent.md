# 3.4.10 — Installing and configuring Ops Agent

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Ops Agent collects supported guest/application telemetry from Compute Engine VMs. It supplies information not necessarily available from platform metrics alone, including guest memory and application logs through configured receivers/processors/pipelines. Configuration and supported integrations determine what is collected.

Successful installation is only one step. The VM needs appropriate writing permissions and a working network path to telemetry services. Parsing errors, wrong file paths, unsupported OS versions, or stopped agent processes can prevent delivery.

## Practical workflow

Use the official installation method for the OS, inspect the agent service, configure the required log/metric receivers, validate configuration, and confirm a known log/metric reaches Google Cloud. Review agent self-logs when collection fails.

## Exam trap

Ops Agent is not the same as VM Manager’s OS Config agent. One collects telemetry; the other supports guest inventory/configuration/patching.

## Check your understanding

**Scenario:** A VM’s application logs remain only in a local file. What should you configure?

<details>
<summary>Answer and reasoning</summary>

A supported Ops Agent log receiver/pipeline for that file, plus permissions and connectivity. Merely creating a Monitoring dashboard cannot ingest the file.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/logging/docs/agent/ops-agent)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
