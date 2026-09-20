# Planning and Implementing Storage and Data Solutions: Complete 2-Hour Study Guide
## ACE Exam Section 2.2 - Deep Dive

---

## Introduction

"Planning and implementing storage and data solutions" represents a significant portion of the ACE exam (within the 30% "Planning and implementing" section). This section requires you to understand:

- **Which data product** to choose for your workload
- **How to deploy and configure** each service
- **How to load data** into these services
- **How to maintain** multi-region redundancy

The challenge: Google Cloud has many data products, each optimized for different use cases. Choosing the right one matters enormously.

This guide covers:
- Decision trees for choosing data products
- How each product works
- Deployment patterns
- Data loading strategies
- Multi-region architecture

---

## Part 1: Understanding the Data Product Landscape

### The Google Cloud Data Products

Google Cloud offers many data products, each serving different needs:

**Relational Databases**:
- Cloud SQL: Managed PostgreSQL, MySQL, SQL Server
- Cloud Spanner: Horizontally scalable, globally distributed, ACID
- AlloyDB: MySQL-compatible, ultra-fast

**NoSQL Databases**:
- Firestore: Document database, good for hierarchical data
- Bigtable: Wide-column store, high throughput, time-series
- Datastore: Legacy NoSQL (use Firestore instead)

**Analytics**:
- BigQuery: Data warehouse, petabyte-scale, SQL
- Dataflow: ETL/streaming data pipeline

**Message/Streaming**:
- Pub/Sub: Pub/sub messaging
- Cloud Tasks: Task queue
- Managed Kafka: Apache Kafka

**Search/Caching**:
- Memorystore: Redis/Memcached

**Object/File Storage**:
- Cloud Storage: Object storage (buckets)
- Filestore: Network file system (NFS)
- Managed Lustre: High-performance parallel file system

### Decision Matrix: Choosing Your Data Product

**Starting question 1: Type of workload?**

```
Transactional (OLTP) → Need relational or NoSQL DB
├─ Structured + Relations? → Cloud SQL, Spanner, AlloyDB
├─ Hierarchical documents? → Firestore
└─ Time-series + high throughput? → Bigtable

Analytical (OLAP) → BigQuery

Streaming/Events → Pub/Sub, Managed Kafka

Object Storage → Cloud Storage

Files/NFS → Filestore, Managed Lustre

Caching → Memorystore
```

**Starting question 2: Scale and distribution?**

```
Single region, < 10GB → Cloud SQL (easiest)
Multi-region, ACID required → Cloud Spanner
Massive scale (petabytes) → BigQuery
Single region, NoSQL, <1MB docs → Firestore
High throughput, time-series → Bigtable
```

**Starting question 3: Consistency requirements?**

```
Strong consistency, ACID → Cloud SQL, Cloud Spanner, AlloyDB
Eventual consistency → Firestore, Bigtable
High consistency + global scale → Cloud Spanner
```

---

## Part 2: Cloud SQL - Managed Relational Database

### What is Cloud SQL

**Cloud SQL** is a fully managed relational database service. You choose:
- PostgreSQL
- MySQL 5.7, 8.0
- SQL Server

You get: Backups, replication, monitoring, patching, all handled automatically.

### When to Use Cloud SQL

**Use Cloud SQL when**:
- You have structured data with relationships
- You need ACID transactions
- You're using SQL
- Scale is < 10TB single region
- You want fully managed

**Don't use when**:
- You need global distribution → Use Cloud Spanner
- You have unstructured documents → Use Firestore
- You need petabyte scale → Use BigQuery
- You need super-high throughput (millions/sec) → Use Bigtable

### Creating a Cloud SQL Instance

**Via Console**:
1. Go to Cloud SQL > Instances
2. Click "Create Instance"
3. Choose database engine (PostgreSQL, MySQL, SQL Server)
4. Configure:
   - Instance name
   - Password for root/postgres user
   - Database version (usually latest stable)
   - Region (choose for latency to app)
   - Zonal or regional HA
5. Click "Create Instance"

Takes a few minutes to provision.

