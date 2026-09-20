# Top 50 GCP IAM Practice Questions for the Associate Cloud Engineer Exam

This study guide contains 50 practice questions specifically focused on **Identity and Access Management (IAM)** on Google Cloud Platform (GCP). It covers resource hierarchies, primitive vs. predefined roles, custom roles, service accounts, IAM conditions, service account impersonation, workforce/workload identity federation, and security best practices.

---

## Section 1: IAM Fundamentals & Resource Hierarchy (Questions 1–10)

### Question 1
**Scenario:** You need to grant a contractor access to view resources in a specific GCP project. What is the most basic IAM element required to define *who* the contractor is?

- **A)** Role
- **B)** Principal (Member/Identity)
- **C)** Permission
- **D)** Policy

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Principal (Member/Identity)**

**Explanation:** In GCP IAM, a **Principal** (also referred to as a member or identity) defines *who* is requesting access. Principals can be a Google Account, a Google Workspace account, a Cloud Identity domain, a Service Account, or a Google Group. 
- A **Role** is a collection of permissions.
- A **Permission** determines what operation can be performed.
- A **Policy** binds principals to roles.
</details>

---

### Question 2
**Scenario:** Your organization enforces a strict policy: access permissions granted at higher levels in the GCP resource hierarchy must automatically apply to lower levels. How does IAM inheritance work across the GCP resource hierarchy?

- **A)** Permissions are inherited downward from Organization -> Folder -> Project -> Resource.
- **B)** Permissions are inherited upward from Resource -> Project -> Folder -> Organization.
- **C)** Permissions are additive and can be denied at lower levels using deny policies only.
- **D)** Permissions are explicit only and are never inherited across levels.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) Permissions are inherited downward from Organization -> Folder -> Project -> Resource.**

**Explanation:** GCP IAM policy evaluation is **inherited hierarchically**. If you grant a user a role at the Organization level, they inherit that role across all Folders, Projects, and Resources beneath that Organization. You cannot override or "revoke" inherited grant access at a lower level using standard allow policies (though IAM Deny policies can block permissions regardless of inheritance).
</details>

---

### Question 3
**Scenario:** An developer has the `roles/viewer` role applied at the Folder level. At the Project level within that Folder, they are granted `roles/editor`. What effective permissions does the developer have on the project?

- **A)** Only `roles/viewer` permissions because folder rules override project rules.
- **B)** Both `roles/viewer` and `roles/editor` permissions (effectively `roles/editor`).
- **C)** No permissions because granting conflicting roles causes an IAM error.
- **D)** Only permissions explicitly defined in a custom role.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Both `roles/viewer` and `roles/editor` permissions (effectively `roles/editor`).**

**Explanation:** IAM policies are **union-based (additive)**. The effective access for a principal on a resource is the union of the permissions granted at that resource level and all parent levels. Since `roles/editor` contains all permissions of `roles/viewer` plus write permissions, the user effectively acts as an Editor on the project.
</details>

---

### Question 4
**Scenario:** Which of the following is **NOT** a valid principal type in GCP IAM policies?

- **A)** `user:alice@example.com`
- **B)** `group:dev-team@example.com`
- **C)** `ip:192.168.1.1/24`
- **D)** `serviceAccount:my-sa@my-project.iam.gserviceaccount.com`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **C) `ip:192.168.1.1/24`**

**Explanation:** GCP IAM principals represent identities (users, groups, domains, service accounts). An IP address or range is not an identity principal. IP restrictions can be configured using VPC Service Controls or IAM Conditions, but cannot be directly specified as an IAM member identifier string in standard bindings.
</details>

---

### Question 5
**Scenario:** A enterprise requires central administration of users and groups for GCP integration. Which service serves as Google’s cloud-based Identity Provider (IdP) for managing users, groups, and SSO?

- **A)** Cloud Identity
- **B)** Google Secret Manager
- **C)** Cloud Key Management Service (KMS)
- **D)** Identity-Aware Proxy (IAP)

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) Cloud Identity**

**Explanation:** **Cloud Identity** (or Google Workspace) is the identity solution that manages users, groups, and single sign-on (SSO) credentials used by Google Cloud IAM.
</details>

---

### Question 6
**Scenario:** You need to prevent all developers in an organization from assigning public IP addresses to Compute Engine instances, regardless of their individual IAM roles. What GCP feature should you use?

