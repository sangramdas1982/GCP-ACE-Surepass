# Google Compute Engine: Complete 1-Hour Study Guide
## For Google Cloud Associate Engineer Certification

---

## Introduction

Welcome to this comprehensive guide on Google Compute Engine, the core compute service in Google Cloud Platform. Whether you're preparing for your Google Cloud Associate Engineer certification or building real-world infrastructure on Google Cloud, this guide will take you through everything you need to master Compute Engine.

Think of Compute Engine as Google's answer to Amazon EC2 or Microsoft Azure Virtual Machines. It provides virtual machines, networking, storage, and all the tools you need to build scalable, reliable compute infrastructure. But more importantly, understanding Compute Engine teaches you how cloud compute works fundamentally—concepts that apply whether you're using Google Cloud, AWS, Azure, or any other cloud provider.

This guide goes beyond just creating instances. We'll explore the architectural decisions behind Compute Engine, how to design for reliability and efficiency, how to secure your compute infrastructure, and how to troubleshoot when things go wrong. This is the knowledge that separates someone who can click buttons from someone who can architect and manage production infrastructure.

---

## Part 1: Compute Engine Fundamentals

### What is Compute Engine?

Google Compute Engine is an Infrastructure-as-a-Service (IaaS) offering from Google Cloud. It provides virtual machines that you can configure and manage.

Here's what that means in practical terms: You don't need to buy physical servers, rack them, install operating systems, or maintain hardware. Instead, you define how many vCPUs you want, how much memory, what operating system, what disks, and Google provides you with a running virtual machine. You pay only for what you use, by the second (with a one-minute minimum commitment).

But here's something important: Compute Engine is not managed. Unlike Google App Engine or Google Cloud Run, which are fully managed platforms where you deploy code and Google handles everything, Compute Engine gives you a virtual machine and you're responsible for everything on that machine: the operating system, patches, security, software, configuration. This is more work but gives you complete flexibility.

### Why Use Compute Engine?

There are several reasons organizations choose Compute Engine:

**Flexibility**: You can run almost anything. Custom applications, legacy software, databases, whatever you want to run on Linux or Windows, you can run on Compute Engine.

**Control**: You have complete control over the machine, networking, and storage. You can configure things however you need.

**Cost efficiency**: You only pay for what you use. You can create machines in seconds and destroy them just as quickly. You can use sustained-use discounts for long-running workloads.

**Integration with Google Cloud**: Compute Engine integrates deeply with other Google Cloud services like Cloud Storage, Cloud SQL, Cloud Pub/Sub, etc.

**Global infrastructure**: Google has data centers worldwide, and you can run instances in any region or zone.

### Key Concepts: Regions and Zones

Google Cloud infrastructure is organized geographically, and you need to understand this structure.

A **region** is a geographic area. North America has regions like `us-central1`, `us-east1`, `us-west1`. Europe has `europe-west1`, `europe-north1`. Each region has low latency internally but higher latency to other regions.

Within each region are **zones**. A zone is an independent physical location within a region. For example, `us-central1` has zones `us-central1-a`, `us-central1-b`, `us-central1-c`, `us-central1-d`.

Zones are isolated, meaning if there's a power outage in zone `a`, it doesn't affect zone `b`. This is why you should distribute your applications across zones for high availability.

When you create a Compute Engine instance, you specify which zone to create it in. The zone determines the hardware, the latency, and the availability.

For the exam and for production use, always think about geographic distribution. If you want high availability, distribute instances across zones. If you need disaster recovery, consider multiple regions.

### Instance Basics

A Compute Engine **instance** is a virtual machine. When you create an instance, you specify:

- **Machine type**: How much CPU and memory
- **Image**: The operating system and software to start with
- **Zone**: Where to create it
- **Disk**: Storage configuration
- **Network**: Which VPC network to connect to
- **Service account**: What identity to run as
- **Startup script**: Commands to run when it starts
- And many other options

Once created, the instance has a public IP address (if you choose), a private IP address, a hostname, and various other attributes that you can reference and manage.

---

## Part 2: Machine Types and Computing Resources

### Understanding Machine Types

A **machine type** is a predefined combination of vCPUs and memory. Google offers several families:

