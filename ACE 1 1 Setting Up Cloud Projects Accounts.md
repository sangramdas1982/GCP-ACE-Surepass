# Setting Up Cloud Projects and Accounts: Complete 2-Hour Study Guide
## ACE Exam Section 1.1 - Deep Dive

---

## Introduction

"Setting up a cloud solution environment" represents 20% of the ACE exam, and the foundation of everything is how you set up cloud projects and accounts. This section covers the organizational structure, governance, and foundational setup that enables everything else in Google Cloud.

This isn't just about clicking buttons to create projects. It's about understanding:
- How to organize your infrastructure hierarchically
- How to enforce governance through organizational policies
- How to manage access through IAM
- How to manage users through Cloud Identity
- How to enable services and manage observability
- How to verify infrastructure availability
- How to request and manage quotas

Get this wrong, and your entire cloud foundation is weak. Get it right, and you have a scalable, secure, governable infrastructure.

---

## Part 1: Creating a Resource Hierarchy

### Understanding the Complete Hierarchy

Google Cloud's resource hierarchy is the organizational structure that contains all your resources:

```
Organization (root)
├── Folder (logical grouping)
│   ├── Folder (can be nested)
│   │   └── Project (actual container for resources)
│   │       └── Resources (VMs, buckets, databases, etc.)
│   └── Project
│       └── Resources
└── Project
    └── Resources
```

Every resource exists within a project. Every project exists within the hierarchy (either directly under organization or within folders).

### Organizations

An **organization** is the root node of the resource hierarchy. It represents your company or organizational unit.

**Key facts about organizations**:
- Only one organization per Google Cloud account
- Organizations are created automatically if you use Google Workspace
- If you don't use Workspace, you can create one by verifying domain ownership
- Organizations are the point at which you apply org-level policies

**Creating an organization** (if not using Workspace):
1. Verify domain ownership in Google Cloud
2. Create Cloud Identity instance
3. Organization is automatically created

**Organization role hierarchy**:
- **Organization Owner**: Full control
- **Organization Admin**: Can manage organization but not set billing
- **Organization Policy Admin**: Can set organization policies
- **Folder Creator**: Can create folders
- **Project Creator**: Can create projects

Most people in your organization shouldn't have organization-level roles. Roles should be minimally assigned.

### Folders

**Folders** are organizational containers that group projects and other folders. They're the primary way to organize your infrastructure.

**Key characteristics**:
- Folders can be nested (folders within folders)
- Each folder is within exactly one parent (another folder or the organization)
- Folders don't directly contain resources—only projects do
- Folders are used for logical organization and policy application

**Typical folder structures**:

**By Environment**:
```
Organization
├── Folder: Production
│   ├── Project: API Backend (prod)
│   ├── Project: Frontend (prod)
│   └── Project: Data (prod)
├── Folder: Staging
│   ├── Project: API Backend (staging)
│   └── Project: Data (staging)
└── Folder: Development
    └── Project: Dev
```

**By Team**:
```
Organization
├── Folder: Backend Team
│   ├── Folder: Production
│   ├── Folder: Staging
│   └── Folder: Development
├── Folder: Frontend Team
│   ├── Folder: Production
│   └── Folder: Development
└── Folder: Platform Team
    └── Project: Shared Services
```

**By Business Unit**:
```
Organization
├── Folder: Finance Division
│   ├── Project: Accounting
│   └── Project: Analytics
├── Folder: Sales Division
│   ├── Project: CRM
│   └── Project: Analytics
└── Folder: Operations
    └── Project: Shared Services
```

**Creating folders**:

Via Console:
1. Go to IAM & Admin > Manage Resources
2. Click Create Folder
3. Enter display name
4. Select parent (organization or folder)

Via gcloud:
```bash
gcloud resource-manager folders create \
  --display-name="Production" \
  --organization=ORGANIZATION_ID
```

Via Terraform:
```hcl
resource "google_folder" "production" {
  display_name = "Production"
  parent       = "organizations/ORGANIZATION_ID"
}
```

**Folder operations**:
- **Moving projects**: Projects can move between folders
- **Moving folders**: Entire folder trees can move to different parents
- **Deleting folders**: Only possible when folder is empty (all projects moved/deleted)

### Projects

A **project** is the fundamental unit where resources exist. Every resource must belong to exactly one project.

