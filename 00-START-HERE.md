# Associate Cloud Engineer: 4 October study route

Based on the [official Associate Cloud Engineer exam guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf), checked 26 September 2026. This pack assumes you already know basic GCP terms. The guide defines four sections; its percentages are approximate and are **not** a promise about individual questions.

| Guide section | Approx. weight | Study file and priority |
|---|---:|---|
| 1. Setting up a cloud solution environment | 20% | `01-CONCEPTS.md`: hierarchy, billing, APIs, quotas, regions, identity |
| 2. Planning and implementing a cloud solution | 30% | `01-CONCEPTS.md`: compute, data, networking, tooling |
| 3. Ensuring successful operation | 30% | `02-OPERATIONS-COMMANDS.md`: changes, monitoring, troubleshooting |
| 4. Configuring access and security | 20% | Both references: IAM, service accounts, federation |

## Eight-day plan (Singapore dates)

- **26 Sep:** Read section 1 and IAM; draw organization → folder → project → resource, then explain policy inheritance and billing aloud.
- **27 Sep:** Compute choices, VM/MIG, GKE Standard versus Autopilot, Cloud Run; do the compute exercises.
- **28 Sep:** Storage and data decision table; explain SQL versus Spanner versus Bigtable versus BigQuery versus Firestore without notes.
- **29 Sep:** VPC, subnets, routes, firewall/NGFW, NAT, VPN/Interconnect, load balancing. Draw one private workload path to the internet.
- **30 Sep:** Operations: snapshots, GKE resources and scaling, Cloud Run revisions, backup/restore, storage lifecycle.
- **1 Oct:** Monitoring and logging plus security/federation. Work through `02-OPERATIONS-COMMANDS.md` and repeat the troubleshooting flow.
- **2 Oct:** Answer `03-PRACTICE.md` closed book. Review every wrong answer by opening the relevant official documentation.
- **3 Oct:** Re-answer wrong items; review command patterns and decision tables. Avoid a large new topic. Check appointment, ID, test environment, and exam logistics in your booking.
- **4 Oct:** Brief recall only before the exam.

If a day is missed, protect the two 30% sections first. Spend about half of each session applying ideas to scenarios rather than rereading. A useful response pattern is: **requirement → product/configuration → why → one tempting but unsuitable choice**.

## Fast recall sheet

1. **Resource scope:** organization and folders hold policies; a project is the main boundary for APIs, quotas, billing association, and many resources. IAM allow policies inherit down the hierarchy; organization policies constrain what can be configured.
2. **Location:** VPC is global; subnet is regional; VM zone is zonal; regional services and redundancy must match the requested failure domain.
3. **Compute:** VM for OS/control; GKE for Kubernetes; Cloud Run for managed app/container execution; Cloud Run functions for function/event style. Distinguish Pod HPA from node autoscaling.
4. **Data:** Cloud Storage objects; Filestore/NetApp managed files; Cloud SQL conventional relational; Spanner horizontally scalable relational; Bigtable low-latency wide-column; Firestore documents; BigQuery analytics; Memorystore cache; Pub/Sub asynchronous messages; Dataflow pipelines.
5. **Networking:** firewall/NGFW controls traffic; routes choose next hop; Cloud NAT gives private instances outbound access without a public IP; load balancers serve inbound traffic; VPN uses encrypted tunnels, Interconnect is dedicated/private connectivity.
6. **Operations:** metrics → alerts; logs → inspect/filter/router sinks; traces/profiles → latency and CPU investigation. Snapshot, image, backup, and replication solve different problems.
7. **Security:** grant smallest suitable role at narrowest useful scope; a service account is a workload identity; impersonation and federation favor short-lived credentials over downloaded keys.

## How to use the pack

Read `01-CONCEPTS.md`, then use `02-OPERATIONS-COMMANDS.md` as a practical checklist. Attempt `03-PRACTICE.md` before viewing the answers. Commands are patterns: replace placeholders, confirm location and permissions, and verify the CLI syntax for the installed version. The exam guide is the source of scope; linked Google documentation is the source for product detail. Product names and features can change.

## Official sources

- [Exam guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf)
- [Choose compute options](https://docs.cloud.google.com/docs/compute-area/choose-compute-options)
- [Google Cloud products](https://docs.cloud.google.com/docs/product-list)
- [Cloud Storage classes](https://docs.cloud.google.com/storage/docs/storage-classes)
- [GKE cluster choices](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/configuration-overview)
- [Workload Identity Federation](https://docs.cloud.google.com/iam/docs/workload-identity-federation)
- [Log routing](https://docs.cloud.google.com/logging/docs/routing/overview)
