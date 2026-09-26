# Foundations for an engineer new to GCP

[Study guide home](README.md)

## 1. What you are actually managing

A cloud service exposes capabilities through an API. The console is a graphical interface to those capabilities; command-line tools and client libraries call APIs too. If a console operation succeeds but a CLI operation fails, compare account, project, location, and requested operation before assuming the tools implement unrelated systems.

The **control plane** configures resources, such as creating a VM. The **data plane** handles workload activity, such as serving web requests or reading application data. Permission to see a resource in a console does not necessarily authorize its data operations.

A useful request model is:

```text
Caller identity → authentication → permission check → resource/API operation
                                      ↓
                           policy, quota, location constraints
```

For networked application traffic, also draw the actual connection path. IAM approval alone does not open a firewall or make a DNS record resolve.

## 2. Managed services and your responsibilities

A VM gives you operating-system control and corresponding responsibility for software configuration and patching. A managed database takes over more infrastructure operations, but you still design schemas, grant access, configure recovery, and write efficient queries. A serverless runtime manages more execution infrastructure, but you still own application behavior, identity, resource settings, and cost consequences.

Choose the level of management that satisfies requirements. “Least operational effort” often favors a suitable managed service. An explicit requirement for a custom kernel, Kubernetes API, particular database engine, or filesystem protocol can rule out an otherwise convenient service.

## 3. Regions, zones, and failure domains

A zone is a deployment/failure domain inside a region. If both application replicas and their only database live in one zone, duplicating just the application process does not remove that zonal dependency. A regional database does not by itself make a single-zone application tier highly available.

Separate these questions: where is the resource configured, where is data stored, which failure can it survive, and how do clients recover? Use the service-specific location and replication documentation linked in the topic notes.

## 4. Networking language

- **IP address:** a network endpoint identifier. Private/internal addresses need a suitable private route; public/external addresses can participate in internet-facing paths.
- **CIDR:** a range such as `10.20.0.0/24`. For IPv4, /24 contains 256 numerical addresses before platform reservations. /23 is larger; /25 is smaller.
- **Route:** selects a next hop for a destination.
- **Firewall:** permits or denies a flow under its effective rules.
- **DNS:** maps names to records such as IP addresses.
- **NAT:** translates addresses/ports for supported connectivity patterns.
- **Load balancer:** distributes supported traffic to healthy backends.

To troubleshoot, follow the packet: name resolution → destination address → route → firewall → listener → application response. This order is a reasoning aid, not a claim that all real systems evaluate components in one identical sequence.

## 5. Containers and Kubernetes

A container image packages application code and dependencies. A running container is an execution instance of an image. Containers do not inherently require Kubernetes: Cloud Run can run supported containers too.

In Kubernetes, a Pod is the scheduled unit. A Deployment maintains desired replicas; a Service provides a stable endpoint; nodes supply execution capacity. Resource requests affect placement. A Pod can exist but remain unscheduled, start but crash, or run without becoming ready. Those states require different fixes.

## 6. Storage models

**Block storage** behaves like a disk attached to a machine. **File storage** offers shared filesystem paths and protocol semantics. **Object storage** stores named objects accessed through object APIs. A database adds a data model and query/transaction behavior on top of storage.

Do not translate every request for “storage” into a bucket. An unchanged NFS application and an archive of media objects have different interface requirements.

## 7. Authentication, authorization, and identity

Authentication establishes identity; authorization determines allowed actions. IAM connects a principal to roles on resources. A service account is a software identity and also a resource whose use can be controlled.

Always ask two separate questions for a deployed application: who is allowed to deploy it with an identity, and what is that identity allowed to access at runtime? A deployment permission failure and a runtime data-access failure usually involve different checks.

## 8. Reliability and recovery vocabulary

- **Availability:** whether the service can serve requests when needed.
- **Durability:** whether stored data survives loss/failure under the service’s design.
- **Backup:** recoverable historical data.
- **Replication:** maintaining additional copies of current data.
- **RPO:** maximum acceptable lost time interval of data.
- **RTO:** target time to restore service.
- **Idempotency:** repeating an operation does not create an unintended additional effect.

Imagine an order processed twice after a retry. A robust handler uses an order/event identifier to avoid double charging. Imagine a replicated database where a user deletes a table: both copies may lose it, so a historical backup/PITR capability matters.

## 9. How to read an exam scenario

1. Extract the required outcome: deploy, troubleshoot, secure, recover, or reduce cost.
2. Mark hard constraints: protocol, region, engine, identity provider, availability, or operational effort.
3. Identify the failing layer or needed capability.
4. Eliminate technically possible choices that violate a constraint.
5. Compare the remaining options using the stated priority, not a universal slogan.

A question asking for notification about cost is different from one asking for automated shutdown. A question asking for read scaling is different from one asking for recovery after corruption. These distinctions are more valuable than memorizing isolated product definitions.

Next: [15-day plan](15-DAY-PLAN.md), then [1.1 Projects and accounts](01-environment/1-1-projects-and-accounts/README.md).
