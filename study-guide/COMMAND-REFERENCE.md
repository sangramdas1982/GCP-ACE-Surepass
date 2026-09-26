# Essential command patterns

[Study guide home](README.md)

Commands are study examples, not an executed transcript. Replace uppercase placeholders. Confirm account, project, region/zone, permissions, and current CLI help before mutations. Use `gcloud COMMAND --help` when a flag differs in your installed version. `gcloud storage` is used for current storage examples; older courses may use `gsutil`.

## Identity and configuration — inspect first

```bash
gcloud auth list
gcloud config list
gcloud config configurations list
gcloud config get-value project
gcloud projects list
gcloud projects describe PROJECT_ID
gcloud organizations list
```

`gcloud auth login` configures a CLI user login when needed. `gcloud auth application-default login` creates local Application Default Credentials for compatible client libraries; these are a separate credential path. Do not assume changing one always changes the other. Cloud Shell normally provides a prepared environment.

Changing the active project affects subsequent commands that omit `--project`:

```bash
gcloud config set project PROJECT_ID
gcloud config set compute/region REGION
gcloud config set compute/zone ZONE
```

## APIs and billing

```bash
gcloud services list --enabled --project=PROJECT_ID
gcloud services enable compute.googleapis.com --project=PROJECT_ID
gcloud billing accounts list
gcloud billing projects describe PROJECT_ID
```

Enabling an API changes project configuration. It does not grant IAM access or create a VM.

## IAM and service accounts

Read-only inspection:

```bash
gcloud projects get-iam-policy PROJECT_ID
gcloud iam service-accounts list --project=PROJECT_ID
gcloud iam service-accounts get-iam-policy SERVICE_ACCOUNT_EMAIL
```

Examples that change IAM; use only on the intended training resources:

```bash
gcloud iam service-accounts create ace-reader --project=PROJECT_ID   --display-name="ACE lab reader"

gcloud storage buckets add-iam-policy-binding gs://BUCKET_NAME   --member="serviceAccount:SERVICE_ACCOUNT_EMAIL"   --role="roles/storage.objectViewer"
```

Impersonated operation, after the caller has the required permission on the account:

```bash
gcloud storage ls gs://BUCKET_NAME   --impersonate-service-account=SERVICE_ACCOUNT_EMAIL
```

This tests the account's bucket access. It does not create a private key.

## Compute and snapshots

```bash
gcloud compute instances list --project=PROJECT_ID
gcloud compute instances list --filter='status=RUNNING'
gcloud compute instances describe VM_NAME --zone=ZONE
gcloud compute ssh VM_NAME --zone=ZONE --tunnel-through-iap
gcloud compute disks list
gcloud compute snapshots list
gcloud compute instance-templates list
gcloud compute instance-groups managed list
```

Creating a snapshot is a mutation that can incur storage cost:

```bash
gcloud compute snapshots create SNAPSHOT_NAME   --source-disk=DISK_NAME --source-disk-zone=ZONE
```

Know why a snapshot, image, machine image, and instance template are different before memorizing commands.

## Networks

```bash
gcloud compute networks list
gcloud compute networks subnets list
gcloud compute routes list
gcloud compute firewall-rules list
gcloud compute addresses list
gcloud compute routers list
gcloud dns managed-zones list
```

A VPC firewall-rule list does not replace inspecting all applicable hierarchical/network policies.

## GKE and Kubernetes

```bash
gcloud container clusters list
gcloud container clusters get-credentials CLUSTER_NAME   --location=LOCATION --project=PROJECT_ID
kubectl config current-context
kubectl get nodes
kubectl get pods -A
kubectl get services -A
kubectl get deployments -n NAMESPACE
kubectl describe pod POD_NAME -n NAMESPACE
kubectl logs POD_NAME -n NAMESPACE
kubectl logs POD_NAME -n NAMESPACE --previous
kubectl get events -n NAMESPACE --sort-by=.metadata.creationTimestamp
kubectl get hpa -n NAMESPACE
```

Application changes:

```bash
kubectl apply -f deployment.yaml
kubectl rollout status deployment/APP_NAME -n NAMESPACE
kubectl rollout history deployment/APP_NAME -n NAMESPACE
kubectl rollout undo deployment/APP_NAME -n NAMESPACE
```

A rollback of a Deployment does not restore an external database or reverse every configuration change.

## Cloud Run

```bash
gcloud run services list --region=REGION
gcloud run services describe SERVICE_NAME --region=REGION
gcloud run revisions list --service=SERVICE_NAME --region=REGION
```

Release patterns that modify a service:

```bash
gcloud run deploy SERVICE_NAME --image=IMAGE_URI   --region=REGION --no-traffic --tag=candidate

gcloud run services update-traffic SERVICE_NAME --region=REGION   --to-revisions=STABLE_REVISION=95,CANDIDATE_REVISION=5
```

Use actual revision names from the list output. Verify authentication, runtime identity, readiness, and schema compatibility before routing traffic.

## Storage and data jobs

```bash
gcloud storage buckets list --project=PROJECT_ID
gcloud storage buckets describe gs://BUCKET_NAME
gcloud storage ls gs://BUCKET_NAME
gcloud storage cp ./sample.txt gs://BUCKET_NAME/sample.txt
gcloud storage cat gs://BUCKET_NAME/sample.txt
gcloud sql instances list
gcloud sql backups list --instance=INSTANCE_NAME
bq ls
bq ls -j
bq show -j --location=LOCATION JOB_ID
gcloud dataflow jobs list --region=REGION
```

Storage copy writes data; the other inspection examples do not create the listed resources. A database resource listing is not a SQL query.

## Logging

Read recent matching entries:

```bash
gcloud logging read 'severity>=ERROR'   --project=PROJECT_ID --limit=20 --format=json
```

Write a harmless test log in a training project:

```bash
gcloud logging write ace-study-log "ACE study telemetry check"   --severity=NOTICE --project=PROJECT_ID
```

Find it in Logs Explorer by the log name and current time window. Logging writes/retention may contribute to usage charges.

## Terraform

```bash
terraform fmt
terraform init
terraform validate
terraform plan
```

`init` can download providers and access a backend. `plan` generally reads remote state/resources and requires suitable credentials. `apply` changes resources. `destroy` deletes managed resources; inspect its plan carefully and use only on your isolated lab configuration.

Official references: [gcloud](https://docs.cloud.google.com/sdk/gcloud/reference), [kubectl](https://kubernetes.io/docs/reference/kubectl/), [Terraform on Google Cloud](https://docs.cloud.google.com/docs/terraform/terraform-overview).
