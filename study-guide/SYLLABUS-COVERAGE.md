# Syllabus coverage and progress

[Study guide home](README.md)

Source: [user-supplied official exam guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf). Reviewed 2026-09-26.

This checklist paraphrases the syllabus; it does not reproduce the PDF. Each official numbered subsection is mapped below. Grouped product examples receive extra notes in 2.2. Local numbering is for navigation only.

## 1.1 — Projects and accounts

- [ ] [Resource hierarchy and project identifiers](01-environment/1-1-projects-and-accounts/01-resource-hierarchy.md)
- [ ] [Organization policies and constraints](01-environment/1-1-projects-and-accounts/02-organization-policies.md)
- [ ] [Granting project IAM roles](01-environment/1-1-projects-and-accounts/03-grant-project-roles.md)
- [ ] [Cloud Identity users and groups](01-environment/1-1-projects-and-accounts/04-cloud-identity.md)
- [ ] [Enabling service APIs](01-environment/1-1-projects-and-accounts/05-enable-apis.md)
- [ ] [Initial observability setup](01-environment/1-1-projects-and-accounts/06-observability-setup.md)
- [ ] [Quotas, limits, and capacity](01-environment/1-1-projects-and-accounts/07-quotas.md)
- [ ] [Standalone organization setup](01-environment/1-1-projects-and-accounts/08-standalone-organizations.md)
- [ ] [Initial cloud networking](01-environment/1-1-projects-and-accounts/09-initial-networking.md)
- [ ] [Regions, zones, and product availability](01-environment/1-1-projects-and-accounts/10-locations.md)
- [ ] [Cloud Asset Inventory and Gemini Cloud Assist](01-environment/1-1-projects-and-accounts/11-asset-inventory.md)
- [ ] [Workforce Identity Federation](01-environment/1-1-projects-and-accounts/12-workforce-federation.md)

## 1.2 — Billing configuration

- [ ] [Creating and administering billing accounts](01-environment/1-2-billing-configuration/01-billing-accounts.md)
- [ ] [Linking projects to billing accounts](01-environment/1-2-billing-configuration/02-link-billing.md)
- [ ] [Budgets and alerts](01-environment/1-2-billing-configuration/03-budgets-alerts.md)
- [ ] [Billing exports and cost analysis](01-environment/1-2-billing-configuration/04-billing-export.md)

## 2.1 — Compute resources

- [ ] [Choosing a compute service](02-planning-and-implementation/2-1-compute-resources/01-compute-selection.md)
- [ ] [Launching VMs and availability policies](02-planning-and-implementation/2-1-compute-resources/02-launch-vm.md)
- [ ] [Persistent Disk, Hyperdisk, and Local SSD](02-planning-and-implementation/2-1-compute-resources/03-vm-disks.md)
- [ ] [Instance templates, MIGs, and autoscaling](02-planning-and-implementation/2-1-compute-resources/04-managed-instance-groups.md)
- [ ] [OS Login and SSH authorization](02-planning-and-implementation/2-1-compute-resources/05-os-login.md)
- [ ] [VM Manager](02-planning-and-implementation/2-1-compute-resources/06-vm-manager.md)
- [ ] [Spot VMs and custom machine types](02-planning-and-implementation/2-1-compute-resources/07-spot-custom-vms.md)
- [ ] [Installing and configuring kubectl](02-planning-and-implementation/2-1-compute-resources/08-kubectl-setup.md)
- [ ] [GKE Standard, Autopilot, regional, and private clusters](02-planning-and-implementation/2-1-compute-resources/09-gke-cluster-design.md)
- [ ] [Deploying a containerized application to GKE](02-planning-and-implementation/2-1-compute-resources/10-gke-app-deployment.md)
- [ ] [Cloud Run functions, Pub/Sub, and Eventarc](02-planning-and-implementation/2-1-compute-resources/11-serverless-events.md)
- [ ] [Choosing GPUs or TPUs](02-planning-and-implementation/2-1-compute-resources/12-gpu-tpu-selection.md)

## 2.2 — Storage and data solutions

