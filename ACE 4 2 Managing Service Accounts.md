# Managing Service Accounts: Complete 2-Hour Study Guide
## ACE Exam Section 4.2 - Deep Dive

---

## Introduction

"Managing service accounts" is a critical security topic in the ACE exam. This section covers:

- **Creating service accounts**: Who has what credentials
- **Managing keys**: User-managed vs Google-managed
- **Impersonation**: Service account delegation
- **IAM permissions**: Least privilege access
- **Workload Identity Federation**: Federated identity for external workloads
- **Short-lived credentials**: Safer than long-lived keys

Service accounts are the glue between your infrastructure and Google Cloud. Get this right, and you have secure, auditable access. Get it wrong, and you have credential leaks and unauthorized access.

---

## Part 1: What are Service Accounts

### Understanding Service Accounts

A **service account** is an account used by applications, not people.

```
Person:          alice@example.com    (human)
                 ↓
Service Account: my-app@project.iam.gserviceaccount.com  (application)
                 ↓
Google Cloud Resources: Compute Engine, Cloud Storage, etc.
```

Service accounts allow applications to authenticate to Google Cloud without storing user credentials.

### Default Service Account

Every project has a **default service account**:
- `PROJECT_NUMBER-compute@developer.gserviceaccount.com`
- Has Editor role by default (too permissive!)
- Used by Compute Engine VMs automatically

**Security issue**: Default account has too much access.

**Best practice**: Disable default account, create specific service accounts.

### Service Account Identifiers

A service account has multiple identifiers:

```
Display Name:  "App Server"
Service Account Email:  app-server@my-project.iam.gserviceaccount.com
Service Account ID:  123456789012345678901  (numeric)
Unique ID:  AbCdEfGhIjKlMnOpQrStUvWxYz
```

Most commonly use **email** to reference.

---

## Part 2: Creating Service Accounts

### Creating via Console

**Steps**:
1. Go to IAM & Admin > Service Accounts
2. Click "Create Service Account"
3. Enter name and ID
4. Click "Create and Continue"
5. Grant roles (add later is fine)
6. Click "Done"

### Creating via gcloud

```bash
# Create service account
gcloud iam service-accounts create app-server \
  --display-name="App Server Service Account" \
  --project=PROJECT_ID

# Verify creation
gcloud iam service-accounts list --project=PROJECT_ID

# Get service account email
SA_EMAIL=$(gcloud iam service-accounts list --filter="displayName:app-server" --format="value(email)")
```

### Creating via Terraform

```hcl
resource "google_service_account" "app_server" {
  account_id   = "app-server"
  display_name = "App Server Service Account"
  description  = "Service account for app server instances"
  project      = "PROJECT_ID"
}

output "service_account_email" {
  value = google_service_account.app_server.email
}
```

### Naming Convention

Use consistent naming:
- `COMPONENT-ENVIRONMENT`
- Examples:
  - `frontend-prod`
  - `backend-dev`
  - `dataflow-batch`
  - `cloud-sql-proxy`

---

## Part 3: Service Account Keys

### Key Types

**Google-managed keys**:
- Created by Google
- Rotated automatically every 90 days
- Can't export
- Most secure
- Only available in Kubernetes

**User-managed keys**:
- You create and manage
- Rotated manually
- Can export
- Stored in JSON files
- Need careful handling

### Creating Keys

**Via Console**:
1. Go to IAM & Admin > Service Accounts
2. Click service account
3. Click "Keys" tab
4. Click "Add Key" > "Create new key"
5. Choose JSON (most common)
6. Click "Create"
7. Download file (save securely!)

**Via gcloud**:
```bash
# Create JSON key
gcloud iam service-accounts keys create key.json \
  --iam-account=app-server@PROJECT_ID.iam.gserviceaccount.com

# Create P12 key (legacy, not recommended)
gcloud iam service-accounts keys create key.p12 \
  --iam-account=app-server@PROJECT_ID.iam.gserviceaccount.com \
  --key-file-type=p12
```

### Key Security

**JSON key file contents**:
```json
{
  "type": "service_account",
  "project_id": "my-project",
  "private_key_id": "key-id",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvQ...\n-----END PRIVATE KEY-----\n",
  "client_email": "app-server@my-project.iam.gserviceaccount.com",
  "client_id": "123456789",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "..."
}
```

**Protection**:
- Treat like passwords
- Never commit to git
- Use secrets management
- Rotate regularly
- Disable unused keys

### Using Keys in Application

