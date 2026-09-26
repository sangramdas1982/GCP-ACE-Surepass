# Mixed practice — 24 original questions

[Study guide home](README.md)

Allow about **50 minutes**, closed book. These are original learning questions, not copied exam items, dumps, or a full-length official mock. Choose one answer except where explicitly told otherwise. Record confidence before opening answers.

## Question 1 — 1.1

A project administrator has permission to create VMs, but creation in an unapproved region is rejected. What is the most targeted first check?

- **A.** The effective organization location policy
- **B.** Whether the administrator has project Viewer
- **C.** Whether the VM has a larger boot disk
- **D.** Whether billing export is enabled

## Question 2 — 1.2

You need an email when your training project approaches its monthly cost target. Which action directly meets this requirement?

- **A.** Reserve a static IP
- **B.** Configure a scoped budget with alert thresholds and recipients
- **C.** Disable every service API now
- **D.** Create a snapshot schedule

## Question 3 — 1.2

Finance wants recurring SQL analysis of project/service usage costs, including credits. What should you configure?

- **A.** Cloud Trace
- **B.** An OS Login role
- **C.** Cloud Billing export to BigQuery
- **D.** A VPC peering connection

## Question 4 — 4.1

A user inherits Editor from a folder and receives Viewer on a child project. What happens under allow policies alone?

- **A.** Viewer replaces Editor
- **B.** The user loses all access
- **C.** Only the latest binding applies
- **D.** The inherited Editor access remains

## Question 5 — 4.2

A developer can create a VM but cannot attach the selected runtime service account because actAs is denied. What permission is relevant?

- **A.** Appropriate Service Account User access on the selected account
- **B.** Storage Object Viewer on an unrelated bucket
- **C.** Billing Account Viewer
- **D.** Cloud SQL database read access

## Question 6 — 4.2

A CI pipeline has a supported external identity provider and must deploy without a stored JSON key. What is the best fit?

- **A.** A public bucket containing a key
- **B.** Workload Identity Federation with restrictive claims and IAM
- **C.** Workforce federation for every container process
- **D.** A shared human password

## Question 7 — 2.1

A small team needs to run a stateless HTTPS container and has no Kubernetes or OS-control requirement. Which is usually the simplest fit?

- **A.** Self-managed Kubernetes on VMs
- **B.** An unmanaged instance group
- **C.** Cloud Run service
- **D.** Cloud Workstations

## Question 8 — 2.1

A MIG has the correct number of instances, but one instance stops responding to application health checks. Which capability replaces it?

- **A.** Billing export
- **B.** HPA
- **C.** Object lifecycle management
- **D.** MIG autohealing

## Question 9 — 2.1

A retryable batch job writes checkpoints and final output to durable storage. It can tolerate worker interruption. Which option may reduce compute cost?

- **A.** Spot VMs
- **B.** A single non-retryable worker with only Local SSD data
- **C.** A permanently overprovisioned VM
- **D.** A larger static IP reservation

## Question 10 — 3.1

A Pod is Pending and events show insufficient memory for scheduling. Which action addresses the indicated layer?

- **A.** Rotate a database password
- **B.** Review Pod requests and available schedulable node capacity
- **C.** Create a Cloud Storage lifecycle rule
- **D.** Change only the application log severity

## Question 11 — 3.1

A GKE Pod cannot pull an Artifact Registry image from another project. What should you verify first?

- **A.** The user’s personal inbox quota
- **B.** The bucket retention duration
- **C.** The image URI and pulling identity’s repository access
- **D.** The HPA maximum replica count only

## Question 12 — 3.1

A new Cloud Run revision should receive a small amount of traffic before full rollout. Which approach fits?

- **A.** Delete the stable revision first
- **B.** Make the service public
- **C.** Increase a database backup retention period
- **D.** Deploy the revision and configure a small traffic split with monitoring

## Question 13 — 2.2

An unchanged application requires a shared NFS filesystem. What is a natural starting product?

- **A.** Filestore
- **B.** Pub/Sub
- **C.** BigQuery
- **D.** A Cloud Storage bucket treated as automatically identical to NFS

## Question 14 — 2.2

A database workload requires global relational transactions, strong consistency, and horizontal scale. Which service is a strong candidate?

- **A.** Memorystore as the only durable store
- **B.** Spanner
- **C.** Pub/Sub
- **D.** Cloud Logging

## Question 15 — 2.2

Two independent systems must each process the messages published on one Pub/Sub topic. How should you model consumption?

- **A.** Two workers sharing one subscription and assuming each receives every message
- **B.** One Cloud DNS record per worker
- **C.** Two subscriptions, one for each independent processing flow
- **D.** No subscriptions are needed

## Question 16 — 3.2

A user accidentally overwrites valid records; the replica has received the same writes. What is the appropriate recovery direction?

- **A.** Add another replica of the corrupted state
- **B.** Increase VM CPU
- **C.** Move the DNS zone
- **D.** Use a suitable backup or configured point-in-time recovery

## Question 17 — 2.3

