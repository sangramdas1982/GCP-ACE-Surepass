# Hands-on exercises

[Study guide home](README.md)

These are short learning labs, not scripts already run against your account. Use a disposable project or temporary Google Skills lab. You need appropriate permissions, enabled APIs, and billing where required. Commands can create charges. Budgets only alert. Clean up after recording what you learned; review retained disks, snapshots, IPs and soft-deleted data.

## Lab 1 — Scope and identity, about 15 minutes

```bash
gcloud auth list
gcloud config list
gcloud projects list
gcloud services list --enabled --project=PROJECT_ID
```

Record the active account and the intended project ID. Explain why using the wrong project can look like a missing resource or disabled API. In the console, identify the same project and compare its display name and number.

**Pass condition:** you can state the exact target and identity before every later command. No cloud cleanup is needed for these inspections.

## Lab 2 — Private VM and controlled SSH, about 40 minutes

Prerequisites: an authorized sandbox, billing, Compute Engine API enablement permission, VM/network creation permission, OS Login permission, and IAP tunnel access. If your organization blocks any choice, use an assigned training lab instead of weakening its policy.

Set values explicitly. Singapore is an example location, not a requirement; use a permitted available region/zone in your lab.

```bash
export ACE_PROJECT='REPLACE_WITH_YOUR_LAB_PROJECT_ID'
export ACE_REGION='asia-southeast1'
export ACE_ZONE='asia-southeast1-b'

gcloud config set project "$ACE_PROJECT"
gcloud services enable compute.googleapis.com iap.googleapis.com

gcloud compute networks create ace-study-vpc --subnet-mode=custom

gcloud compute networks subnets create ace-study-subnet   --network=ace-study-vpc --region="$ACE_REGION" --range=10.20.0.0/24

gcloud compute firewall-rules create ace-study-iap-ssh   --network=ace-study-vpc --direction=INGRESS --action=ALLOW   --rules=tcp:22 --source-ranges=35.235.240.0/20 --target-tags=ace-study-ssh

gcloud compute instances create ace-study-vm   --zone="$ACE_ZONE" --machine-type=e2-micro   --subnet=ace-study-subnet --no-address --no-service-account   --image-family=debian-12 --image-project=debian-cloud   --tags=ace-study-ssh --metadata=enable-oslogin=TRUE

gcloud compute ssh ace-study-vm --zone="$ACE_ZONE" --tunnel-through-iap
```

This VM deliberately has no workload service account and no external IP. The lab tests administration, not application access to Google APIs. Once connected, inspect the hostname and basic OS information, then exit. If the connection fails, distinguish IAP permission, OS Login permission, firewall, and SSH service issues.

**Pass condition:** explain what each command creates, why the subnet is regional, why the firewall targets a tag, and why IAP access does not itself grant OS login.

**Cleanup — only these lab resources:**

```bash
gcloud compute instances delete ace-study-vm --zone="$ACE_ZONE"
gcloud compute firewall-rules delete ace-study-iap-ssh
gcloud compute networks subnets delete ace-study-subnet --region="$ACE_REGION"
gcloud compute networks delete ace-study-vpc
```

Read deletion prompts and check whether the boot disk was deleted. Use `gcloud compute disks list` to inspect leftovers. Do not delete unrelated disks.

## Lab 3 — Private object storage, about 25 minutes

Prerequisites: a sandbox project and permission to create/manage a bucket. Bucket names are globally unique. Do not use sensitive data.

```bash
export ACE_BUCKET='REPLACE_WITH_A_GLOBALLY_UNIQUE_LAB_BUCKET'
export ACE_REGION='asia-southeast1'
printf 'hello from ACE practice\n' > ace-sample.txt

gcloud storage buckets create "gs://$ACE_BUCKET"   --location="$ACE_REGION" --uniform-bucket-level-access

gcloud storage cp ace-sample.txt "gs://$ACE_BUCKET/ace-sample.txt"
gcloud storage ls "gs://$ACE_BUCKET"
gcloud storage cat "gs://$ACE_BUCKET/ace-sample.txt"
gcloud storage buckets describe "gs://$ACE_BUCKET"
```

