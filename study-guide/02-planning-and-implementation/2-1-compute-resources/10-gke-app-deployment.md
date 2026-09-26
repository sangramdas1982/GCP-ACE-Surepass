# 2.1.10 — Deploying a containerized application to GKE

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A container image packages executable application content. A Pod runs one or more closely related containers. A Deployment maintains replicas and rollout state for typically stateless applications. A Service supplies a stable access endpoint for selected Pods whose individual addresses can change.

The deployment must reference a reachable image, set appropriate resource requests, expose the correct container port, and use health probes. Readiness controls whether traffic should reach a Pod; liveness can restart a stuck container; startup probes allow slow initialization without premature failure.

## Practical workflow

Push the image to Artifact Registry, verify image-pull permissions, apply a Deployment and Service manifest, then inspect `kubectl get pods`, `kubectl describe pod POD_NAME`, and `kubectl logs POD_NAME`. Verify Service selectors match Pod labels.

## Exam trap

A Running Pod need not be Ready. A Service with no endpoints may have a selector mismatch or no ready matching Pods, rather than a firewall failure.

## Check your understanding

**Scenario:** An image deploys successfully but receives traffic before initialization completes. Which probe directly controls traffic readiness?

<details>
<summary>Answer and reasoning</summary>

A readiness probe. A liveness probe addresses restarting a container and is not a substitute for excluding an initializing Pod from service traffic.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/deploy-app-cluster)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
