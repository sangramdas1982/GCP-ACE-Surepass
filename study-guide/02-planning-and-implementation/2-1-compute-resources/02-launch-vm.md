# 2.1.02 — Launching VMs and availability policies

[Subsection index](README.md) · [Study guide home](../../README.md)

Syllabus subsection: **2.1**. Topic numbers after the subsection are local study-note numbers, not official Google numbering.

## Understand the concept

A VM configuration includes zone, machine type, boot image, disks, network interfaces, service account, and access settings. The image supplies the operating system; machine type determines CPU and memory. A VM is zonal even when attached services have broader scope.

Availability policy controls behavior such as host maintenance handling and automatic restart when supported. Live migration moves a running VM between hosts for supported maintenance cases; automatic restart brings it back after qualifying failures. Neither turns one zonal VM into a multi-zone application. Some hardware and provisioning models impose different maintenance behavior.

## Practical workflow

Select the correct project and zone; verify API, billing, quota, and network; choose an image and machine type; attach a least-privileged identity; establish administrative access; then verify boot and application health. Inspect `gcloud compute instances describe VM_NAME --zone=ZONE`.

## Exam trap

A successful VM creation operation does not prove the application is listening or reachable. Automatic restart is not a backup or regional disaster-recovery plan.

## Check your understanding

**Scenario:** A VM runs after deployment, but the website is inaccessible. Should you immediately recreate the VM?

<details>
<summary>Answer and reasoning</summary>

First verify the application listener, port, firewall, IP and routing path. The compute resource can be healthy while the application or network is misconfigured.

</details>

## Official reference

- [Product documentation](https://docs.cloud.google.com/compute/docs/instances/create-start-instance)


Reviewed against the supplied syllabus on 2026-09-26. Examples are study scenarios, not real exam questions.