- **A)** IAM Custom Roles
- **B)** Organization Policies
- **C)** VPC Firewall Rules
- **D)** Service Account Scopes

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Organization Policies**

**Explanation:** **Organization Policies** provide centralized, programmatic control over your organization's cloud resources. While IAM focuses on *who* can do *what*, Organization Policies enforce constraints on *resources* regardless of who is acting (e.g., restricting public IPs or restricting external service account creation).
</details>

---

### Question 7
**Scenario:** What format does a standard GCP IAM permission take?

- **A)** `[project].[service].[action]`
- **B)** `[service].[resource].[action]`
- **C)** `[role].[service].[permission]`
- **D)** `[action].[service].[resource]`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) `[service].[resource].[action]`**

**Explanation:** GCP permissions follow the naming convention `service.resource.action`. For example: `compute.instances.list`, `storage.buckets.create`, or `pubsub.topics.publish`.
</details>

---

### Question 8
**Scenario:** What happens when an IAM Deny policy rule matches a principal attempting to perform an action?

- **A)** The operation proceeds if the user has the Primitive Owner role.
- **B)** The operation is denied, overriding any IAM Allow policies granted to the user.
- **C)** An alert is logged, but access is allowed if an explicit grant exists.
- **D)** The deny rule applies only if the user has no inherited permissions.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) The operation is denied, overriding any IAM Allow policies granted to the user.**

**Explanation:** **IAM Deny policies** override all IAM allow policies. If a deny rule matches the request, access is blocked regardless of any permissions granted via project or folder roles.
</details>

---

### Question 9
**Scenario:** You want to list all resources and their attached IAM policies across your entire GCP organization in a searchable interface. Which tool should you use?

- **A)** Cloud Monitoring
- **B)** Cloud Asset Inventory
- **C)** Cloud Audit Logs
- **D)** Security Command Center Standard

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Cloud Asset Inventory**

**Explanation:** **Cloud Asset Inventory** allows you to export and query resource metadata and IAM policies across an entire GCP Organization in real time or historically.
</details>

---

### Question 10
**Scenario:** A company wants to use their existing Active Directory (AD) to authenticate users into GCP without creating new passwords. What is the recommended approach?

- **A)** Import user passwords into Cloud Identity via CSV.
- **B)** Federated single sign-on (SSO) using Google Cloud Directory Sync (GCDS) and SAML/OIDC.
- **C)** Create service accounts for every user in Active Directory.
- **D)** Attach AD group tokens directly inside IAM policies.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Federated single sign-on (SSO) using Google Cloud Directory Sync (GCDS) and SAML/OIDC.**

**Explanation:** Use **GCDS** to synchronize users and groups from Active Directory into Cloud Identity, then set up SAML 2.0 / OIDC single sign-on (SSO) with your IdP (e.g., AD FS, Okta, Microsoft Entra ID).
</details>

---

## Section 2: Roles & Permissions (Questions 11–20)

### Question 11
**Scenario:** What are the three Primitive (Basic) roles in Google Cloud Platform?

- **A)** Administrator, User, Guest
- **B)** Owner, Editor, Viewer
- **C)** Root, Admin, ReadOnly
- **D)** Manager, Developer, Auditor

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Owner, Editor, Viewer**

**Explanation:** The original historical roles in GCP are called **Primitive roles** (or Basic roles): `Owner` (`roles/owner`), `Editor` (`roles/editor`), and `Viewer` (`roles/viewer`). Google recommends using Predefined roles instead of Primitive roles in production due to security risks.
</details>

---

### Question 12
**Scenario:** Which capability is possessed by a **Project Owner** (`roles/owner`) that an **Project Editor** (`roles/editor`) does NOT have?

- **A)** Restarting Compute Engine VMs.
- **B)** Editing Cloud Storage objects.
- **C)** Modifying IAM policies and project billing associations.
- **D)** Viewing Cloud Audit Logs.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **C) Modifying IAM policies and project billing associations.**

**Explanation:** `roles/editor` can create, modify, and delete most resources but **cannot** manage access control (IAM policies) or change project-level billing connections. Only `roles/owner` (or specific administrative roles like `roles/resourcemanager.projectIamAdmin`) can modify IAM permissions.
</details>

---