**General Purpose** machines (`n1`, `n2`, `n2d`, `e2`) are balanced between CPU and memory. They're suitable for most applications. The `e2` family is the most cost-effective, while `n2` and `n2d` offer better performance. Most applications use general-purpose machines.

**Memory Optimized** machines (`m1`, `m2`) have more memory relative to CPU. They're for applications that need lots of memory, like in-memory databases or data processing.

**Compute Optimized** machines (`c2`, `c2d`) have more CPU relative to memory. They're for computationally intensive tasks like scientific computing or video transcoding.

**High Memory** machines are specialized for extremely memory-intensive workloads.

For each family, you choose a specific size like `n2-standard-4` (4 vCPUs, 16GB memory) or `n2-standard-8` (8 vCPUs, 32GB memory).

Understanding these families is important for the exam. You'll see scenarios like "An application needs lots of memory for data processing" and you need to know to recommend a memory-optimized machine.

### Custom Machine Types

If predefined machine types don't fit your needs, you can create custom machine types with specific vCPU and memory combinations. Google charges based on the actual resources, which is sometimes more economical than predefined types.

### Resource Requests and Overcommitment

When you create an instance with 4 vCPUs, you get 4 vCPUs dedicated to your instance. Google doesn't overcommit—you get what you ask for. This is different from container orchestration where resources are shared.

However, on your instance, the operating system and applications still need to manage resources. If you ask for 4 vCPUs, your application gets access to 4 vCPUs, but the OS scheduler determines which applications get CPU time at any moment.

### CPU Platforms

Google uses different processors from Intel, AMD, and others. When you create an instance, it's placed on a specific processor. For most applications, this doesn't matter. But some applications have licensing or performance characteristics tied to specific processors.

You can specify CPU platform preferences when creating an instance, but Google will use the best available. This is rarely a concern for the exam.

---

## Part 3: Images and Operating Systems

### What is an Image?

An **image** is a disk snapshot that contains an operating system and potentially software. When you create an instance, you specify an image, and that image is used to initialize the instance's boot disk.

Google provides public images:

**Debian and Ubuntu Linux**: These are the most common. Ubuntu is familiar to many developers. Debian is more minimal.

**RHEL and CentOS**: For organizations that prefer Red Hat-based distributions.

**Windows Server**: For applications that require Windows.

**Others**: Google provides images for various other operating systems.

The choice of image depends on your needs and what software you want to run. Ubuntu is a safe default for Linux. For licensing reasons, RHEL is more expensive than CentOS, which is more expensive than Debian or Ubuntu.

### Custom Images

You can create custom images. This is useful if you have software or configuration that you want to bake into an image and reuse across multiple instances.

The process: Create an instance, install software, configure it the way you want, then create an image from that instance's disk. Now you have a reusable image that you can use to create new instances.

Custom images save startup time because software is already installed, but they're more complex to maintain. Every time software needs updating, you need to create a new image.

### Image Families

Google groups images into families. For example, `debian-11` is an image family for Debian 11. When you specify an image, you can specify a specific version or the latest in a family. This allows your Terraform code to always use the latest Debian 11 without hardcoding specific versions.

---

## Part 4: Storage and Disks

### Persistent Disks

Every Compute Engine instance needs a **boot disk**, which contains the operating system. By default, this is a **Persistent Disk**, which is Google's block storage service.

Persistent Disks have several important characteristics:

**Durability**: Data is replicated across multiple locations within a zone. If a disk physically fails, your data is safe.

**Snapshots**: You can take snapshots of persistent disks, which are stored in Cloud Storage. This is useful for backups and creating new disks from snapshots.

**Resizing**: You can resize persistent disks without stopping the instance, though the OS still needs to expand the filesystem to use the new space.

**Attachment**: You can attach multiple persistent disks to a single instance, giving you flexible storage configurations.

You can also detach a disk from one instance and attach it to another, which is useful for moving data or recovering from instance failures.

### Disk Types

Google offers several disk types:

**Standard Persistent Disks** are cost-effective but have lower IOPS (input/output operations per second). They're suitable for most applications that aren't I/O intensive.

**Balanced Persistent Disks** offer a middle ground between cost and performance. They're suitable for most applications.

**SSD Persistent Disks** are fast but more expensive. They're for applications that need high I/O performance, like databases or data warehouses.

