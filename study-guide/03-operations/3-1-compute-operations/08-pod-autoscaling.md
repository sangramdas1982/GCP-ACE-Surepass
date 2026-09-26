# 3.1.08 — Horizontal and vertical Pod autoscaling

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Horizontal Pod Autoscaling (HPA) changes replica count based on supported metrics. Vertical Pod Autoscaling (VPA) recommends or adjusts resource requests according to its configured update mode and platform support. Node autoscaling changes machine capacity. These three levels solve different problems.

CPU utilization-based HPA uses CPU requests as an important reference. Poorly chosen requests can distort the signal. Vertical changes can require recreation depending on the supported configuration. Avoid configuring HPA and VPA to compete over the same CPU/memory signals without a supported design.

## Practical workflow

Set realistic requests, choose the scaling metric and bounds, verify metric collection, and generate load. Inspect replica decisions, pending Pods, and node capacity. Keep enough headroom and account for startup time and downstream limits.

## Exam trap

HPA cannot make a single nonparallelizable task use more memory. VPA does not directly increase replica count.

## Check your understanding

**Scenario:** A web service needs more identical workers as request demand rises. HPA or VPA?

<details>
<summary>Answer and reasoning</summary>

HPA is the natural fit for replica scaling. VPA addresses resource sizing per Pod, while node scaling may also be needed to host extra replicas.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/horizontalpodautoscaler)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