### Question 13
**Scenario:** You need to give an auditor access to view Cloud Storage bucket settings and list objects, but not read object data. Which type of role should you select?

- **A)** Primitive Role (`roles/viewer`)
- **B)** Predefined Role (`roles/storage.objectViewer` or `roles/storage.admin`)
- **C)** Predefined Role (`roles/storage.insightsViewer` or `roles/storage.bucketViewer`)
- **D)** Primitive Role (`roles/owner`)

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **C) Predefined Role (`roles/storage.insightsViewer` or `roles/storage.bucketViewer`)**

**Explanation:** **Predefined roles** offer fine-grained access maintained by Google for specific services. Primitive viewer (`roles/viewer`) gives broad read access across almost all services. To follow least privilege, use specific predefined roles like `roles/storage.bucketViewer`.
</details>

---

### Question 14
**Scenario:** You create a custom IAM role to allow junior engineers to restart Compute Engine instances. A new feature is released for Compute Engine that requires a new permission. How are custom roles updated when GCP releases new service features?

- **A)** Google automatically updates custom roles with new permissions.
- **B)** Custom roles must be manually updated by an admin to include newly released permissions.
- **C)** Custom roles automatically inherit from predefined parent roles.
- **D)** Custom roles expire after 90 days if not manually updated.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Custom roles must be manually updated by an admin to include newly released permissions.**

**Explanation:** Unlike Predefined roles (which Google automatically updates when services change), **Custom roles are not automatically maintained by Google**. You must manually manage and add permissions as new features are released.
</details>

---

### Question 15
**Scenario:** Which permission allows a user to grant or revoke IAM roles on a GCP project?

- **A)** `resourcemanager.projects.get`
- **B)** `resourcemanager.projects.setIamPolicy`
- **C)** `iam.roles.create`
- **D)** `iam.serviceAccounts.actAs`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) `resourcemanager.projects.setIamPolicy`**

**Explanation:** The `setIamPolicy` permission on a resource (in this case, `resourcemanager.projects.setIamPolicy`) is required to bind principals to roles on that resource.
</details>

---

### Question 16
**Scenario:** You need to grant a user permission to view BigQuery dataset schemas, but no predefined role matches your exact compliance requirement. What should you create?

- **A)** A Custom Role combining specific permissions like `bigquery.datasets.get`.
- **B)** A Primitive Owner role restricted with an organization policy.
- **C)** A Google Group containing standard predefined roles.
- **D)** A Service Account with temporary access keys.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) A Custom Role combining specific permissions like `bigquery.datasets.get`.**

**Explanation:** When Predefined roles do not meet your exact requirements or violate the principle of least privilege, you should create a **Custom Role** containing only the specific list of permissions required.
</details>

---

### Question 17
**Scenario:** At which levels in the GCP resource hierarchy can **Custom Roles** be created?

- **A)** Resource level only.
- **B)** Project level and Organization level only.
- **C)** Folder level only.
- **D)** Any level including bucket and VM instances.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Project level and Organization level only.**

**Explanation:** Custom roles can be created only at the **Project** level or **Organization** level. They cannot be defined at the Folder level or directly on individual resources.
</details>

---

### Question 18
**Scenario:** Why does Google strongly discourage the routine operational use of Primitive roles in production environments?

- **A)** Primitive roles do not support audit logging.
- **B)** Primitive roles are too broad and violate the Principle of Least Privilege.
- **C)** Primitive roles incur extra charges per user per month.
- **D)** Primitive roles cannot be assigned to Google Groups.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Primitive roles are too broad and violate the Principle of Least Privilege.**

**Explanation:** Primitive roles (`Owner`, `Editor`, `Viewer`) grant widespread access across almost every GCP service in a project. This creates significant security risks compared to granular Predefined roles.
</details>

---

### Question 19
**Scenario:** A DevOps team member needs to deploy Cloud Functions, but shouldn't have access to modify BigQuery datasets or Compute Engine instances in the same project. What role should be assigned?

- **A)** `roles/editor`
- **B)** `roles/developer`
- **C)** `roles/cloudfunctions.developer`
- **D)** `roles/owner`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **C) `roles/cloudfunctions.developer`**

**Explanation:** `roles/cloudfunctions.developer` is a predefined role that grants full permissions to create, edit, deploy, and delete Cloud Functions without granting permissions to other unrelated services like BigQuery or Compute Engine.
</details>

