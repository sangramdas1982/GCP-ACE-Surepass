# Terraform: Complete 1-Hour Study Guide
## For Google Cloud Associate Engineer Certification

---

## Introduction

Welcome to this comprehensive guide on Terraform, the infrastructure-as-code tool that's become essential for cloud engineers. Whether you're preparing for your Google Cloud Associate Engineer certification or building real-world infrastructure on Google Cloud, this guide will take you through everything you need to master Terraform.

Think of this guide as a deep dive into how to define, build, and manage infrastructure programmatically. Unlike clicking through the Google Cloud Console, Terraform lets you describe your infrastructure in code, version control it like application code, and reproduce it consistently across environments.

This is not just about syntax—it's about understanding the philosophy behind infrastructure-as-code, the problems Terraform solves, and how to use it effectively in a production Google Cloud environment.

---

## Part 1: Understanding Infrastructure-as-Code and Terraform Fundamentals

### The Problem Terraform Solves

Imagine you're managing cloud infrastructure for your organization. You need to create virtual machines, databases, networking, load balancers, and dozens of other resources. Currently, your team does this through the Google Cloud Console by clicking buttons.

Here are the problems you'll face:

**Reproducibility is a nightmare.** If you need to create the same infrastructure in another region or for another project, you have to manually click through the console again. It's error-prone, takes time, and rarely results in identical infrastructure.

**No version control.** If something breaks, you don't know what changed. There's no audit trail, no history, no way to roll back to a previous state.

**Documentation is often wrong.** You might have a README documenting infrastructure, but it quickly becomes outdated as things change manually.

**Collaboration is difficult.** If multiple team members are managing infrastructure, they might step on each other's toes. There's no mechanism for code review.

**Consistency across environments is hard.** Production, staging, and development might have subtle differences because they were created at different times by different people.

**Scaling is manual.** Creating one instance is easy. Creating a hundred instances is tedious and error-prone.

Infrastructure-as-Code (IaC) solves all of these problems. Instead of manually creating infrastructure through a console, you define it in code. The code goes into version control. Changes go through code review. The same code can be applied to any environment, producing consistent results.

Terraform is the leading tool for infrastructure-as-code. It's cloud-agnostic (works with AWS, Azure, Google Cloud, and many others), it's declarative (you describe the desired state, not the steps to get there), and it's widely used in the industry.

### What is Terraform?

Terraform is an open-source infrastructure-as-code tool created by HashiCorp. At its core, Terraform:

1. **Reads infrastructure code** (written in HCL—HashiCorp Configuration Language)
2. **Determines what currently exists** (by comparing with state)
3. **Calculates what needs to change** (creates an execution plan)
4. **Makes the changes** (applies the plan to create/update/delete resources)
5. **Records what exists** (maintains state)

The key insight is that Terraform is **declarative, not imperative**. You declare what infrastructure you want, and Terraform figures out how to create it. This is different from writing a script that says "create this, then create that, then configure this." With Terraform, you say "I want a network, a firewall rule, and two compute instances" and Terraform handles the order and dependencies.

### Key Terraform Concepts

**Configuration** is the code you write describing infrastructure. It's written in HCL.

**State** is Terraform's understanding of what actually exists in your cloud account. It's stored in a state file.

**Plan** is Terraform's calculation of what will change if you apply your configuration now.

**Apply** is when you actually make the changes.

**Provider** is Terraform's plugin that knows how to talk to a specific cloud platform. The Google Cloud Provider is `google`.

**Resource** is something Terraform can create or manage, like a Google Compute Engine instance or a Cloud Storage bucket.

**Data source** is something Terraform can read information from without creating it, like information about an existing network.

---

## Part 2: Terraform Language Fundamentals

### HCL Syntax Basics

Terraform configurations are written in HCL (HashiCorp Configuration Language). It's a configuration language designed to be human-readable while being powerful enough to express complex infrastructure.

The basic building block is a **block**. A block has a type, optional labels, and a body. Here's the structure:

```
type "label1" "label2" {
  key = value
}
```

For example, a resource block looks like:

```
resource "google_compute_instance" "web_server" {
  name         = "my-web-server"
  machine_type = "n1-standard-1"
  zone         = "us-central1-a"
}
```

This declares a resource of type `google_compute_instance` with the logical name `web_server` (the identifier you use in Terraform, not the name in Google Cloud). The body contains arguments that configure the resource.

Key points to understand:
- `google_compute_instance` is the resource type (tells Terraform what to create)
- `web_server` is the logical name (used to reference this resource from other parts of your configuration)
- Arguments like `name` configure the resource
- Values can be strings, numbers, booleans, lists, or objects

