# 3.3.03 — Custom static routes

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Routes select a next hop for destination address ranges. They do not grant permission through firewalls. A custom static route can steer traffic toward a supported next hop, for example an appliance, when its scope and configuration are valid.

Routing precedence is more nuanced than one global “lowest priority wins” rule. Route category, destination specificity, applicability, and priority all matter. For comparable applicable routes, a more specific destination often matters before numeric priority. A next-hop VM performing forwarding must be correctly configured at both the cloud and guest levels.

## Practical workflow

Draw the destination prefix and intended next hop, inspect existing effective routes, verify appliance forwarding and return paths, create the route, and test from an applicable source. Check whether tags or other constraints limit its applicability.

## Exam trap

A route through an appliance can fail if return traffic takes an incompatible path. A firewall allow cannot repair a missing or wrong route.

## Check your understanding

**Scenario:** Traffic is allowed by firewall policy but sent toward the wrong next hop. What should you inspect?

<details>
<summary>Answer and reasoning</summary>

The applicable routes, destination ranges, and route precedence. Adding a duplicate allow rule does not change route selection.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/vpc/docs/routes)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
