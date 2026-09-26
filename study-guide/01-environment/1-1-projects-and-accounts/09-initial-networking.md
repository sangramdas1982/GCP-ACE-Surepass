# 1.1.09 — Initial cloud networking

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A VPC defines a private network for resources. Its subnets allocate regional IP ranges; a VM attaches to a subnet in its region. Plan nonoverlapping address ranges if networks will later connect to each other or on-premises. Public reachability additionally depends on addresses, routes, firewall policy, and the application listener.

Auto-mode networks create predefined regional subnets. Custom mode gives explicit control over subnet placement and CIDR ranges. A default network can be convenient for learning but should not be assumed to exist or satisfy production security requirements.

## Practical workflow

Draw the required regions and address ranges; create a custom VPC and subnet; add only the necessary access rules; identify how private workloads reach APIs or external services. Record source, destination, protocol, and port for each required flow.

## Exam trap

A subnet is regional, not zonal. Creating a route does not itself grant firewall permission. Removing an external IP does not establish a complete private-access design.

## Check your understanding

**Scenario:** VMs in two zones of one region need the same subnet. Must you create one subnet per zone?

<details>
<summary>Answer and reasoning</summary>

No. A regional subnet can serve VMs across zones in that region. Separate subnets may be useful for other reasons, but zone boundaries do not require them.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/vpc/docs/subnets)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
