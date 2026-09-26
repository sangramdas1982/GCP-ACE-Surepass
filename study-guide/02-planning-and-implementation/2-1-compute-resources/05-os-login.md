# 2.1.05 — OS Login and SSH authorization

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

OS Login ties Linux login access to Google identity and IAM instead of managing SSH keys independently in instance or project metadata. OS Login roles distinguish ordinary login from administrative login with elevated privileges. Enabling it changes which SSH key-management path applies.

A successful SSH session still requires a reachable transport path. IAP TCP forwarding can provide controlled access without a public VM address, but needs its own permissions and firewall path. Depending on the VM identity and organization boundary, additional service-account or external-user access permissions may be required.

## Practical workflow

Enable OS Login in appropriate metadata, grant the needed login role, confirm any required access to the attached service account, and verify TCP connectivity. Diagnose identity, OS authorization, and network transport separately.

## Exam trap

Adding another metadata SSH key will not fix an OS Login role problem. OS Admin Login is broader than normal OS Login.

## Check your understanding

**Scenario:** Employees must lose Linux VM access when their central access is revoked. Should each VM keep manually maintained personal SSH keys?

<details>
<summary>Answer and reasoning</summary>

OS Login provides centrally managed identity-based login. Network controls still apply, and the administrative role should only be granted when sudo access is required.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/oslogin)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