The network team must manage shared subnets while application teams own VMs in separate projects. What fits?

- **A.** Shared VPC
- **B.** A budget alert
- **C.** A single object ACL
- **D.** Cloud Profiler

## Question 18 — 3.3

A private VM must initiate connections to arbitrary public package repositories without an external IP. What should you evaluate?

- **A.** A private DNS zone alone
- **B.** Public Cloud NAT with appropriate routing and egress rules
- **C.** Private Google Access as universal internet access
- **D.** An IAM Viewer role alone

## Question 19 — 2.3

Public HTTPS requests need routing to different backends based on URL path. Which family fits?

- **A.** A generic L4-only passthrough solution
- **B.** Cloud Router by itself
- **C.** An external Application Load Balancer
- **D.** A service-account key

## Question 20 — 3.4

You need to determine which principal deleted a VM. Where should you start?

- **A.** Guest memory metrics
- **B.** A cache hit-rate chart
- **C.** VPC Flow Logs only
- **D.** Relevant Cloud Audit Logs

## Question 21 — 3.4

An API is slow and you want to locate the downstream call consuming most of its request duration. Which tool fits?

- **A.** Cloud Trace with appropriate instrumentation
- **B.** Billing budgets
- **C.** OS Login
- **D.** Storage class transitions

## Question 22 — 3.2

A CMEK-protected service cannot use its key after a permission change. Which identity commonly needs key-use permission?

- **A.** Every internet user
- **B.** The documented service identity using that key
- **C.** Only the billing viewer
- **D.** Only a random project group

## Question 23 — 2.4

A team wants a reviewed preview of Terraform changes before modifying resources. Which command fits?

- **A.** terraform apply without review
- **B.** terraform init only
- **C.** terraform plan
- **D.** Delete the state file

## Question 24 — 4.2

Choose TWO sound practices for a GKE application needing access to one bucket.

- **A.** Grant broad project Editor to every node for convenience
- **B.** Store a downloaded service-account key in every container image
- **C.** Make the bucket public
- **D.** Use a distinct Kubernetes workload identity with supported federation
- **E.** Grant only the required access on the target bucket

---

# Answers and explanations

Count a multiple-select answer correct only when the entire required set is selected. Treat a correct guess as a revision gap. This score is not a prediction of the certification outcome.

## Answer 1 — A

**Why:** A configuration constraint can block an otherwise authorized creation request.

**Why the alternatives fail:** Viewer adds no missing create capability; disk size does not address location policy; billing export is reporting, not deployment authorization.

[Review subsection 1.1](01-environment/1-1-projects-and-accounts/README.md)

## Answer 2 — B

**Why:** A scoped budget provides the requested spending notifications.

**Why the alternatives fail:** A static IP and snapshots do not monitor costs. Disabling APIs interrupts work and does not satisfy the requested alerting behavior. A budget is not an automatic spending cap.

[Review subsection 1.2](01-environment/1-2-billing-configuration/README.md)

## Answer 3 — C

**Why:** Billing export makes supported cost records available for SQL-based analysis.

**Why the alternatives fail:** Trace studies request latency; OS Login governs Linux access; peering provides networking. None provides billing line items.

[Review subsection 1.2](01-environment/1-2-billing-configuration/README.md)

## Answer 4 — D

**Why:** Allow permissions combine; the child grant does not downgrade an ancestor grant.

**Why the alternatives fail:** There is no generic last-write-wins or most-specific-role-wins rule for these bindings. To reduce access, address the inappropriate grant or another applicable access control.

[Review subsection 4.1](04-access-and-security/4-1-iam-policies-and-roles/README.md)

## Answer 5 — A

**Why:** The deployer needs permission to act as the selected runtime identity.

**Why the alternatives fail:** Data and billing roles do not authorize account attachment. The account separately needs its own runtime resource permissions.

[Review subsection 4.2](04-access-and-security/4-2-service-accounts-and-workload-identity/README.md)

## Answer 6 — B

**Why:** Workload federation fits external software identities and short-lived access.

**Why the alternatives fail:** A public key file or shared password creates credential exposure. Workforce federation addresses human users rather than the stated software workload.

[Review subsection 4.2](04-access-and-security/4-2-service-accounts-and-workload-identity/README.md)

## Answer 7 — C

**Why:** Cloud Run provides a managed service runtime for the stated workload.

**Why the alternatives fail:** The VM/Kubernetes options add management without a stated need. Workstations are developer environments rather than the intended production serving platform.

[Review subsection 2.1](02-planning-and-implementation/2-1-compute-resources/README.md)

## Answer 8 — D

**Why:** Autohealing reacts to unhealthy instances and can recreate them.

**Why the alternatives fail:** Billing and object lifecycle are unrelated. HPA operates Kubernetes replicas; autoscaling a MIG also differs from replacing an unhealthy member.

[Review subsection 2.1](02-planning-and-implementation/2-1-compute-resources/README.md)

## Answer 9 — A

**Why:** Spot fits interruption-tolerant work when retries and external state are designed correctly.

