# 1.1.04 — Cloud Identity users and groups

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **1.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud Identity provides managed workforce accounts and groups. It is an identity directory, while Google Cloud IAM determines access to cloud resources. Creating a user does not automatically give that user permission to administer projects. A group lets you grant access once and manage membership centrally.

Small organizations can manage accounts through the Admin console. Larger organizations can synchronize an existing directory using supported tools such as Google Cloud Directory Sync, or automate supported user/group operations through directory APIs. Directory synchronization and single sign-on are different: synchronization manages account records; SSO delegates authentication.

## Practical workflow

Identify the authoritative directory, verify organizational ownership where required, create administrative recovery arrangements, provision users/groups, and then bind cloud roles to groups. Test both login and resource authorization because success in one does not establish the other.

## Exam trap

Cloud Identity is not a replacement for resource IAM. Directory administrator privileges and Google Cloud project privileges are different.

## Check your understanding

**Scenario:** Employees change teams frequently. Should you repeatedly edit individual IAM bindings?

<details>
<summary>Answer and reasoning</summary>

Grant roles to team groups and update group membership. This reduces policy churn and makes responsibility-based access easier to audit.

</details>

## Official reference

- [Product documentation](https://cloud.google.com/identity/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
