# 3.3.02 — Static internal and external IP addresses

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An ephemeral IP is allocated with a resource lifecycle; a reserved static address is held for reuse according to its scope and type. Static addresses are useful when clients or external systems depend on a stable endpoint. Internal and external addresses serve different reachability needs.

Global versus regional scope must match the consuming resource. A regional VM external address cannot simply be substituted for every global load-balancer frontend. Reserved addresses may incur charges, including when unused, so maintain an inventory and release unneeded reservations after checking dependencies.

## Practical workflow

Identify the required endpoint type and scope, reserve a compatible address, attach it to the supported resource, and verify clients/DNS use it. Inspect `gcloud compute addresses list --project=PROJECT_ID`.

## Exam trap

A static IP is not a firewall exception or a guarantee of application availability. DNS can point to a stable frontend without pinning every backend VM address.

## Check your understanding

**Scenario:** A partner allowlists an application’s egress IP. Why can an ephemeral address be a poor fit?

<details>
<summary>Answer and reasoning</summary>

It may change through lifecycle events, breaking the allowlist. Use a suitable stable egress design, such as appropriately configured NAT addresses, for that requirement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/vpc/docs/reserve-static-external-ip-address)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