- [ ] [Choosing databases, analytics, messaging, and pipelines](02-planning-and-implementation/2-2-storage-and-data-solutions/01-data-service-selection.md)
- [ ] [Object, file, and parallel filesystem storage](02-planning-and-implementation/2-2-storage-and-data-solutions/02-storage-selection.md)
- [ ] [Cloud Storage classes and access costs](02-planning-and-implementation/2-2-storage-and-data-solutions/03-storage-classes.md)
- [ ] [Loading and transferring data](02-planning-and-implementation/2-2-storage-and-data-solutions/04-load-transfer-data.md)
- [ ] [Multi-region redundancy and disaster recovery](02-planning-and-implementation/2-2-storage-and-data-solutions/05-multi-region-data.md)
- [ ] [Cloud SQL: relational applications, HA, and replicas](02-planning-and-implementation/2-2-storage-and-data-solutions/06-cloud-sql.md)
- [ ] [AlloyDB for PostgreSQL](02-planning-and-implementation/2-2-storage-and-data-solutions/07-alloydb.md)
- [ ] [Spanner: distributed relational transactions](02-planning-and-implementation/2-2-storage-and-data-solutions/08-spanner.md)
- [ ] [Firestore: document data and indexes](02-planning-and-implementation/2-2-storage-and-data-solutions/09-firestore.md)
- [ ] [Bigtable: wide-column and row-key design](02-planning-and-implementation/2-2-storage-and-data-solutions/10-bigtable.md)
- [ ] [BigQuery: analytical jobs and cost controls](02-planning-and-implementation/2-2-storage-and-data-solutions/11-bigquery.md)
- [ ] [Pub/Sub: topics, subscriptions, acknowledgments](02-planning-and-implementation/2-2-storage-and-data-solutions/12-pubsub.md)
- [ ] [Dataflow: batch and streaming pipelines](02-planning-and-implementation/2-2-storage-and-data-solutions/13-dataflow.md)
- [ ] [Managed Service for Apache Kafka](02-planning-and-implementation/2-2-storage-and-data-solutions/14-managed-kafka.md)
- [ ] [Memorystore: caching and in-memory access](02-planning-and-implementation/2-2-storage-and-data-solutions/15-memorystore.md)
- [ ] [NetApp Volumes: enterprise file workloads](02-planning-and-implementation/2-2-storage-and-data-solutions/16-netapp-volumes.md)
- [ ] [Managed Lustre: parallel filesystem workloads](02-planning-and-implementation/2-2-storage-and-data-solutions/17-managed-lustre.md)

## 2.3 — Networking resources

- [ ] [Custom VPC, Shared VPC, and VPC Peering](02-planning-and-implementation/2-3-networking-resources/01-vpc-shared-peering.md)
- [ ] [VPC firewall rules and Cloud NGFW policies](02-planning-and-implementation/2-3-networking-resources/02-firewall-rules.md)
- [ ] [Network tags, secure tags, and service accounts](02-planning-and-implementation/2-3-networking-resources/03-firewall-targets.md)
- [ ] [Cloud VPN, Interconnect, and peering](02-planning-and-implementation/2-3-networking-resources/04-hybrid-connectivity.md)
- [ ] [Choosing and configuring load balancers](02-planning-and-implementation/2-3-networking-resources/05-load-balancers.md)
- [ ] [Premium and Standard Network Service Tiers](02-planning-and-implementation/2-3-networking-resources/06-network-service-tiers.md)

## 2.4 — Infrastructure and AI-assisted tooling

- [ ] [Terraform, Fabric FAST, Config Connector, and Helm](02-planning-and-implementation/2-4-infrastructure-and-ai-assisted-tooling/01-infrastructure-as-code.md)
- [ ] [Gemini CLI, Antigravity, Cloud Assist, and Application Design Center](02-planning-and-implementation/2-4-infrastructure-and-ai-assisted-tooling/02-ai-assisted-tools.md)

## 3.1 — Compute operations

- [ ] [Connecting to Compute Engine instances](03-operations/3-1-compute-operations/01-remote-vm-access.md)
- [ ] [Viewing and inspecting running VMs](03-operations/3-1-compute-operations/02-vm-inventory.md)
- [ ] [Snapshots, images, and recovery](03-operations/3-1-compute-operations/03-snapshots-images.md)
- [ ] [Inspecting GKE nodes, Pods, and Services](03-operations/3-1-compute-operations/04-gke-inventory.md)
- [ ] [GKE access to Artifact Registry](03-operations/3-1-compute-operations/05-artifact-registry-access.md)
- [ ] [Managing GKE node pools](03-operations/3-1-compute-operations/06-node-pools.md)
- [ ] [Pods, Deployments, Services, and StatefulSets](03-operations/3-1-compute-operations/07-kubernetes-resources.md)
- [ ] [Horizontal and vertical Pod autoscaling](03-operations/3-1-compute-operations/08-pod-autoscaling.md)
- [ ] [Autopilot Pod resource requests](03-operations/3-1-compute-operations/09-autopilot-requests.md)
- [ ] [Deploying Cloud Run revisions](03-operations/3-1-compute-operations/10-cloud-run-revisions.md)
- [ ] [Traffic splitting and progressive delivery](03-operations/3-1-compute-operations/11-traffic-splitting.md)
- [ ] [Cloud Run autoscaling and concurrency](03-operations/3-1-compute-operations/12-cloud-run-scaling.md)
- [ ] [Attaching and operating GPUs and TPUs](03-operations/3-1-compute-operations/13-attach-accelerators.md)
- [ ] [Deploying and operating Agent Runtime](03-operations/3-1-compute-operations/14-agent-runtime.md)
- [ ] [Workbench and BigQuery notebooks](03-operations/3-1-compute-operations/15-notebooks.md)
- [ ] [Cloud Workstations and developer environments](03-operations/3-1-compute-operations/16-cloud-workstations.md)

## 3.2 — Storage and data operations