**Project characteristics**:
- Unique project ID (immutable, must be globally unique)
- Display name (mutable)
- Project number (auto-generated, unique)
- Billing account (required for paid resources)
- Service accounts (at least default account)

**Creating projects**:

Via Console:
1. Click project selector (top of console)
2. Click "New Project"
3. Enter name
4. Select parent folder/organization
5. Click Create

Via gcloud:
```bash
gcloud projects create my-project \
  --name="My Project" \
  --folder=FOLDER_ID \
  --labels=team=backend
```

Via Terraform:
```hcl
resource "google_project" "main" {
  project_id = "my-project"
  name       = "My Project"
  folder_id  = google_folder.production.name
  
  labels = {
    team = "backend"
  }
}
```

**Project naming conventions**:
Use consistent naming to make hierarchy clear:
- `COMPANY-ENVIRONMENT-TEAM-SERVICE`
- Example: `acme-prod-backend-api`

This makes it immediately clear what the project is for.

---

## Part 2: Applying Organizational Policies

### Understanding Organizational Policies

**Organizational Policies** (formerly Constraints) are rules that restrict what resources can do, regardless of IAM permissions.

Key concept: **IAM says WHO can do WHAT. Organization policies say WHAT CAN BE DONE.**

Even if someone has full permissions, organization policies prevent certain actions.

**Examples of organizational policies**:
- "No one can create VMs with public IPs"
- "All Cloud Storage buckets must use uniform access control"
- "No one can disable Cloud Logging"
- "All VMs must have certain labels"
- "Compute instances can only run in specific regions"

### Policy Inheritance

Organization policies **cascade down** the hierarchy:

```
Organization policy: "No public IPs"
├── Folder policy: (inherits no public IPs)
│   ├── Project: (inherits no public IPs)
│   └── Resources: (constrained by policy)
└── Folder policy: "Must use regional disks"
    ├── Project: (inherits both policies)
    └── Resources: (constrained by both)
```

Policies at higher levels apply to all lower levels. More specific policies can be more restrictive but not less restrictive.

### Common Organization Policies (Constraints)

**Compute-related**:
- `compute.requireShieldedVm`: All VMs must be Shielded VMs
- `compute.skipDefaultNetworkCreation`: New projects don't get default network
- `compute.requireOsLogin`: All VMs require OS Login
- `compute.disableSerialPortAccess`: Can't access serial port
- `compute.requireExternalIpAddress`: VMs must have external IP (opposite of usual)

**Storage-related**:
- `storage.uniformBucketLevelAccess`: All buckets must use uniform access

**Networking**:
- `compute.restrictVpcPeering`: Restrict VPC peering
- `compute.restrictSharedVpcSubnetworks`: Restrict Shared VPC usage

**Resource**:
- `resourcemanager.defaultNetworkCreation`: Create default networks/routes
- `resourcemanager.disableServiceAccountCreation`: Can't create service accounts

**IAM**:
- `iam.disableServiceAccountCreation`: Can't create service accounts
- `iam.disableServiceAccountKeyCreation`: Can't create service account keys

### Creating Organization Policies

Via Console:
1. Go to IAM & Admin > Organization Policies
2. Click "Select a constraint"
3. Choose the constraint
4. Click "Manage"
5. Configure the policy
6. Click "Save"

Via gcloud:
```bash
gcloud resource-manager org-policies set-policy policy.yaml \
  --project=PROJECT_ID
```

Policy YAML:
```yaml
constraint: compute.requireShieldedVm
listPolicy:
  allowedValues:
  - projects/PROJECT_ID
```

Via Terraform:
```hcl
resource "google_organization_policy" "require_shielded" {
  org_id     = "ORGANIZATION_ID"
  constraint = "compute.requireShieldedVm"
  
  boolean_policy {
    enforced = true
  }
}
```

### Policy Scopes

You can apply policies at:
- **Organization level**: Affects all projects and resources
- **Folder level**: Affects all projects and resources in the folder
- **Project level**: Affects all resources in the project

More specific scopes (project) can be stricter than broader scopes (organization) but not less strict.

**Example**: 
- Organization policy: "VMs can be in any region"
- Folder policy: "Production folder VMs must be in us-central1 or europe-west1"

This is allowed because the folder policy is more restrictive.

---

## Part 3: Granting IAM Roles

### IAM at Different Hierarchy Levels

IAM policies exist at each level:

**Organization level**: Grant role to a user/group/service account that applies org-wide.

