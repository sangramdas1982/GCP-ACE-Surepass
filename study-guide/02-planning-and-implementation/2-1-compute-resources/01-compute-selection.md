# 2.1.01 — Choosing a compute service

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Compute Engine provides virtual machines and operating-system control. Choose it for custom OS requirements, legacy software, or workloads needing VM-specific capabilities. GKE runs Kubernetes workloads and exposes Kubernetes deployment, networking, and scheduling concepts. Choose it when those capabilities are requirements and the team can support that model.

Cloud Run runs supported container workloads with managed infrastructure: services handle requests, while jobs run tasks to completion. Cloud Run functions provide a function-oriented deployment model, useful for event handlers. Agent Runtime provides managed deployment and operation of agentic applications. Choose based on actual requirements; a container alone does not imply a Kubernetes requirement.

## Practical workflow

Underline workload constraints: OS access, Kubernetes APIs, HTTP/event trigger, batch completion, accelerator compatibility, and management effort. Eliminate services that fail a hard constraint, then compare simplicity, cost, and availability.

## Exam trap

“Serverless” means infrastructure management is abstracted, not that execution has no limits, cost, or security configuration.

## Check your understanding

**Scenario:** A small team has a stateless HTTP container and no Kubernetes requirement. Which is usually the simplest managed starting point?

<details>
<summary>Answer and reasoning</summary>

Cloud Run. GKE could run it but adds Kubernetes concerns, while a VM requires more server administration. Reconsider if another stated constraint rules out Cloud Run.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/run/docs/overview/what-is-cloud-run)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