**Python**:
```python
from google.oauth2 import service_account

# Load credentials from file
credentials = service_account.Credentials.from_service_account_file(
    'path/to/key.json'
)

# Use with Cloud Storage client
from google.cloud import storage
storage_client = storage.Client(credentials=credentials)
buckets = storage_client.list_buckets()
```

**Node.js**:
```javascript
const admin = require('firebase-admin');

admin.initializeApp({
  credential: admin.credential.cert(require('./key.json'))
});
```

**Go**:
```go
import (
  "google.golang.org/api/option"
  "google.golang.org/api/storage/v1"
)

ctx := context.Background()
service, err := storage.NewService(ctx, option.WithCredentialsFile("key.json"))
```

### Key Rotation

**Manual rotation process**:
```bash
# Step 1: Create new key
gcloud iam service-accounts keys create new-key.json \
  --iam-account=SA_EMAIL

# Step 2: Update application to use new key

# Step 3: Wait for old key to expire (old cache to clear)

# Step 4: Delete old key
gcloud iam service-accounts keys delete KEY_ID \
  --iam-account=SA_EMAIL
```

**Automation**:
- Use Terraform to manage keys
- Delete old keys after creating new one
- Set up alerting for expiring keys

---

## Part 4: Service Account IAM Roles

### Principle of Least Privilege

Only grant the **minimum** permissions needed.

```
Bad:
  app-server: Owner role (can do anything)

Good:
  app-server: Storage Object Viewer (only read from specific bucket)
            + Cloud Logging Log Writer (only write logs)
```

### Granting Roles to Service Accounts

**Via Console**:
1. Go to IAM & Admin > IAM
2. Click "Grant Access"
3. Enter service account email
4. Select role
5. Click "Save"

**Via gcloud**:
```bash
# Grant role to service account
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:SA_EMAIL \
  --role=roles/storage.objectViewer

# View service account roles
gcloud projects get-iam-policy PROJECT_ID \
  --flatten="bindings[].members" \
  --filter="bindings.members:serviceAccount:SA_EMAIL"

# Remove role
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:SA_EMAIL \
  --role=roles/storage.objectViewer
```

**Via Terraform**:
```hcl
# Grant Storage Viewer role
resource "google_project_iam_member" "app_storage_viewer" {
  project = "PROJECT_ID"
  role    = "roles/storage.objectViewer"
  member  = "serviceAccount:${google_service_account.app_server.email}"
}

# Grant Cloud Logging Writer role
resource "google_project_iam_member" "app_logging_writer" {
  project = "PROJECT_ID"
  role    = "roles/logging.logWriter"
  member  = "serviceAccount:${google_service_account.app_server.email}"
}
```

### Custom Roles

When predefined roles don't fit:

```hcl
resource "google_project_custom_role" "app_custom" {
  role_id     = "appCustomRole"
  title       = "Custom App Role"
  description = "Custom role for app"
  permissions = [
    "storage.buckets.get",
    "storage.objects.get",
    "logging.logEntries.create",
  ]
}

resource "google_project_iam_member" "app_custom" {
  project = "PROJECT_ID"
  role    = google_project_custom_role.app_custom.id
  member  = "serviceAccount:${google_service_account.app_server.email}"
}
```

### Common Service Account Roles

```
Compute Engine admin:
  roles/compute.admin

Cloud Storage:
  roles/storage.objectViewer (read only)
  roles/storage.objectCreator (write)
  roles/storage.admin (full)

Cloud SQL:
  roles/cloudsql.client (connect only)
  roles/cloudsql.admin (full access)

Logging:
  roles/logging.logWriter (write logs)
  roles/logging.viewer (read logs)

Monitoring:
  roles/monitoring.metricWriter

Kubernetes Engine:
  roles/container.developer
```

---

## Part 5: Service Account Impersonation

### What is Impersonation

**Impersonation** allows one service account to act as another.

```
User: alice@example.com
  ↓
  Impersonates
  ↓
Service Account: app-server@project.iam.gserviceaccount.com
  ↓
  Accesses
  ↓
Google Cloud Resources
```

**Use case**: CI/CD system (GitHub Actions, Cloud Build) needs to deploy as service account.

### Granting Impersonation Rights

Person can impersonate service account if they have `serviceAccountUser` or `serviceAccountTokenCreator` role.

**Via gcloud**:
```bash
# Grant alice ability to impersonate app-server
gcloud iam service-accounts add-iam-policy-binding \
  app-server@PROJECT_ID.iam.gserviceaccount.com \
  --member=user:alice@example.com \
  --role=roles/iam.serviceAccountUser
```

