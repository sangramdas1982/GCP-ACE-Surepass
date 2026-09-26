# 3.1.13 — Attaching and operating GPUs and TPUs

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Accelerator operation requires a compatible host/service configuration, location, quota, software stack, and workload placement. GPUs may require drivers and compatible libraries. TPUs require a supported runtime/framework. Reserving hardware without configuring the application to use it produces cost without useful acceleration.

Maintenance, scheduling, availability, and machine-shape restrictions vary by accelerator. In Kubernetes, the workload must request the supported accelerator resource and match eligible capacity. Observe accelerator utilization together with input throughput and host CPU/memory.

## Practical workflow

Check compatibility and quota, provision supported accelerator capacity, install or select the required runtime, run a small validation workload, and inspect utilization. Shut down or delete lab capacity promptly after recording results.

## Exam trap

An accelerator cannot necessarily be hot-attached to any running VM. Do not assume the host OS alone makes a machine-learning framework use the device.

## Check your understanding

**Scenario:** A GPU VM is expensive but accelerator utilization stays near zero. What should you investigate?

<details>
<summary>Answer and reasoning</summary>

Framework/device configuration and data-input bottlenecks before buying more GPUs. The application may still run on CPU or wait for data.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/gpus/create-vm-with-gpus)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
