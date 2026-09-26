# Exam decision tables

[Study guide home](README.md)

These are recall aids after reading the notes. Always apply every scenario constraint.

## Compute and scaling

| Requirement | Starting choice | Main distinction |
|---|---|---|
| Full OS control or legacy VM application | Compute Engine | You manage guest software |
| Kubernetes APIs and orchestration | GKE | Choose Standard/Autopilot by control needs |
| Managed stateless HTTP container | Cloud Run service | Revisions, concurrency, autoscaling |
| Run a supported container task to completion | Cloud Run job | Job execution differs from request serving |
| Supported event handler | Cloud Run functions/service + trigger | Idempotency and trigger identity matter |
| Managed agentic application runtime | Agent Runtime | Runtime tool/data access still needs IAM |
| More application replicas | HPA or service-specific scaling | Does not necessarily add node capacity |
| More CPU/memory per Pod | VPA/resource sizing | Different from replica count |
| More GKE node capacity | Cluster/node autoscaling | Scheduling constraints still apply |
| Replace an unhealthy MIG VM | Autohealing | Different from demand-based autoscaling |

## Data and storage

| Requirement | Starting choice |
|---|---|
| Conventional MySQL/PostgreSQL/SQL Server | Cloud SQL |
| Demanding PostgreSQL-compatible workload | Compare AlloyDB and Cloud SQL |
| Horizontally scalable relational transactions | Spanner |
| Document-oriented application data | Firestore |
| High-throughput key/range-oriented wide-column data | Bigtable |
| Large analytical SQL scans | BigQuery |
| In-memory caching | Memorystore |
| Decoupled event delivery | Pub/Sub |
| Kafka API/ecosystem requirement | Managed Service for Apache Kafka |
| Batch/stream transformation | Dataflow |
| Named object storage | Cloud Storage |
| Shared NFS | Filestore |
| Supported enterprise NFS/SMB requirements | NetApp Volumes |
| High-throughput parallel filesystem | Managed Lustre |
| Durable VM block disk | Persistent Disk/Hyperdisk |
| Disposable fast VM scratch | Local SSD |

## Networking

| Requirement/problem | Relevant feature |
|---|---|
| Central network team, separate application projects | Shared VPC |
| Connect separate compatible VPCs privately | VPC Peering; understand nontransitivity |
| Encrypted hybrid tunnel | Cloud VPN |
| Dedicated/partner hybrid connectivity | Interconnect; analyze encryption separately |
| Private VM initiating public internet traffic | Public Cloud NAT with required routing |
| Eligible private resources accessing Google APIs | Private Google Access or appropriate private API design |
| Host/path routing for HTTP(S) | Application Load Balancer |
| Supported TCP/UDP transport distribution | Appropriate Network Load Balancer |
| Resolve names privately inside selected VPCs | Cloud DNS private zone |
| Permit a specific connection | Effective firewall rule/policy |
| Select where packets go | Route |

## Identity

| Need | Mechanism |
|---|---|
| Human access via external identity provider | Workforce Identity Federation |
| External software/CI access without stored keys | Workload Identity Federation |
| GKE workload access to Google APIs | Workload Identity Federation for GKE |
| Deployer attaches a service account | Appropriate actAs permission, commonly Service Account User |
| Caller mints credentials as an account | Appropriate impersonation/token-creation permission |
| Runtime reads an object | Role on the target bucket/resource |
| Central Linux login access | OS Login |
| Restrict allowed resource configuration | Organization Policy |
| Grant an action to an identity | IAM role binding |

## Operations and recovery

| Question | Evidence/control |
|---|---|
| Who changed a resource? | Cloud Audit Logs |
| Which network flows occurred? | VPC Flow Logs with sampling caveats |
| Which supported firewall rule matched? | Firewall Rules Logging |
| Which downstream call is slow? | Trace |
| Which code consumes CPU/memory? | Profiler |
| Why is a database query slow? | Query Insights/query execution and index tools |
| Is a provider incident affecting us? | Personalized Service Health |
| What is this application's consolidated health? | Cloud Hub |
| What resources exist? | Cloud Asset Inventory |
| Recover before accidental data corruption | Backup/PITR |
| Continue through a supported infrastructure failure | HA/replication/failover design |
| Control ownership/lifecycle of encryption keys | CMEK with Cloud KMS |

## Traps worth explaining aloud

1. A budget sends alerts; it is not a guaranteed hard spending cap.
2. A project-level Viewer grant does not remove a stronger inherited allow grant.
3. API enablement, IAM permission, quota, capacity, and location availability are separate checks.
4. Multi-zone does not equal multi-region.
5. A replica is not a historical backup.
6. A static IP does not make an application healthy or bypass firewalls.
7. A running Pod/VM is not proof of application readiness.
8. A Service selector must match appropriate ready Pods.
9. Image-pull identity and application runtime identity can differ.
10. A service account's own policy is different from roles held by that account elsewhere.
11. Route permission and firewall permission are different.
12. Cloud Storage class and bucket location are separate choices.
13. Archive storage is online, with retrieval/minimum-duration economics.
14. A Terraform plan previews; apply changes; state maps configuration to managed resources.
15. AI-generated advice needs verification against actual requirements and telemetry.

See individual notes for official references and qualifications.
