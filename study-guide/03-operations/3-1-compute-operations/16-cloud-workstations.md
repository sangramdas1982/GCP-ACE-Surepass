# 3.1.16 — Cloud Workstations and developer environments

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **3.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

Cloud Workstations provides managed cloud development environments. Administrators define shared configurations such as machine resources, container images, networking, and lifecycle settings; developers use individual workstations derived from those configurations. This helps standardize tooling and keep access within an organizational network design.

A development environment is not the production application hosting service. The workstation’s runtime permissions and repository access still need control. Persistent storage and running compute have separate lifecycle implications, so stopping an environment does not mean every associated charge or stored artifact disappears.

## Practical workflow

Choose an appropriate workstation configuration, verify repository and private resource access, start the environment, test the toolchain, and stop it after use. Keep configuration changes reproducible so another developer can obtain the same setup.

## Exam trap

Do not choose Cloud Workstations to serve production HTTP traffic merely because developers can run a local server in it.

## Check your understanding

**Scenario:** A team needs standardized private development environments without configuring every engineer’s laptop. Which service fits?

<details>
<summary>Answer and reasoning</summary>

Cloud Workstations. Cloud Run serves deployed workloads; it does not provide the same interactive development-environment model.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/workstations/docs/overview)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