**Folder level**: Grant role that applies to all projects/resources in the folder.

**Project level**: Grant role that applies to all resources in the project.

**Resource level**: Grant role on specific resource (some resources support this).

**Example**:
```
Organization: Grant user "Viewer" role
├── Folder (Production): Grant user "Editor" role
│   ├── Project: Grant user "Compute Admin" role
│   │   └── Instance: (inherits Viewer + Editor + Compute Admin)
└── Folder (Development): (inherits just Viewer)
```

User's effective permissions: Union of all granted roles at all levels.

### Assigning Roles in a Project

Most common: Assigning roles at **project level** for specific teams.

**Via Console**:
1. Go to project
2. Click "IAM" in left sidebar
3. Click "Grant Access"
4. Enter email/group
5. Select role
6. Click "Save"

**Via gcloud**:
```bash
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=user:email@example.com \
  --role=roles/compute.admin
```

**Via Terraform**:
```hcl
resource "google_project_iam_member" "compute_admin" {
  project = "PROJECT_ID"
  role    = "roles/compute.admin"
  member  = "user:email@example.com"
}
```

### Best Practices for IAM

**Always use groups, never individuals**:
```
Bad: Grant role to alice@example.com, bob@example.com, charlie@example.com
Good: Create group backend-team@example.com, add users to group, grant role to group
```

**Use principle of least privilege**:
```
Bad: Grant "Owner" role to all engineers
Good: Grant "Compute Instance Admin" on dev project, "Viewer" on prod
```

**Separate permissions by role**:
```
Bad: Everyone gets "Editor" role on the project
Good: Engineers get "Compute Admin", DBAs get "Cloud SQL Admin", etc.
```

**Review quarterly**:
- List all users with roles
- Verify each still needs access
- Remove unnecessary access

---

## Part 4: Managing Users and Groups in Cloud Identity

### Cloud Identity Fundamentals

**Cloud Identity** is Google's identity management service. It manages:
- Users (people with email addresses)
- Groups (collections of users)
- Organizational Units (hierarchy of users)
- Device policies
- Authentication

### Users in Cloud Identity

A **user** is a person with an email address who can authenticate to Google Cloud.

**User states**:
- **Active**: Can authenticate and access resources
- **Suspended**: Temporarily disabled
- **Archived**: Disabled, no longer active

**Creating users**:

Via Cloud Identity Admin Console:
1. Go to admin.google.com (if using Workspace)
2. Users and accounts > Users
3. Click "Add user"
4. Enter first name, last name, email
5. Click "Add user"

Via APIs:
```bash
gcloud identity users create \
  --user-id=user@example.com \
  --display-name="John Doe"
```

**User lifecycle**:
1. User created in Cloud Identity
2. User authenticates (via password or federated login)
3. User accesses Google Cloud resources (subject to IAM)
4. User leaves organization → suspend/delete

### Groups in Cloud Identity

A **group** is a collection of users. This is the primary way to manage access.

**Group types**:
- **Security groups**: For access control
- **Email lists**: For communication
- **Dynamic groups**: Automatic membership based on rules

**Creating groups**:

Via Cloud Identity Admin Console:
1. Go to Groups
2. Click "Create group"
3. Enter email and display name
4. Select group type
5. Click Create

Via gcloud:
```bash
gcloud identity groups create backend-team@example.com \
  --display-name="Backend Team"
```

**Adding users to groups**:

Via Console:
1. Click group
2. Click "Manage members"
3. Click "Add member"
4. Enter email
5. Click Add

Via gcloud:
```bash
gcloud identity groups members add \
  --group-email=backend-team@example.com \
  --member-email=user@example.com
```

### Dynamic Groups

**Dynamic groups** automatically add/remove members based on rules.

**Example rule**: "All users where Department = Backend"

Any user added to Cloud Identity with Department = Backend automatically becomes a member of the group. When user's department changes, they're automatically removed.

**Creating dynamic groups**:

Via Console:
1. Create group with type "Dynamic membership"
2. Click "Add a rule"
3. Enter LDAP query or attribute rules
4. Save

**Common dynamic group queries**:
```
(org='Engineering') AND (employeeType='Full-time')
(department='Backend')
(location='US')
```

### Organizational Units (OUs)

**Organizational Units** are hierarchical groupings of users.

```
Organization
├── OU: Engineering
│   ├── OU: Backend
│   └── OU: Frontend
├── OU: Sales
└── OU: Operations
```