---

### Question 20
**Scenario:** An admin sets a custom role launch stage to `DISABLED`. What happens to principals assigned to this role?

- **A)** They automatically switch to `roles/viewer`.
- **B)** They can no longer perform any of the permissions contained in that custom role.
- **C)** They retain permissions for 24 hours until caches expire.
- **D)** The project is automatically locked down.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) They can no longer perform any of the permissions contained in that custom role.**

**Explanation:** Setting a custom role's stage to `DISABLED` revokes the effective permissions granted by that role for all members assigned to it.
</details>

---

## Section 3: Service Accounts & Key Management (Questions 21–30)

### Question 21
**Scenario:** What is a Service Account in GCP?

- **A)** An identity used by an individual human developer to log in via web browser.
- **B)** A special account used by an application or compute workload, rather than an individual end user.
- **C)** A billing account used to process credit card payments.
- **D)** An external identity provider group mapping.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) A special account used by an application or compute workload, rather than an individual end user.**

**Explanation:** A **Service Account** is an identity associated with non-human workloads (such as a VM instance, Cloud Run service, or GKE pod) to authenticate and call GCP APIs.
</details>

---

### Question 22
**Scenario:** Compute Engine default service accounts are created automatically in new projects. What is the default IAM role assigned to these default service accounts if automatic role grant is enabled?

- **A)** `roles/owner`
- **B)** `roles/editor`
- **C)** `roles/viewer`
- **D)** `roles/compute.admin`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) `roles/editor`**

**Explanation:** By default (unless disabled by Organization Policy), the Compute Engine default service account is automatically granted the primitive **`Editor`** (`roles/editor`) role. Security best practice recommends disabling this automatic grant and attaching least-privilege predefined roles instead.
</details>

---

### Question 23
**Scenario:** An application running on a Compute Engine instance needs to read files from a Cloud Storage bucket. What is the most secure way to authenticate the application?

- **A)** Hardcode a service account JSON key inside the application source code.
- **B)** Assign a service account with `roles/storage.objectViewer` to the Compute Engine instance and use Application Default Credentials (ADC).
- **C)** Store service account credentials in a public GitHub repository.
- **D)** Grant the user running the VM the `roles/owner` role.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Assign a service account with `roles/storage.objectViewer` to the Compute Engine instance and use Application Default Credentials (ADC).**

**Explanation:** Attaching a custom user-managed service account directly to the VM metadata allows applications using standard GCP SDKs to automatically retrieve short-lived tokens via **Application Default Credentials (ADC)** without storing service account keys anywhere on disk.
</details>

---

### Question 24
**Scenario:** A developer requests a downloadable service account JSON key file to run a script from their local laptop. What major risk is introduced by generating service account keys?

- **A)** Service account keys expire every 1 hour automatically, causing script failures.
- **B)** User-managed service account keys do not expire automatically and pose a credential leak security risk if mishandled.
- **C)** Downloadable keys can only be generated by Google support.
- **D)** Downloadable keys disable Cloud Audit logging.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) User-managed service account keys do not expire automatically and pose a credential leak security risk if mishandled.**

**Explanation:** User-managed keys (JSON/P12 files) do not have built-in expiration dates. If leaked or committed to public code repositories, attackers gain long-term unauthorized access until manually revoked.
</details>

---

### Question 25
**Scenario:** What permission must a developer have to attach a service account to a VM instance or deploy a Cloud Function that runs as that service account?

- **A)** `iam.serviceAccounts.actAs` (or `roles/iam.serviceAccountUser`)
- **B)** `iam.serviceAccounts.create`
- **C)** `resourcemanager.projects.get`
- **D)** `roles/owner`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) `iam.serviceAccounts.actAs` (or `roles/iam.serviceAccountUser`)**

**Explanation:** To attach a service account to a resource (like a Compute Engine VM or Cloud Run service), the user performing the deployment must have the **`iam.serviceAccounts.actAs`** permission on that service account (provided by `roles/iam.serviceAccountUser`).
</details>

---

### Question 26
**Scenario:** Which email format represents a default Compute Engine service account?

