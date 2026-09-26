# 4.1 — IAM policies and roles

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Every authorization decision starts with a principal, an action, and a resource. Understand direct grants, inherited access, role scope, conditions, and deny policies.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Viewing and creating IAM policies](01-iam-policy.md)
- [ ] [02. Role attachment and policy inheritance](02-iam-inheritance.md)
- [ ] [03. Basic, predefined, and custom IAM roles](03-role-types.md)

## Worked study exercise: Reason about effective IAM

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Draw an organization/folder/project/bucket hierarchy.
2. Add a folder-level viewer binding, a bucket-specific object role, and a hypothetical applicable deny.
3. Predict access for three named identities and explain every grant/denial path.
4. Read your sandbox project's IAM policy without changing unrelated bindings.
5. Compare a basic, predefined, and custom role for one narrow task.

**Success evidence:** explain why a child Viewer binding does not downgrade an inherited Editor grant and why role definition differs from role assignment.

**Cleanup:** remove only bindings deliberately created for the exercise. Do not overwrite a shared project's whole policy.

## How to answer this subsection's scenarios

1. Identify the concrete objective and every hard constraint.
2. State the resource and identity involved; separate configuration, permission, and connectivity failures.
3. Eliminate options that violate a requirement before comparing cost or convenience.
4. Prefer the simplest supported solution that satisfies all stated requirements; a managed service is not automatically correct if it lacks a required capability.
5. Explain why the most tempting alternative fails. Record uncertain answers in the [mistake log](../../MISTAKE-LOG.md).

## Completion check

- [ ] I can explain every linked topic in my own words.
- [ ] I can complete the exercise or explain its expected observations.
- [ ] I can distinguish the services/controls commonly confused here.
- [ ] I can answer the topic scenarios without relying on remembered wording.
