# 1.1.12 — Workforce Identity Federation

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Workforce Identity Federation lets people authenticate through an external identity provider and access Google Cloud using IAM. A workforce pool groups external human identities; a provider defines trust with an OIDC or SAML identity provider. Attribute mappings connect external claims to Google identity attributes, and conditions can narrow which identities are accepted.

Authentication proves who the user is. Resource IAM then decides what that user can do. Federation avoids requiring a separately managed Google user account for every external person, but still requires deliberate access grants and a supported sign-in flow.

## Practical workflow

Configure the workforce pool/provider, map a stable subject and necessary group attributes, restrict accepted identities, grant the federated principal or principal set appropriate resource roles, and test both permitted and denied access.

## Exam trap

Workforce means humans. Workload Identity Federation serves software workloads; similar terminology does not make them interchangeable.

## Check your understanding

**Scenario:** Contractors must use the company identity provider to access the console without separate Google directory accounts. Which federation model fits?

<details>
<summary>Answer and reasoning</summary>

Workforce Identity Federation. A service-account key identifies an application and is not an appropriate substitute for individual human identities.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/iam/docs/workforce-identity-federation)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