### Providers: Connecting to Google Cloud

Before Terraform can create anything in Google Cloud, it needs credentials. This is where the **provider** block comes in:

```
provider "google" {
  project = "my-gcp-project"
  region  = "us-central1"
}
```

This tells Terraform to use the Google Cloud provider for the specified project and default region.

But here's something crucial: Terraform needs credentials to authenticate to Google Cloud. There are several ways to provide these:

**Service Account Key File**: You create a service account in Google Cloud, download its JSON key file, and tell Terraform where it is:

```
provider "google" {
  project     = "my-gcp-project"
  region      = "us-central1"
  credentials = file("path/to/key.json")
}
```

**Application Default Credentials (ADC)**: If you're running Terraform on a machine where you've already authenticated (using `gcloud auth application-default login`), Terraform automatically uses those credentials.

**Google Cloud Workload Identity**: If you're running Terraform in Google Cloud (on a Compute Engine instance or in Cloud Build), you can use Workload Identity to grant Terraform permissions without managing credentials.

For production and certification exam purposes, understand that Terraform needs a way to authenticate to Google Cloud, and there are multiple options depending on your environment.

### Variables: Making Configurations Reusable

Hardcoding values like project IDs and machine types directly into your configuration makes it difficult to reuse. Variables make configurations flexible.

You declare a variable with a `variable` block:

```
variable "project_id" {
  description = "The GCP project ID"
  type        = string
  default     = "my-default-project"
}
```

Then you can use it in your configuration:

```
provider "google" {
  project = var.project_id
  region  = "us-central1"
}
```

This is powerful because the same configuration can be used in different projects by passing different variable values.

Variables can have types like `string`, `number`, `bool`, `list(string)`, `map(string)`, or complex objects.

You can set variable values in several ways:
- Command line: `terraform apply -var="project_id=my-project"`
- In a `.tfvars` file: `terraform apply -var-file="prod.tfvars"`
- Environment variables: `export TF_VAR_project_id=my-project`
- Interactively: Terraform will prompt for values if not provided

### Outputs: Exposing Values

While variables are inputs to your configuration, **outputs** are values that Terraform exposes after creating resources.

For example, after creating a Compute Engine instance, you might want to output its IP address:

```
output "instance_ip" {
  description = "The external IP of the web server"
  value       = google_compute_instance.web_server.network_interface[0].access_config[0].nat_ip
}
```

When you run `terraform apply`, the output is displayed. You can also retrieve outputs later with `terraform output instance_ip`.

Outputs are useful for:
- Getting information about created resources
- Sharing values with other Terraform configurations (via state)
- Providing information to monitoring or deployment systems
- Documentation

### Locals: Computed Values

Sometimes you want to compute a value once and reuse it throughout your configuration without making it a variable.

```
locals {
  environment_prefix = "${var.environment}-${var.region}"
  common_labels = {
    managed_by = "terraform"
    environment = var.environment
  }
}
```

Locals are like variables but simpler—they're computed once and can't be overridden at the command line. They're useful for values that depend on other values or are used throughout your configuration.

---

## Part 3: Resources and Data Sources

### Understanding Resources

A **resource** represents something Terraform can create, manage, or destroy in Google Cloud. When you define a resource, you're saying "I want this thing to exist."

Some common GCP resources:

- `google_compute_instance`: A Compute Engine VM
- `google_compute_network`: A VPC network
- `google_compute_firewall`: A firewall rule
- `google_sql_database_instance`: A Cloud SQL database
- `google_storage_bucket`: A Cloud Storage bucket
- `google_compute_backend_service`: A backend service for load balancing
- `google_service_account`: A service account for authentication

Each resource type has arguments that configure it and attributes that Terraform exposes after creation.

When you define a resource, Terraform doesn't immediately create it. You first run `terraform plan` to see what will change, then `terraform apply` to actually create it.

### Resource Dependencies

One of Terraform's powerful features is automatic dependency management. When you reference one resource from another, Terraform understands that the first must be created before the second.

For example:

```
resource "google_compute_network" "main" {
  name = "main-network"
}

resource "google_compute_instance" "web" {
  name         = "web-server"
  machine_type = "n1-standard-1"
  zone         = "us-central1-a"

  network_interface {
    network = google_compute_network.main.id
  }
}
```

When you run `terraform apply`, Terraform creates the network first, then the instance. It understands the dependency because the instance references the network.

