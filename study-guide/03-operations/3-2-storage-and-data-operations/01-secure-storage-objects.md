# 3.2.01 — Managing and securing Cloud Storage objects

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.2**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A bucket contains named objects and bucket-level configuration. Uniform bucket-level access uses IAM instead of separate object ACLs, simplifying consistent access management. Public access prevention guards against supported public-access grants. Signed URLs grant time-limited access to a specific operation without making the whole bucket public.

Object versioning, soft delete, retention policies, and holds solve different protection needs. Retention can prevent deletion for a required period; versioning retains generations under its rules; soft delete provides a recovery window where configured. Each can affect cost and deletion behavior.

## Practical workflow

Inspect bucket IAM and access settings, identify who needs which object actions, use narrow roles, and test access as the intended identity. Review versioning/soft-delete/retention settings before deletion or lifecycle changes.

## Exam trap

Public access prevention does not mean no authenticated user can access the bucket. Signed URLs are bearer access: anyone possessing a valid URL can use it within its constraints.

## Check your understanding

**Scenario:** A partner needs temporary access to one private download. Must the bucket become public?

<details>
<summary>Answer and reasoning</summary>

No. A suitable signed URL can provide bounded access. Granting public access to the whole bucket is broader and longer-lived than required.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/storage/docs/access-control)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
