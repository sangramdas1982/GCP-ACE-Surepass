# 3.1.01 — Connecting to Compute Engine instances

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Remote access has three layers: reaching the VM, authenticating the user, and authorizing the OS login. Linux commonly uses SSH; Windows administration commonly uses RDP. Public IP access, a VPN/private network path, and IAP TCP forwarding offer different transport designs.

For SSH, distinguish OS Login from metadata-based keys. For IAP, the caller needs suitable IAP access and VM login permissions, and firewall policy must allow the documented IAP TCP-forwarding range to the intended port. The VM also needs a running SSH service. Serial-console diagnostics can help with boot issues when appropriately authorized.

## Practical workflow

Try `gcloud compute ssh VM_NAME --zone=ZONE`; use `--tunnel-through-iap` for the intended IAP route. For failure, identify timeout versus authentication denial, then inspect firewall/route or identity/OS configuration accordingly.

## Exam trap

Opening SSH to the whole internet is not the first answer to an authentication problem. IAP authorization alone does not grant Linux login rights.

## Check your understanding

**Scenario:** A VM has no external IP and administrators need controlled SSH access. Is adding a public IP the only option?

<details>
<summary>Answer and reasoning</summary>

No. IAP TCP forwarding can fit, with the required IAM, firewall, and OS login configuration. A private network path is another possibility depending on the scenario.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/connect/ssh-using-iap)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