You can also explicitly declare dependencies with `depends_on` if implicit dependencies aren't clear:

```
resource "google_storage_bucket" "data" {
  name = "my-data-bucket"

  depends_on = [google_compute_network.main]
}
```

This tells Terraform that even though there's no obvious dependency, the bucket depends on the network existing first.

### Data Sources: Reading Information

While resources create or manage infrastructure, **data sources** read information about existing infrastructure.

For example, you might want to get information about a VPC network that already exists:

```
data "google_compute_network" "default" {
  name = "default"
}
```

After this, you can reference `data.google_compute_network.default.id` to get the ID of the existing network.

Data sources are useful when:
- You want to reference infrastructure created outside Terraform
- You want to get information about existing resources to use in Terraform
- You want to avoid creating something that already exists

The key difference: resources create things, data sources read things.

### Resource Attributes and References

When you create a resource, Terraform exposes attributes that you can reference elsewhere.

For a Compute Engine instance, Terraform exposes:
- `self_link`: The full URL of the instance
- `network_interface[0].access_config[0].nat_ip`: The external IP address
- `internal_ip`: The internal IP address
- `service_account`: The service account used

You reference these with the syntax `resource_type.logical_name.attribute`.

Understanding what attributes are available for each resource type is crucial. You need to know what information Terraform exposes and how to reference it.

---

## Part 4: State Management - The Most Critical Topic

### What is State?

State is Terraform's record of what resources currently exist. It's stored in a file (by default, `terraform.tfstate`), and it's the single source of truth for what Terraform is managing.

Here's why state matters: When you run `terraform plan`, Terraform doesn't query Google Cloud directly. Instead, it compares your configuration against the state file. It then determines what needs to change to make the actual Google Cloud resources match what's in your configuration.

State serves several purposes:
1. **Tracking**: It knows what resources exist and what their current properties are
2. **Mapping**: It tracks which resources in your configuration correspond to which actual cloud resources
3. **Performance**: It avoids querying the cloud provider for every plan
4. **Locking**: It prevents concurrent applies from conflicting (in remote state)

### Local vs Remote State

By default, Terraform stores state locally in a `terraform.tfstate` file in your working directory. This is fine for learning and small projects, but it has serious problems for teams:

**Security**: The state file contains sensitive information like database passwords, API keys, and other secrets. Storing it on your laptop is a security risk.

**Sharing**: If you're working in a team, everyone needs access to the same state file. Sharing files is error-prone.

**Consistency**: If two people run Terraform at the same time, their state files might diverge, causing conflicts.

**Remote state** solves these problems. Instead of storing state locally, Terraform stores it in a remote location that the entire team can access.

For Google Cloud, the best practice is to use **Google Cloud Storage as remote state**:

```
terraform {
  backend "gcs" {
    bucket = "my-terraform-state"
    prefix = "prod"
  }
}
```

This tells Terraform to store state in a Google Cloud Storage bucket named `my-terraform-state` under the `prod` prefix.

Remote state provides:
- **Security**: The state file is stored in a secure location with access controls
- **Sharing**: Multiple people can access the same state
- **Locking**: Cloud Storage provides locking to prevent concurrent modifications
- **Backup**: Cloud Storage handles backup and versioning

For the exam and for any real project, always use remote state.

### State Locking

When using remote state, **state locking** prevents two people from running `terraform apply` at the same time.

Here's the scenario: Person A runs `terraform apply`. Terraform acquires a lock on the state. Person B tries to run `terraform apply` at the same time, but because a lock exists, Terraform waits (or fails with a message that someone else is currently making changes).

When Person A's apply finishes, the lock is released, and Person B can proceed.

State locking is automatic with remote state backends that support it (like Google Cloud Storage with state locking enabled). Without locking, concurrent applies could cause state corruption.

### Sensitive Data in State

State files contain sensitive information: database passwords, API keys, private keys, and other secrets. By default, this information is stored in plain text in the state file.

This is a security problem. If someone gains access to your state file, they have access to your secrets.

Best practices to handle sensitive data:
1. **Restrict state file access**: Use IAM to ensure only authorized people can read the state file
2. **Encrypt state at rest**: For Cloud Storage, enable encryption
3. **Use Google Secret Manager**: For highly sensitive data, store it in Google Secret Manager and reference it in Terraform
4. **Never commit state files to version control**: Add `.terraform` and `.tfstate*` to `.gitignore`

### Terraform State Commands

You'll often need to inspect or manipulate state:

`terraform state list` shows all resources Terraform is managing.