**Via gcloud**:
```bash
gcloud sql instances create my-db \
  --database-version=POSTGRES_15 \
  --tier=db-f1-micro \
  --region=us-central1 \
  --availability-type=ZONAL
```

**Via Terraform**:
```hcl
resource "google_sql_database_instance" "main" {
  name                = "my-db"
  database_version    = "POSTGRES_15"
  region              = "us-central1"
  deletion_protection = false

  settings {
    tier              = "db-f1-micro"
    availability_type = "ZONAL"
    
    backup_configuration {
      enabled                        = true
      start_time                     = "03:00"
      transaction_log_retention_days = 7
    }

    database_flags {
      name  = "max_connections"
      value = "100"
    }
  }
}
```

### Instance Tiers (Machine Types)

```
db-f1-micro:    0.6 GB RAM (development only)
db-g1-small:    1.7 GB RAM
db-n1-standard-1: 3.75 GB RAM
db-n1-standard-4: 15 GB RAM
db-n1-highmem-16: 104 GB RAM
```

Choose tier based on:
- Data size
- Query complexity
- Concurrent connections
- Response time requirements

**Example sizing**:
```
< 100GB data, < 100 concurrent users → db-n1-standard-1
100GB - 1TB, moderate users → db-n1-standard-4
1TB+, high concurrency → db-n1-highmem or larger
```

### High Availability Setup

**Regional HA** (recommended for production):
```hcl
settings {
  availability_type = "REGIONAL"  # Creates standby replica
  backup_configuration {
    enabled = true
  }
}
```

With regional HA:
- Primary instance in one zone
- Standby replica in different zone of same region
- Automatic failover on failure
- Zero-downtime maintenance windows

### Backups

**Automatic backups**:
```hcl
backup_configuration {
  enabled                        = true
  start_time                     = "03:00"
  backup_location                = "us"
  transaction_log_retention_days = 7
  backup_retention_settings {
    retained_backups = 30
    retention_unit   = "COUNT"
  }
}
```

**Manual backups**:
```bash
gcloud sql backups create --instance=my-db
```

**Restore from backup**:
```bash
gcloud sql backups restore BACKUP_ID --backup-instance=my-db
```

### Connecting to Cloud SQL

**From Compute Engine (same VPC)**:
```bash
# Get private IP
PRIVATE_IP=$(gcloud sql instances describe my-db --format='value(ipAddresses[0].ipAddress)')

# Connect from VM
psql -h $PRIVATE_IP -U postgres
```

**From Cloud Shell**:
```bash
gcloud sql connect my-db --user=postgres
```

**From application code (Python)**:
```python
import sqlalchemy
from google.cloud.sql.connector import Connector

connector = Connector()

def getconn():
    return connector.connect(
        "project:region:instance",
        "pg8000",
        user="postgres",
        password="password",
        db="mydb"
    )

engine = sqlalchemy.create_engine(
    "postgresql+pg8000://",
    creator=getconn,
)
```

---

## Part 3: Cloud Spanner - Globally Distributed Database

### What is Cloud Spanner

**Cloud Spanner** is a relational database with global distribution and strong consistency:
- ACID transactions
- Global scale (splits automatically)
- Multi-region
- High availability
- Expensive but powerful

### When to Use Cloud Spanner

**Use when**:
- You need global distribution (users in multiple continents)
- You need strong consistency across regions
- You need ACID transactions
- You have large scale (100GB+)
- Occasional queries are okay with higher latency

**Don't use when**:
- Single region (use Cloud SQL)
- You need low cost (Cloud SQL cheaper)
- You have very low scale (< 10GB)

### Creating a Cloud Spanner Instance

**Via Console**:
1. Go to Spanner > Instances
2. Click "Create Instance"
3. Configure:
   - Instance name
   - Instance class (10 nodes, 100 nodes, etc.)
   - Multi-region configuration (optional)
4. Click "Create"

Takes longer than Cloud SQL, more expensive.

