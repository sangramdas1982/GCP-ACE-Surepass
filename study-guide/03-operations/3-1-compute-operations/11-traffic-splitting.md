# 3.1.11 — Traffic splitting and progressive delivery

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A canary release sends a small fraction of traffic to a new version while the stable version handles the rest. Cloud Run supports percentage-based routing among revisions. Monitor error rate, latency, and business outcomes before increasing the new revision's share.

GKE traffic management depends on the chosen Service, Gateway, ingress, mesh, or rollout tooling. Merely creating two Deployments does not universally provide exact weighted traffic control. Cloud Run functions have generation/deployment-model differences; verify the supported release mechanism rather than assuming every legacy function behaves like a current Cloud Run service.

## Practical workflow

Define success/rollback criteria, deploy the candidate, assign a small supported traffic weight, compare metrics, and increase or revert. Keep state and schema compatibility in mind throughout the rollout.

## Exam trap

Traffic percentage is not a promise of exactly that fraction over a tiny sample. A healthy infrastructure probe alone does not establish correct business behavior.

## Check your understanding

**Scenario:** A team wants to expose 5% of users’ requests to a new Cloud Run revision before full rollout. What should it use?

<details>
<summary>Answer and reasoning</summary>

Revision traffic splitting with monitoring and a rollback path. Replacing the existing revision for 100% of traffic does not meet the gradual exposure requirement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