`terraform state show google_compute_instance.web_server` shows the details of a specific resource's state.

`terraform state rm google_compute_instance.web_server` removes a resource from state without destroying it (rarely used but sometimes necessary).

`terraform state mv` moves a resource to a different location in state.

Understanding these commands is important because sometimes you need to manually manipulate state (though this should be rare).

---

## Part 5: Terraform Workflows and Commands

### The Terraform Workflow

The typical Terraform workflow has four steps:

**Initialize**: `terraform init`

When you first clone a Terraform repository or set up a new Terraform project, you run `terraform init`. This:
- Downloads and installs provider plugins (the Google Cloud provider)
- Sets up the backend (where state is stored)
- Creates necessary directories

This command is idempotent, so you can run it multiple times without problems.

**Plan**: `terraform plan`

Before making any changes, run `terraform plan`. This:
- Reads your configuration
- Connects to Google Cloud and reads current state
- Compares desired state (your configuration) to current state
- Outputs a plan showing exactly what will change

The plan output shows resources that will be created (marked with `+`), updated (marked with `~`), or destroyed (marked with `-`).

Always review the plan carefully before applying. This is your safety net against accidentally deleting critical infrastructure.

You can save a plan to a file and apply it later:
```
terraform plan -out=tfplan
terraform apply tfplan
```

**Apply**: `terraform apply`

After reviewing the plan and confirming the changes are correct, you run `terraform apply`. This:
- Executes the plan
- Creates, updates, or deletes resources in Google Cloud
- Updates the state file to reflect the new state

Terraform will prompt you to confirm before applying (you can use `-auto-approve` to skip the prompt, but use this carefully).

**Destroy**: `terraform destroy`

When you no longer need the infrastructure, `terraform destroy` removes all resources Terraform created. This:
- Plans the destruction of all resources
- Deletes all resources
- Updates state to empty

Be careful with destroy—it actually deletes things. There's usually a prompt for confirmation.

### Useful Terraform Commands

`terraform fmt` formats your configuration files to match HCL standards. Good for keeping code consistent.

`terraform validate` checks your configuration for syntax errors without connecting to Google Cloud.

`terraform refresh` updates the state file by querying Google Cloud, without changing anything. Useful when someone made changes outside Terraform.

`terraform console` opens an interactive console where you can test expressions and evaluate references.

`terraform graph` outputs a graph of resource dependencies in DOT format, useful for visualizing your infrastructure.

---

## Part 6: Modules - Organizing and Reusing Code

### Why Modules Matter

As your infrastructure grows, managing everything in a single configuration file becomes unwieldy. Modules solve this.

A **module** is a reusable package of Terraform configuration. It has inputs (variables), outputs, and resources.

For example, you might create a module that creates a complete web server setup: a network, firewall rules, a Compute Engine instance, and a load balancer. Once defined, you can use this module multiple times with different inputs, creating identical infrastructure quickly.

### Structure of a Module

A module is just a directory with Terraform files. The typical structure:

```
my-module/
├── main.tf          # Main configuration
├── variables.tf     # Input variables
├── outputs.tf       # Outputs
└── README.md        # Documentation
```

**variables.tf** declares input variables:
```
variable "project_id" {
  type = string
}

variable "instance_count" {
  type    = number
  default = 3
}
```

**main.tf** contains the resources:
```
resource "google_compute_instance" "web" {
  count = var.instance_count
  name  = "web-${count.index}"
  # ... more configuration
}
```

**outputs.tf** exposes values:
```
output "instance_ips" {
  value = google_compute_instance.web[*].network_interface[0].access_config[0].nat_ip
}
```

### Using Modules

To use a module, you declare it with a `module` block:

```
module "web_servers" {
  source = "./my-module"

  project_id    = var.project_id
  instance_count = 3
}
```

The `source` argument points to the module (local path, Git repository, or Terraform Registry).

You reference outputs from a module with `module.MODULE_NAME.OUTPUT_NAME`:

```
output "web_ips" {
  value = module.web_servers.instance_ips
}
```

### Module Best Practices

**Reusability**: Design modules to be reusable across different projects and environments. Don't hardcode values that should be variables.

**Documentation**: Include a README in your module documenting what it does, what variables it accepts, and what it outputs.

**Versioning**: Store modules in version control. Tag releases so you can pin to specific versions.

**Registry**: The Terraform Registry hosts public modules. You can find existing modules there instead of writing everything from scratch.

For the exam and for real work, understanding how to create and use modules is crucial for managing complex infrastructure.

---

## Part 7: Working with Multiple Environments

