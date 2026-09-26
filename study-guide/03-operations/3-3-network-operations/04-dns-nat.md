# 3.3.04 — Cloud DNS, Cloud NAT, and Private Google Access

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud DNS provides managed name resolution using public or private zones. Public zones require correct delegation for internet resolution; private zones apply to authorized VPCs. An A record maps a name to an IPv4 address; DNS resolving correctly does not prove network or application access.

Public Cloud NAT lets eligible private resources initiate outbound internet connections without individual external IPs. It does not permit arbitrary unsolicited inbound connections. Private Google Access addresses access from eligible internal-only resources to supported Google APIs/services; it is not general internet NAT. Cloud NAT also has private NAT capabilities, so identify which mode a question describes.

## Practical workflow

For DNS, check record, zone visibility, delegation and TTL/caching. For outbound access, distinguish Google APIs from arbitrary internet destinations, then inspect routes, firewall egress, NAT configuration and ports as applicable.

## Exam trap

Cloud Router supports NAT configuration/control functions; it is not a manually managed forwarding VM. DNS, routing, and NAT solve different layers.

## Check your understanding

**Scenario:** A private VM must download packages from public internet repositories without accepting unsolicited inbound traffic. What fits?

<details>
<summary>Answer and reasoning</summary>

Public Cloud NAT with the required routing/firewall configuration. A private DNS zone or Private Google Access alone does not provide arbitrary internet egress.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/nat/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
