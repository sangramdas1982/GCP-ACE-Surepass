# 2.1.09 — GKE Standard, Autopilot, regional, and private clusters

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

GKE manages Kubernetes control-plane infrastructure. In Standard, you configure and operate node pools with more control over machines and nodes. In Autopilot, Google manages more node infrastructure and applies workload constraints/defaults; you still manage application correctness, IAM, networking choices, and resource requirements.

Regional control planes improve resilience across zones compared with a single-zone control-plane arrangement. Private-node and control-plane endpoint settings address network exposure, not whether a cluster is regional or Autopilot. Treat management mode, location, and network isolation as separate design axes.

## Practical workflow

Choose the management mode, region, workload IP ranges, endpoint access method, and workload identity design. Confirm the required application features are supported in that mode. After creation, test both administrator access and a workload deployment.

## Exam trap

Autopilot does not mean unlimited resources or no configuration. A private cluster still needs deliberate API access, image access, and outbound connectivity.

## Check your understanding

**Scenario:** A team needs Kubernetes but wants to minimize node administration. Which mode is the starting choice?

<details>
<summary>Answer and reasoning</summary>

GKE Autopilot, provided its capabilities fit the workload. Choose Standard when a concrete node-level requirement makes that additional control necessary.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/autopilot-overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
