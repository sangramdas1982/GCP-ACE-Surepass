# 3.1.07 — Pods, Deployments, Services, and StatefulSets

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A Pod is the basic scheduled workload unit. A Deployment manages interchangeable replicas and rolling updates. A StatefulSet supplies stable identity and ordered behavior useful for certain stateful workloads, often paired with persistent volume claims. A Service provides stable discovery/traffic distribution to matching Pods.

ConfigMaps store non-secret configuration; Secrets store sensitive configuration objects but still require proper access and encryption practices. PersistentVolumeClaims request storage through a storage class or matching volume. These objects cooperate rather than replace one another.

## Practical workflow

Choose the controller based on workload identity/state requirements, declare resources in manifests, apply them, inspect rollout status, and confirm Service label selectors and storage binding. Use namespaces for organization and policy scope, not as an automatic complete security boundary.

## Exam trap

A StatefulSet does not automatically configure database replication or application backups. A bare Pod has no Deployment controller maintaining replicas.

## Check your understanding

**Scenario:** A clustered application requires stable per-replica identities and persistent volumes. Which controller is a better starting point?

<details>
<summary>Answer and reasoning</summary>

StatefulSet. A Deployment is designed for more interchangeable replicas and does not offer the same stable identity behavior.

</details>

## Official reference

- [Product documentation](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