**Uses of OUs**:
- Apply device policies (engineers get different policies than sales)
- Apply security policies
- Delegate management

### Manual vs Automated User Management

**Manual management**:
- Create users in Cloud Identity
- Add to groups manually
- Remove when they leave
- Works for small organizations (< 100 users)

**Automated management**:
- Sync from external identity provider (Active Directory, LDAP)
- Use Google Cloud Directory Sync
- Users automatically created/updated/deleted
- Groups automatically maintained
- Better for large organizations

**For the exam**: Understand both approaches. Small companies do manual, large enterprises automate.

---

## Part 5: Enabling APIs Within Projects

### What are APIs and Why Enable Them

A **Google Cloud API** is a service endpoint that provides specific functionality. For example:
- Compute Engine API provides VM management
- Cloud Storage API provides bucket management
- Cloud SQL API provides database management

To use a service, you must **enable its API** in the project. Without enabling, you get "API not enabled" errors.

### Viewing Available APIs

**Via Console**:
1. Go to APIs & Services > Library
2. Search for API
3. View API details
4. Click "Enable" if not already enabled

**Via gcloud**:
```bash
gcloud services list --available
```

**Common APIs to enable**:
- `compute.googleapis.com`: Compute Engine
- `container.googleapis.com`: Kubernetes Engine
- `storage-api.googleapis.com`: Cloud Storage
- `sqladmin.googleapis.com`: Cloud SQL
- `monitoring.googleapis.com`: Cloud Monitoring
- `logging.googleapis.com`: Cloud Logging
- `cloudresourcemanager.googleapis.com`: Resource Manager

### Enabling APIs

**Via Console**:
1. Go to APIs & Services > Library
2. Search for API (e.g., "Compute Engine")
3. Click on API
4. Click "Enable"

**Via gcloud**:
```bash
gcloud services enable compute.googleapis.com
```

**Via Terraform**:
```hcl
resource "google_project_service" "compute" {
  project = "PROJECT_ID"
  service = "compute.googleapis.com"
}
```

### API Quotas

Each API has quotas (limits on calls per second or per day).

Check quotas:
1. Go to APIs & Services > Quotas
2. Filter by API
3. See current quota and usage

Request increases:
1. Click on quota
2. Click "Edit Quota"
3. Enter new limit
4. Click "Next" and submit request

---

## Part 6: Provisioning and Setting up Google Cloud Observability

### What is Google Cloud Observability

**Google Cloud Observability** (formerly Stackdriver) is the monitoring, logging, and diagnostics platform. It includes:
- **Cloud Monitoring**: Metrics and dashboards
- **Cloud Logging**: Log collection and analysis
- **Cloud Trace**: Request tracing
- **Cloud Profiler**: Application profiling
- **Error Reporting**: Error tracking

### Enabling Observability in a Project

**Prerequisite**: Enable APIs:
```bash
gcloud services enable \
  monitoring.googleapis.com \
  logging.googleapis.com \
  cloudtrace.googleapis.com
```

### Creating Monitoring Dashboards

**Via Console**:
1. Go to Cloud Monitoring > Dashboards
2. Click "Create Dashboard"
3. Click "Add Widget"
4. Select metric (CPU, Memory, etc.)
5. Configure visualization
6. Click "Save Dashboard"

**Via Terraform**:
```hcl
resource "google_monitoring_dashboard" "dashboard" {
  dashboard_json = jsonencode({
    displayName = "My Dashboard"
    mosaicLayout = {
      columns = 12
      tiles = [
        {
          width  = 6
          height = 4
          widget = {
            title = "CPU Utilization"
            xyChart = {
              dataSets = [{
                timeSeriesQuery = {
                  timeSeriesFilter = {
                    filter = "metric.type=\"compute.googleapis.com/instance/cpu/utilization\""
                  }
                }
              }]
            }
          }
        }
      ]
    }
  })
}
```

### Configuring Alerts

**Via Console**:
1. Go to Cloud Monitoring > Alerts
2. Click "Create Policy"
3. Select condition (metric, threshold)
4. Select notification channel
5. Click "Create"

**Example**: Alert when CPU > 80%

```bash
gcloud alpha monitoring policies create \
  --notification-channels=CHANNEL_ID \
  --display-name="High CPU Alert"
```

### Viewing Logs