**Local SSDs** are extremely fast NVMe SSDs that are physically attached to the machine. They have the highest performance but aren't persistent—they're destroyed when the instance stops. They're used for temporary data, caches, or when you need maximum I/O performance.

For the exam, understand the trade-offs: Standard is cheap but slow, SSD is expensive but fast, local SSDs are fastest but temporary.

### Disk Size and Pricing

Persistent disks are priced by the amount of storage, regardless of how much you actually use. A 100GB disk costs the same whether you use 10GB or 100GB. Additional storage is cheap.

This means you should provision adequate disk space upfront without worrying too much about cost. Running out of disk space is worse than paying for extra space you don't use.

### Regional Persistent Disks

By default, persistent disks are zonal—they exist in a single zone. If that zone has an outage, you lose access.

**Regional Persistent Disks** are replicated across two zones in a region. They provide higher availability but cost more and have slightly lower performance.

For critical systems, regional persistent disks are recommended.

---

## Part 5: Networking with Compute Engine

### VPC Networks and Subnets

Compute Engine instances connect to **VPC networks**, which are isolated virtual networks within Google Cloud. Your instances can communicate with each other and with the internet through networks.

When you create an instance, you specify which VPC network to connect to. By default, there's a `default` network, but you should create custom networks for your applications.

Within a network, you create **subnets**, which are IP address ranges in specific regions. An instance's private IP comes from a subnet.

For example, you might have:
- Network: `production`
- Subnet in us-central1: 10.0.0.0/24
- Subnet in us-east1: 10.1.0.0/24

Instances in the same network can communicate directly by private IP, regardless of zone or region.

### Private and Public IP Addresses

Every instance has a **private IP address** from its subnet's IP range. This is used for internal communication.

Optionally, an instance can have a **public (external) IP address**. This is used to communicate with the internet. Without a public IP, an instance can't reach the internet (though it can reach Google Cloud services through private routes).

You can reserve external IP addresses so they don't change when you stop/start an instance. Ephemeral IPs change.

### Firewall Rules

**Firewall rules** control inbound and outbound traffic to instances. By default, all outbound traffic is allowed and all inbound traffic is denied. You need to explicitly allow inbound traffic.

Firewall rules specify:
- Direction (ingress or egress)
- Source/destination IP ranges
- Protocol (TCP, UDP, etc.)
- Ports
- Target (which instances the rule applies to)

For example, you might create a rule that allows TCP traffic on port 80 and 443 from anywhere:

```
Allow TCP 80, 443 from 0.0.0.0/0 to instances tagged "web-server"
```

Rules use **tags** to target instances. You can tag instances with labels like `web-server`, `database`, etc., and then write rules that apply to all instances with those tags.

### Routes and Traffic

Routes determine how packets are sent from your instance. By default, Google creates routes for:
- Local traffic (within the VPC)
- A default route to the internet (for instances with public IPs)

You can create custom routes if you need more complex networking, like directing traffic through a NAT gateway or a VPN.

### NAT Gateway for Private Instances

If you have instances that don't need public IPs but need to reach the internet (for software updates, reaching APIs), you can use a **Cloud NAT gateway**. This allows private instances to initiate outbound connections.

Cloud NAT is a managed service, so you don't need to manage a NAT instance yourself.

---

## Part 6: Security and Identity

### Service Accounts

Every Compute Engine instance runs under a **service account**, which is a special Google Cloud identity. Service accounts have IAM roles that determine what the instance can do in Google Cloud.

For example, you might have an instance that needs to read from Cloud Storage. You would:
1. Create a service account
2. Grant it the `Storage Object Viewer` role
3. Configure the instance to use that service account

The instance can then access Cloud Storage without credentials being stored on the instance.

### Scopes and Permissions

When you create an instance, you specify which **scopes** it has. Scopes are coarse-grained permissions that Google Cloud services respect.

For example, the `cloud-platform` scope gives the instance access to all Google Cloud services. The `storage-ro` scope gives read-only access to Cloud Storage.

