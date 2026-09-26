# 4.2.03 — Assigning service accounts to resources

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A VM or managed runtime can run with an attached service account and obtain credentials through the platform rather than a downloaded private key. The deploying principal needs permission to configure the resource and, commonly, to act as the selected service account. The attached account separately needs permissions to access downstream resources.

This creates two authorization questions: may the deployer attach this identity, and may the runtime identity perform its work? Service Account User includes actAs behavior; it should not be confused with token-minting capabilities provided by Service Account Token Creator.

## Practical workflow

Create/select a dedicated runtime account, grant its target-resource roles, grant the deployer only the necessary actAs access on that account, and deploy the resource with the account selected. Verify the actual runtime identity after deployment.

## Exam trap

Granting Service Account User to a deployer does not itself grant the runtime account access to a database or bucket.

## Check your understanding

**Scenario:** A developer can deploy a VM but gets an actAs error when selecting a particular service account. What is missing?

<details>
<summary>Answer and reasoning</summary>

Appropriate permission to act as that account, assuming other deployment permissions exist. Giving the service account more bucket access will not fix the deployer’s attach permission.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/attach-service-accounts)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
