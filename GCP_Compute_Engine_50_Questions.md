# GCP COMPUTE ENGINE
## Top 50 Practice Questions

### Google Cloud Engineer Certification Exam

**Exam Pattern:** Multiple Choice & Scenario-Based Questions  
**Difficulty Level:** Medium to Advanced

---

## TABLE OF CONTENTS

1. Instance Creation & Management (Questions 1-8)
2. Machine Types & Sizing (Questions 9-14)
3. Images, Snapshots & Disks (Questions 15-22)
4. Networking & Firewall (Questions 23-29)
5. Instance Groups & Autoscaling (Questions 30-36)
6. Load Balancing (Questions 37-42)
7. Security & IAM (Questions 43-48)
8. Monitoring, Logging & Cost (Questions 49-50)

---

## SECTION 1: INSTANCE CREATION & MANAGEMENT

### Q1: Local SSD Persistence
You need to create a VM instance that must persist data on a local SSD. The instance will be used for high-speed data processing. What should you consider when creating this instance?

- A) Local SSDs provide persistent storage that survives instance termination
- B) Local SSDs are ephemeral and data will be lost if the instance is stopped or migrated
- C) Local SSDs can be shared across multiple instances
- D) Local SSDs automatically replicate to other zones

**✓ Answer: B**

---

### Q2: Preemptible Instance Termination
A preemptible VM instance was terminated unexpectedly during a batch processing job. What is the key characteristic of preemptible instances you should have considered?

- A) They provide 99.95% SLA uptime
- B) Google Cloud can terminate them at any time, providing up to 30 seconds notice
- C) They are ideal for production workloads requiring high availability
- D) They cost the same as standard instances but with better performance

**✓ Answer: B**

---

### Q3: Multiple Identical VM Instances
You want to automate the creation of multiple identical VM instances with a specific configuration. What is the best approach?

- A) Manually create each instance using the Cloud Console
- B) Create an instance template and use it with Managed Instance Groups
- C) Use custom startup scripts for each instance
- D) Create a Deployment Manager template

**✓ Answer: B**

---

### Q4: Startup Scripts Persistence
Your VM instance needs to execute tasks when it starts. Which of these methods would persist across instance restarts?

- A) Startup scripts specified in the instance metadata
- B) gcloud commands run after instance creation
- C) SSH commands executed manually
- D) Cron jobs in the instance's crontab

**✓ Answer: A**

---

### Q5: Moving VM Instance Across Regions
You need to move a running VM instance from us-central1 to europe-west1. What is the recommended approach?

- A) Use the gcloud compute instances move command
- B) Create an image from the instance, then create a new instance in the target zone
- C) Use Storage Transfer Service to migrate the disk
- D) Stop the instance and use Migrate for Compute Engine

**✓ Answer: D**

---

### Q6: Troubleshooting Startup Script
An instance is failing to start, and you need to troubleshoot the startup script. Where would you check the output?

- A) Cloud Shell history
- B) Instance serial port output in the Cloud Console
- C) Cloud Storage logs bucket
- D) Pub/Sub message queue

**✓ Answer: B**

---

### Q7: Resizing Instance vCPUs
You have an instance with 4 vCPUs and need to increase it to 8 vCPUs. The instance must remain in the same zone. What must you do?

- A) The instance can be resized without stopping
- B) Stop the instance, resize it, and restart it
- C) Create a new instance with the larger size
- D) Resize the boot disk to increase compute resources

**✓ Answer: B**

---

### Q8: Static External IP Address
Your application requires that the VM instance always has a static external IP address. What should you do?

- A) External IPs are always static by default
- B) Configure a dynamic external IP in the instance metadata
- C) Reserve a static external IP address and associate it with the instance
- D) Use the instance's internal IP for external connections

**✓ Answer: C**

---

## SECTION 2: MACHINE TYPES & SIZING

### Q9: Web Server Machine Type
You need to choose a machine type for a web server that will handle moderate traffic. Which machine type family is most appropriate?

