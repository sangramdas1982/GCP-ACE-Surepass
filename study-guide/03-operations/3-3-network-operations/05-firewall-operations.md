# 3.3.05 — Operating firewall rules and policies

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.3**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

When a connection fails, identify the real source and destination as seen at the firewall. Load balancers, proxies, NAT, and health checks can change which source ranges or identities need access. Inspect effective policies rather than only the one rule the team remembers creating.

Firewall logging and Connectivity Tests can provide evidence, with coverage depending on configuration. Preserve a known administrative access path when narrowing rules. Test the required traffic and a traffic flow that should remain blocked; success of only the permitted test does not prove the policy is appropriately restrictive.

## Practical workflow

Capture the traffic tuple and failure time, inspect matching rule targets and priorities, read available logs, change the narrowest relevant rule, and retest. Record why the rule exists and its owner.

## Exam trap

Disabling broad firewall policy to diagnose one port can expose unrelated workloads. A health-check failure may require the documented probe source ranges rather than arbitrary public access.

## Check your understanding

**Scenario:** An application works directly but the load balancer marks all backends unhealthy. What network detail should you verify?

<details>
<summary>Answer and reasoning</summary>

The health-check path, port, and permitted probe source ranges, alongside application health. Direct access from a workstation tests a different source path.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/firewall/docs/firewall-rules-logging)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