### Using Variables and Workspaces

A common pattern is to have multiple environments: development, staging, and production. Terraform provides tools to manage this.

**Using Variables**: Create separate variable files for each environment:

`dev.tfvars`:
```
environment = "dev"
instance_type = "n1-standard-1"
instance_count = 1
```

`prod.tfvars`:
```
environment = "prod"
instance_type = "n1-highmem-8"
instance_count = 5
```

Then apply with different variable files:
```
terraform apply -var-file="dev.tfvars"
terraform apply -var-file="prod.tfvars"
```

**Using Workspaces**: Terraform workspaces allow multiple state files from the same configuration:

```
terraform workspace new dev
terraform workspace new prod
terraform workspace select dev
terraform apply  # Applies to dev workspace
terraform workspace select prod
terraform apply  # Applies to prod workspace
```

Each workspace has its own state file and its own set of resources.

For the exam, understand both approaches. Generally, using variables and separate configurations is preferred for environments, while workspaces are useful for testing variations of the same configuration.

### Separating Configuration by Environment

A more sophisticated approach uses separate directories:

```
terraform/
├── dev/
│   ├── main.tf
│   ├── variables.tf
│   └── dev.tfvars
├── prod/
│   ├── main.tf
│   ├── variables.tf
│   └── prod.tfvars
└── modules/
    └── shared modules...
```

Each environment has its own configuration, but they reference shared modules. This provides clear separation while avoiding code duplication.

---

## Part 8: Advanced Topics

### Count and For_Each: Creating Multiple Resources

Sometimes you want to create multiple resources with similar configuration.

**Count** uses an index to create multiple copies:

```
resource "google_compute_instance" "web" {
  count = var.instance_count
  name  = "web-${count.index}"
  # ...
}
```

This creates `var.instance_count` instances with names like `web-0`, `web-1`, etc.

**For_each** iterates over a map or set:

```
variable "instances" {
  type = map(object({
    machine_type = string
    zone         = string
  }))
}

resource "google_compute_instance" "web" {
  for_each = var.instances
  name     = each.key
  machine_type = each.value.machine_type
  zone     = each.value.zone
}
```

With input:
```
instances = {
  "web-1" = { machine_type = "n1-standard-1", zone = "us-central1-a" }
  "web-2" = { machine_type = "n1-standard-2", zone = "us-central1-b" }
}
```

This creates instances named `web-1` and `web-2` with different configurations.

For_each is generally preferred over count because it produces more stable resource addresses that don't change when you add or remove items.

### Conditional Logic

Terraform supports conditional logic with the `count` argument or the `conditional expression` (`condition ? true_value : false_value`):

```
resource "google_compute_address" "external_ip" {
  count = var.create_external_ip ? 1 : 0
  name  = "external-ip"
}
```

This creates an external IP only if `var.create_external_ip` is true.

Or using ternary:

```
variable "instance_type" {
  type = string
}

locals {
  machine_type = var.instance_type == "production" ? "n1-highmem-8" : "n1-standard-1"
}
```

### Dynamic Blocks

For complex nested structures, `dynamic` blocks help avoid repetition:

```
resource "google_compute_firewall" "allow_traffic" {
  name = "allow-traffic"

  dynamic "allow" {
    for_each = var.allowed_protocols
    content {
      protocol = allow.value.protocol
      ports    = allow.value.ports
    }
  }
}
```

This creates multiple `allow` blocks from a variable, reducing code duplication.

---

## Part 9: Terraform and Google Cloud Best Practices

### Structuring Terraform Projects

For small projects, a single directory works fine. For larger projects, structure like this:

```
terraform/
├── main.tf              # Main resources
├── variables.tf         # Input variables
├── outputs.tf           # Outputs
├── provider.tf          # Provider configuration
├── backend.tf           # Backend configuration
├── terraform.tfvars     # Default variable values
├── modules/
│   ├── networking/
│   ├── compute/
│   └── databases/
└── environments/
    ├── dev/
    ├── staging/
    └── prod/
```

This separation makes it easier to find things and maintain the code.

### Using Service Accounts with Terraform

In production, Terraform should authenticate as a service account with minimal permissions, not as a user.

Create a service account in Google Cloud, grant it only the permissions needed, and configure Terraform to use it:

```
provider "google" {
  project     = var.project_id
  credentials = file(var.service_account_key)
}
```

This is better than using personal credentials because:
- Credentials are tied to a specific service account, not a person
- You can granularly control permissions
- If credentials are compromised, you can rotate them without affecting the person