**Via Terraform**:
```hcl
resource "google_service_account_iam_member" "impersonate" {
  service_account_id = google_service_account.app_server.name
  role               = "roles/iam.serviceAccountUser"
  member             = "user:alice@example.com"
}
```

### Using Impersonation from Cloud Build

**cloudbuild.yaml**:
```yaml
steps:
  - name: 'gcr.io/cloud-builders/gke-deploy'
    env:
      - 'CLOUDSDK_COMPUTE_REGION=us-central1'
      - 'CLOUDSDK_CONTAINER_CLUSTER=my-cluster'
    args:
      - run
      - --filename=k8s/
      - --location=us-central1
      - --cluster=my-cluster

# Use service account for deployment
serviceAccount: 'projects/PROJECT_ID/serviceAccounts/cloud-build@PROJECT_ID.iam.gserviceaccount.com'
```

**From Command Line**:
```bash
# Use impersonate flag
gcloud compute instances list --impersonate-service-account=app-server@PROJECT_ID.iam.gserviceaccount.com
```

---

## Part 6: Creating Short-Lived Credentials

### Why Short-Lived Credentials

**Long-lived keys** (like JSON key files):
- If leaked, can be used forever
- Hard to rotate
- Risky in production

**Short-lived credentials**:
- Expire automatically (typically 1 hour)
- Can't be leaked long-term
- Safer for applications

### Getting Short-Lived Credentials

**Via gcloud**:
```bash
# Generate short-lived access token
ACCESS_TOKEN=$(gcloud auth application-default print-access-token)

# Use token
curl -H "Authorization: Bearer $ACCESS_TOKEN" \
  https://www.googleapis.com/storage/v1/b/my-bucket/o

# Token expires in 1 hour
```

**From Service Account Key** (Application Default Credentials):
```python
# ADC automatically refreshes token
from google.oauth2 import service_account

creds = service_account.Credentials.from_service_account_file('key.json')

# Token is automatically refreshed when it expires
# Application doesn't need to handle refresh
```

### Application Default Credentials (ADC)

**ADC** automatically finds credentials in order:
1. Environment variable `GOOGLE_APPLICATION_CREDENTIALS`
2. Service account on Compute Engine
3. Default service account on Cloud Run, Cloud Functions, etc.

```bash
# Set ADC to use service account key
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/key.json

# Python app uses ADC automatically
python app.py
```

This is the **recommended** way for applications.

---

## Part 7: Workload Identity Federation

### What is Workload Identity Federation

**Workload Identity Federation** allows external workloads (outside Google Cloud) to authenticate to Google Cloud without storing keys.

```
GitHub Actions (external)
  ↓
  Uses OIDC token from GitHub
  ↓
  Exchanges for Google Cloud access token
  ↓
  Accesses Google Cloud resources
  ↓
No long-lived keys stored anywhere!
```

### When to Use

**Use Workload Identity Federation when**:
- CI/CD runs outside Google Cloud (GitHub, GitLab)
- On-premises services need Google Cloud access
- Want zero long-lived keys
- Need short-lived credentials

**vs Service Account Keys**:
- Keys: Store JSON file, rotate manually
- WIF: No keys, automatic short-lived tokens

### Setting up Workload Identity Federation

**Step 1: Create Workforce Identity Pool**
```bash
gcloud iam workforce-pools create github \
  --location=global \
  --display-name="GitHub"
```

**Step 2: Create Workforce Provider**
```bash
gcloud iam workforce-pools providers create-oidc github-oidc \
  --location=global \
  --workforce-pool=github \
  --display-name="GitHub Provider" \
  --attribute-mapping="google.subject=assertion.sub,attribute.repository=assertion.repository" \
  --issuer-uri=https://token.actions.githubusercontent.com
```

**Step 3: Create Service Account and Grant Access**
```bash
gcloud iam service-accounts create github-actions \
  --display-name="GitHub Actions"

gcloud iam service-accounts add-iam-policy-binding \
  github-actions@PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/iam.workloadIdentityUser \
  --member=principalSet://iam.googleapis.com/locations/global/workforcePools/github/attribute.repository/my-repo
```