- **A)** `[project-number]-compute@developer.gserviceaccount.com`
- **B)** `[project-id]@appspot.gserviceaccount.com`
- **C)** `service-[project-number]@compute-system.iam.gserviceaccount.com`
- **D)** `admin@[project-id].iam.gserviceaccount.com`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) `[project-number]-compute@developer.gserviceaccount.com`**

**Explanation:** Compute Engine default service accounts follow the naming convention `PROJECT_NUMBER-compute@developer.gserviceaccount.com`. (Note: `PROJECT_ID@appspot.gserviceaccount.com` is the App Engine default service account).
</details>

---

### Question 27
**Scenario:** What is the difference between Google-managed keys and User-managed keys for Service Accounts?

- **A)** Google-managed keys are downloaded as JSON files; User-managed keys are stored in KMS.
- **B)** Google-managed keys are automatically rotated by Google and stored securely; User-managed keys are generated and rotated by the user.
- **C)** Google-managed keys never expire; User-managed keys expire every 24 hours.
- **D)** User-managed keys can only be used on-premises.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Google-managed keys are automatically rotated by Google and stored securely; User-managed keys are generated and rotated by the user.**

**Explanation:** Google automatically handles creation, storage, and rotation (roughly every two weeks) of **Google-managed keys** used internally by GCP services. **User-managed keys** are created, downloaded, and managed manually by users.
</details>

---

### Question 28
**Scenario:** You want to temporarily allow a script running on your local machine to authenticate as a service account without downloading a permanent key file. Which technique should you use?

- **A)** Service Account Impersonation (`gcloud auth application-default login --impersonate-service-account`)
- **B)** Export the project owner credentials to your local `.bashrc`.
- **C)** Create a primitive owner role for your personal Gmail account.
- **D)** Enable Service Account Key Auto-Download in GCP Console.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) Service Account Impersonation (`gcloud auth application-default login --impersonate-service-account`)**

**Explanation:** **Service Account Impersonation** allows an authenticated user to generate short-lived OAuth 2.0 access tokens on behalf of a service account without generating or storing long-lived service account key files.
</details>

---

### Question 29
**Scenario:** How does IAM view a Service Account in terms of identity and resource concepts?

- **A)** A service account is exclusively an identity and cannot be a resource.
- **B)** A service account is exclusively a resource and cannot be an identity.
- **C)** A service account is BOTH an identity (can be granted roles) AND a resource (can have IAM policies attached to it).
- **D)** A service account is neither identity nor resource.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **C) A service account is BOTH an identity (can be granted roles) AND a resource (can have IAM policies attached to it).**

**Explanation:** A Service Account is unique because it is **both an identity** (it can be given permissions to access resources) and **a resource** (other users can be granted permissions *on* the service account, such as `roles/iam.serviceAccountUser` or `roles/iam.serviceAccountTokenCreator`).
</details>

---

### Question 30
**Scenario:** You want to enforce an organizational rule that completely prevents team members from creating user-managed service account keys. Which feature should you configure?

- **A)** An Organization Policy constraint: `iam.disableServiceAccountKeyCreation`
- **B)** A VPC firewall rule blocking port 443
- **C)** Remove the `roles/viewer` role from all developers
- **D)** Enable Cloud KMS key destruction

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) An Organization Policy constraint: `iam.disableServiceAccountKeyCreation`**

**Explanation:** The organization policy constraint **`constraints/iam.disableServiceAccountKeyCreation`** prevents users from creating new user-managed service account keys across the organization or project.
</details>

---

## Section 4: Advanced Access Control & Policy Binding (Questions 31–40)

### Question 31
**Scenario:** You need to grant a user `roles/storage.objectAdmin` on a Cloud Storage bucket, but ONLY between 09:00 AM and 05:00 PM on weekdays. What IAM feature allows this?

- **A)** IAM Conditions
- **B)** Service Account Scopes
- **C)** Cloud Armor Rules
- **D)** VPC Service Controls

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) IAM Conditions**

**Explanation:** **IAM Conditions** allow you to define conditional attribute-based access control policies in IAM bindings based on factors like time/date, request attributes, or resource names using Common Expression Language (CEL).
</details>

---

### Question 32
**Scenario:** A company wants to grant a contractor temporary access to view logs for 4 hours. Which IAM condition attribute expression type is best suited for setting access expiry?