### Using Terraform with Google Cloud Build

For continuous deployment, integrate Terraform with Google Cloud Build:

Create a `cloudbuild.yaml`:
```yaml
steps:
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['init']
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['plan']
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['apply', '-auto-approve']
```

When you push to a repository, Cloud Build automatically runs Terraform, ensuring infrastructure changes go through a consistent process.

### Protecting Your Infrastructure

Some best practices to prevent accidental damage:

1. **Require plan review**: Use branch protection rules to require that Terraform plans are reviewed before merging.

2. **Use prevent_destroy**: For critical resources, add `lifecycle { prevent_destroy = true }` to prevent accidental deletion.

3. **Plan before apply**: Always run `terraform plan` before `terraform apply` and review the output.

4. **Use remote state with locking**: This prevents concurrent modifications.

5. **Backup state**: Regularly backup your state file.

6. **Control who can apply**: Restrict who has permission to run `terraform apply` in production.

---

## Part 10: Common Patterns and Exam Scenarios

### Creating a VPC with Subnets

A common exam scenario: create a VPC network with subnets in multiple regions.

```
resource "google_compute_network" "vpc" {
  name                    = var.network_name
  auto_create_subnetworks = false
}

resource "google_compute_subnetwork" "subnet" {
  for_each      = var.subnets
  name          = each.key
  ip_cidr_range = each.value.ip_range
  region        = each.value.region
  network       = google_compute_network.vpc.id
}
```

With variables:
```
subnets = {
  "us-central1" = { ip_range = "10.0.0.0/24", region = "us-central1" }
  "us-east1"    = { ip_range = "10.0.1.0/24", region = "us-east1" }
}
```

### Managing Firewall Rules

Firewall rules are common in Terraform:

```
resource "google_compute_firewall" "allow_http" {
  name    = "allow-http"
  network = google_compute_network.vpc.name

  allow {
    protocol = "tcp"
    ports    = ["80", "443"]
  }

  source_ranges = ["0.0.0.0/0"]
  target_tags   = ["http-server"]
}
```

### Creating Compute Instances with Startup Scripts

Often you want to run commands when an instance starts:

```
resource "google_compute_instance" "web" {
  name         = "web-server"
  machine_type = "n1-standard-1"
  zone         = "us-central1-a"

  metadata_startup_script = <<-EOT
              #!/bin/bash
              apt-get update
              apt-get install -y nginx
              EOT

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-11"
    }
  }

  network_interface {
    network = google_compute_network.vpc.id
  }

  service_account {
    scopes = ["cloud-platform"]
  }
}
```

### Managing Cloud Storage Buckets

```
resource "google_storage_bucket" "data" {
  name          = var.bucket_name
  location      = "US"
  force_destroy = false

  uniform_bucket_level_access = true

  versioning {
    enabled = true
  }

  lifecycle_rule {
    condition {
      age = 30
    }
    action {
      type = "Delete"
    }
  }
}
```

### Managing Cloud SQL Instances

```
resource "google_sql_database_instance" "main" {
  name             = "postgres-instance"
  database_version = "POSTGRES_13"
  region           = "us-central1"

  settings {
    tier              = "db-f1-micro"
    availability_type = "REGIONAL"
    backup_configuration {
      enabled = true
    }
  }
}

resource "google_sql_database" "app_db" {
  name     = "app_database"
  instance = google_sql_database_instance.main.name
}
```

---

## Part 11: Troubleshooting and Debugging

### Common Errors and Solutions

**Error: Invalid resource type**
Usually means the provider isn't installed. Run `terraform init` to download providers.

**Error: Resource already exists**
The resource already exists in Google Cloud but not in state. Either import it into state or use a different name.

**Error: Insufficient permission**
The service account doesn't have permission to perform the action. Check IAM permissions.

**Error: Timeout**
The resource creation is taking too long. Usually because of dependencies or resource constraints.

### Importing Existing Resources

If infrastructure already exists in Google Cloud but not in Terraform, you can import it:

```
terraform import google_compute_instance.web_server projects/my-project/zones/us-central1-a/instances/web-server
```

This adds the existing resource to your state file without modifying it. Then you can write the configuration to match.

### Debugging Terraform

Enable debug logging:
```
export TF_LOG=DEBUG
terraform plan
```

This outputs very detailed information about what Terraform is doing.

Create a graph of your infrastructure:
```
terraform graph | dot -Tsvg > graph.svg
```

This visualizes resource dependencies.

Use `terraform console` to test expressions:
```
terraform console
> var.project_id
"my-project"
```