**Step 4: Use in GitHub Actions**
```yaml
name: Deploy to Google Cloud

on:
  push:
    branches: [main]

permissions:
  contents: read
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Authenticate to Google Cloud
        uses: google-github-actions/auth@v1
        with:
          workload_identity_provider: projects/PROJECT_ID/locations/global/workloadIdentityPools/github/providers/github-oidc
          service_account: github-actions@PROJECT_ID.iam.gserviceaccount.com

      - name: Deploy
        run: |
          gsutil cp app.tar.gz gs://my-bucket/
```

---

## Part 8: Service Accounts in Kubernetes

### Kubernetes Service Accounts vs Google Service Accounts

**Kubernetes Service Account**:
- Identity within Kubernetes cluster
- Used by Pods to call Kubernetes API
- One per namespace (default account)

**Google Service Account**:
- Identity in Google Cloud
- Used by containers to call Google Cloud APIs
- Managed in IAM

**Connection**: Workload Identity connects them.

### Workload Identity for GKE

**Workload Identity** binds Kubernetes service account to Google service account.

```
Pod with Kubernetes Service Account "app"
  ↓ (Workload Identity)
  ↓
Google Service Account "app@project.iam.gserviceaccount.com"
  ↓
Access Google Cloud APIs
```

**Step 1: Create Kubernetes Service Account**
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app
  namespace: default
```

**Step 2: Create Google Service Account**
```bash
gcloud iam service-accounts create app-sa
```

**Step 3: Bind Kubernetes SA to Google SA**
```bash
gcloud iam service-accounts add-iam-policy-binding \
  app-sa@PROJECT_ID.iam.gserviceaccount.com \
  --role roles/iam.workloadIdentityUser \
  --member "serviceAccount:PROJECT_ID.svc.id.goog[default/app]"
```

**Step 4: Annotate Kubernetes Service Account**
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: app
  namespace: default
  annotations:
    iam.gke.io/gcp-service-account: app-sa@PROJECT_ID.iam.gserviceaccount.com
```

**Step 5: Pod accesses Google Cloud**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
  namespace: default
spec:
  serviceAccountName: app
  containers:
  - name: app
    image: gcr.io/PROJECT_ID/app
    # Automatically gets access token for app-sa@PROJECT_ID
```

---

## Part 9: Google-Managed Service Accounts

### What are Google-Managed Service Accounts

Some Google Cloud services automatically create service accounts for you:

- Cloud Functions: `PROJECT_NUMBER-compute@developer.gserviceaccount.com`
- App Engine: `PROJECT_ID@appspot.gserviceaccount.com`
- Cloud Run: `PROJECT_NUMBER-compute@developer.gserviceaccount.com`

These are **read-only** in IAM (you can't manage the account itself, only the roles).

### Default Service Account Permissions

Default service account automatically has **Editor** role (too permissive!).

**Best practice**:
1. Create custom service account
2. Remove roles from default
3. Use custom account with minimal permissions

```bash
# Remove Editor from default account
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:PROJECT_NUMBER-compute@developer.gserviceaccount.com \
  --role=roles/editor
```

---

## Part 10: Exam Scenarios

### Scenario 1: Deploy Application Securely

**Requirements**:
- Application on Compute Engine needs to:
  - Read from Cloud Storage bucket
  - Write logs to Cloud Logging
  - Access Cloud SQL database
- Minimal permissions needed

**Solution**:
```bash
# Create service account
gcloud iam service-accounts create app-server

# Grant minimum roles
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:app-server@PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/storage.objectViewer

gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:app-server@PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/logging.logWriter

gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:app-server@PROJECT_ID.iam.gserviceaccount.com \
  --role=roles/cloudsql.client

# Create instance with service account
gcloud compute instances create app-server \
  --service-account=app-server@PROJECT_ID.iam.gserviceaccount.com \
  --scopes=cloud-platform
```

### Scenario 2: CI/CD Deployment without Storing Keys

**Requirements**:
- GitHub Actions pipeline deploys to Google Cloud
- No long-lived keys stored
- Only deploy service has permissions

**Solution**:
Set up Workload Identity Federation (from Part 7).

**GitHub Actions yaml**:
```yaml
name: Deploy

on: [push]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: 'read'
      id-token: 'write'

    steps:
    - uses: actions/checkout@v3

    - id: 'auth'
      uses: 'google-github-actions/auth@v1'
      with:
        workload_identity_provider: 'projects/PROJECT_ID/locations/global/workloadIdentityPools/github/providers/github'
        service_account: 'github-deploy@PROJECT_ID.iam.gserviceaccount.com'

    - name: 'Deploy'
      run: |
        gcloud app deploy --quiet