- A) Memory-optimized (M-series) instances
- B) Compute-optimized (C-series) instances
- C) General-purpose (N-series) instances
- D) GPU-attached instances

**✓ Answer: C**

---

### Q10: High CPU, Low Memory Workload
Your workload involves real-time data processing with high CPU requirements but low memory needs. What machine type should you select?

- A) n1-standard (general purpose)
- B) c2-standard (compute-optimized)
- C) m1-megamem (memory-optimized)
- D) e2-micro (shared-core)

**✓ Answer: B**

---

### Q11: Cost-Efficient Batch Processing
You want to maximize cost efficiency for a batch processing job that can tolerate some latency. Which machine type family offers the best price-to-performance ratio?

- A) High-memory machine types
- B) E2 (cost-optimized, shared-core) machine types
- C) Compute-optimized machine types
- D) Custom machine types only

**✓ Answer: B**

---

### Q12: Custom Machine Type Configuration
Your application requires exactly 5 vCPUs and 24 GB of memory, but no standard machine type matches this configuration. What option is available?

- A) Use a smaller machine type and scale horizontally
- B) Create a custom machine type with the exact specifications
- C) Request Google Cloud to create a new standard machine type
- D) Upgrade to the next larger standard machine type

**✓ Answer: B**

---

### Q13: Right-Sizing Recommendations
You want to analyze the right-sizing recommendations for your instances. Which GCP service provides this?

- A) Recommender API
- B) Cloud Monitoring
- C) Cloud Advisor
- D) Cost Management API

**✓ Answer: A**

---

### Q14: Cost Savings for ML Training
For a machine learning training job running overnight, which commitment-based pricing model provides the best savings?

- A) Preemptible pricing
- B) On-demand pricing
- C) 1-year Commitment
- D) 3-year Commitment

**✓ Answer: D**

---

## SECTION 3: IMAGES, SNAPSHOTS & DISKS

### Q15: Creating Custom Images
You need to create a custom image containing your application with all dependencies pre-installed. What is the recommended approach?

- A) Create an instance, install software, create an image from the instance
- B) Use Compute Engine's image builder tool
- C) Upload a pre-built image from your on-premises environment
- D) Use Cloud Build to create container images only

**✓ Answer: A**

---

### Q16: Restoring Previous Disk State
You want to restore a disk to a previous state from 3 days ago. What should you use?

- A) Disk snapshots
- B) Disk images
- C) Backup plans
- D) Version history from Cloud Storage

**✓ Answer: A**

---

### Q17: Snapshot vs Image Difference
What is the key difference between a snapshot and an image?

- A) Snapshots are faster to create than images
- B) Images contain an OS and can be used to create instances; snapshots are point-in-time backups of disks
- C) Snapshots can only be used within the same zone
- D) Images cannot be shared across projects

**✓ Answer: B**

---

### Q18: Snapshot Storage Costs
You have a 100 GB persistent disk and want to take a snapshot. What storage costs apply?

- A) The full 100 GB is charged immediately
- B) Only changed blocks are stored, subsequent snapshots use incremental storage
- C) Snapshots are free; only restore costs apply
- D) The cost is a flat fee regardless of size

**✓ Answer: B**

---

### Q19: Multiple Persistent Disks
You need to attach multiple persistent disks to a single VM instance. What is the maximum number of disks you can attach?

- A) 1 (boot disk only)
- B) 4
- C) 16
- D) Unlimited

**✓ Answer: C**

---

### Q20: Resizing Persistent Disk
A customer asks if they can resize a persistent disk while the instance is running. What's the correct answer?

- A) No, the instance must be stopped first
- B) Yes, you can resize the disk, but you must then expand the file system from within the OS
- C) No, you must create a new disk with larger size
- D) Resizing is not supported in Compute Engine

**✓ Answer: B**

---

### Q21: Copying Snapshot Across Regions
You want to copy a snapshot from us-central1 to europe-west1 for disaster recovery. What's the best approach?

- A) Snapshots are automatically replicated globally
- B) Create an image from the snapshot, store it in Cloud Storage, then create a new image in the target region
- C) Use gcloud compute snapshots copy command
- D) Download the snapshot file and upload it to the target region

