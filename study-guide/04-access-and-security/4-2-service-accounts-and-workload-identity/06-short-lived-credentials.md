# 4.2.06 — Short-lived access tokens and ID tokens

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **4.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

An OAuth access token authorizes calls to APIs according to its identity and applicable permissions. An ID token asserts identity for a target audience, such as an authenticated service endpoint. They are not interchangeable simply because both are bearer tokens.

Attached workload identities, federation, and impersonation can obtain short-lived credentials without distributing a permanent private key. Tokens expire and must be refreshed through a supported flow. A downloaded service-account key can mint credentials until revoked or otherwise invalidated, so its exposure can be more enduring.

## Practical workflow

Identify whether the destination expects an access token or an ID token and, for ID tokens, the required audience. Use a supported credential library/metadata/federation flow, refresh correctly, and avoid printing tokens into logs.

## Exam trap

An ID token with the wrong audience can fail even when the identity has an invoker role. A valid token does not bypass IAM.

## Check your understanding

**Scenario:** An authenticated Cloud Run endpoint rejects a token intended for another audience. Should you make the service public?

<details>
<summary>Answer and reasoning</summary>

No. Obtain the correct token type and audience and verify invocation permission. Public access would evade rather than solve the authentication requirement.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/create-short-lived-credentials-direct)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
