# 3.1 — Compute operations

[Domain index](../README.md) · [Study guide home](../../README.md) · [15-day plan](../../15-DAY-PLAN.md)

Deployment is only the beginning. Operate by inspecting state, diagnosing failures, scaling the correct layer, releasing changes gradually, and restoring service with a clear rollback path.

## Topic notes

Read these in order for the first pass. Tick a topic only when you can answer its scenario without opening the answer. Section 2.2 includes additional service-specific notes to expand the grouped product examples in the syllabus.

- [ ] [01. Connecting to Compute Engine instances](01-remote-vm-access.md)
- [ ] [02. Viewing and inspecting running VMs](02-vm-inventory.md)
- [ ] [03. Snapshots, images, and recovery](03-snapshots-images.md)
- [ ] [04. Inspecting GKE nodes, Pods, and Services](04-gke-inventory.md)
- [ ] [05. GKE access to Artifact Registry](05-artifact-registry-access.md)
- [ ] [06. Managing GKE node pools](06-node-pools.md)
- [ ] [07. Pods, Deployments, Services, and StatefulSets](07-kubernetes-resources.md)
- [ ] [08. Horizontal and vertical Pod autoscaling](08-pod-autoscaling.md)
- [ ] [09. Autopilot Pod resource requests](09-autopilot-requests.md)
- [ ] [10. Deploying Cloud Run revisions](10-cloud-run-revisions.md)
- [ ] [11. Traffic splitting and progressive delivery](11-traffic-splitting.md)
- [ ] [12. Cloud Run autoscaling and concurrency](12-cloud-run-scaling.md)
- [ ] [13. Attaching and operating GPUs and TPUs](13-attach-accelerators.md)
- [ ] [14. Deploying and operating Agent Runtime](14-agent-runtime.md)
- [ ] [15. Workbench and BigQuery notebooks](15-notebooks.md)
- [ ] [16. Cloud Workstations and developer environments](16-cloud-workstations.md)

## Worked study exercise: Diagnose and recover a workload

Allow about 35–50 minutes; use the design-only version when lab access or billing permissions are unavailable.

1. Inspect an existing lab VM or Kubernetes workload with list/describe/log commands.
2. For a disposable GKE workload, temporarily set an invalid image reference and inspect events; restore the valid image immediately afterward.
3. Explain how an image-pull failure differs from a running container crash and an unschedulable Pod.
4. For Cloud Run, inspect revisions and describe a no-traffic deployment followed by a small canary and rollback.
5. For the VM, create a test snapshot only if time/budget permits, restore a disk, and verify a known test file.

**Success evidence:** identify the failing layer before choosing a repair. A restore is complete only after the expected data is readable.

**Cleanup:** remove any test snapshot/restored disk and deployed training workloads. Stopping a VM is not the same as deleting its storage.

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