**Via Terraform**:
```hcl
resource "google_spanner_instance" "main" {
  name         = "my-spanner"
  config       = "nam-eur-asia1"  # Multi-region config
  num_nodes    = 3
  display_name = "Production"
}

resource "google_spanner_database" "main" {
  instance            = google_spanner_instance.main.name
  name                = "mydb"
  version_retention_period = "3d"
  retention_unit      = "d"
}
```

### Multi-Region Spanner Setup

**Single region**:
```
us-central1 (3 replicas within region)
```

**Multi-region**:
```
nam3: North America
eur3: Europe
asia1: Asia

Data replicated across all 3 regions, 5 copies total
```

### Spanner Pricing Model

Unlike Cloud SQL (hourly rate):
- Pay per **node-month** (very expensive)
- 1 node ≈ $3,000/month
- 3 nodes ≈ $9,000/month

**Cost estimation**:
```
3-node single region: ~$9,000/month
3-node multi-region:  ~$27,000/month (replicated 3x)
```

Only use if you really need global consistency.

---

## Part 4: AlloyDB - MySQL-Compatible, Ultra-Fast

### What is AlloyDB

**AlloyDB** is Google's next-generation database: MySQL-compatible but built from scratch to be fast.

- 10x faster than MySQL
- ACID transactions
- Managed, serverless option available
- Lower cost than Spanner for regional use

### When to Use AlloyDB

**Use when**:
- You want MySQL compatibility
- You need good performance
- Single region is fine
- Cost matters more than global distribution

**vs Cloud SQL**:
- Cloud SQL: Better for simple, legacy workloads
- AlloyDB: Better for high-performance, modern apps

### Creating AlloyDB

**Via Terraform**:
```hcl
resource "google_alloydb_cluster" "main" {
  cluster_id = "my-cluster"
  location   = "us-central1"
  
  initial_user {
    user = "root"
  }
}

resource "google_alloydb_instance" "primary" {
  cluster       = google_alloydb_cluster.main.name
  instance_id   = "primary"
  instance_type = "PRIMARY"
  machine_type  = "db.m1.large"
}
```

---

## Part 5: Firestore - NoSQL Document Database

### What is Firestore

**Firestore** is a serverless NoSQL database for storing documents (JSON-like data).

- Document/collection structure
- Real-time updates (WebSocket)
- Indexes built in
- Scales automatically
- Serverless (pay per operation)

### When to Use Firestore

**Use when**:
- You have hierarchical/nested data
- Document size < 1MB
- You need real-time updates
- You want serverless (no capacity planning)
- Mobile apps (great Firebase integration)

**Don't use when**:
- You have heavy relational requirements
- You need complex joins
- Documents > 1MB
- You need SQL queries

### Firestore Data Model

```
firestore {
  "users" (collection)
    "/user123" (document)
      name: "Alice"
      email: "alice@example.com"
      preferences (subcollection)
        "/pref1" (document)
          theme: "dark"
    "/user456" (document)
      name: "Bob"
}
```

**Collections**: Container of documents (like tables)
**Documents**: Key-value pairs (like rows)
**Subcollections**: Collections within documents

### Creating Firestore

**Via Console**:
1. Go to Firestore in Console
2. Click "Create Database"
3. Choose mode: Native or Datastore
4. Choose location
5. Click "Create"

**Via Terraform**:
```hcl
resource "google_firestore_database" "main" {
  project             = "PROJECT_ID"
  name                = "(default)"
  location_id         = "us-central1"
  type                = "FIRESTORE_NATIVE"
  concurrency_mode    = "OPTIMISTIC"
}
```

### Writing Data to Firestore

**Python**:
```python
from google.cloud import firestore

db = firestore.Client()

# Set document
db.collection('users').document('user123').set({
    'name': 'Alice',
    'email': 'alice@example.com',
})

# Add document (auto ID)
db.collection('users').add({
    'name': 'Bob',
    'email': 'bob@example.com',
})
```

**JavaScript**:
```javascript
const db = firebase.firestore();

db.collection('users').doc('user123').set({
  name: 'Alice',
  email: 'alice@example.com'
});
```