- [ ] [Managing and securing Cloud Storage objects](03-operations/3-2-storage-and-data-operations/01-secure-storage-objects.md)
- [ ] [Object lifecycle management](03-operations/3-2-storage-and-data-operations/02-object-lifecycle.md)
- [ ] [Querying Cloud SQL, BigQuery, Bigtable, Spanner, Firestore, and AlloyDB](03-operations/3-2-storage-and-data-operations/03-query-data.md)
- [ ] [Estimating storage and database costs](03-operations/3-2-storage-and-data-operations/04-data-costs.md)
- [ ] [Database backups and restoration](03-operations/3-2-storage-and-data-operations/05-database-backup-restore.md)
- [ ] [Monitoring Dataflow and BigQuery jobs](03-operations/3-2-storage-and-data-operations/06-job-status.md)
- [ ] [Database Center fleet management](03-operations/3-2-storage-and-data-operations/07-database-center.md)
- [ ] [Customer-managed encryption keys](03-operations/3-2-storage-and-data-operations/08-cmek.md)

## 3.3 — Network operations

- [ ] [Expanding a subnet IPv4 range](03-operations/3-3-network-operations/01-resize-subnets.md)
- [ ] [Static internal and external IP addresses](03-operations/3-3-network-operations/02-static-addresses.md)
- [ ] [Custom static routes](03-operations/3-3-network-operations/03-static-routes.md)
- [ ] [Cloud DNS, Cloud NAT, and Private Google Access](03-operations/3-3-network-operations/04-dns-nat.md)
- [ ] [Operating firewall rules and policies](03-operations/3-3-network-operations/05-firewall-operations.md)

## 3.4 — Monitoring and logging

- [ ] [Metric-based alerts and notification channels](03-operations/3-4-monitoring-and-logging/01-metric-alerts.md)
- [ ] [Application metrics and log-based metrics](03-operations/3-4-monitoring-and-logging/02-custom-metrics.md)
- [ ] [Audit logs, VPC Flow Logs, and firewall logs](03-operations/3-4-monitoring-and-logging/03-audit-flow-firewall-logs.md)
- [ ] [Routing logs to BigQuery, Cloud Storage, Pub/Sub, and external systems](03-operations/3-4-monitoring-and-logging/04-log-export.md)
- [ ] [Log buckets, retention, views, and Log Analytics](03-operations/3-4-monitoring-and-logging/05-log-buckets-router.md)
- [ ] [Viewing and filtering logs](03-operations/3-4-monitoring-and-logging/06-logs-explorer.md)
- [ ] [Reading individual log entries and correlating requests](03-operations/3-4-monitoring-and-logging/07-log-details.md)
- [ ] [Trace, Profiler, Query Insights, and index advice](03-operations/3-4-monitoring-and-logging/08-diagnostic-tools.md)
- [ ] [Personalized Service Health](03-operations/3-4-monitoring-and-logging/09-personalized-service-health.md)
- [ ] [Installing and configuring Ops Agent](03-operations/3-4-monitoring-and-logging/10-ops-agent.md)
- [ ] [Managed Service for Prometheus](03-operations/3-4-monitoring-and-logging/11-managed-prometheus.md)
- [ ] [Gemini Cloud Assist for monitoring](03-operations/3-4-monitoring-and-logging/12-gemini-monitoring.md)
- [ ] [Active Assist and resource optimization](03-operations/3-4-monitoring-and-logging/13-active-assist.md)
- [ ] [Cloud Hub events and application health](03-operations/3-4-monitoring-and-logging/14-cloud-hub.md)

## 4.1 — IAM policies and roles

- [ ] [Viewing and creating IAM policies](04-access-and-security/4-1-iam-policies-and-roles/01-iam-policy.md)
- [ ] [Role attachment and policy inheritance](04-access-and-security/4-1-iam-policies-and-roles/02-iam-inheritance.md)
- [ ] [Basic, predefined, and custom IAM roles](04-access-and-security/4-1-iam-policies-and-roles/03-role-types.md)

## 4.2 — Service accounts and workload identity

- [ ] [User-managed accounts and Google-managed service agents](04-access-and-security/4-2-service-accounts-and-workload-identity/01-create-service-accounts.md)
- [ ] [Least privilege for workload identities](04-access-and-security/4-2-service-accounts-and-workload-identity/02-least-privilege-service-accounts.md)
- [ ] [Assigning service accounts to resources](04-access-and-security/4-2-service-accounts-and-workload-identity/03-attach-service-account.md)
- [ ] [Permissions held by versus permissions on a service account](04-access-and-security/4-2-service-accounts-and-workload-identity/04-service-account-policy.md)
- [ ] [Service account impersonation](04-access-and-security/4-2-service-accounts-and-workload-identity/05-impersonation.md)
- [ ] [Short-lived access tokens and ID tokens](04-access-and-security/4-2-service-accounts-and-workload-identity/06-short-lived-credentials.md)
- [ ] [Google Cloud identities for GKE applications](04-access-and-security/4-2-service-accounts-and-workload-identity/07-gke-workload-identity.md)
- [ ] [Workload Identity Federation for external workloads](04-access-and-security/4-2-service-accounts-and-workload-identity/08-external-workload-federation.md)
