# ACE operations and command patterns

Section 3 is approximately 30% of the [official exam guide](https://services.google.com/fh/files/misc/associate_cloud_engineer_exam_guide_english.pdf). Commands here illustrate the action and scope, not a script to execute unchanged. `PROJECT_ID`, `REGION`, `ZONE`, `CLUSTER`, `VM`, `BUCKET`, and `SA_EMAIL` are placeholders. CLI flags and products evolve; use `gcloud help COMMAND` for exact syntax.

## Start by checking context

```bash
gcloud auth list
gcloud config list
gcloud config set project PROJECT_ID
gcloud services list --enabled
gcloud compute regions list
gcloud compute zones list
gcloud compute project-info describe
```

The active project, identity, region/zone, enabled APIs, and quota are first checks when a command fails. Cloud Shell comes with tools; a local workstation needs gcloud installation/authentication. Application Default Credentials and gcloud CLI login are related but distinct credential contexts.

## Setup, billing, and access

```bash
gcloud projects describe PROJECT_ID
gcloud services enable compute.googleapis.com --project=PROJECT_ID
gcloud projects get-iam-policy PROJECT_ID
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member="group:ops@example.com" --role="roles/viewer"
gcloud iam service-accounts create workload-sa \
  --display-name="Workload service account"
gcloud iam service-accounts list
```

For billing links/budgets/exports, use the Cloud Billing console or relevant APIs and check account permissions. For a denied operation: identify principal → resource → required permission → inherited bindings/conditions → organization policy → API/quota. Avoid assigning Editor/Owner just to make a command work. Cloud Asset Inventory helps inspect resources across scope.

## Compute Engine and disks

```bash
gcloud compute instances list --project=PROJECT_ID
gcloud compute instances describe VM --zone=ZONE
gcloud compute ssh VM --zone=ZONE
gcloud compute disks list
gcloud compute snapshots list
gcloud compute images list
gcloud compute disks snapshot DISK --zone=ZONE --snapshot-names=SNAPSHOT_NAME
gcloud compute instance-templates list
gcloud compute instance-groups managed list
```

If SSH fails, check VM status, OS Login/IAM or SSH keys, network route/firewall or IAP path, and guest agent. Snapshot schedules automate disk backups; images are reusable VM boot sources. For MIG upgrades, create/change an instance template and start a controlled rollout; inspect health checks and autoscaling. For availability, distinguish zonal versus regional MIG and storage design. VM Manager helps patch/inventory fleets. Spot VMs can terminate; checkpoint work.

## GKE and Kubernetes

```bash
gcloud container clusters list
gcloud container clusters get-credentials CLUSTER --region=REGION --project=PROJECT_ID
kubectl get nodes
kubectl get pods -A
kubectl get services -A
kubectl get deployments -A
kubectl describe pod POD -n NAMESPACE
kubectl logs POD -n NAMESPACE
kubectl rollout status deployment/APP -n NAMESPACE
kubectl rollout undo deployment/APP -n NAMESPACE
gcloud container node-pools list --cluster=CLUSTER --region=REGION
```

Use `--zone=ZONE` for a zonal cluster. The kubeconfig context from `get-credentials` determines where `kubectl` runs. A Service has a stable endpoint; a Deployment manages replicas; a StatefulSet maintains stable Pod identity and storage. For a Pending Pod, read Events via `describe`: insufficient CPU/memory, resource quota, node selector/taint, persistent volume, and Pod admission are different fixes. For ImagePullBackOff, check image URL/tag, Artifact Registry permissions, and network. For CrashLoopBackOff, inspect previous logs, probes, configuration, and dependencies. HPA scales Pods from metrics; cluster autoscaler scales Standard node pools when Pods cannot be scheduled. Autopilot provisions nodes and uses Pod resource requests. Workload Identity Federation for GKE avoids exported service-account keys.

## Cloud Run and event handling

```bash
gcloud run services list --region=REGION
gcloud run services describe SERVICE --region=REGION
gcloud run revisions list --service=SERVICE --region=REGION
gcloud run deploy SERVICE --image=IMAGE_URL --region=REGION
gcloud run services update-traffic SERVICE \
  --to-revisions=REVISION_A=90,REVISION_B=10 --region=REGION
```

Check revision status, logs, traffic percentages, min/max instances, concurrency, ingress, and the runtime service account. A new revision does not mean all users must immediately receive it. Eventarc connects supported event providers to destinations; verify trigger filters, region, invoker identity, and destination permissions. Review [Cloud Run traffic management](https://docs.cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration).

## Storage, data, and encryption

```bash
gcloud storage buckets list
gcloud storage ls gs://BUCKET
gcloud storage cp ./file.txt gs://BUCKET/path/file.txt
gcloud storage buckets describe gs://BUCKET
gcloud sql instances list
bq ls
bq query --use_legacy_sql=false 'SELECT 1 AS test'
```

Bucket IAM controls access at bucket scope; object access and uniform bucket-level access settings matter. Lifecycle rules transition or delete by conditions; retention policy may prevent deletion until its duration ends. Soft delete/versioning and lifecycle have distinct recovery/cost implications. For database incidents, check instance health, connections, replica lag, query performance, backup coverage, and restore target. Use Database Center for fleet visibility; BigQuery/Dataflow job status for failed batch/stream processing. CMEK failure can be a KMS IAM, disabled key, or unavailable key issue; avoid deleting keys needed to decrypt data. Verify recovery in a test environment.

## VPC and connectivity

```bash
gcloud compute networks list
gcloud compute networks subnets list
gcloud compute firewall-rules list
gcloud compute routes list
gcloud compute addresses list
gcloud compute routers list
gcloud compute routers nats list --router=ROUTER --region=REGION
```

Trace a failed connection in order: source and destination IP/port → DNS resolution → VPC/subnet and routes → relevant firewall/NGFW policy and targets → NAT/VPN/Interconnect/load balancer → application listener. For private VM outbound internet, check Cloud NAT on the relevant regional router and route/firewall. For public inbound, check frontend/forwarding rule, backend health, firewall, and application. Expanding a subnet's primary IPv4 range must respect existing address ranges and product constraints. A static IP reservation keeps an address stable, but does not create a route or open a port. See [VPC overview](https://docs.cloud.google.com/vpc/docs/overview).

## Observability and diagnosis

| Symptom/question | Start with | Next tool |
|---|---|---|
| Service latency or errors | Monitoring metrics/dashboard and alert incident | Logs Explorer; Trace for slow request path; Profiler for CPU/memory use |
| Who changed a resource? | Cloud Audit Logs | Filter principal, method, resource, time; check log type/availability |
| Traffic blocked or dropped | Firewall rule logs / VPC Flow Logs | Rule priority, route, target, source, destination |
| SQL latency | Cloud SQL metrics or Query Insights | Query/index investigation; index advisor where supported |
| Spend or underused capacity | Billing reports/export, Active Assist | Quotas and utilization; verify recommendations |
| Broad service event | Personalized Service Health, Cloud Hub | Scope affected projects and workloads |

Cloud Monitoring ingests built-in and custom metrics; metric-based alert policies notify on conditions. A logs-based metric converts matching log entries into a metric that can drive an alert. Cloud Logging uses Logs Explorer to filter/detail entries, log buckets to store them, and Log Router sinks to route to supported destinations such as BigQuery, Cloud Storage, or Pub/Sub. `_Required` and `_Default` have different handling; know Data Access audit logs may require enabling and can have cost implications. VPC Flow Logs are connection metadata, not packet captures. Ops Agent collects VM telemetry. Managed Service for Prometheus handles Prometheus metrics. Gemini Cloud Assist can assist monitoring analysis; validate its findings. See [log routing](https://docs.cloud.google.com/logging/docs/routing/overview), [audit logs](https://docs.cloud.google.com/logging/docs/audit), and [Monitoring](https://docs.cloud.google.com/monitoring/docs/monitoring-overview).

Example Logs Explorer filter (adapt resource and time):

```text
resource.type="gce_instance"
severity>=ERROR
```

Example incident workflow for an SRE: verify user impact and time window; inspect alert metric and affected resource; correlate recent deployments/configuration changes and audit logs; isolate network versus application versus dependency; mitigate or roll back; then document cause and add a targeted alert. This maps well to exam scenarios that ask for the *next best operational action*.

## IAM and federation troubleshooting

A workload needs both an identity and a permission. Check which service account the VM, GKE Pod, or Cloud Run service actually runs as. Then check resource-level IAM, relevant conditional bindings, API enablement, and any organization policy. For a local operator, `gcloud --impersonate-service-account=SA_EMAIL ...` uses short-lived credentials when the operator has permission to impersonate. For an external CI job, Workload Identity Federation can exchange its identity provider token; for a GKE Pod, use Workload Identity Federation for GKE. Keep a clear distinction between `roles/iam.serviceAccountUser` (attach/actAs) and `roles/iam.serviceAccountTokenCreator` (mint short-lived credentials), applying only the required role for the action. See [impersonation](https://docs.cloud.google.com/iam/docs/service-account-impersonation).
