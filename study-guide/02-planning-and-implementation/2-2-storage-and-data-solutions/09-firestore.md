# 2.2.09 — Firestore: document data and indexes

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Firestore stores documents organized into collections. Documents contain fields rather than rows in a conventional relational schema, and applications retrieve them through supported queries and SDKs. It fits document-oriented application data and can support transactional operations within its documented constraints.

Indexes are central to query execution. Some query combinations require composite indexes, and the error may identify the required index. Data modeling should follow access patterns; relational joins are not simply translated into an identical document query. Location and database mode must be chosen deliberately.

## Practical workflow

Design collections/documents around reads and writes, choose database mode/location, configure client security or server IAM as appropriate, create required indexes, and test queries plus denied access. Review read/write operation cost as well as stored bytes.

## Exam trap

Server-client IAM and mobile/web security rules are different authorization paths. Do not assume one rule mechanism applies identically to every client.

## Check your understanding

**Scenario:** A document query reports a missing composite index. Is the correct response to grant the caller Owner?

<details>
<summary>Answer and reasoning</summary>

No. Create the appropriate supported index and wait for readiness. Broader permissions do not make an unindexed query executable.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/firestore/native/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