Scopes are a legacy mechanism. The modern approach is to use **Workload Identity** (if you're running on GKE) or **service accounts with specific IAM roles**. But scopes are still relevant for Compute Engine, especially for the exam.

### IAM Roles for Service Accounts

Service accounts have IAM role bindings that determine their permissions. You might grant a service account the `Compute Instance Admin` role, which allows it to create and manage instances.

For Compute Engine, understanding service accounts and IAM is critical for security. An instance should have the minimum permissions needed to do its job (principle of least privilege).

### Metadata and Application Credentials

The instance can query its metadata to find out what service account it's running under and get access tokens for that account. This allows applications running on the instance to authenticate to Google Cloud services without managing credentials.

Applications can use the Google Cloud client libraries, which automatically use the instance's service account credentials.

---

## Part 7: Instance Templates and Instance Groups

### Instance Templates

An **instance template** is a reusable blueprint for creating instances. It specifies machine type, image, disks, networking, service account, startup script, and other configuration.

When you create instances from a template, they all have the same configuration. This ensures consistency and makes it easy to scale.

Templates are immutable—you can't modify an existing template. If you need to change configuration, you create a new template and create instances from the new template.

For the exam, understand that instance templates are the foundation of scalable infrastructure.

### Instance Groups

An **instance group** is a collection of instances managed as a single unit. There are two types:

**Managed Instance Groups** use an instance template to create instances automatically. You specify how many instances you want, and the group creates and manages them. If an instance fails, the group automatically creates a replacement.

Managed instance groups are used with auto-scalers to automatically increase or decrease the number of instances based on load.

**Unmanaged Instance Groups** are just collections of existing instances. Google doesn't manage them, but they can be used with load balancers.

### Auto-Scaling

A **managed instance group** can have an **autoscaler** that automatically adjusts the number of instances based on metrics like CPU utilization or request rate.

You specify:
- Minimum number of instances
- Maximum number of instances
- Metric and target value (e.g., "keep CPU at 70%")

If actual CPU exceeds 70%, the autoscaler creates more instances. If it drops below 70%, the autoscaler removes instances (respecting the minimum).

Autoscaling is crucial for handling variable load and cost-efficiently using resources.

---

## Part 8: Startup and Shutdown Scripts

### Startup Scripts

A **startup script** is a script that runs when an instance starts. It's specified in the instance configuration.

For Linux, startup scripts are shell scripts. For Windows, they're PowerShell scripts.

Common uses:
- Installing software
- Configuring the system
- Downloading application code
- Starting services

For example:
```bash
#!/bin/bash
apt-get update
apt-get install -y nginx
systemctl start nginx
```

Startup scripts are crucial for automating instance initialization. Without them, you'd have to SSH into each instance and manually install software.

### Shutdown Scripts

Similarly, a **shutdown script** runs when an instance shuts down. It's useful for cleanup tasks.

### Startup and Shutdown via Metadata

You can provide startup and shutdown scripts through:
- The GCP Console
- Command line tools
- Terraform or other IaC tools
- Instance metadata

The advantage of metadata is that you can update scripts without recreating instances, though you need to restart instances for the new scripts to run.

---

## Part 9: Monitoring, Logging, and Troubleshooting

### Serial Port and VM Inspector

When an instance won't boot or is behaving strangely, the **serial port console** is invaluable. It shows the raw console output, including kernel boot messages.

You can access it from the GCP Console: select the instance, then "Serial port 1 (COM1)".

**VM Inspector** provides a detailed view of instance state, metrics, and recent operations.

### Google Cloud Logging

Instance logs are automatically sent to **Google Cloud Logging**. The agent on each instance forwards:
- Syslog messages
- Application logs written to standard locations
- Custom logs you configure

You can query these logs from the GCP Console or using gcloud commands.

### Google Cloud Monitoring

**Cloud Monitoring** collects metrics from instances:
- CPU utilization
- Memory usage
- Disk I/O
- Network throughput
- And many others

You can create dashboards and set up alerts based on metrics.

### SSH Access and Troubleshooting

You can SSH into instances from the GCP Console or the command line. The Console provides a browser-based terminal.

Common troubleshooting steps:
1. Check the serial port console for boot messages
2. SSH in and check logs
3. Check metrics for resource issues
4. Check firewall rules to ensure traffic is allowed

### Cloud Logging Agent

By default, instances run the **Cloud Logging agent**, which automatically collects logs and sends them to Cloud Logging. You can configure it to collect additional logs.

If you're using custom logging, you need to ensure the agent is running and configured correctly.

---

## Part 10: Instance Lifecycle and Lifecycle Policies

### Instance States

An instance can be in several states:

**PROVISIONING**: Being created.

**STAGING**: Preparing to start.

**RUNNING**: The instance is on and ready to use.

**STOPPING**: Being stopped.

**TERMINATED**: Stopped.

**SUSPENDING/SUSPENDED**: Suspended (only for instances that support it).

**REPAIRING**: Google Cloud is trying to fix issues.

You pay for running instances (and storage for terminated instances). You don't pay for stopped instances (except for resources like IP addresses and disks).

### Stop vs Delete

When you **stop** an instance, it's turned off but still exists. The boot disk is preserved, IP addresses are released (unless they're static), and you don't pay for compute. You can restart a stopped instance.

