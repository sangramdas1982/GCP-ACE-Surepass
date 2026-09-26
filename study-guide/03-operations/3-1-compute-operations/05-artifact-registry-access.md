# 3.1.05 — GKE access to Artifact Registry

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Artifact Registry stores container images. GKE nodes need appropriate access to pull those images; image-pull identity is distinct from the identity application code uses after startup. Cross-project repositories require permission on the repository project or repository itself for the relevant pulling identity.

An image reference includes location/registry hostname, project, repository, image, and tag or digest. Tags can move, while a digest identifies immutable image content. Pull failures can arise from a wrong path, missing image, IAM denial, network restrictions, or other node configuration—not only application credentials.

## Practical workflow

Check the exact image URI, repository existence/location, node pulling service account, and Artifact Registry Reader access at the narrowest supported scope. Inspect Pod events to distinguish NotFound, unauthorized, and network errors.

## Exam trap

Granting a Kubernetes workload identity permission to read application data does not necessarily give the node permission to pull its container image.

## Check your understanding

**Scenario:** A GKE cluster in project A cannot pull an image from project B. Where should you investigate access?

<details>
<summary>Answer and reasoning</summary>

Inspect the identity used for image pulls and its permission on the Artifact Registry repository in B. Changing only project A application roles may not solve cross-project repository access.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/artifact-registry/docs/integrate-gke)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