**✓ Answer: B**

---

### Q22: Sharing Images Across Projects
Your organization needs to create instances from the same image in multiple projects. How should you share the image?

- A) Images can only be used within the same project
- B) Use IAM to grant compute.images.get permission on the image to other projects
- C) Export the image to Cloud Storage and download it in each project
- D) Create duplicate images in each project

**✓ Answer: B**

---

## SECTION 4: NETWORKING & FIREWALL

### Q23: Same VPC Communication
You create two instances in the same VPC network but different subnets. Can they communicate without firewall rules?

- A) Yes, same VPC allows automatic communication
- B) No, firewall rules must be created to allow traffic
- C) Only if they're in the same region
- D) Yes, but only for internal traffic not external

**✓ Answer: B**

---

### Q24: Restricting SSH Access
You need to restrict SSH access to instances from only your office IP range (203.0.113.0/24). What should you do?

- A) Create a firewall rule with priority 1000, source IP range 203.0.113.0/24, protocol TCP port 22
- B) Use OS-level iptables on each instance
- C) Configure the instances to only accept connections from that IP
- D) This is not possible in Compute Engine

**✓ Answer: A**

---

### Q25: External IP Inaccessibility
An instance needs both internal and external IP addresses, but you want the external IP to be inaccessible from the internet. What should you configure?

- A) Remove the external IP
- B) Assign an internal IP only
- C) Reserve a static external IP, assign it, then use a firewall rule to deny all ingress traffic
- D) Use Cloud NAT instead

**✓ Answer: C**

---

### Q26: Private Google APIs Access
You need to allow instances to communicate with Google Cloud APIs without using external IPs. What solution should you use?

- A) Create firewall rules to allow API traffic
- B) Use service accounts with appropriate IAM roles
- C) Enable VPC Service Controls or use Private IP for Google APIs
- D) Configure external load balancers

**✓ Answer: C**

---

### Q27: Multiple Port Firewall Rules
Your application needs to serve traffic on HTTP (port 80) and HTTPS (port 443). What's the most efficient way to configure firewall rules?

- A) Create two separate firewall rules
- B) Create one firewall rule allowing both ports
- C) Create one rule for port 80 and rely on implicit HTTPS
- D) Firewall rules cannot handle multiple ports

**✓ Answer: B**

---

### Q28: Firewall Logging
You want to log all traffic that is dropped by firewall rules. What option should you enable on the firewall rule?

- A) Enable monitoring on the firewall rule
- B) Enable logging on the firewall rule
- C) Use Cloud Audit Logs only
- D) This is not available for firewall rules

**✓ Answer: B**

---

### Q29: Firewall Rule Priority
A firewall rule has priority 1000 to deny traffic, and another rule has priority 500 to allow the same traffic. Which rule takes precedence?

- A) Priority 1000 (higher number = higher priority)
- B) Priority 500 (lower number = higher priority)
- C) Both rules apply equally
- D) The deny rule always takes precedence regardless of priority

**✓ Answer: B**

---

## SECTION 5: INSTANCE GROUPS & AUTOSCALING

### Q30: Managed Instance Group Purpose
What is the primary purpose of a Managed Instance Group (MIG)?

- A) Manual management of instances
- B) Automatically maintain availability and scale instances based on policies
- C) Store instance backup data
- D) Monitor individual instance performance metrics

**✓ Answer: B**

---

### Q31: Zonal MIG High Availability
You have a zonal Managed Instance Group and want to ensure high availability. What should you do?

- A) Convert it to regional Managed Instance Group
- B) Manually create instances in multiple zones
- C) Enable cross-zone autoscaling
- D) Nothing, MIGs are inherently HA

**✓ Answer: A**

---

### Q32: Autoscaling Configuration
You want to configure autoscaling to add instances when CPU utilization exceeds 70%. How do you achieve this?