When you **delete** an instance, it's destroyed. The boot disk is deleted unless you explicitly preserve it. You can't restart a deleted instance.

For temporary work or testing, stopping is better than deleting because you can restart. For long-lived infrastructure, deleting unused resources is important for cleanup.

### Disks and Deletion

When you delete an instance, the boot disk is deleted by default. But you can configure the instance to keep the disk on deletion. This is useful if you want to preserve the disk for later analysis or reuse.

Data disks (non-boot disks) are never automatically deleted. You need to explicitly delete them.

---

## Part 11: Advanced Instance Configuration

### Preemptible Instances

**Preemptible instances** are up to 80% cheaper than regular instances, but Google can terminate them with 30 seconds notice if Google needs the capacity for regular instances.

They're suitable for:
- Batch jobs that can tolerate interruptions
- Testing and development
- Cost-sensitive workloads

They're not suitable for long-running services that need high availability.

### Sole-Tenant Nodes

For compliance or licensing reasons, you might need instances that run on hardware not shared with other customers.

**Sole-tenant nodes** are physical servers that run only your instances. You pay a premium, but you have exclusive hardware access.

### GPU and TPU

Compute Engine supports **GPUs** (graphics processing units) and **TPUs** (tensor processing units) for machine learning and compute-intensive workloads.

You can attach GPUs or TPUs to instances for significantly higher performance for specific workloads.

### Custom Networks and Advanced Networking

Beyond basic VPC setup, you can:
- Create VPC peering to connect separate networks
- Use Cloud Interconnect for dedicated connections between your data center and Google Cloud
- Set up VPNs for secure connections
- Use Cloud CDN for content delivery

These are more advanced but important for production deployments.

---

## Part 12: Snapshots and Backup Strategies

### Creating and Restoring Snapshots

A **snapshot** is a point-in-time copy of a persistent disk stored in Cloud Storage. You can create snapshots of any persistent disk.

Snapshots are useful for:
- Backups
- Creating new disks
- Recovery from data corruption

You can restore from a snapshot by creating a new disk from the snapshot.

Snapshots are incremental—only changed data is stored in each new snapshot, so they're cost-efficient for frequent snapshots.

### Snapshot Schedules

You can set up **snapshot schedules** to automatically take snapshots on a schedule (daily, weekly, etc.).

This is important for disaster recovery. Regular snapshots ensure you can recover from data loss.

### Regional Snapshots

By default, snapshots are stored in a single region. You can copy snapshots to other regions for disaster recovery.

**Regional snapshots** are automatically replicated across regions, providing geographic redundancy.

---

## Part 13: Load Balancing and Compute Engine

### External Load Balancers

**External load balancers** distribute traffic from the internet across multiple instances.

Google Cloud offers several types:

**Network Load Balancer** operates at the transport layer (Layer 4). It's fast and suitable for non-HTTP protocols and extreme throughput.

**HTTP(S) Load Balancer** operates at the application layer (Layer 7). It understands HTTP and can route based on hostnames, URL paths, etc. It's suitable for web applications.

### Backend Services and Health Checks

A **backend service** is a configuration that specifies a set of instances and how traffic should be distributed. It includes:
- Which instance groups to distribute traffic to
- Health checks (to ensure instances are healthy)
- Session affinity (sticky sessions)
- Connection timeouts

**Health checks** periodically test instances to ensure they're healthy. If an instance fails the health check, the load balancer stops sending traffic to it.