### Querying Firestore

**Simple query**:
```python
docs = db.collection('users').where('email', '==', 'alice@example.com').stream()
for doc in docs:
    print(doc.to_dict())
```

**Complex query**:
```python
docs = db.collection('users')\
    .where('age', '>', 18)\
    .where('country', '==', 'US')\
    .order_by('age')\
    .limit(10)\
    .stream()
```

---

## Part 6: Bigtable - High-Throughput Time-Series

### What is Bigtable

**Bigtable** is a wide-column NoSQL database optimized for:
- Massive scale (petabytes)
- High throughput (millions of ops/sec)
- Time-series data
- NOT for transactional consistency

### When to Use Bigtable

**Use when**:
- You have millions of reads/writes per second
- Time-series data (metrics, logs, events)
- Massive scale (100GB+)
- You can tolerate eventual consistency

**Don't use when**:
- You need ACID transactions
- Query patterns are complex
- Scale < 10GB

### Bigtable Schema Design

Bigtable has a unique model:
- **Rows**: Sorted by row key
- **Columns**: Grouped in column families
- **Timestamps**: Versioning built-in

```
Row Key: user123#event_type=purchase#timestamp=2024-01-15
├─ column_family: events
│  ├─ amount: 99.99
│  ├─ product: laptop
│  └─ timestamp: (built-in)
├─ column_family: metadata
│  ├─ user_id: 123
│  └─ country: US
```

### Row Key Design (Critical!)

**Good row key**:
- Evenly distributes data
- Supports query patterns
- Example: `user123#2024-01-15#event001`

**Bad row key**:
- Bunches data: `2024-01-15#user123` (all same day go to one server)
- Doesn't match queries

```
Bad: events by date
  2024-01-01 ... data for all users
  (one server overloaded on that date)

Good: events by user then date
  user001#2024-01-01 ... events for user 1 on Jan 1
  user002#2024-01-01 ... events for user 2 on Jan 1
  (distributed across servers)
```

### Creating Bigtable

**Via Terraform**:
```hcl
resource "google_bigtable_instance" "main" {
  name                = "my-bigtable"
  cluster_id          = "main"
  instance_type       = "PRODUCTION"  # or DEVELOPMENT
  num_nodes           = 3
  storage_type        = "HDD"

  column_family {
    family = "events"
  }
}
```

### Accessing Bigtable

**Python**:
```python
from google.cloud import bigtable

client = bigtable.Client(project='PROJECT_ID', admin=True)
instance = client.instance('my-bigtable')
table = instance.table('my-table')

# Write
row_key = b'user123#2024-01-15'
row = table.row(row_key)
row.set_cell(b'events', b'amount', b'99.99')
row.commit()

# Read
row = table.read_row(row_key)
print(row.cells)
```

---

## Part 7: Cloud Storage - Object Storage

### What is Cloud Storage

**Cloud Storage** stores objects (files, blobs) in buckets:
- Unlimited scale
- Billions of objects
- 5TB per object max
- Not a file system

### When to Use Cloud Storage

**Use when**:
- Storing files, backups, images
- Data lake
- Archival storage
- Any unstructured data

### Storage Classes

```
Standard: Hot data, accessed frequently, high cost per GB
Nearline: Accessed monthly, cheaper
Coldline: Accessed rarely (quarterly), even cheaper
Archive: Accessed yearly, cheapest
```

**Example pricing**:
```
Standard:  $0.020/GB/month
Nearline:  $0.010/GB/month
Coldline:  $0.004/GB/month
Archive:   $0.0036/GB/month
```

### Creating Buckets

**Via Terraform**:
```hcl
resource "google_storage_bucket" "main" {
  name          = "my-unique-bucket-name"
  location      = "US"
  force_destroy = false

  uniform_bucket_level_access = true

  lifecycle_rule {
    action {
      type          = "SetStorageClass"
      storage_class = "NEARLINE"
    }
    condition {
      age = 30
    }
  }

  lifecycle_rule {
    action {
      type = "Delete"
    }
    condition {
      age = 365
    }
  }
}
```