- **A)** `request.time < timestamp("2026-10-15T18:00:00Z")`
- **B)** `resource.name.startsWith("projects/my-project")`
- **C)** `request.auth.claims.email`
- **D)** `device.is_approved`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) `request.time < timestamp("2026-10-15T18:00:00Z")`**

**Explanation:** The `request.time` attribute in IAM Conditions allows you to check the current timestamp against a target threshold to configure automatic expiration of access.
</details>

---

### Question 33
**Scenario:** You want to allow a developer to manage Compute Engine instances whose names start with `dev-`. What IAM Condition function should you use in the condition expression?

- **A)** `resource.name.startsWith("projects/_/zones/_/instances/dev-")`
- **B)** `request.user.contains("dev")`
- **C)** `resource.type == "developer"`
- **D)** `iam.serviceAccount.name("dev")`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) `resource.name.startsWith("projects/_/zones/_/instances/dev-")`**

**Explanation:** Resource attribute evaluation in IAM Conditions allows matching against full or partial resource names using functions like `startsWith()` or `endsWith()`.
</details>

---

### Question 34
**Scenario:** What is the relationship between IAM roles and legacy API Scopes on a Compute Engine instance?

- **A)** API Scopes override IAM roles and grant extra permissions even if IAM denies them.
- **B)** Access is determined by the INTERSECTION of IAM permissions and API Scopes (both must allow the action).
- **C)** API Scopes have replaced IAM roles entirely.
- **D)** API Scopes apply only to internal Google employees.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Access is determined by the INTERSECTION of IAM permissions and API Scopes (both must allow the action).**

**Explanation:** Legacy API scopes set a maximum boundary on what the instance service account can perform. Effective access is the **intersection** of API scopes and IAM roles. Google recommends setting the scope to `cloud-platform` ("Allow full access to all Cloud APIs") on the VM and managing actual access exclusively via IAM roles.
</details>

---

### Question 35
**Scenario:** You have an application running in Amazon Web Services (AWS) EC2 that needs to store files in a GCP Cloud Storage bucket. You want to avoid using downloadable GCP service account keys. What solution should you implement?

- **A)** Workload Identity Federation
- **B)** Workforce Identity Federation
- **C)** Cloud Identity Directory Sync
- **D)** Chrome Enterprise Core

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) Workload Identity Federation**

**Explanation:** **Workload Identity Federation** lets external workloads (AWS, Azure, GitHub Actions, on-premises Kubernetes) authenticate to GCP resources directly using short-lived tokens, eliminating the need for long-lived service account key files.
</details>

---

### Question 36
**Scenario:** Your company wants corporate employees to access Google Cloud Console using their existing Okta identity credentials without synchronizing user accounts into Google Cloud Identity. Which feature should you use?

- **A)** Workload Identity Federation
- **B)** Workforce Identity Federation
- **C)** Primitive IAM roles
- **D)** Service Account impersonation

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Workforce Identity Federation**

**Explanation:** **Workforce Identity Federation** extends external identity providers (like Azure AD/Entra ID, Okta, Ping Identity) to human users (workforce) so they can access GCP resources directly using OpenID Connect (OIDC) or SAML 2.0 without requiring Cloud Identity accounts.
</details>

---

### Question 37
**Scenario:** What command-line tool is primarily used by Cloud Engineers to inspect and bind IAM policies to projects?

- **A)** `kubectl`
- **B)** `gcloud`
- **C)** `gsutil`
- **D)** `bq`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) `gcloud`**

**Explanation:** The **`gcloud` CLI** (e.g., `gcloud projects add-iam-policy-binding`) is the primary CLI tool used to manage GCP IAM policies and infrastructure settings.
</details>

---

### Question 38
**Scenario:** You execute the following command:
`gcloud projects add-iam-policy-binding my-project --member='user:alice@example.com' --role='roles/storage.objectViewer'`

What is the result of this command?

- **A)** Alice replaces all existing object viewers on `my-project`.
- **B)** `roles/storage.objectViewer` is added to Alice’s existing permissions on `my-project` without removing other bindings.
- **C)** Alice’s account is deleted and recreated as a service account.
- **D)** `my-project` is set to read-only for all users.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) `roles/storage.objectViewer` is added to Alice’s existing permissions on `my-project` without removing other bindings.**

**Explanation:** The `add-iam-policy-binding` command appends a single role-member binding to the existing project IAM policy policy array without overwriting other members or bindings.
</details>

