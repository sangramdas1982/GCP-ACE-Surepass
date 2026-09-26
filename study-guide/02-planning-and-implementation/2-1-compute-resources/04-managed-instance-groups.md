# 2.1.04 — Instance templates, MIGs, and autoscaling

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An instance template records a repeatable VM configuration. A managed instance group (MIG) uses a template to create and maintain instances. A regional MIG distributes instances across zones; a zonal MIG does not. Autoscaling changes group size in response to a configured signal such as CPU or load-balancing utilization.

Autohealing replaces unhealthy VMs using a health check. Load-balancer health checks control where traffic goes; autohealing health checks trigger recreation, so overly aggressive checks can cause destructive churn. Templates are immutable: release a new template and roll the group toward it. Stateful MIG features exist, but ordinary stateless services are the simplest scaling case.

## Practical workflow

Create a template, create a group, set min/max sizes and the scaling signal, configure an appropriate initial delay and health check, and test a gradual rollout. Put session state outside disposable instances when designing a stateless group.

## Exam trap

Autoscaling responds to demand; autohealing responds to health. Neither substitutes for the other. An unmanaged instance group does not provide the same template-driven management.

## Check your understanding

**Scenario:** Traffic is steady but one VM stops serving correctly. Which feature should replace it?

<details>
<summary>Answer and reasoning</summary>

Autohealing. Autoscaling may leave the instance count unchanged because demand has not increased.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/instance-groups)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
