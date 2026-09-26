# 3.1.04 — Inspecting GKE nodes, Pods, and Services

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A cluster contains nodes that run Pods; controllers reconcile desired workloads; Services expose selected Pods. Investigate from the correct context and namespace. Node readiness, Pod readiness, scheduling status, restart counts, and Service endpoints tell different parts of the story.

A Pending Pod may lack schedulable resources or storage. ImagePullBackOff suggests image retrieval issues. CrashLoopBackOff suggests repeated container startup/runtime failure. These are clues, not final diagnoses; read events and logs before changing configuration.

## Practical workflow

Use `kubectl get nodes`, `kubectl get pods -A`, and `kubectl get services -A`. Then use `kubectl describe pod POD_NAME -n NAMESPACE` and `kubectl logs POD_NAME -n NAMESPACE --previous` when examining a prior crashed container.

## Exam trap

Listing only the default namespace can hide the workload. Deleting Pods may temporarily mask a failure without fixing its cause.

## Check your understanding

**Scenario:** A Pod is Pending and events report insufficient memory. Should you first reset its database password?

<details>
<summary>Answer and reasoning</summary>

No. Investigate resource requests and available node capacity/scheduling constraints. The error occurs before normal application execution.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/troubleshooting/deployed-workloads)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