### Using Instance Groups with Load Balancers

You create a load balancer, configure a backend service that references an instance group, and the load balancer distributes traffic.

If the instance group has autoscaling, it automatically scales based on traffic, and the load balancer discovers the new instances.

This is the architecture for a scalable, highly available web application.

---

## Part 14: Compute Engine Best Practices

### Designing for High Availability

For production services:
- Use managed instance groups with autoscaling
- Distribute across multiple zones
- Use load balancers
- Set up health checks
- Use Cloud Monitoring and alerting

This ensures your service remains available even if individual instances or zones fail.

### Cost Optimization

- Use preemptible instances where appropriate
- Use sustained-use discounts for long-running workloads
- Right-size instances (don't use oversized machines)
- Delete unused resources
- Use Cloud Storage instead of persistent disks for archival data

### Security Best Practices

- Use service accounts with minimal permissions
- Use firewall rules to restrict traffic
- Keep images patched and updated
- Use custom images to pre-configure security settings
- Monitor logs and metrics for anomalies
- Use VPC Service Controls to restrict data access

### Automation and Infrastructure-as-Code

- Always use Terraform (or similar) to define infrastructure
- Use instance templates for consistency
- Use startup scripts for initialization
- Avoid manual configuration
- Store Terraform state in remote backends

### Monitoring and Alerting

- Set up Cloud Monitoring dashboards
- Create alerts for critical metrics
- Monitor logs for errors
- Track costs and set up budget alerts
- Use VM Inventory to track all instances

---

## Part 15: Common Exam Scenarios

### Scenario 1: Scaling a Web Application

**Problem**: You need to handle variable traffic.

**Solution**:
1. Create an instance template with your web application
2. Create a managed instance group from the template
3. Set up an autoscaler to scale based on CPU or request rate
4. Create an HTTP(S) load balancer pointing to the instance group
5. Set up health checks to ensure instances are healthy

This automatically scales up during high traffic and scales down during low traffic.

### Scenario 2: Long-Running Batch Job

**Problem**: You have a job that takes 8 hours to complete.

**Solution**:
1. Use preemptible instances to reduce cost (if interruption is tolerable)
2. Or use regular instances with checkpointing so the job can resume if interrupted
3. Use startup scripts to download data and start the job
4. Use shutdown scripts to upload results before the instance stops

### Scenario 3: High Availability Database

**Problem**: You need a MySQL database that remains available even if a zone fails.

**Solution**:
1. Use Cloud SQL (managed) instead of self-hosted on Compute Engine
2. Or if self-hosted, use two instances with replication:
   - Primary in zone A
   - Replica in zone B
   - Set up health checks and automatic failover

### Scenario 4: Private Instances with Internet Access

**Problem**: Instances need to update software but shouldn't have public IPs.

**Solution**:
1. Don't assign public IPs to instances
2. Set up a Cloud NAT gateway
3. Route outbound traffic through Cloud NAT
4. Instances can now reach the internet without public IPs

### Scenario 5: Disaster Recovery

**Problem**: You need to recover from a complete zone failure.

**Solution**:
1. Use regional persistent disks
2. Distribute instances across zones
3. Take regular snapshots and store in multiple regions
4. Document your recovery procedures
5. Test recovery regularly

---

## Part 16: Troubleshooting Common Issues

### Instance Won't Start

**Check**:
1. Serial port console for boot messages
2. If custom image, ensure it's compatible with the machine type
3. If using startup script, check for syntax errors
4. Disk space (full disks can prevent booting)

### Instance Stops Unexpectedly

**Check**:
1. Instance metrics for resource exhaustion (CPU, memory)
2. Cloud Logging for errors
3. Health check configuration
4. Whether autoscaler is terminating the instance

### SSH Connection Refused

**Check**:
1. Firewall rules allowing SSH (port 22)
2. Instance has public IP (unless using Cloud IAP)
3. Service account has necessary permissions
4. Instance is in RUNNING state

### Network Connectivity Issues

**Check**:
1. Firewall rules allow the traffic
2. Network configuration (subnets, routing)
3. Cloud Logging for network errors
4. Whether instances are in the same network

### Performance Issues

**Check**:
1. Cloud Monitoring metrics (CPU, memory, disk I/O, network)
2. Whether instance is right-sized for workload
3. Whether disks are SSD or standard
4. Network bandwidth

---

## Part 17: Key Compute Engine Concepts Review

### Resource Hierarchy

Understanding how Compute Engine fits in Google Cloud:
- **Projects** contain resources
- **Regions** are geographic areas
- **Zones** are isolated locations within regions
- **Networks** connect instances
- **Instances** run in zones and subnets

### Networking Hierarchy

- **VPC Networks** span regions
- **Subnets** exist in single regions
- **Routes** determine how traffic flows
- **Firewall Rules** control traffic
- **Instances** connect to subnets

### Compute Resources

- **Machine Types** define CPU and memory
- **Images** define OS and software
- **Persistent Disks** provide durable storage
- **Local SSDs** provide temporary, fast storage
- **Snapshots** provide backups

### Management

- **Instance Templates** provide reusable blueprints
- **Managed Instance Groups** automatically manage instances
- **Autoscalers** adjust instance count based on load
- **Load Balancers** distribute traffic
- **Health Checks** monitor instance health

---

## Part 18: Exam-Focused Topics

### What You'll Definitely See

**Creating and managing instances**: How to create, start, stop, delete.

**Networking**: VPC setup, firewall rules, load balancing.

**Scaling**: Instance groups, autoscaling, load distribution.

**Images and disks**: Choosing images, disk types, snapshots.

**Security**: Service accounts, IAM, firewall rules.

**Troubleshooting**: Common issues and solutions.

### Key Differences to Understand

**Instance vs Image**: An image is a template, an instance is a running VM.

**Zone vs Region**: Instances exist in zones, subnets exist in regions.

**Public vs Private IP**: Public for internet, private for internal.

**Stop vs Delete**: Stop preserves the instance, delete destroys it.

**Regular vs Preemptible**: Preemptible is cheaper but can be terminated.

### Practice Scenarios

Before the exam, practice:

1. **Creating an instance** from scratch using the Console, gcloud, and Terraform
2. **Setting up networking** with custom networks and firewall rules
3. **Creating instance templates** and managed instance groups
4. **Configuring autoscaling** and observing automatic scaling
5. **Setting up load balancing** across instances
6. **Creating and restoring snapshots**
7. **Using startup scripts** to initialize instances
8. **Troubleshooting** various failure scenarios
9. **Managing service accounts** and IAM
10. **Using Cloud Monitoring** to track instance health

---

## Part 19: Advanced Patterns

### Blue-Green Deployments

To deploy a new version with zero downtime:
1. Create a new instance group with the new version
2. Add it to the load balancer's backend
3. Remove the old instance group from the backend
4. Delete the old instance group

Traffic automatically shifts to the new version.

### Canary Deployments

To gradually roll out a new version:
1. Create a new instance group with 10% of traffic
2. Monitor metrics to ensure it's healthy
3. Gradually increase traffic to the new version
4. Once 100% is on the new version, delete the old one

This allows you to catch issues before they affect all users.

### Multi-Region Deployment

For disaster recovery and global availability:
1. Create instance groups in multiple regions
2. Use a global load balancer (Cloud Load Balancer)
3. Route traffic based on geography or capacity
4. Monitor and failover if a region fails

---

## Part 20: Key gcloud and Console Operations

### Common gcloud Commands

```
gcloud compute instances create INSTANCE_NAME \
  --image-family=debian-11 \
  --image-project=debian-cloud \
  --zone=us-central1-a

gcloud compute instances list                    # List instances

gcloud compute instances describe INSTANCE_NAME  # Get details

gcloud compute instances start INSTANCE_NAME     # Start instance

gcloud compute instances stop INSTANCE_NAME      # Stop instance

gcloud compute instances delete INSTANCE_NAME    # Delete instance

gcloud compute instances reset INSTANCE_NAME     # Reboot instance

gcloud compute ssh INSTANCE_NAME                 # SSH into instance

gcloud compute disks list                        # List disks

gcloud compute disks snapshot DISK_NAME          # Create snapshot

gcloud compute instance-templates create TEMPLATE_NAME  # Create template

gcloud compute instance-groups managed create GROUP_NAME \
  --template=TEMPLATE_NAME \
  --size=3
```

### Console Operations

- Navigate to Compute Engine > Instances to manage instances
- Use the Create Instance button for new instances
- Use Serial port 1 for console access
- Use the SSH button for web-based terminal
- Check Metrics tab for monitoring

---

## Part 21: Cost Considerations

### Pricing Components

- **Compute**: Per second, varies by machine type and region
- **Storage**: Persistent disks and snapshots
- **Network**: Data egress (data ingress is free)
- **Public IP**: Static IPs cost extra

### Cost Optimization

- Use preemptible instances (70-90% cheaper)
- Use managed instance groups with autoscaling (pay only for what you need)
- Use the right machine type (don't over-provision)
- Use sustained-use discounts for long-running workloads
- Use committed use discounts for predictable workloads
- Delete unused resources
- Use Cloud Storage instead of persistent disks for archival

### Example Cost Calculation

A `n2-standard-4` instance (4 vCPU, 16 GB memory) in us-central1:
- Roughly $0.15 per hour
- $110 per month for always-on
- Preemptible version: $0.035 per hour
- With sustainted-use discount: ~$0.10 per hour

---

## Conclusion

Google Compute Engine is the foundation of Google Cloud Platform. Understanding it deeply—not just how to create instances, but how to design scalable, reliable, secure, and cost-efficient infrastructure—is essential for anyone working with Google Cloud.

The key to mastering Compute Engine:

1. **Understand the fundamentals**: Regions, zones, networks, instances.

2. **Know the components**: Machine types, images, disks, firewall rules.

3. **Practice hands-on**: Create instances, configure networking, set up load balancing.

4. **Focus on automation**: Use Terraform, instance templates, startup scripts.

5. **Design for production**: Use managed instance groups, autoscaling, load balancing, health checks.

6. **Understand security**: Service accounts, IAM, firewall rules, monitoring.

7. **Optimize costs**: Use preemptible instances, right-size machines, automate scaling.

8. **Monitor and troubleshoot**: Set up monitoring, understand common issues, know how to debug.

Compute Engine is more manual than fully managed services, but this gives you power and flexibility. As you continue your journey in cloud engineering and SRE, mastering Compute Engine prepares you for any compute platform—the principles apply everywhere.

Good luck with your certification!

---

## Quick Reference

### Machine Type Families

```
General Purpose: n1, n2, n2d, e2
Memory Optimized: m1, m2
Compute Optimized: c2, c2d
High Memory: m2-ultramem
```

### Disk Types

```
Standard Persistent Disk    - Cost-effective, lower IOPS
Balanced Persistent Disk    - Middle ground
SSD Persistent Disk         - Fast, high IOPS
Local SSD                   - Fastest, non-persistent
Regional Persistent Disk    - Replicated across zones
```

### Common Firewall Rules

```
Allow SSH (port 22) from your IP
Allow HTTP (port 80) from anywhere
Allow HTTPS (port 443) from anywhere
Allow custom ports for applications
```

### Instance States

```
PROVISIONING - Being created
STAGING - Preparing to start
RUNNING - Instance is on
STOPPING - Being stopped
TERMINATED - Stopped
SUSPENDING - Being suspended
SUSPENDED - Suspended
REPAIRING - Being repaired
```

### Important Concepts

```
Region          - Geographic area (us-central1)
Zone            - Isolated location in region (us-central1-a)
VPC Network     - Isolated virtual network
Subnet          - IP range in specific region
Firewall Rule   - Controls traffic
Route           - Determines traffic path
Service Account - Identity for instances
Image           - OS and software blueprint
Snapshot        - Point-in-time disk copy
Instance Group  - Collection of identical instances
Load Balancer   - Distributes traffic
Health Check    - Monitors instance status
```

### Pricing Components

```
Compute      - Per vCPU-hour (varies by machine type)
Memory       - Usually included in machine type
Disk Storage - Per GB-month
Snapshots    - Per GB-month
Data Egress  - Per GB (data ingress is free)
Public IP    - Static IPs cost extra
```

---

**Total estimated reading time: 60 minutes**
**Word count: ~10,200 words**

This markdown file is formatted to be easily converted to audio using any text-to-speech tool. The structure with headers and clear sections makes it easy to pause and review individual concepts.