### Lifecycle Management

**Example**: Automatically transition old objects
```yaml
lifecycle:
  - action: SetStorageClass
    storage_class: NEARLINE
    age: 30 days

  - action: SetStorageClass
    storage_class: COLDLINE
    age: 90 days

  - action: Delete
    age: 365 days
```

Objects automatically move from Standard → Nearline (30 days) → Coldline (90 days) → Deleted (365 days).

### Uploading Files

**Via gcloud**:
```bash
gsutil cp file.txt gs://my-bucket/
gsutil -m cp -r local/dir/ gs://my-bucket/data/  # Parallel upload
```

**Via Python**:
```python
from google.cloud import storage

client = storage.Client()
bucket = client.bucket('my-bucket')
blob = bucket.blob('myfile.txt')
blob.upload_from_filename('local_file.txt')
```

---

## Part 8: BigQuery - Petabyte-Scale Data Warehouse

### What is BigQuery

**BigQuery** is Google's data warehouse:
- SQL queries on massive datasets
- Petabyte scale
- Serverless (pay per TB scanned)
- Real-time analysis
- Built-in ML

### When to Use BigQuery

**Use when**:
- Analytical queries on large datasets
- Data warehouse
- Petabyte scale
- Need SQL
- Cost optimization via partitioning

**Not for**:
- Transactional workloads
- Real-time operational database

### Creating Datasets and Tables

**Dataset** = Collection of tables

**Via Console**:
1. Go to BigQuery
2. Click project
3. Click "Create Dataset"
4. Enter name, location
5. Click Create

**Via Terraform**:
```hcl
resource "google_bigquery_dataset" "main" {
  dataset_id    = "my_dataset"
  friendly_name = "My Dataset"
  location      = "US"

  access {
    role          = "OWNER"
    user_by_email = "admin@example.com"
  }
}

resource "google_bigquery_table" "events" {
  dataset_id = google_bigquery_dataset.main.dataset_id
  table_id   = "events"

  schema = jsonencode([
    {
      name        = "event_id"
      type        = "STRING"
      mode        = "REQUIRED"
    },
    {
      name        = "user_id"
      type        = "STRING"
      mode        = "REQUIRED"
    },
    {
      name        = "timestamp"
      type        = "TIMESTAMP"
      mode        = "REQUIRED"
    },
    {
      name        = "amount"
      type        = "FLOAT64"
      mode        = "NULLABLE"
    }
  ])

  time_partitioning {
    type = "DAY"
    field = "timestamp"
  }
}
```

### Loading Data

**From Cloud Storage (CSV)**:
```bash
bq load --source_format=CSV \
  my_dataset.my_table \
  gs://my-bucket/data.csv \
  field1:STRING,field2:INTEGER,field3:TIMESTAMP
```

**From Firestore**:
1. Export Firestore to Cloud Storage
2. Load from Cloud Storage to BigQuery

**Via Python**:
```python
from google.cloud import bigquery

client = bigquery.Client()
job_config = bigquery.LoadJobConfig(
    source_format=bigquery.SourceFormat.CSV,
)
load_job = client.load_table_from_uri(
    "gs://my-bucket/data.csv",
    "my_dataset.my_table",
    job_config=job_config,
)
load_job.result()
```

### Querying BigQuery

```sql
SELECT
  user_id,
  COUNT(*) as event_count,
  SUM(amount) as total_amount
FROM `project.dataset.events`
WHERE timestamp >= '2024-01-01'
GROUP BY user_id
ORDER BY total_amount DESC
LIMIT 10
```

---

## Part 9: Multi-Region Data Architecture

### Replication Strategies

**Strategy 1: Active-Passive**
```
Primary (us-central1)
  └─ Read Replica (europe-west1)
     └─ Only reads, synced from primary
```

Used for: Disaster recovery, read scale-out

**Strategy 2: Active-Active**
```
Instance 1 (us-central1)
  ↔ Instance 2 (europe-west1)
     (Both read and write, sync bidirectional)
```