Inspect IAM, default class, location, soft delete, and retention in the console. Explain how a lifecycle policy could move older objects to a colder class. Write the policy on paper; do not lock a retention policy for this exercise.

**Pass condition:** retrieve the exact test content and explain why an unauthenticated user should not automatically be able to do so.

**Cleanup:**

```bash
gcloud storage rm "gs://$ACE_BUCKET/ace-sample.txt"
gcloud storage buckets delete "gs://$ACE_BUCKET"
```

Check configured soft-delete/retention behavior. Visible deletion can retain recoverable data for a period and may retain associated cost. Remove your local sample file when no longer needed.

## Lab 4 — Kubernetes deployment diagnosis, about 35 minutes

Use an existing **training** GKE cluster; creating a dedicated cluster can add cost and setup time. Fetch its credentials, check the context, and use a separate namespace. The example image is a Google sample; confirm repository availability if an image-pull failure occurs.

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: ace-study
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
  namespace: ace-study
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
      - name: hello
        image: us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: 250m
            memory: 512Mi
          limits:
            cpu: 500m
            memory: 512Mi
        readinessProbe:
          httpGet:
            path: /
            port: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: hello
  namespace: ace-study
spec:
  selector:
    app: hello
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP
```

Save as `ace-deployment.yaml`, apply it, then inspect:

```bash
kubectl apply -f ace-deployment.yaml
kubectl get pods,services -n ace-study
kubectl rollout status deployment/hello -n ace-study
kubectl port-forward service/hello 8080:80 -n ace-study
```

While port forwarding runs, open `http://localhost:8080`. No external load balancer is requested. Stop port forwarding afterward. Inspect a Pod's events and logs. Optional: deliberately use a nonexistent image tag, observe the pull failure, then restore `1.0` and verify recovery.

**Pass condition:** explain Pod/Deployment/Service, selector matching, request sizing, and readiness.

**Cleanup:** `kubectl delete namespace ace-study` deletes resources in that lab namespace. Verify no needed resources were placed there. A preexisting training cluster remains; if you created a separate cluster, delete it through its own supported workflow.

## Lab 5 — Terraform reading and planning, about 25 minutes

This is a small configuration example. It is not necessary to apply it to understand the workflow.

```hcl
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
    }
  }
}

variable "project_id" { type = string }
variable "bucket_name" { type = string }

provider "google" {
  project = var.project_id
  region  = "asia-southeast1"
}

resource "google_storage_bucket" "study" {
  name                        = var.bucket_name
  location                    = "ASIA-SOUTHEAST1"
  uniform_bucket_level_access = true
  force_destroy               = false
  labels = {
    purpose = "ace-study"
  }
}
```

Explain each block, choose an organization-compatible location, and supply a globally unique bucket name. If you initialize locally, use suitable Application Default Credentials and review the selected provider version/lock file. Production configurations should apply deliberate provider version constraints and an appropriate shared state design.

Run `terraform fmt`, `terraform init`, `terraform validate`, and `terraform plan` only in the isolated configuration directory. An apply is optional and incurs real resource creation. Explain the difference between changing a label and a property that forces replacement. Never paste credentials into the configuration or commit sensitive state.

**Pass condition:** describe configuration, provider, state, plan, apply, and drift in your own words.

**Cleanup:** if you applied, review `terraform plan -destroy` and destroy only the lab-managed resources. An occupied bucket with `force_destroy=false` should not be silently emptied by this configuration.

## Lab 6 — Logs and troubleshooting, about 20 minutes

Use the test log command in [Command reference](COMMAND-REFERENCE.md). Find it in Logs Explorer, expand the entry, and record the log name, timestamp, severity, and project/resource details. Then intentionally use the wrong severity filter and explain the empty result.

Design an alert that fires only after a sustained condition. Identify a real notification recipient without sending test notifications to other people. Inspect existing telemetry if you have authorized access; do not invent incident evidence.

**Pass condition:** distinguish no data collected from data hidden by a wrong query, and an incident from a delivered notification.

Further official guided labs are available through the [ACE learning path](https://www.skills.google/paths/11). The domain notes link official product documentation for every underlying concept.