**Why the alternatives fail:** Losing the only data copy is not acceptable. Overprovisioning need not reduce cost, and IP reservation size is not a compute scheduling strategy.

[Review subsection 2.1](02-planning-and-implementation/2-1-compute-resources/README.md)

## Answer 10 — B

**Why:** Scheduling depends on requests and compatible capacity. Check constraints and node autoscaling as well.

**Why the alternatives fail:** The other changes do not make the Pod schedulable and act on unrelated layers.

[Review subsection 3.1](03-operations/3-1-compute-operations/README.md)

## Answer 11 — C

**Why:** The path and pulling identity are directly involved in image retrieval.

**Why the alternatives fail:** Email and bucket retention are irrelevant. HPA settings do not establish image permissions; scaling more failing Pods may amplify symptoms.

[Review subsection 3.1](03-operations/3-1-compute-operations/README.md)

## Answer 12 — D

**Why:** A controlled split enables progressive exposure and a quick routing rollback.

**Why the alternatives fail:** Deleting the stable revision removes a rollback option. Public access and database retention do not implement a canary.

[Review subsection 3.1](03-operations/3-1-compute-operations/README.md)

## Answer 13 — A

**Why:** Filestore provides managed NFS file storage, subject to the required tier/capacity.

**Why the alternatives fail:** Messaging and analytical SQL are different interfaces. Object storage does not automatically supply all required NFS semantics.

[Review subsection 2.2](02-planning-and-implementation/2-2-storage-and-data-solutions/README.md)

## Answer 14 — B

**Why:** Spanner matches the distributed relational requirement.

**Why the alternatives fail:** A cache, messaging service, and log store do not provide the stated relational transaction model.

[Review subsection 2.2](02-planning-and-implementation/2-2-storage-and-data-solutions/README.md)

## Answer 15 — C

**Why:** Independent subscriptions maintain independent consumption flows.

**Why the alternatives fail:** Workers on one subscription commonly share its work. DNS is unrelated, and topic publication alone does not create the required consumer subscription state.

[Review subsection 2.2](02-planning-and-implementation/2-2-storage-and-data-solutions/README.md)

## Answer 16 — D

**Why:** Historical recovery can restore a point before the unwanted change.

**Why the alternatives fail:** Another current replica preserves the same bad state. CPU and DNS do not reconstruct previous records.

[Review subsection 3.2](03-operations/3-2-storage-and-data-operations/README.md)

## Answer 17 — A

**Why:** Shared VPC separates centralized network ownership from service-project workload administration.

**Why the alternatives fail:** The other choices address cost, object access, or code profiling and do not share host-project networking.

[Review subsection 2.3](02-planning-and-implementation/2-3-networking-resources/README.md)

## Answer 18 — B

**Why:** Public NAT supplies the relevant outbound translation path for eligible resources.

**Why the alternatives fail:** DNS only resolves names; Private Google Access targets supported Google APIs/services; Viewer is an API authorization role, not general internet connectivity.

[Review subsection 3.3](03-operations/3-3-network-operations/README.md)

## Answer 19 — C

**Why:** An Application Load Balancer understands supported HTTP routing attributes such as paths.

**Why the alternatives fail:** A transport-only option lacks the needed application routing; Cloud Router exchanges routes; a credential is not a traffic distributor.

[Review subsection 2.3](02-planning-and-implementation/2-3-networking-resources/README.md)

## Answer 20 — D

**Why:** Audit logs record supported resource-management activity and identity information.

**Why the alternatives fail:** Memory/cache metrics do not identify the deletion caller. Network flow records describe communication, not the full API authorization event.

[Review subsection 3.4](03-operations/3-4-monitoring-and-logging/README.md)

## Answer 21 — A

**Why:** Traces and spans expose request-level latency across instrumented components.

**Why the alternatives fail:** The other choices address unrelated financial, login, or storage lifecycle concerns.

[Review subsection 3.4](03-operations/3-4-monitoring-and-logging/README.md)

## Answer 22 — B

**Why:** The service’s documented encryption identity must be authorized on the compatible key.

**Why the alternatives fail:** Public or unrelated roles do not establish the correct key-use path. Human key administration and service cryptographic use are distinct.

[Review subsection 3.2](03-operations/3-2-storage-and-data-operations/README.md)

## Answer 23 — C

**Why:** Plan presents proposed resource actions based on configuration, state and observed resources.

**Why the alternatives fail:** Apply executes changes. Init prepares providers/backend. Deleting state is not a safe preview and can lose the management mapping.

[Review subsection 2.4](02-planning-and-implementation/2-4-infrastructure-and-ai-assisted-tooling/README.md)

## Answer 24 — D, E

**Why:** Distinct workload identity and narrow bucket access separate workloads and minimize privileges.

**Why the alternatives fail:** Broad node permissions couple applications. Embedded keys create long-lived secrets, and a public bucket defeats the requested controlled access.

[Review subsection 4.2](04-access-and-security/4-2-service-accounts-and-workload-identity/README.md)