---

### Question 39
**Scenario:** You need to audit who changed an IAM policy binding on a project yesterday. Which log stream inside Cloud Audit Logs should you inspect?

- **A)** Data Access Audit Logs
- **B)** Admin Activity Audit Logs
- **C)** System Event Audit Logs
- **D)** Policy Analyzer Realtime Stream

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Admin Activity Audit Logs**

**Explanation:** **Admin Activity Audit Logs** contain log entries for API calls or administrative actions that modify configuration or metadata of resources (such as updating IAM policies via `setIamPolicy`). These logs are enabled by default and free of charge.
</details>

---

### Question 40
**Scenario:** What is the primary purpose of Policy Intelligence (Policy Analyzer / Policy Troubleshooter) in GCP IAM?

- **A)** Automatically encrypting service account keys with AES-256.
- **B)** Analyzing, troubleshooting, and diagnosing why a user has or does not have specific permissions on a resource.
- **C)** Preventing network traffic from crossing internet gateways.
- **D)** Converting AWS IAM policies into GCP Terraform code.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Analyzing, troubleshooting, and diagnosing why a user has or does not have specific permissions on a resource.**

**Explanation:** **Policy Troubleshooter** and **Policy Analyzer** help security teams understand effective access, debug permission issues, and evaluate access scenarios across complex resource hierarchies.
</details>

---

## Section 5: Best Practices, Security & Scenario Questions (Questions 41–50)

### Question 41
**Scenario:** You are configuring access for a team of 15 data analysts who need access to BigQuery. What is the recommended IAM administration best practice?

- **A)** Grant individual IAM permissions to each developer's personal Google account email separately.
- **B)** Create a Google Group (e.g., `data-analysts@example.com`), assign the appropriate IAM role to the group, and add developers to the group.
- **C)** Create 15 service accounts and give JSON key files to each analyst.
- **D)** Grant the entire team the `roles/owner` role at the Organization level.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Create a Google Group (e.g., `data-analysts@example.com`), assign the appropriate IAM role to the group, and add developers to the group.**

**Explanation:** **Use Google Groups to manage access**. Assigning roles to groups rather than individual users greatly simplifies onboarding, offboarding, and auditing.
</details>

---

### Question 42
**Scenario:** Which principle dictates that users and service accounts should be granted only the minimum permissions necessary to perform their assigned tasks?

- **A)** Principle of Separation of Duties
- **B)** Principle of Least Privilege
- **C)** Defense in Depth
- **D)** Zero Trust Architecture

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Principle of Least Privilege**

**Explanation:** The **Principle of Least Privilege** is the foundational security principle of giving a subject only those privileges necessary to perform its duties.
</details>

---

### Question 43
**Scenario:** A security policy requires that developers should be able to restart Compute Engine instances in a staging environment, but must request temporary approval to modify production instances. Which GCP tool/feature facilitates just-in-time (JIT) privileged access management?

- **A)** IAM Privilege Access Manager (PAM)
- **B)** VPC Service Controls
- **C)** Cloud Armor
- **D)** Identity-Aware Proxy

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) IAM Privilege Access Manager (PAM)**

**Explanation:** **Privilege Access Manager (PAM)** allows organizations to implement Just-In-Time (JIT) access requests, approvals, and temporary privilege escalation in GCP.
</details>

---

### Question 44
**Scenario:** You receive a security alert that an employee left the company today. Their account was deactivated in Cloud Identity. What happens to their active access across GCP?

- **A)** They retain access until someone manually removes them from every project IAM policy.
- **B)** Once their Cloud Identity user account is suspended or deleted, all authenticated requests made by that user are immediately blocked by GCP.
- **C)** Access remains active for 30 days.
- **D)** Service account keys generated by the user continue working indefinitely unless revoked.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Once their Cloud Identity user account is suspended or deleted, all authenticated requests made by that user are immediately blocked by GCP.**

**Explanation:** Suspending or deleting a user account in Cloud Identity invalidates authentication, preventing the account from obtaining valid tokens to access any GCP resources.
</details>

---

### Question 45
**Scenario:** An application deployed on Google Kubernetes Engine (GKE) needs access to Pub/Sub topics. What is the Google-recommended method to grant IAM roles directly to GKE Kubernetes Service Accounts (KSA)?

