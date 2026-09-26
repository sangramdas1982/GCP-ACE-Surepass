# 2.1 — Compute resources

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Start with workload requirements: operating-system control, Kubernetes compatibility, request or event processing, accelerators, and operational effort. Then select a deployment and availability model.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Choosing a compute service](01-compute-selection.md)
- [ ] [02. Launching VMs and availability policies](02-launch-vm.md)
- [ ] [03. Persistent Disk, Hyperdisk, and Local SSD](03-vm-disks.md)
- [ ] [04. Instance templates, MIGs, and autoscaling](04-managed-instance-groups.md)
- [ ] [05. OS Login and SSH authorization](05-os-login.md)
- [ ] [06. VM Manager](06-vm-manager.md)
- [ ] [07. Spot VMs and custom machine types](07-spot-custom-vms.md)
- [ ] [08. Installing and configuring kubectl](08-kubectl-setup.md)
- [ ] [09. GKE Standard, Autopilot, regional, and private clusters](09-gke-cluster-design.md)
- [ ] [10. Deploying a containerized application to GKE](10-gke-app-deployment.md)
- [ ] [11. Cloud Run functions, Pub/Sub, and Eventarc](11-serverless-events.md)
- [ ] [12. Choosing GPUs or TPUs](12-gpu-tpu-selection.md)

## Worked study exercise: Deploy one small workload, then explain alternatives

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Use the complete VM lab in [Hands-on exercises](../../HANDS-ON-LABS.md). Create a custom network, a small private VM, and controlled SSH access if your environment permits it.
2. Inspect the VM image, disk, service account, subnet, and zone. Confirm an SSH session.
3. Draw how an instance template, regional MIG, health check, and load balancer would turn this into a resilient stateless service.
4. Explain when the same application would fit Cloud Run or GKE instead.
5. Complete a short GKE or Cloud Run lab using a temporary training environment when available; do not keep multiple compute platforms running for this exercise.

**Success evidence:** identify both the deployment choice and the operational responsibility it creates. Explain autoscaling versus autohealing and private access versus authentication.

**Cleanup:** remove the VM and its lab boot disk, then the lab firewall/network resources. Review disks, static addresses, and managed services separately.

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