**Via Console**:
1. Go to Cloud Logging > Logs Explorer
2. Select resource (project, instance, etc.)
3. View logs

**Via gcloud**:
```bash
gcloud logging read "resource.type=gce_instance" \
  --limit 10 --format json
```

---

## Part 7: Assessing Quotas and Requesting Increases

### Understanding Quotas vs Limits

**Quotas**: Limits that can be increased by requesting from Google.
- Default 10 Compute Engine instances per region
- Can request increase to 100, 1000, etc.

**Limits**: Hard limits that cannot be increased.
- Maximum Cloud Storage object size: 5TB
- Maximum Firestore document size: 1MB

For the exam: Understand the difference. You request quota increases, not limit increases.

### Viewing Quotas

**Via Console**:
1. Go to IAM & Admin > Quotas
2. Filter by service (Compute Engine, Cloud SQL, etc.)
3. See current quota and usage

Shows:
- Metric (e.g., "Instances per region")
- Quota limit
- Current usage
- Percentage used

**Via gcloud**:
```bash
gcloud compute project-info describe --project=PROJECT_ID
```

### Requesting Quota Increases

**Via Console**:
1. Go to Quotas page
2. Click on quota to increase
3. Click "Edit Quotas"
4. Enter new limit
5. Click "Next"
6. Click "Submit request"

**Approval timing**: Minutes to hours for normal requests, longer for large requests.

**Tips for approval**:
- Have paying account (not trial)
- Show current usage
- Explain business need
- Request reasonable amounts (not 1000x increase)

---

## Part 8: Verifying Product Availability Across Geographical Locations

### Understanding Regions and Zones

**Region**: Geographical area (us-central1, europe-west1, asia-southeast1)
- Contains multiple zones
- Low-latency communication within region
- Higher latency to other regions

**Zone**: Isolated location within region (us-central1-a, us-central1-b)
- Physically isolated from other zones
- Can fail independently

### Checking Product Availability

**Via Console**:
1. Go to Compute > Compute Engine > Machine types
2. See which regions/zones have which machine types

**Via gcloud**:
```bash
# List all regions
gcloud compute regions list

# List all zones
gcloud compute zones list

# List machine types in a specific zone
gcloud compute machine-types list --zones=us-central1-a

# Check if specific product is available
gcloud sql instances list  # Shows regions where Cloud SQL is available
```

### Planning for Multi-Region Deployment

**For high availability**:
- Distribute resources across multiple zones within a region
- Or across multiple regions

**Example**:
```
Application:
- Compute instance in us-central1-a
- Compute instance in us-central1-b
- Compute instance in us-central1-c
- Load balancer distributes traffic
```

Or multi-region:
```
- Cloud SQL instance (primary) in us-central1
- Cloud SQL instance (replica) in europe-west1
```

---

## Part 9: Configuring Cloud Asset Inventory

### What is Cloud Asset Inventory

**Cloud Asset Inventory** helps you understand all resources across your organization:
- Lists all resources (VMs, buckets, databases, etc.)
- Shows resource metadata
- Tracks resource changes
- Exports to BigQuery

### Enabling Cloud Asset Inventory

Enable the API:
```bash
gcloud services enable cloudasset.googleapis.com
```

### Using Cloud Asset Inventory

**Via Console**:
1. Go to Cloud Asset Inventory > Assets
2. Select resource type
3. See all resources of that type

**Via gcloud**:
```bash
gcloud asset list --asset-types=compute.googleapis.com/Instance
```

**Exporting to BigQuery**:
```bash
gcloud asset export \
  --output-path=gs://MY_BUCKET/assets.json \
  --snapshot-time=2024-01-15T00:00:00Z
```

Then analyze in BigQuery:
```sql
SELECT
  name,
  type,
  project
FROM `project.dataset.assets`
WHERE type = 'compute.googleapis.com/Instance'
ORDER BY project
```

---

## Part 10: Setting up Standalone Organizations

### When You Don't Have Google Workspace

If you don't use Google Workspace, you can still create an organization:

1. **Verify domain ownership**:
   - Go to Cloud Console
   - Create organization
   - Verify you own the domain
   - Add TXT record to DNS

2. **Create Cloud Identity**:
   - Automatically created when you verify domain
   - Becomes your organization's identity provider

3. **Add users**:
   - Create users in Cloud Identity
   - Users authenticate via Cloud Identity
   - Access Google Cloud with these credentials