- A) Set CPU threshold in instance metadata
- B) Create an autoscaling policy on the Managed Instance Group with CPU utilization metric
- C) Use Cloud Monitoring alerts
- D) Configure the load balancer settings

**✓ Answer: B**

---

### Q33: Autoscaling Cooldown Period
Your autoscaling policy has a cooldown period of 5 minutes. What does this mean?

- A) Instances cool down for 5 minutes before becoming available
- B) After scaling action, no new scaling action occurs for 5 minutes
- C) Instances are deleted after 5 minutes of inactivity
- D) Metrics are sampled every 5 minutes

**✓ Answer: B**

---

### Q34: Rolling Updates
You need to deploy a rolling update to 50 instances managed by MIG with zero downtime. What feature should you use?

- A) Manual instance updates
- B) Managed rolling updates through the instance template
- C) SSH into each instance individually
- D) Delete and recreate the entire MIG

**✓ Answer: B**

---

### Q35: Managed vs Unmanaged Instance Groups
What is the difference between a Managed Instance Group and an Unmanaged Instance Group?

- A) Managed groups provide autoscaling and automated updates; unmanaged groups are manual collections
- B) Unmanaged groups are more reliable
- C) Managed groups can only use preemptible instances
- D) There is no functional difference

**✓ Answer: A**

---

### Q36: Updating Instance Template
You've deployed a new instance template, and you want to automatically update existing instances in your MIG to use it. What's the correct approach?

- A) Manually delete and recreate each instance
- B) Update the template and use 'Update Instances' with rolling update strategy
- C) Instances automatically update when template changes
- D) You must create a new MIG with the new template

**✓ Answer: B**

---

## SECTION 6: LOAD BALANCING

### Q37: Multi-Region Traffic Distribution
Your application has instances in us-central1 and europe-west1. You need to distribute traffic across both regions. Which load balancer type is appropriate?

- A) Internal TCP/UDP load balancer
- B) External HTTP(S) load balancer (global)
- C) Network TCP/UDP load balancer
- D) Only Cloud Endpoints

**✓ Answer: B**

---

