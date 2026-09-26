# 2.3.04 — Cloud VPN, Interconnect, and peering

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud VPN establishes IPsec-encrypted connectivity over public network paths. HA VPN uses a highly available design when configured with the required tunnels, peer topology, and routing. Cloud Router exchanges dynamic routes using BGP; it is a route-control service, not an appliance forwarding every packet.

Cloud Interconnect provides dedicated or partner connectivity for high-bandwidth private hybrid access. It is not automatically equivalent to IPsec encryption; evaluate encryption requirements and supported options separately. VPC Peering connects VPC networks and is not itself an on-premises VPN service.

## Practical workflow

Compare bandwidth, latency consistency, availability, encryption, deployment lead time, and cost. For hybrid connectivity, plan nonoverlapping IP space, redundant paths, Cloud Router/BGP where required, and firewall policy on both sides.

## Exam trap

Private connectivity is not synonymous with encryption. One tunnel or one physical path may fail the requested availability design.

## Check your understanding

**Scenario:** A branch office needs encrypted connectivity quickly without a dedicated physical circuit. Which service fits?

<details>
<summary>Answer and reasoning</summary>

Cloud VPN. Interconnect may suit sustained high-bandwidth private connectivity but introduces different provisioning and design requirements.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/network-connectivity/docs/vpn/concepts/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
