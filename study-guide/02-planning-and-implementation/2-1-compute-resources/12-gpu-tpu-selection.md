# 2.1.12 — Choosing GPUs or TPUs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

GPUs accelerate parallel workloads and have broad framework and library support, including CUDA-based software. TPUs are specialized accelerators optimized for supported machine-learning operations and execution stacks. Neither is automatically the best choice for every AI workload.

Check framework compatibility, model size, memory, interconnect requirements, availability, quota, and total cost. An application can spend most of its time waiting on storage or preprocessing; attaching a faster accelerator will not remove that bottleneck. Inference and training also have different latency and throughput requirements.

## Practical workflow

Identify the model/framework, profile the bottleneck, select supported hardware and region, verify quota, and measure end-to-end performance. Include storage throughput and input pipelines in the design.

## Exam trap

A CPU-only web service does not become efficient merely because the project includes a GPU. Accelerator requests also require compatible machine and service configurations.

## Check your understanding

**Scenario:** A workload depends on a CUDA-only library. Should you choose a TPU simply because the application uses machine learning?

<details>
<summary>Answer and reasoning</summary>

No. Choose compatible GPU infrastructure unless the software is deliberately ported and validated. Compatibility is a hard constraint before comparing theoretical performance.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/tpu/docs/intro-to-tpu)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