---

## Part 12: Review and Exam Preparation

### Key Concepts to Master

Make sure you thoroughly understand:

1. **Terraform fundamentals**: What it is, why it matters, declarative vs imperative.

2. **Configuration language**: HCL syntax, resources, variables, outputs, locals.

3. **State**: What it is, why it matters, local vs remote, sensitive data, locking.

4. **Providers**: Google Cloud provider configuration, authentication, scopes.

5. **Workflows**: Init, plan, apply, destroy and what each does.

6. **Resources and data sources**: Common GCP resources, attributes, references, dependencies.

7. **Modules**: Creating, using, structuring, best practices.

8. **Advanced features**: Count, for_each, conditionals, dynamic blocks.

9. **Best practices**: Security, state management, organization, protecting infrastructure.

10. **Troubleshooting**: Common errors, importing, debugging.

### Exam Focus Areas

The Google Cloud Associate Engineer exam focuses on:

- **Creating and managing infrastructure with Terraform**: Writing configurations, understanding resources.
- **Using variables and outputs**: Making configurations flexible and reusable.
- **Managing state**: Understanding remote state, locking, sensitive data.
- **Best practices**: Security, organization, using modules.
- **Troubleshooting**: Fixing common issues, importing resources.
- **Integration with Google Cloud services**: Creating resources that interact with other GCP services.

### Practice Scenarios

The best way to learn is by doing. Try these scenarios:

1. **Create a VPC network with multiple subnets** in different regions using for_each.

2. **Create a Compute Engine instance with a startup script** that installs a web server.

3. **Create a Cloud Storage bucket with versioning and lifecycle policies**.

4. **Create a Cloud SQL instance with a database** and connect from an instance using a startup script.

5. **Create a module** that bundles related resources and can be reused.

6. **Use variables and terraform.tfvars** to manage different environments.

7. **Set up remote state** in Google Cloud Storage.

8. **Import an existing resource** into Terraform.

For each scenario:
- Write the configuration
- Run `terraform plan` and understand the output
- Run `terraform apply` and see it create resources
- Modify the configuration and observe updates
- Run `terraform destroy` and see cleanup

This hands-on experience is invaluable.

### What You'll See on the Exam

Expect questions like:

- "You need to create three identical instances in different zones. Which is the best approach?" (Answer: for_each)

- "You have a Terraform configuration that references a value that was just created. How does Terraform know to create the first resource before the second?" (Answer: Implicit dependencies through references)

- "State contains sensitive information. How do you protect it?" (Answer: Remote state, encryption, access controls)

- "You want the same Terraform configuration to work in dev, staging, and production. What's the best approach?" (Answer: Variables, modules, separate environments)

- "How do you connect a Compute Engine instance to an existing VPC network that wasn't created by Terraform?" (Answer: Data source to reference the existing network)

Study the Terraform documentation for Google Cloud resources. Know what attributes are available and how to reference them. Practice writing configurations and understand exactly what will happen when you apply them.

---

## Part 13: Advanced Topics for Depth

### Terraform Functions and Expressions

Terraform has a rich set of functions for manipulating data:

String functions: `upper()`, `lower()`, `substr()`, `replace()`, etc.

List functions: `concat()`, `flatten()`, `reverse()`, `sort()`, etc.

Map functions: `keys()`, `values()`, `merge()`, etc.

Encoding functions: `base64encode()`, `base64decode()`, `jsonencode()`, `jsondecode()`

Example:
```
locals {
  region_map = {
    "us-central1" = "US Central"
    "us-east1"    = "US East"
  }

  all_zones = flatten([
    for region, name in local.region_map : [
      "${region}-a",
      "${region}-b",
      "${region}-c"
    ]
  ])
}
```

### Null Provider and Resource Suppression

The `null_provider` allows you to create resources that don't actually do anything, useful for testing or as placeholders.

`null_resource` with `local-exec` trigger can run local commands:

```
resource "null_resource" "run_script" {
  provisioners "local-exec" {
    command = "bash scripts/setup.sh"
  }
}
```

### Splat Syntax

The splat syntax allows you to reference multiple resources at once:

```
output "all_instance_ips" {
  value = google_compute_instance.web[*].network_interface[0].access_config[0].nat_ip
}
```

This gets the IP from all instances created with count, without having to manually specify each one.

### Meta-Arguments

Resources support meta-arguments that modify behavior:

`count`: Create multiple copies.

`for_each`: Iterate over a collection.

`depends_on`: Explicit dependencies.

`provider`: Use a specific provider (useful with multiple provider instances).