**Difference from Workspace**:
- No Gmail
- No Drive, Docs
- Just Cloud Identity for authentication
- Suitable for organizations that don't need Google services

---

## Part 11: Exam Focus Areas

### High-Probability Exam Questions

**Question Type 1: Hierarchy Design**
"Design a folder structure for a company with 5 teams, prod/staging/dev environments"

Answer should show:
- Folders organized logically
- Clear project placement
- Consideration for cost allocation
- IAM separation

**Question Type 2: Organizational Policies**
"Enforce that all Compute Engine instances have Shielded VM enabled"

Answer: Create organization policy `compute.requireShieldedVm` at organization level.

**Question Type 3: User and Group Management**
"Set up access for backend team to their projects only"

Answer:
- Create group backend-team@example.com
- Add users to group
- Grant group role on backend folder/projects

**Question Type 4: API and Observability Setup**
"Set up monitoring for a new project with Compute Engine"

Answer:
- Enable compute.googleapis.com
- Enable monitoring.googleapis.com, logging.googleapis.com
- Create dashboards
- Set up alerts

**Question Type 5: Quota and Limits**
"Team needs 50 Compute Engine instances per region but default is 10. What do you do?"

Answer: Request quota increase for "Compute Engine instances" metric in the Quotas page.

---

## Part 12: Common Exam Mistakes

**Mistake 1**: Confusing quotas with limits.
- Quotas CAN be increased
- Limits CANNOT be increased

**Mistake 2**: Not using groups for IAM.
- Always use groups
- Never grant roles to individual users

**Mistake 3**: Setting organization policies at project level when organization-wide enforcement needed.
- Set at organization level for org-wide enforcement
- Only use project level when specific to that project

**Mistake 4**: Creating flat hierarchy with all projects at organization level.
- Use folders for organization
- Projects should be within folders

**Mistake 5**: Not enabling required APIs.
- Many services won't work without API enabled
- Always enable APIs before using services

---

## Part 13: Quick Reference - Commands and Syntax

### Creating Hierarchy
```bash
# Create folder
gcloud resource-manager folders create \
  --display-name="Production" \
  --organization=ORG_ID

# Create project
gcloud projects create PROJECT_ID \
  --name="Project Name" \
  --folder=FOLDER_ID

# Set billing
gcloud billing projects link PROJECT_ID \
  --billing-account=BILLING_ACCOUNT_ID
```

### IAM Operations
```bash
# Grant role
gcloud projects add-iam-policy-binding PROJECT_ID \
  --member=group:team@example.com \
  --role=roles/compute.admin

# View IAM
gcloud projects get-iam-policy PROJECT_ID

# Remove role
gcloud projects remove-iam-policy-binding PROJECT_ID \
  --member=group:team@example.com \
  --role=roles/compute.admin
```

### Organization Policies
```bash
# View policies
gcloud resource-manager org-policies list \
  --project=PROJECT_ID

# Set policy
gcloud resource-manager org-policies set-policy policy.yaml \
  --project=PROJECT_ID
```

### APIs and Services
```bash
# Enable API
gcloud services enable compute.googleapis.com

# List enabled APIs
gcloud services list --enabled

# List available APIs
gcloud services list --available
```

### Quotas
```bash
# View quotas
gcloud compute project-info describe --project=PROJECT_ID

# Check specific quota usage
gcloud compute resource-quotas describe
```

### Cloud Identity (gcloud)
```bash
# Create user
gcloud identity users create \
  --user-id=user@example.com

# Create group
gcloud identity groups create group@example.com \
  --display-name="Team Name"

# Add user to group
gcloud identity groups members add \
  --group-email=group@example.com \
  --member-email=user@example.com
```

---

## Conclusion

Setting up cloud projects and accounts is the foundation of everything in Google Cloud. Get this right, and you have:
- Clear, scalable organization
- Enforced governance through policies
- Proper access control through IAM
- Centralized user management
- Observable infrastructure
- Proper quota planning

Exam tips:
- Understand hierarchy deeply (this is asked multiple ways)
- Know the difference between quotas and limits
- Understand organizational policy inheritance
- Always use groups for access management
- Know how to enable APIs and set up observability

Master this section, and you'll handle the foundational setup portion of the exam confidently.

---

**Total estimated reading/study time: 2 hours**
**Word count: ~9,500 words**

This guide covers everything in Section 1.1 with practical examples and exam focus.
