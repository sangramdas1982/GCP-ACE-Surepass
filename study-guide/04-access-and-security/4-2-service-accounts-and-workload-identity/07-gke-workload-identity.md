# 4.2.07 — Google Cloud identities for GKE applications

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A Kubernetes ServiceAccount identifies a workload within Kubernetes; an IAM service account is a Google Cloud identity. Workload Identity Federation for GKE lets workloads access Google Cloud through supported federated identity mechanisms without distributing JSON keys. Depending on service compatibility/design, grant access directly to the workload principal or use the supported IAM service-account impersonation linkage.

Use a distinct Kubernetes ServiceAccount for each workload responsibility. This avoids granting every Pod the broad permissions of a shared node identity. Image-pull permissions remain a separate node/repository concern.

## Practical workflow

Confirm the cluster’s supported federation configuration, assign a Kubernetes ServiceAccount to the Pod, grant the appropriate Google Cloud resource access or supported linkage, and test from the running workload. Inspect which identity the application actually uses.

## Exam trap

A Kubernetes ServiceAccount and IAM service account sharing a name are not automatically linked. A JSON key in a Kubernetes Secret is not the preferred federation design.

## Check your understanding

**Scenario:** Two applications in one cluster need different bucket permissions. Should both inherit a broad node account?

<details>
<summary>Answer and reasoning</summary>

Use distinct workload identities and grant each only its required bucket access. Node-level shared credentials unnecessarily couple their privileges.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/workload-identity)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
