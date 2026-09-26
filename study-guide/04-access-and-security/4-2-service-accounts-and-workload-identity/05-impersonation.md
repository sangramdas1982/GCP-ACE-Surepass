# 4.2.05 — Service account impersonation

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Impersonation lets an authenticated caller obtain short-lived credentials representing a service account, subject to appropriate permission. This supports administrative workflows and testing without distributing a private key. The target service account’s roles determine what its token can access.

Keep source identity and target identity clear. The source is authorized to impersonate; the target is authorized on the final resource. Audit evidence can preserve information about this delegation chain for supported operations, making impersonation preferable to anonymous sharing of a long-lived key.

## Practical workflow

Grant the appropriate token-creation permission narrowly on the target account, then use a supported CLI/API flow. A CLI pattern is `gcloud ... --impersonate-service-account=SERVICE_ACCOUNT_EMAIL`. Verify the intended operation and inspect audit details.

## Exam trap

Service Account User and Token Creator are different roles. Knowing an account’s email address is not enough to impersonate it.

## Check your understanding

**Scenario:** An engineer needs temporary CLI access as a deployment account without downloading its private key. What should you use?

<details>
<summary>Answer and reasoning</summary>

Service account impersonation with narrowly granted permission and short-lived credentials. A new JSON key creates a longer-lived secret management burden.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/service-account-impersonation)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