### Q38: URL Path Routing
You need to route traffic based on URL paths (/api/* to backend A, /static/* to backend B). Which load balancer feature supports this?

- A) Network load balancer
- B) HTTP(S) load balancer with URL maps
- C) Internal load balancer
- D) TCP load balancer

**✓ Answer: B**

---

### Q39: Connection Draining
Your backend instances are experiencing high latency. You've enabled connection draining on the load balancer. What does this accomplish?

- A) Closes all connections immediately
- B) Allows existing connections to complete before removing the instance from the pool
- C) Reduces latency automatically
- D) Monitors latency metrics

**✓ Answer: B**

---

### Q40: Health Checks
You want to ensure the load balancer only sends traffic to healthy instances. What should you configure?

- A) Firewall rules only
- B) Health checks on the backend service
- C) Instance monitoring only
- D) Nothing, this is automatic

**✓ Answer: B**

---

### Q41: Non-HTTP Traffic Load Balancing
You need load balancing for non-HTTP traffic (e.g., custom protocol on port 5000). What load balancer type should you use?

- A) HTTP(S) load balancer
- B) Network load balancer (TCP/UDP)
- C) Internal load balancer only
- D) This is not supported

**✓ Answer: B**

---

### Q42: SSL Certificate Expiration
An SSL certificate for your HTTPS load balancer is about to expire. What will happen to traffic?

- A) Traffic continues uninterrupted; expiration doesn't affect current connections
- B) HTTPS traffic is dropped; clients must wait for certificate renewal
- C) The load balancer automatically generates a new certificate
- D) Traffic is redirected to HTTP

**✓ Answer: B**

---

## SECTION 7: SECURITY & IAM

### Q43: Instance Management Permission
You need to grant a developer permission to create and manage instances in a specific project. What is the most appropriate IAM role?

- A) roles/owner
- B) roles/compute.admin
- C) roles/compute.editor
- D) roles/compute.osLogin

**✓ Answer: B**

---

### Q44: Service Account Authentication
A service account needs to authenticate to Google Cloud APIs from a Compute Engine instance. What is the recommended approach?

- A) Store API keys in instance metadata
- B) Attach the service account to the instance and use Application Default Credentials
- C) Embed credentials in application code
- D) Use SSH keys for API authentication

**✓ Answer: B**

---

### Q45: Service Account Restriction
You want to restrict which service accounts can be used by instances in a project. What organization policy constraint should you use?

- A) compute.restrictServiceAccountUse
- B) iam.allowedPolicyMemberDomains
- C) compute.skipDefaultServiceAccountCreation
- D) iam.workforcePools

**✓ Answer: A**

---

### Q46: Data Encryption at Rest
Your organization requires that all data must be encrypted. Where do you encrypt data at rest for persistent disks?

- A) In the instance application layer only
- B) Using Google-managed keys or customer-managed CMEK by default
- C) On external storage only
- D) Data at rest is not encrypted in Compute Engine

**✓ Answer: B**

---

### Q47: SSH Password Authentication
You need to ensure instances cannot be accessed via SSH using passwords. What should you configure?

- A) Disable SSH in firewall rules
- B) Use OS Login and remove password authentication from SSH configuration
- C) Instances are secure by default
- D) Create instances without network interfaces

**✓ Answer: B**

---

### Q48: Administrative Actions Audit
You want to audit all administrative actions performed on Compute Engine resources. Where should you look?

- A) Cloud Monitoring metrics only
- B) Cloud Audit Logs (Admin Activity)
- C) Instance system logs
- D) The operation is not audited

**✓ Answer: B**

---

## SECTION 8: MONITORING, LOGGING & COST

### Q49: Monitoring Multiple Instances
You need to monitor CPU, memory, and disk utilization across 100 instances. What is the most efficient approach?

- A) SSH into each instance and check manually
- B) Install the Monitoring Agent and use Cloud Monitoring dashboards
- C) Use OS-level monitoring tools only
- D) Create custom scripts for each instance

**✓ Answer: B**

---

### Q50: Cost Optimization
Your GCP bill for Compute Engine is higher than expected. Where should you check for cost optimization opportunities?

- A) Cloud Console Billing section and Recommender API
- B) Cloud Monitoring only
- C) Instance labels
- D) Audit logs

**✓ Answer: A**

---

## ANSWER KEY

| | | | | |
|---|---|---|---|---|
| 1. B | 2. B | 3. B | 4. A | 5. D |
| 6. B | 7. B | 8. C | 9. C | 10. B |
| 11. B | 12. B | 13. A | 14. D | 15. A |
| 16. A | 17. B | 18. B | 19. C | 20. B |
| 21. B | 22. B | 23. B | 24. A | 25. C |
| 26. C | 27. B | 28. B | 29. B | 30. B |
| 31. A | 32. B | 33. B | 34. B | 35. A |
| 36. B | 37. B | 38. B | 39. B | 40. B |
| 41. B | 42. B | 43. B | 44. B | 45. A |
| 46. B | 47. B | 48. B | 49. B | 50. A |

---

## STUDY TIPS & EXAM PREPARATION

### 1. Focus Areas
- Managed Instance Groups and autoscaling (high-weight topic)
- Load balancing configurations and routing
- Networking and firewall rules
- VM sizing and cost optimization
- Security and IAM concepts

### 2. Exam Strategy
- Expect scenario-based questions that require understanding interactions between services
- Pay attention to keywords: 'must', 'should', 'recommended', 'best'
- Questions test real-world decision-making, not just theoretical knowledge
- Read all options before selecting—some answers are partially correct

### 3. Hands-on Practice
- Create instances with different machine types
- Practice creating and managing MIGs
- Test load balancer configurations
- Work with snapshots and custom images
- Set up monitoring and logging
- Experiment with different networking configurations

### 4. Key Documentation to Review
- Compute Engine best practices
- Pricing and cost optimization
- Network architecture patterns
- Security and IAM roles reference
- Autoscaling and instance group scaling options
- Load balancing routing strategies

---

**Good luck with your exam! 🚀**