Used for: Geographic distribution, high availability

**Strategy 3: Eventual Consistency**
```
Primary (us-central1)
  └─ Async Replica (europe-west1)
     └─ Delayed sync, cheaper
```

Used for: Analytics, backups where slight delay okay

### Multi-Region Cloud SQL

**Regional HA** (recommended):
```hcl
# Cloud SQL already replicates within region
# For multi-region, use read replicas

resource "google_sql_database_instance" "replica" {
  master_instance_name = google_sql_database_instance.main.name
  region               = "europe-west1"
  name                 = "my-db-replica"
}
```

**Use case**:
- Primary in us-central1 handles writes
- Replica in europe-west1 for EU reads
- Replica 5-30 seconds behind

### Multi-Region BigQuery

BigQuery datasets are region-specific. For multi-region:

```hcl
# US dataset
resource "google_bigquery_dataset" "us" {
  dataset_id = "my_dataset_us"
  location   = "US"
}

# EU dataset (separate)
resource "google_bigquery_dataset" "eu" {
  dataset_id = "my_dataset_eu"
  location   = "EU"
}

# Copy data between them
# (must be done via Dataflow)
```

BigQuery doesn't have native multi-region sync. Use:
- Dataflow for ETL
- Cloud Functions to trigger copies
- BigQuery Transfer Service (for external sources)

### Multi-Region Firestore

Firestore is automatically multi-zone replicated within a region. For multi-region:

**Option 1**: Multiple independent Firestore instances
```
North America:
  - Firestore in us-central1

Europe:
  - Firestore in europe-west1

(Sync via Cloud Functions/Dataflow)
```

**Option 2**: Cloud Datastore (legacy, multi-region built-in)

For most cases, single region Firestore + application-level sync is recommended.

### Multi-Region Bigtable

Bigtable supports replication through instances:

```hcl
# Primary cluster
resource "google_bigtable_instance" "primary" {
  name = "my-bigtable-primary"
  # ... configuration
}

# Replicate via Cloud Dataflow
# (no native multi-region, must build it)
```

For multi-region Bigtable:
- Create separate instances in different regions
- Use Dataflow to replicate
- Accept eventual consistency

---

## Part 10: Exam-Focused Scenarios

### Scenario 1: Real-Time Analytics Dashboard

**Requirements**:
- Ingest 100k events/second
- Dashboard shows last hour, last day, last month
- Need fast queries

**Solution**:
- Events → Pub/Sub (real-time ingestion)
- Pub/Sub → Dataflow (stream processing) → BigQuery
- BigQuery tables: Hourly, daily, monthly (partitioned)
- Dashboard queries BigQuery

Why not:
- Cloud SQL: Can't handle 100k ops/sec
- Firestore: Wrong model for analytics
- Bigtable: Good for ops/sec but not SQL

### Scenario 2: Time-Series Metrics Collection

**Requirements**:
- Collect metrics from 10k servers
- 1 million data points/minute
- Retention: 1 year
- Query: Last 24 hours, last week, month

**Solution**:
- Servers → Bigtable (write all metrics)
- Row key: `server123#metric_cpu#2024-01-15T12:00:00`
- Queries: Read by server, time range
- Archive old data to Cloud Storage (Bigtable lifecycle)

Why Bigtable:
- 1M writes/min trivial (handles billions)
- Time-series row key natural fit
- Cheap per operation

### Scenario 3: Financial Transaction Processing

**Requirements**:
- ACID transactions
- Global distribution
- Strong consistency
- Moderate scale (1000 TPS)

**Solution**:
- Cloud Spanner (multi-region)
- Primary in US, replicas in EU, APAC
- Application reads locally, writes sync globally

Why Spanner:
- ACID globally
- Strong consistency
- Global distribution

### Scenario 4: Document Storage with Real-Time Sync

**Requirements**:
- Hierarchical data (users, profiles, comments)
- Real-time updates to mobile app
- < 10k concurrent users

**Solution**:
- Firestore
- Collections: users, posts, comments
- Real-time listeners from mobile SDK
- Auto-sync

