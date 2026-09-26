# 15-day exam preparation plan

[Study guide home](README.md)

**Time budget:** 45–60 hours total. This is an intensive study plan, not a guaranteed passing formula. Start at Day 1 whenever you begin; no fixed exam booking is assumed.

## Daily rhythm

- 80 minutes: concepts and topic notes.
- 50 minutes: hands-on exercise or equivalent design/diagnosis exercise.
- 40 minutes: scenario questions, including explanation of rejected alternatives.
- 10 minutes: closed-book recall of yesterday’s weak points.
- Optional fourth hour: finish dense sections, revisit errors, or do a temporary Google Skills lab.

For a two-hour full mock, use the remaining 1–2 hours for review rather than also attempting the normal rhythm. Days 6, 9 and 12 are dense; use the optional hour and carry unresolved items into Day 13.

## Day 1 — Foundations and environment

**Read:** [1.1](01-environment/1-1-projects-and-accounts/README.md)

Read Foundations and all 1.1 notes. Draw your project hierarchy; inspect account, project, APIs, quota, locations, network, and identity setup.

**Exit check:** Explain an API failure versus IAM denial versus quota failure.

## Day 2 — Billing and IAM

**Read:** [1.2](01-environment/1-2-billing-configuration/README.md); [4.1](04-access-and-security/4-1-iam-policies-and-roles/README.md)

Study all billing and IAM notes. Design a budget/export and trace inherited access.

**Exit check:** Explain why budget ≠ cap and child Viewer ≠ removal of inherited Editor.

## Day 3 — Service accounts and federation

**Read:** [4.2](04-access-and-security/4-2-service-accounts-and-workload-identity/README.md)

Study all 4.2 notes; revisit Workforce Federation in 1.1. Draw deployer, runtime, target resource, and token flow.

**Exit check:** Choose actAs, impersonation, workforce federation, or workload federation correctly.

## Day 4 — VMs and instance groups

**Read:** [2.1](02-planning-and-implementation/2-1-compute-resources/README.md)

Study compute selection through Spot/custom VMs. Complete the VM lab if available.

**Exit check:** Choose disks, OS Login, patching, and MIG behavior for a given requirement.

## Day 5 — VM operation and recovery

**Read:** [3.1](03-operations/3-1-compute-operations/README.md)

Read remote access, inventory, snapshots/images. Revisit regional design and accelerators. Practice SSH diagnosis and a restore design.

**Exit check:** Separate resource RUNNING state from application health and template from backup.

## Day 6 — Kubernetes and GKE

**Read:** [2.1](02-planning-and-implementation/2-1-compute-resources/README.md); [3.1](03-operations/3-1-compute-operations/README.md)

Read kubectl/cluster/application notes, then GKE inventory, registry access, node pools, workload objects, HPA/VPA, Autopilot requests.

**Exit check:** Diagnose Pending, ImagePullBackOff, CrashLoopBackOff and readiness problems.

## Day 7 — Cloud Run and event processing

**Read:** [2.1](02-planning-and-implementation/2-1-compute-resources/README.md); [3.1](03-operations/3-1-compute-operations/README.md)

Read serverless events, revisions, traffic splitting, autoscaling. Review Pub/Sub and idempotency.

**Exit check:** Plan a canary and rollback; explain concurrency, min/max instances and retries.

## Day 8 — Object and file storage

**Read:** [2.2](02-planning-and-implementation/2-2-storage-and-data-solutions/README.md); [3.2](03-operations/3-2-storage-and-data-operations/README.md)

Read storage selection/classes, transfers and multi-region notes. Read object security/lifecycle. Complete the small bucket lab.

**Exit check:** Choose object, block, NFS, enterprise file, or parallel storage and lifecycle settings.

## Day 9 — Databases and data pipelines

**Read:** [2.2](02-planning-and-implementation/2-2-storage-and-data-solutions/README.md); [3.2](03-operations/3-2-storage-and-data-operations/README.md)

Read every service-specific 2.2 note and remaining 3.2 operations notes, including recovery, cost, jobs, Database Center and CMEK.

**Exit check:** Choose the data service and distinguish HA, replicas, backups and PITR.

## Day 10 — Network architecture

**Read:** [2.3](02-planning-and-implementation/2-3-networking-resources/README.md)

Read all 2.3 notes. Draw Shared VPC, peering and a hybrid path. Choose load balancers from requirements.

**Exit check:** Explain scope, nontransitivity, firewall targets, VPN/Interconnect and tiers.

## Day 11 — Network operations

**Read:** [3.3](03-operations/3-3-network-operations/README.md)

Read all 3.3 notes. Inspect routes/firewall; calculate CIDRs; diagnose DNS, NAT and private Google API access cases.

**Exit check:** Trace a failing connection without changing unrelated layers.

## Day 12 — Monitoring and diagnostics

**Read:** [3.4](03-operations/3-4-monitoring-and-logging/README.md)

Read all 3.4 notes. Generate/find a test log, design an alert, and trace a log sink writer permission.

**Exit check:** Select metrics, audit logs, flow/firewall logs, traces, profiles or service health.

## Day 13 — Tooling and remaining AI objectives

**Read:** [2.4](02-planning-and-implementation/2-4-infrastructure-and-ai-assisted-tooling/README.md); [3.1](03-operations/3-1-compute-operations/README.md)

Read 2.4; finish GPU/TPU operations, Agent Runtime, notebooks and Workstations. Review SYLLABUS-COVERAGE.md for unchecked topics.

**Exit check:** Explain each tool’s job and identify remaining coverage gaps.

## Day 14 — Mixed assessment and repair

**Read:** [All domains](SYLLABUS-COVERAGE.md)

Take the included 24-question set in 50 minutes without notes; review every wrong/guessed answer. If available, use an additional reputable full-length timed mock.

**Exit check:** Explain every distractor and repair the three weakest areas.

## Day 15 — Final readiness and recall

**Read:** [All domains](SYLLABUS-COVERAGE.md)

Use fresh official samples or another fresh practice set, then review decision tables, command patterns and mistakes. Keep the last session light.

**Exit check:** Explain unfamiliar scenarios without depending on memorized answers.

## Readiness decision

As a personal study target, aim for roughly 80–85% on fresh, reputable practice material and the ability to explain your choices. This is not Google’s published passing threshold and does not predict a pass. Repeated questions can inflate your score. Treat lucky guesses as incorrect for revision.

If a major domain remains unfamiliar, use remaining time on its core distinctions and consider your readiness honestly. Do not spend the final night learning every obscure option while neglecting IAM, networking, compute operations, data selection, or recovery.

Official preparation and sample questions are linked from the [certification page](https://cloud.google.com/learn/certification/cloud-engineer). The [Google Skills learning path](https://www.skills.google/paths/11) supplies structured training/labs; access terms may vary.
