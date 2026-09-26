# ACE concept and service-selection guide

Use the [official guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf) as the checklist. Each decision below asks what the workload needs, what scope applies, and what you would operate afterward.

## 1. Set up the environment (~20%)

### Hierarchy, identity, and governance

`Organization → folders → projects → resources`. A project has a project ID (chosen, permanent), project number (assigned), and display name (changeable). IAM allow-policy bindings grant a principal a role at a resource and generally pass down to descendants. A folder role can therefore affect many projects. An organization policy is a constraint on allowed configurations, not a substitute for an IAM permission. Evaluate both when an administrator appears authorized but a resource operation fails. Use groups for human access; Cloud Identity manages users/groups; workforce identity federation lets external human identities access Google Cloud without copying each user into Cloud Identity. Workload identity federation is for external **workloads**, a separate use case.

A new project task often needs these checks: organization/folder placement; billing link; API enablement; quotas; region/zone support; VPC/subnet; observability; IAM; organization policies. APIs are enabled per project. Quotas can be project, region, or other scopes and are distinct from billing budgets. A budget alerts on spend; it does not automatically stop resource usage. Billing export supports detailed analysis, commonly to BigQuery. Cloud Asset Inventory answers inventory/history/search questions; Gemini Cloud Assist can help analyze resources, but verify proposed changes. A standalone organization and Cloud Identity setup are administrative setup topics in the guide.

**Scenario:** A project owner cannot create an external IP. Check applicable organization policy and quota before adding another IAM role. A billing alert is not a quota increase.

### Locations and resilience

A zone is a deployment area within a region. Put independent replicas in different zones for a zonal failure; use regional resources where the product supports them. Multi-region data placement addresses a wider failure domain, but the exact replication/backup settings differ by product. Check product availability in the desired location. Decide *failure domain, RPO, RTO, and data residency* before choosing a region or replica design. A snapshot/backup helps recovery; it does not by itself keep a service running during an outage.

## 2. Plan and implement (~30%)

### Compute selection

| Requirement | Likely choice | Key distinction |
|---|---|---|
| Guest OS, custom software, full VM control | Compute Engine | You manage VM configuration and lifecycle. |
| Identical VMs, autoscaling and rolling updates | Managed instance group (MIG) + instance template | Template defines instances; autoscaler sizes the group. |
| Kubernetes objects, controllers, custom cluster behavior | GKE Standard | You configure node pools and more cluster infrastructure. |
| Kubernetes with less node management | GKE Autopilot | GKE manages node provisioning; specify sensible Pod requests. |
| Managed containerized web/API app or job | Cloud Run | Revision based deployments; set scaling, ingress, and traffic split. |
| Function style event handler | Cloud Run functions | Integrate events through supported triggers/Eventarc. |
| Managed agent execution | Agent Runtime on Gemini Enterprise Agent Platform | Recognize the guide's newer product name and former Vertex AI Agent Engine name. |
| Fault-tolerant interruptible work | Spot VM | Can be preempted; never rely on it alone for strict continuity. |

For VMs, compare custom machine types, availability/maintenance policy, SSH keys versus OS Login, and VM Manager for fleet management. OS Login centralizes SSH authorization with IAM. Instance templates are the immutable blueprint used by MIGs; update by creating a new template and rolling it out. Zonal Persistent Disk is scoped to a zone; regional Persistent Disk replicates across two zones in a region for supported configurations; Hyperdisk offers performance-oriented variants. Match attachment and replication details to the workload. GPUs favor broadly supported accelerated workloads; TPUs are specialized for supported ML workloads and frameworks—check availability and compatibility.

For GKE, distinguish a regional cluster (regional control plane and nodes configured across zones), a private cluster (restricted node/control-plane network exposure choices), Standard, and Autopilot. A Deployment manages stateless Pod replicas and rollout; a StatefulSet manages stable identity/storage for stateful Pods; a Service gives stable network access. A Kubernetes ServiceAccount is not a Google Cloud IAM service account. Horizontal Pod Autoscaler changes Pod replica count; Vertical Pod Autoscaler adjusts resource recommendations/requests; node-pool autoscaling changes nodes. A pending Pod may be due to capacity, quotas, taints, requests, image pull, or permissions. Artifact Registry access depends on node/workload identity and repository permissions. In Autopilot, resource requests affect placement and cost.

