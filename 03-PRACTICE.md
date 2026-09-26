# ACE scenario practice

Original study questions based on the [official exam guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf). These are **not actual exam questions**. Choose the best answer unless marked “select two.” Work closed book, then explain why the alternatives are weaker.

## Questions

1. An engineer with Project Owner cannot create a VM with an external IP. Which item should they inspect first? A. Billing budget alert B. Organization policy restricting external IPs C. Cloud Storage lifecycle D. BigQuery job history
2. A new project must send alerts at 80% of expected monthly spend. What do you set? A. Quota B. Budget with alert thresholds C. Firewall D. Service perimeter
3. A web API ships as a container and needs automatic scale with minimal cluster management and a 5% canary. Choose: A. Single VM B. Cloud Run revisions and traffic split C. Filestore D. Bigtable
4. A team needs Kubernetes controllers and explicit node pool configuration. Choose: A. Cloud Run B. GKE Standard C. BigQuery D. Cloud Storage
5. A regional GKE cluster has too few nodes when more Pods are scheduled; an HPA already increases replicas. Which mechanism adds nodes in Standard mode? A. PodDisruptionBudget B. Node-pool cluster autoscaler C. Object lifecycle D. Cloud NAT
6. A small SQL Server application needs a managed relational instance and backups. Choose: A. Cloud SQL B. Bigtable C. BigQuery D. Pub/Sub
7. A globally distributed application requires scalable relational transactions. Choose: A. BigQuery B. Spanner C. Memorystore D. Cloud Storage
8. Analysts run large SQL aggregations across historical events. Choose: A. BigQuery B. Cloud SQL primary C. Firestore D. Filestore
9. A telemetry service retrieves rows by device key at high volume and low latency. Choose: A. Bigtable B. BigQuery C. Cloud Storage Archive D. Filestore
10. Logs must be kept cheaply and are expected to be accessed less than once a year. Which Cloud Storage class is most aligned with long retention? A. Standard B. Nearline C. Archive D. Regional Persistent Disk
11. Private VMs need outbound access to update packages but must keep no external IP. Choose: A. Cloud NAT B. Cloud DNS alone C. External load balancer D. VPC Peering alone
12. Two projects need centralized network administration with workloads in service projects. Choose: A. Shared VPC B. Public IPs C. BigQuery D. Cloud Trace
13. A production network denies traffic despite a valid route. What should you inspect next? A. Cloud Storage class B. Firewall/NGFW policy and matching target/priority C. VM image family D. Budget
14. A VM disk must be recoverable to a prior point in time. Choose: A. Snapshot B. HPA C. VPC Flow Logs D. Cloud DNS
15. A GKE Pod cannot pull an Artifact Registry image. Which pair is most relevant? A. Image reference and repository IAM for pulling identity B. Billing budget and DNS zone C. Backup and Spanner schema D. Archive class and lifecycle
16. Users report slow requests after a rollout. Which pair best investigates latency and the change? A. Cloud Trace and revision/deployment history B. Storage Transfer Service and Filestore C. Cloud NAT and Archive D. Billing account and Cloud Identity group
17. The security team needs a queryable export of selected logs to BigQuery. Choose: A. Log Router sink B. HPA C. Disk snapshot D. Instance template
18. An external CI system needs Google Cloud access without downloaded service-account keys. Choose: A. Workload Identity Federation B. Cloud VPN C. Persistent Disk D. Cloud DNS
19. A developer needs to attach a specific service account to a VM; the service account already has bucket read access. What is the separate permission concern? A. Permission to act as the service account B. Archive retrieval fee C. Route priority D. HPA target
20. Which option identifies users from an external identity provider for Google Cloud console access? A. Workforce Identity Federation B. Workload Identity Federation for CI C. Cloud NAT D. Managed instance group
21. **Select two.** A Cloud Run revision has errors after deployment and only 10% of traffic is directed to it. Which actions directly limit user impact and aid investigation? A. Send the 10% back to the prior healthy revision B. Inspect revision logs and metrics C. Delete the billing account D. Widen all IAM roles
22. **Select two.** A private VM cannot reach an internet package repository. Which checks directly matter? A. Applicable Cloud NAT/route configuration B. Egress firewall/NGFW policy C. BigQuery partitioning D. IAM custom role description
23. The company wants to compare resource inventory across projects and analyze configurations. Choose: A. Cloud Asset Inventory B. Cloud SQL backup C. Cloud Run traffic split D. Persistent Disk snapshot
24. A batch application is restartable and tolerates interruptions. The team wants reduced compute cost. Choose: A. Spot VMs B. Sole production VM with no backup C. Archive objects as compute D. Dedicated Interconnect
25. A database has a highly available replica but data was accidentally deleted and replicated. What is still necessary? A. Tested backups/point-in-time recovery where supported B. More VPC routes C. Additional log sink only D. A static IP

## Answers and reasoning

| # | Answer | Why |
|---:|:---:|---|
| 1 | B | Organization policy can constrain a Project Owner. Check the exact policy and error. |
| 2 | B | Budgets monitor spend; quotas limit resource/API consumption. |
| 3 | B | Cloud Run scales managed containers and splits traffic among revisions. |
| 4 | B | Standard exposes node-pool and Kubernetes control choices. |
| 5 | B | HPA increases Pods; cluster autoscaler adds Standard nodes. |
| 6 | A | Cloud SQL supports managed SQL Server and backups. |
| 7 | B | Spanner targets scalable relational transactions. |
| 8 | A | BigQuery is an analytical warehouse. |
| 9 | A | Bigtable is designed for high-throughput, low-latency key access. |
| 10 | C | Archive fits rare access/long retention; evaluate minimum-duration and retrieval costs. |
| 11 | A | Cloud NAT provides outbound translation to eligible private resources. |
| 12 | A | Shared VPC centralizes a host project's network across service projects. |
| 13 | B | A route is necessary but does not grant network permission. |
| 14 | A | A snapshot captures disk state for restoration. |
| 15 | A | Wrong image reference or pull identity/repository IAM can block pulls. |
| 16 | A | Trace shows latency path; rollout history ties change to symptom. |
| 17 | A | Log Router sinks route selected entries to BigQuery. |
| 18 | A | Federation exchanges trusted external credentials for short-lived access. |
| 19 | A | Attaching/acting as the identity is separate from its bucket read role. |
| 20 | A | Workforce federation covers human workforce identities. |
| 21 | A, B | Roll traffic back and inspect the failing revision. |
| 22 | A, B | Outbound path needs routing/NAT and matching policy permission. |
| 23 | A | Asset Inventory is designed for cross-resource inventory and analysis. |
| 24 | A | Spot VMs suit interruptible, checkpointable work. |
| 25 | A | HA may replicate a bad write; recovery needs backups/PITR when available. |

## Self-check prompts

Explain each in one minute: (1) IAM role versus organization policy, (2) GKE HPA versus node autoscaling, (3) Cloud SQL versus Spanner versus BigQuery, (4) Cloud NAT versus external load balancer, (5) metrics versus logs versus traces, (6) workload versus workforce federation, (7) availability versus backup, (8) Standard/Nearline/Coldline/Archive, (9) Shared VPC versus Peering, (10) Cloud Run revision rollback.

**Score interpretation:** 21–25: revisit explanations for misses; 16–20: repeat the decision tables and operational flow; 0–15: focus on the four guide sections and retake after hands-on review. This is a study diagnostic, not a prediction of exam outcome.
