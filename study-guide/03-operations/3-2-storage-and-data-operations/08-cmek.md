# 3.2.08 — Customer-managed encryption keys

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Google Cloud encrypts stored data by default. CMEK lets customers control supported encryption keys through Cloud KMS, adding control over key access, lifecycle, rotation, and related governance. It does not mean data was previously stored unencrypted.

The service identity using the key needs the appropriate cryptographic permission, commonly on the key itself. Key location must satisfy the data service’s compatibility rules. Disabling or destroying a key version can make dependent data unavailable; rotating a key does not universally re-encrypt all existing data immediately.

## Practical workflow

Check product CMEK support, create a key in a compatible location, grant the documented service identity access, configure the resource to use it, and validate operation. Separate key administration from data administration where required.

## Exam trap

Granting a human KMS admin role does not necessarily authorize the service agent to encrypt/decrypt. CMEK controls encryption keys, not who can query plaintext through an authorized service.

## Check your understanding

**Scenario:** A CMEK-protected resource becomes unavailable after its key is disabled. What is the likely dependency?

<details>
<summary>Answer and reasoning</summary>

The service can no longer use the required key version. Restore authorized key availability if possible; broadening unrelated project IAM is not the direct fix.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/kms/docs/cmek)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