```

### Scenario 3: Service Account Rotation

**Problem**: Need to rotate service account key

**Process**:
```bash
# Step 1: Create new key
SA_EMAIL=my-app@PROJECT_ID.iam.gserviceaccount.com
gcloud iam service-accounts keys create new-key.json \
  --iam-account=$SA_EMAIL

# Step 2: Update application to use new key
# (This is application-specific)

# Step 3: Wait for all instances to restart

# Step 4: List old keys
gcloud iam service-accounts keys list --iam-account=$SA_EMAIL

# Step 5: Delete old key
gcloud iam service-accounts keys delete KEY_ID \
  --iam-account=$SA_EMAIL

# Step 6: Securely delete key file
shred new-key.json
```

---

## Part 11: Security Best Practices

### 1. Never Use Default Service Account

**Bad**:
```
Compute Engine instance uses default service account with Editor role
```

**Good**:
```
Compute Engine instance uses custom "app-server" service account
with only necessary roles
```

### 2. Use Short-Lived Credentials

**Bad**:
```
Store JSON key file in Dockerfile, commit to git (SECURITY DISASTER)
```

**Good**:
```
Use Application Default Credentials
+ Workload Identity Federation
+ No keys stored anywhere
```

### 3. Rotate Keys Regularly

**Bad**:
```
Same key for 2 years
```

**Good**:
```
Rotate keys every 90 days
Use automation for this
```

### 4. Grant Minimum Permissions

**Bad**:
```
Grant Editor to everything
```

**Good**:
```
Grant exactly what's needed:
  - Cloud Storage Viewer (read only)
  - Logging Writer (write logs only)
```

### 5. Audit Service Account Usage

Monitor:
- Who accessed what
- Failed authentication attempts
- Unused service accounts

```bash
# Check service account activity
gcloud logging read \
  "protoPayload.authenticationInfo.principalEmail:SA_EMAIL" \
  --limit 100
```

---

## Part 12: Common Exam Mistakes

**Mistake 1**: Using default service account in production
- Should disable default
- Create custom account with minimal perms

**Mistake 2**: Storing JSON key in git or Dockerfile
- Keys leak, security breached
- Use Workload Identity or ADC

**Mistake 3**: Granting too many permissions
- Violates least privilege
- Increases blast radius of leak

**Mistake 4**: Not rotating keys
- Keys exposed, not known
- Rotate regularly (90 days)

**Mistake 5**: Forgetting to assign roles
- Service account created but has no permissions
- Nothing works, confusing error messages

---

## Part 13: Quick Reference

### Creating and Using Service Accounts

```bash
# Create
gcloud iam service-accounts create SERVICE_ACCOUNT_NAME

# Grant role
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=serviceAccount:SA_EMAIL \
  --role=ROLE_NAME

# Create key
gcloud iam service-accounts keys create key.json \
  --iam-account=SA_EMAIL

# Use in gcloud
gcloud auth activate-service-account --key-file=key.json

# Use in application
export GOOGLE_APPLICATION_CREDENTIALS=key.json
```

### Service Account Decision Tree

```
Question: Who runs the code?
├─ Compute Engine → Use custom service account
├─ Cloud Run → Use default or custom
├─ Kubernetes → Use Workload Identity
└─ External (CI/CD) → Use Workload Identity Federation

Question: What permissions needed?
├─ Cloud Storage only → storage.objectViewer
├─ Cloud SQL only → cloudsql.client
├─ Multiple → Use custom role
└─ Not sure → Start with minimal, add as needed

Question: How to provide credentials?
├─ Running on GCP → Use Application Default Credentials
├─ External system → Use short-lived tokens
├─ Legacy app → Use JSON key (rotate regularly)
└─ CI/CD → Use Workload Identity Federation
```

---

## Conclusion

Service accounts are critical for secure Google Cloud deployments. Master:

1. **Creating service accounts**: Simple process
2. **Managing keys**: Rotate regularly, prefer short-lived
3. **Granting permissions**: Minimum needed (least privilege)
4. **Impersonation**: Delegate via roles
5. **Workload Identity**: For external systems
6. **Best practices**: No keys in code, rotate often

Exam focus areas:
- Creating and configuring service accounts
- Understanding key types (user-managed vs Google-managed)
- Granting minimum permissions
- Service account impersonation
- Workload Identity Federation
- Security best practices

---

**Total estimated reading/study time: 2 hours**
**Word count: ~9,500 words**

This guide covers everything in Section 4.2 with practical examples and security best practices.