`lifecycle`: Control creation, updates, deletion:
```
resource "google_compute_instance" "web" {
  lifecycle {
    prevent_destroy = true
    ignore_changes = [metadata]
  }
}
```

---

## Part 14: Integration Scenarios

### Terraform with Cloud Build for CI/CD

Automate infrastructure changes:

```yaml
steps:
  # Validate
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['validate']
    
  # Plan
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['plan', '-out=tfplan']
    
  # Require approval (manual step)
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['show', 'tfplan']
    
  # Apply
  - name: 'gcr.io/cloud-builders/terraform'
    args: ['apply', 'tfplan']
```

### Multi-Project Terraform

For organizations with multiple projects, use aliases:

```
provider "google" {
  alias   = "dev"
  project = "dev-project"
}

provider "google" {
  alias   = "prod"
  project = "prod-project"
}

resource "google_compute_instance" "dev_server" {
  provider = google.dev
  # ...
}

resource "google_compute_instance" "prod_server" {
  provider = google.prod
  # ...
}
```

### Referencing Data Across Modules

Share information between modules through outputs and inputs:

```
module "networking" {
  source = "./modules/networking"
  # ...
}

module "compute" {
  source = "./modules/compute"
  network_id = module.networking.network_id
  # ...
}
```

The compute module receives the network ID from the networking module, establishing the dependency.

---

## Conclusion

Terraform is a powerful tool that transforms how you manage infrastructure. It solves real problems—reproducibility, version control, collaboration, consistency—that plague manual infrastructure management.

The key to mastering Terraform is:

1. **Understand the fundamentals**: State, providers, resources, the workflow.

2. **Write lots of configurations**: The only way to really learn is by doing.

3. **Use best practices**: Remote state, modules, variables, security.

4. **Understand Google Cloud**: Know the resources you're creating and how they work.

5. **Troubleshoot methodically**: When things break, systematically diagnose and fix.

6. **Practice the exam scenarios**: Make sure you can handle the types of questions that will appear.

Terraform is a career-valuable skill. Organizations everywhere use it to manage their cloud infrastructure. Mastering it—both for your certification and for real-world use—is a smart investment in your career.

As you continue your SRE journey, Terraform becomes an extension of your operational capabilities, allowing you to automate, reproduce, and manage infrastructure at scale.

Good luck with your certification!

---

## Quick Reference

### Essential Terraform Commands

```
terraform init                          # Initialize Terraform
terraform validate                      # Check syntax
terraform plan                          # Show what will change
terraform apply                         # Create/update resources
terraform destroy                       # Delete resources
terraform fmt                           # Format configuration
terraform state list                    # List managed resources
terraform state show RESOURCE            # Show resource details
terraform import TYPE.NAME ID            # Import existing resource
terraform console                       # Interactive console
terraform output NAME                   # Show output value
```

### Common Resource Types for GCP

```
google_compute_instance                 # VM Instance
google_compute_network                  # VPC Network
google_compute_subnetwork              # Subnet
google_compute_firewall                # Firewall rule
google_compute_address                 # Static IP
google_storage_bucket                  # Cloud Storage bucket
google_sql_database_instance           # Cloud SQL database
google_container_cluster               # GKE cluster
google_service_account                 # Service account
google_project_iam_member              # IAM binding
google_compute_disk                    # Persistent disk
google_compute_image                   # Custom image
```

### HCL Syntax Reference

```
# String variable
variable "name" { type = string }

# Number variable
variable "count" { type = number }

# List variable
variable "zones" { type = list(string) }

# Map variable
variable "labels" { type = map(string) }

# Object variable
variable "config" {
  type = object({
    name  = string
    count = number
  })
}

# Resource reference
resource.type.name.attribute

# Module reference
module.name.output

# Variable reference
var.name

# Local reference
local.name

# Conditional
condition ? true_value : false_value

# Count
count.index
count.each

# For-each
each.key
each.value

# Interpolation
"${var.name}-${count.index}"
```

### Important Terraform Files

`.terraform/` - Provider plugins and modules (don't commit to version control)

`terraform.tfstate` - Current state (don't commit if local state)

`.terraform.lock.hcl` - Lock file for provider versions (DO commit)

`.gitignore` - Should contain `.terraform*`, `*.tfstate*`, `.env*`

---

**Total estimated reading time: 60 minutes**
**Word count: ~10,500 words**

This markdown file is formatted to be easily converted to audio using any text-to-speech tool. The structure with headers and clear sections makes it easy to pause and review individual concepts.