For Cloud Run, each deployment creates a revision. Splitting traffic allows gradual rollout or rollback; min/max instances and concurrency influence latency, cost, and scaling. Events from Pub/Sub or Cloud Storage can be delivered through Eventarc or product-specific mechanisms; confirm event source, region, and permissions. The [compute options guide](https://docs.cloud.google.com/docs/compute-area/choose-compute-options) is the decision reference.

### Data and storage selection

| Need | Likely product | Watch for |
|---|---|---|
| Durable object files, backups, static assets | Cloud Storage | Bucket location/class, IAM, lifecycle, versioning/soft delete, encryption. |
| Shared NFS file system | Filestore | File semantics rather than objects. |
| Enterprise managed file volumes | Google Cloud NetApp Volumes | Protocol/performance and migration requirements. |
| High-performance managed Lustre file system | Managed Lustre | Specialized HPC/AI file workload. |
| Conventional MySQL/PostgreSQL/SQL Server | Cloud SQL | HA, backups, connection handling; limited horizontal write scaling. |
| PostgreSQL-compatible high-performance relational | AlloyDB | Relational and PostgreSQL compatibility requirement. |
| Global/horizontal relational transactions | Spanner | Large-scale strongly consistent relational design. |
| Flexible document database | Firestore | Document/query model and indexes. |
| Massive low-latency key/wide-column access | Bigtable | Access patterns and row-key design matter. |
| Analytical SQL over large data | BigQuery | Warehouse/analytics, not a transactional OLTP database. |
| In-memory cache | Memorystore | Cache is not the durable system of record. |
| Event buffering/decoupling | Pub/Sub or managed Apache Kafka | Match messaging protocol and ecosystem. |
| Stream/batch data processing | Dataflow | Pipeline processing, not primary storage. |

Cloud Storage class is an access/cost choice, not a different API: Standard for frequent access; Nearline for infrequent access (30-day minimum); Coldline for rarer access (90-day minimum); Archive for long retention (365-day minimum). Retrieval and early-deletion charges can matter. Lifecycle rules can transition/delete objects based on conditions. Evaluate location independently of class; a multi-region bucket and an Archive object answer different questions. Upload with CLI; use Storage Transfer Service for large/scheduled source transfers; BigQuery can load from Cloud Storage. Use backups and restoration tests for databases; HA and replicas do not replace backup. CMEK gives customer control of key lifecycle through Cloud KMS and introduces key-permission/availability dependencies. See [storage classes](https://docs.cloud.google.com/storage/docs/storage-classes) and [database products](https://docs.cloud.google.com/docs/databases).

### Networking selection

VPC networks are global and subnets are regional. Custom mode VPC lets you explicitly choose subnet CIDRs. Shared VPC lets service projects use a host project's centrally managed network. VPC Peering joins two VPC networks for private connectivity but is not transitive; it does not replace Shared VPC governance. A route selects where packets go; a firewall/Cloud NGFW policy decides whether traffic is permitted. Ingress applies to traffic entering a target resource; egress applies to traffic leaving it. Rule decisions depend on priority, direction, action, target, source/destination, protocol and port. Check applicable hierarchical/network firewall policies and secure Tags/service-account targeting. Secure Tags are governed resources, distinct from plain network tags.

| Need | Choose | Key condition |
|---|---|---|
| Private VM outbound internet, no external IP | Cloud NAT | NAT handles outbound connections; firewall/routing still apply. |
| Private tunnel to on-premises | Cloud VPN | Encrypted IPsec over internet. |
| Dedicated private connectivity | Cloud Interconnect | Capacity and physical connectivity requirements. |
| Private connectivity between VPCs | Peering or Shared VPC | Different ownership/control patterns. |
| Distribute inbound requests | Load balancer | Choose application (HTTP/S) versus network (TCP/UDP), internal versus external, and scope. |
| Names to addresses | Cloud DNS | Public versus private zone selection. |
| Control internet path/performance/cost | Network Service Tiers | Premium versus Standard is about external traffic path and product support. |

Subnet primary range can be expanded within constraints; review overlaps, existing allocations, secondary ranges, and dependent resources. Reserve static internal/external IPs when stable addresses are required. Cloud NAT is not a way for internet clients to initiate inbound traffic to private VMs. VPC Flow Logs show sampled connection metadata; firewall rule logging records matching decisions when enabled. See [VPC documentation](https://docs.cloud.google.com/vpc/docs/overview).

### Deployment tooling and AI

Terraform declares infrastructure and tracks state; use reviewable plans and controlled state. Config Connector manages Google Cloud resources as Kubernetes resources; Helm packages Kubernetes manifests; Fabric FAST is a Google Cloud foundation automation toolkit. Gemini CLI, Google Antigravity, Gemini Cloud Assist, and Application Design Center appear in the guide as AI-assisted planning/implementation tools. Know their purpose at recognition level, review generated code/plans, and do not assume generated recommendations have IAM approval.

## 4. Access and security (~20%)

A principal (user, group, service account, federated principal) gets a role, which groups permissions, at a resource scope. Basic roles are broad; predefined roles are service/task oriented; custom roles contain selected supported permissions. Start with least privilege at the smallest useful scope. A service account has two distinct relationships: **who may act as or manage it**, and **which resources it may access**. Granting a human Service Account User does not itself grant the service account access to a bucket. Attaching a service account to a VM and using its identity can avoid static keys. Service account impersonation mints short-lived credentials when caller permission allows. Workload Identity Federation lets external jobs exchange trusted credentials for access; Workload Identity Federation for GKE maps Kubernetes workload identities to Google Cloud access; Workforce Identity Federation is for people. Google-managed service agents perform actions for services; avoid stripping their required permissions. See [IAM overview](https://docs.cloud.google.com/iam/docs/overview), [service account impersonation](https://docs.cloud.google.com/iam/docs/service-account-impersonation), [workload federation](https://docs.cloud.google.com/iam/docs/workload-identity-federation), and [GKE workload identity](https://docs.cloud.google.com/kubernetes-engine/docs/concepts/workload-identity).

## Common exam traps

- Project Owner does not override an organization policy, unavailable API, location, or quota.
- A budget alert does not cap costs. A quota is a service limit, not a spending plan.
- VM node scaling, Pod HPA, and Cloud Run instance scaling are separate controls.
- A private subnet does not automatically provide NAT; a route does not grant firewall permission.
- BigQuery is analytical; Bigtable is low-latency wide-column; Spanner is relational transactions at scale.
- Cloud Storage class describes access pattern; bucket location describes geography.
- An image is a VM boot template; a disk snapshot is a point-in-time disk backup; a database backup uses database recovery features.
- IAM grants access; organization policy limits configurations; audit logs record activity.
- Replication/HA addresses availability; backups address recoverability; test both against RPO/RTO.