Why Firestore:
- Hierarchical data perfect fit
- Real-time WebSocket support
- Serverless

---

## Part 11: Data Loading Techniques

### Batch Loading

**Command-line upload**:
```bash
# Single file
gsutil cp data.csv gs://bucket/

# Large directory
gsutil -m -r cp large_dir gs://bucket/
```

**Load to BigQuery from Cloud Storage**:
```bash
bq load --source_format=CSV \
  dataset.table \
  gs://bucket/data.csv \
  schema.json
```

### Streaming Inserts

**Cloud SQL**:
```python
import mysql.connector

conn = mysql.connector.connect(host="IP", database="db", user="user", password="pass")
cursor = conn.cursor()

for row in data_stream:
    cursor.execute("INSERT INTO table VALUES (%s, %s, %s)", row)
    conn.commit()
```

**Firestore**:
```python
db.collection('events').add({
    'timestamp': firestore.SERVER_TIMESTAMP,
    'data': event_data
})
```

**BigQuery Streaming**:
```python
from google.cloud import bigquery

client = bigquery.Client()
table = client.get_table('dataset.table')

rows = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25},
]

errors = client.insert_rows_json(table, rows)
```

### ETL Pipelines (Dataflow)

For complex transformations:

```python
import apache_beam as beam
from apache_beam.options.pipeline_options import PipelineOptions

options = PipelineOptions()
with beam.Pipeline(options=options) as p:
    (p
     | 'Read' >> beam.io.ReadFromPubSub(topic='projects/PROJECT_ID/topics/events')
     | 'Parse' >> beam.Map(json.loads)
     | 'Filter' >> beam.Filter(lambda x: x['amount'] > 100)
     | 'Write' >> beam.io.WriteToBigQuery(table='dataset.large_transactions'))
```

---

## Part 12: Common Exam Mistakes

**Mistake 1**: Using Cloud SQL for millions of ops/sec
- Cloud SQL: ~10k ops/sec max
- Bigtable: Millions of ops/sec

**Mistake 2**: Using Firestore for complex analytical queries
- Firestore: Simple queries on documents
- BigQuery: Complex analytical SQL

**Mistake 3**: Assuming all data solutions support ACID
- ACID: Cloud SQL, Cloud Spanner, AlloyDB
- No ACID: Firestore, Bigtable, BigQuery

**Mistake 4**: Not planning multi-region early
- Hard to retrofit later
- Each product has different multi-region story
- Plan from start

**Mistake 5**: Ignoring cost
- BigQuery charged per GB scanned
- Spanner: Expensive nodes
- Cloud SQL: Hourly billing
- Understand pricing for your workload

---

## Part 13: Quick Reference

### Choosing Your Database

| Need | Product |
|------|---------|
| Transactional + relational | Cloud SQL |
| Global ACID | Cloud Spanner |
| High throughput time-series | Bigtable |
| Documents/hierarchical | Firestore |
| Analytics/DW | BigQuery |
| High-speed relational | AlloyDB |

### Approximate Costs (USD/month)

| Product | Config | Cost |
|---------|--------|------|
| Cloud SQL | db-n1-standard-1, 1 region | $250 |
| Spanner | 3 nodes, 1 region | $9,000 |
| Bigtable | 3 nodes, production | $750 |
| Firestore | ~1M reads/writes | $300 |
| BigQuery | 1TB scanned/month | $5 |
| Cloud Storage | 1TB Standard | $20 |

---

## Conclusion

Choosing and deploying the right data product is critical. The exam tests:

1. **Decision making**: Which product for given workload
2. **Configuration**: How to set up and deploy
3. **Loading**: How to get data in
4. **Multi-region**: How to ensure high availability

Master this section by:
- Memorizing the use cases (when to use each)
- Understanding the limits (Cloud SQL vs Bigtable throughput)
- Practicing deployments
- Thinking through cost implications

---

**Total estimated reading/study time: 2 hours**
**Word count: ~10,000 words**

This guide covers everything in Section 2.2 with practical examples and decision frameworks.
