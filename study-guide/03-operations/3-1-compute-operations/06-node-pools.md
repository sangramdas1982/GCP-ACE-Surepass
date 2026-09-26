# 3.1.06 — Managing GKE node pools

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A node pool groups nodes with a common configuration such as machine type and other supported settings. Standard clusters can use separate pools for different workload needs. Labels, taints, tolerations, and affinity influence placement, but each mechanism has a different purpose: tolerating a taint permits placement, not necessarily forces it.

Cluster autoscaling adjusts node capacity when workloads cannot be scheduled or capacity is unnecessary, within configured constraints. Node updates/removal should respect workload disruption and available capacity. Autopilot abstracts much of this node-pool administration.

## Practical workflow

Inspect pools and their size, machine configuration, autoscaling limits, and workload placement. When moving workloads, add compatible capacity, verify scheduling, and drain/remove old capacity through supported operations. Check Pod disruption budgets and storage dependencies.

## Exam trap

Increasing a Pod replica count does not itself guarantee room on existing nodes. Conversely, more nodes do not automatically increase application replicas.

## Check your understanding

**Scenario:** Pods cannot schedule because all nodes are full. Will changing only an HPA replica maximum necessarily help?

<details>
<summary>Answer and reasoning</summary>

No. The cluster needs schedulable capacity, potentially through node autoscaling or pool changes. Additional requested Pods can remain Pending.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/node-pools)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
