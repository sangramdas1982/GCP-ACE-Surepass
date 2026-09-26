# 3.1.15 — Workbench and BigQuery notebooks

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A notebook combines executable code, outputs, and narrative for exploration. Workbench provides managed notebook environments with underlying compute, identity, storage, and networking choices. BigQuery notebooks integrate analysis with warehouse-oriented workflows. The syllabus uses updated Workbench branding; older material may call it Vertex AI Workbench.

Notebook access and data access are separate. A person able to open an environment may still lack dataset permission, while an overprivileged runtime identity may expose more data than intended. Idle compute and accelerators can continue to cost money depending on lifecycle settings.

## Practical workflow

Choose the notebook environment, runtime identity and region, grant only required data access, verify package/runtime configuration, and enable supported idle management. Save/version important notebooks and verify what storage persists when compute is stopped or deleted.

## Exam trap

A saved notebook is not necessarily a backup of all local datasets or credentials. Do not embed private keys in notebook cells.

## Check your understanding

**Scenario:** A notebook opens but a BigQuery query is denied. Does restarting the notebook necessarily fix it?

<details>
<summary>Answer and reasoning</summary>

No. Inspect the identity executing the query, job-creation permission, and dataset/table permissions. A compute restart does not create authorization.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/vertex-ai/docs/workbench/introduction)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