- **A)** Store a service account private key in a Kubernetes Secret.
- **B)** Workload Identity (GKE Workload Identity)
- **C)** Compute Engine Default Service Account with Primitive Owner role.
- **D)** Mount service account keys on an NFS persistent volume.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Workload Identity (GKE Workload Identity)**

**Explanation:** **GKE Workload Identity** links Kubernetes Service Accounts (KSA) to Google Cloud Service Accounts (GSA), allowing GKE pods to authenticate natively without managing long-lived key files in Kubernetes secrets.
</details>

---

### Question 46
**Scenario:** Which of the following commands correctly creates a custom role named `instanceRestarter` in a project named `my-project` using a YAML definition file?

- **A)** `gcloud iam roles create instanceRestarter --project=my-project --file=role-definition.yaml`
- **B)** `gcloud compute instances add-role instanceRestarter --file=role-definition.yaml`
- **C)** `gcloud projects add-iam-policy-binding my-project --role=instanceRestarter`
- **D)** `gsutil iam create instanceRestarter my-project`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) `gcloud iam roles create instanceRestarter --project=my-project --file=role-definition.yaml`**

**Explanation:** The `gcloud iam roles create` command with the `--project` flag and `--file` specification is used to create custom roles at the project level.
</details>

---

### Question 47
**Scenario:** You want to audit unused permissions granted to users across your projects to reduce overall attack surface. Which IAM feature provides automatic recommendations to downscope roles?

- **A)** IAM Recommender (Role Recommender)
- **B)** Cloud Security Scanner
- **C)** Cloud Trace
- **D)** Eventarc

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) IAM Recommender (Role Recommender)**

**Explanation:** **IAM Recommender** analyzes historical permission usage over the last 90 days using machine learning and recommends safer, smaller predefined or custom roles to enforce least privilege.
</details>

---

### Question 48
**Scenario:** A company wants to ensure that Cloud Storage buckets cannot be made publicly accessible on the internet, even if an administrator mistakenly grants `allUsers` the `roles/storage.objectViewer` role. What should be enabled?

- **A)** Public Access Prevention (PAP) or Organization Policy `storage.publicAccessPrevention`
- **B)** Service Account impersonation
- **C)** Cloud Key Management Service (KMS) auto-rotation
- **D)** Storage Object Versioning

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **A) Public Access Prevention (PAP) or Organization Policy `storage.publicAccessPrevention`**

**Explanation:** **Public Access Prevention (PAP)** on Cloud Storage (enforced via bucket settings or Organization Policy) blocks public access bindings like `allUsers` and `allAuthenticatedUsers`, safeguarding data against accidental exposure.
</details>

---

### Question 49
**Scenario:** What special principal identifier is used in GCP IAM policies to represent ANY user on the internet, authenticated or unauthenticated?

- **A)** `allAuthenticatedUsers`
- **B)** `allUsers`
- **C)** `anyone@google.com`
- **D)** `*`

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) `allUsers`**

**Explanation:**
- `allUsers` represents anyone on the internet (public access).
- `allAuthenticatedUsers` represents any account authenticated with a Google account or service account anywhere in the world.
</details>

---

### Question 50
**Scenario:** You are designing a CI/CD pipeline in GitHub Actions to deploy infrastructure to Google Cloud using Terraform. Which combination of practices offers the maximum security posture?

- **A)** Generate a user-managed JSON service account key, base64 encode it, and save it in GitHub Secrets.
- **B)** Use **Workload Identity Federation** to let GitHub Actions impersonate a specific GCP Service Account without storing long-lived keys, and grant that Service Account fine-grained predefined roles.
- **C)** Grant the GitHub runner full `roles/owner` access using basic HTTP header auth.
- **D)** Grant individual developers `roles/resourcemanager.organizationAdmin` and manually trigger deployments from local laptops.

<details>
<summary><b>Answer and Explanation</b></summary>

**Correct Answer:** **B) Use Workload Identity Federation to let GitHub Actions impersonate a specific GCP Service Account without storing long-lived keys, and grant that Service Account fine-grained predefined roles.**

**Explanation:** Using **Workload Identity Federation** avoids service account key creation entirely. GitHub Actions trades its OIDC token for short-lived GCP credentials dynamically during runtime, drastically reducing security exposure while maintaining full access control via fine-grained IAM roles.
</details>