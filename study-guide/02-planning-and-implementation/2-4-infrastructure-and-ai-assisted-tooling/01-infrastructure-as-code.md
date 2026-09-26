# 2.4.01 — Terraform, Fabric FAST, Config Connector, and Helm

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.4**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Infrastructure as code records desired resources in version-controlled configuration. Terraform uses providers, configuration, and state to plan and apply infrastructure changes. State connects configuration to actual resources and can contain sensitive information; use an appropriate remote backend and controlled access for team work.

Fabric FAST is an opinionated foundation approach in Cloud Foundation Fabric for bootstrapping Google Cloud environments. Config Connector represents Google Cloud resources as Kubernetes objects reconciled by controllers. Helm packages and templates Kubernetes manifests into chart releases. These tools operate at different layers; Helm is not a replacement for every Google Cloud resource provider.

## Practical workflow

For Terraform: initialize providers/backend, format and validate, inspect `terraform plan`, review replacements/deletions, apply the reviewed change, and verify deployed state. Use reusable modules and avoid multiple tools competing to own the same resource.

## Exam trap

Manually deleting state does not safely delete the corresponding resource. An imperative console change can create drift from the declared configuration.

## Check your understanding

**Scenario:** A team needs a preview of which infrastructure resources will be added, changed, or replaced before deployment. Which Terraform step provides it?

<details>
<summary>Answer and reasoning</summary>

`terraform plan`. `apply` performs changes, while `init` prepares providers/backend and is not the resource-change review.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/docs/terraform/terraform-overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
