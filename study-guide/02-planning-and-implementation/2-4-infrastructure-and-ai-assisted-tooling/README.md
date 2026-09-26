# 2.4 — Infrastructure and AI-assisted tooling

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Learn what each tool manages, where desired configuration lives, and how a proposed change becomes a deployed resource. Generated configuration still requires review and verification.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Terraform, Fabric FAST, Config Connector, and Helm](01-infrastructure-as-code.md)
- [ ] [02. Gemini CLI, Antigravity, Cloud Assist, and Application Design Center](02-ai-assisted-tools.md)

## Worked study exercise: Review a Terraform change

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Read the small Terraform example in [Hands-on exercises](../../HANDS-ON-LABS.md).
2. Identify the provider, project, region, resource, and state responsibility.
3. If Terraform is available, run formatting/validation after initialization; inspect a plan only in an authorized sandbox.
4. Change a non-destructive label and predict the proposed update. Explain why a name/location change may have different replacement behavior.
5. Compare what Terraform, Helm, Config Connector, and Fabric FAST would each manage. Review AI-generated configuration using the same checks.

**Success evidence:** distinguish desired configuration, plan, state, and actual resource. Explain how to detect drift and why secrets can be present in state.

**Cleanup:** no apply is required. If you applied a lab configuration, inspect a destroy plan and delete only those tracked lab resources.

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
