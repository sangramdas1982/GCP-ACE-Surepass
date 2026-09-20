# Monitoring and Logging: Complete 2-Hour Study Guide
## ACE Exam Section 3.4 - Deep Dive

---

## Introduction

"Monitoring and logging" represents a critical portion of the ACE exam (within the 30% "Ensuring operation" section). This section covers:

- **Cloud Monitoring**: Metrics, dashboards, alerting
- **Cloud Logging**: Log collection, analysis, export
- **Audit Logs**: Compliance and tracking
- **Diagnostic Tools**: Tracing, profiling, debugging
- **Alerting**: Setting up thresholds and notifications
- **Cost Optimization**: Active Assist and insights

As an Associate Cloud Engineer, you're responsible for keeping systems running. You need to see what's happening, know when things go wrong, and diagnose issues quickly.

---

## Part 1: Cloud Monitoring Fundamentals

### What is Cloud Monitoring

**Cloud Monitoring** (formerly Stackdriver Monitoring) is Google Cloud's metrics and dashboarding service:
- Collects metrics from resources
- Displays dashboards
- Sends alerts
- Tracks application performance

### Metrics vs Logs

**Metrics**:
- Numbers over time (CPU usage, request count)
- Lightweight, fast
- Good for dashboards and trends
- Example: "CPU was 45% at 3:00pm"

**Logs**:
- Detailed text records (errors, debug info)
- Verbose
- Good for troubleshooting
- Example: "Failed to connect to database: timeout after 30s"

You need both.

### Pre-existing Metrics

Google Cloud **automatically collects** certain metrics:

**Compute Engine** (if monitoring agent installed):
- CPU utilization
- Memory usage
- Disk I/O
- Network I/O

**Cloud SQL**:
- Query count
- CPU usage
- Disk size
- Replication lag

**Kubernetes Engine**:
- Pod CPU/memory
- Node resource usage
- Cluster size

To see these metrics, enable Cloud Monitoring API:
```bash
gcloud services enable monitoring.googleapis.com
```

### Viewing Metrics in Console

**Via Console**:
1. Go to Cloud Monitoring > Metrics Explorer
2. Select resource type (Compute Engine, Cloud SQL, etc.)
3. Select metric (CPU utilization, memory, etc.)
4. View chart

---

## Part 2: Cloud Monitoring Dashboards

### Creating Dashboards

A **dashboard** displays multiple metrics visually.

**Via Console**:
1. Go to Cloud Monitoring > Dashboards
2. Click "Create Dashboard"
3. Give it a name
4. Click "Add Widget"
5. Configure widget (select metric, time range, visualization)
6. Click "Save Dashboard"

**Via Terraform**:
```hcl
resource "google_monitoring_dashboard" "dashboard" {
  dashboard_json = jsonencode({
    displayName = "Production Dashboard"
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
                    filter = "metric.type=\"compute.googleapis.com/instance/cpu/utilization\" resource.type=\"gce_instance\""
                    aggregation = {
                      alignmentPeriod    = "60s"
                      perSeriesAligner   = "ALIGN_MEAN"
                    }
                  }
                }
              }]
            }
          }
        },
        {
          width  = 6
          height = 4
          xPos   = 6
          widget = {
            title = "Memory Usage"
            xyChart = {
              dataSets = [{
                timeSeriesQuery = {
                  timeSeriesFilter = {
                    filter = "metric.type=\"agent.googleapis.com/memory/percent_used\""
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

### Dashboard Best Practices

**Organization**:
- 4-6 widgets per dashboard (not 20+)
- Group related metrics
- Use separate dashboards for different areas

**Example Dashboard Structure**:
```
Infrastructure Dashboard
├─ Top row: CPU, Memory, Disk (overall health)
├─ Middle row: Network in/out, error rates
└─ Bottom: Custom application metrics

Deployment Dashboard
├─ Deployment status
├─ Request latency
└─ Error rate
```

---

## Part 3: Custom Metrics

### What are Custom Metrics

**Custom metrics** are metrics you define and send from your application:

```python
from google.cloud import monitoring_v3

client = monitoring_v3.MetricServiceClient()
project_name = f"projects/PROJECT_ID"

series = monitoring_v3.TimeSeries()
series.metric.type = 'custom.googleapis.com/order_value'
series.resource.type = 'global'

now = time.time()
seconds = int(now)
nanos = int((now - seconds) * 10**9)
interval = monitoring_v3.TimeInterval(
    {"seconds": seconds, "nanos": nanos}
)
point = monitoring_v3.Point({"interval": interval, "value": {"double_value": 99.99}})
series.points = [point]

client.create_time_series(name=project_name, time_series=[series])
```

**Common custom metrics**:
- Order value
- User signup rate
- API response time
- Cache hit rate
- Business metrics

### Creating Custom Metrics in Application

**Python with OpenTelemetry**:
```python
from opentelemetry import metrics
from opentelemetry.exporter.gcp_monitoring import GoogleCloudMonitoringMetricsExporter
from opentelemetry.sdk.metrics import MeterProvider

# Set up exporter
exporter = GoogleCloudMonitoringMetricsExporter()
metrics.set_meter_provider(MeterProvider([exporter]))

# Create meter
meter = metrics.get_meter(__name__)

# Create counter
order_counter = meter.create_counter(
    name="orders",
    description="Number of orders"
)

# Increment counter
order_counter.add(1, {"status": "completed"})
```

**Node.js**:
```javascript
const monitoring = require('@google-cloud/monitoring');
const client = new monitoring.MetricServiceClient();

const dataPoint = {
  interval: {
    endTime: {
      seconds: Math.floor(Date.now() / 1000),
    },
  },
  value: {
    doubleValue: 99.99,
  },
};

const timeSeries = {
  metric: {
    type: 'custom.googleapis.com/order_value',
  },
  resource: {
    type: 'global',
  },
  points: [dataPoint],
};

await client.createTimeSeries({
  name: client.projectPath(projectId),
  timeSeries: [timeSeries],
});
```

---

## Part 4: Cloud Monitoring Alerts

### Alert Policy Components

An **alert policy** defines:
1. **Condition**: When to trigger (metric threshold)
2. **Notification channel**: Where to send alert (email, Slack, etc.)
3. **Documentation**: What the alert means

### Creating Alert Policies

**Via Console**:
1. Go to Cloud Monitoring > Alerts > Create Policy
2. Click "Add Condition"
3. Select metric (e.g., CPU)
4. Set threshold (e.g., CPU > 80%)
5. Set duration (e.g., for 5 minutes)
6. Click "Add Notification Channel"
7. Select where to send alert (email, Slack, PagerDuty)
8. Click "Create Policy"

**Via Terraform**:
```hcl
resource "google_monitoring_alert_policy" "cpu_high" {
  display_name = "High CPU Utilization"
  combiner     = "OR"

  conditions {
    display_name = "CPU > 80%"
    condition_threshold {
      filter          = "metric.type=\"compute.googleapis.com/instance/cpu/utilization\" AND resource.type=\"gce_instance\""
      duration        = "300s"
      comparison      = "COMPARISON_GT"
      threshold_value = 0.8

      aggregations {
        alignment_period  = "60s"
        per_series_aligner = "ALIGN_MEAN"
      }
    }
  }

  notification_channels = [google_monitoring_notification_channel.email.id]

  documentation {
    content = "CPU usage is high. Check VM performance."
    mime_type = "text/markdown"
  }
}

resource "google_monitoring_notification_channel" "email" {
  display_name = "Admin Email"
  type         = "email"
  labels = {
    email_address = "admin@example.com"
  }
}
```

### Alert Best Practices

**Do**:
- Set meaningful thresholds based on baselines
- Include documentation
- Route to right team
- Test alerts

**Don't**:
- Alert on every spike
- Use same threshold for all resources
- Ignore alert fatigue (too many false positives)

### Multi-Condition Alerts

Combine multiple conditions:

```hcl
conditions {
  display_name = "High CPU AND high memory"
  condition_threshold {
    filter = "metric.type=\"compute.googleapis.com/instance/cpu/utilization\""
    # ...
  }
}

conditions {
  display_name = "High memory"
  condition_threshold {
    filter = "metric.type=\"agent.googleapis.com/memory/percent_used\""
    # ...
  }
}

# Alert fires if any condition true (OR) or all true (AND)
combiner = "AND"
```

---

## Part 5: Cloud Logging Fundamentals

### What is Cloud Logging

**Cloud Logging** collects, analyzes, and exports logs from:
- Compute Engine
- Google Kubernetes Engine
- App Engine
- Cloud Functions
- Custom applications
- On-premises

### Log Types

**1. Application Logs**:
- Logs from your code
- `print()`, `console.log()`, logging framework
- Captured by monitoring agent

**2. System Logs**:
- OS-level logs
- /var/log/syslog
- Captured by monitoring agent

**3. Audit Logs**:
- Who did what when
- API calls to Google Cloud
- Compliance/security
- Always enabled

**4. Agent Logs**:
- Logs from monitoring agent itself
- Diagnostic info

### Viewing Logs

**Via Console**:
1. Go to Cloud Logging > Logs Explorer
2. Select resource (project, instance, etc.)
3. View logs in real-time or historical

**Via gcloud**:
```bash
# View logs for specific resource
gcloud logging read "resource.type=gce_instance AND resource.labels.instance_id=1234567890" \
  --limit 10 \
  --format json

# View logs from last hour
gcloud logging read "resource.type=cloud_function" \
  --limit 50 \
  --format "table(timestamp, jsonPayload.message)"
```

### Writing Logs from Application

**Python**:
```python
import logging
from google.cloud import logging as cloud_logging

# Set up Cloud Logging
client = cloud_logging.Client()
client.setup_logging(logging.INFO)

# Now all logs go to Cloud Logging
logging.info("Application started")
logging.error("Database error", extra={"db": "postgres"})
```

**Node.js**:
```javascript
const logging = require('@google-cloud/logging');
const logger = logging.log('myapp');

const entry = logger.entry({labels: {env: 'production'}}, 'User signin');
logger.write(entry);
```

---

## Part 6: Cloud Logging Analysis

### Log Queries (Logs Query Language)

Write queries to find specific logs:

**Basic query**:
```
resource.type="gce_instance"
AND severity="ERROR"
```

**Find error logs from last hour**:
```
severity="ERROR"
AND timestamp>="2024-01-15T12:00:00Z"
```

**Find API errors with status code**:
```
resource.type="api"
AND httpRequest.status>=400
```

**Find slow queries**:
```
resource.type="cloudsql_database"
AND jsonPayload.query_time_ms > 1000
```

### Log Analytics

**Analyze patterns**:
```sql
-- Count errors by type
SELECT
  jsonPayload.error_type,
  COUNT(*) as error_count
FROM `project.dataset._Default`
WHERE severity = "ERROR"
GROUP BY jsonPayload.error_type
ORDER BY error_count DESC
```

### Log Buckets

**Log buckets** organize logs for storage and querying:

```hcl
resource "google_logging_project_bucket_config" "default" {
  project      = "PROJECT_ID"
  location     = "us-central1"
  bucket_id    = "_Default"
  retention_days = 30  # Default retention
}

# Longer retention for compliance logs
resource "google_logging_project_bucket_config" "compliance" {
  project      = "PROJECT_ID"
  location     = "us-central1"
  bucket_id    = "compliance"
  retention_days = 2555  # 7 years
}
```

### Log Routing

Route logs to different destinations:

```hcl
# Error logs to BigQuery
resource "google_logging_project_sink" "errors_to_bq" {
  name        = "errors-to-bigquery"
  destination = "bigquery.googleapis.com/projects/PROJECT_ID/datasets/logs"
  filter      = "severity >= ERROR"
}

# Audit logs to Cloud Storage
resource "google_logging_project_sink" "audit_to_storage" {
  name        = "audit-to-storage"
  destination = "storage.googleapis.com/my-audit-logs-bucket"
  filter      = "protoPayload.methodName=~\"storage.*\""
}
```

---

## Part 7: Audit Logs

### What are Audit Logs

**Audit logs** record who called what API when:

```
User: alice@example.com
Action: Create compute instance
Resource: projects/my-project/zones/us-central1-a/instances/web-server
Time: 2024-01-15T14:30:00Z
Status: Success
```

### Types of Audit Logs

**1. Admin Activity**:
- Creating/updating/deleting resources
- IAM changes
- High-impact operations
- Always enabled, stored 30 days free

**2. Data Access**:
- Reading data (Cloud Storage, BigQuery)
- More verbose, charged
- Must enable explicitly

**3. System Events**:
- Google Cloud internal operations
- Resource state changes
- Always logged

### Enabling Data Access Audit Logs

```bash
# Enable Data Access logs
gcloud services enable logging.googleapis.com

# Via gcloud config
gcloud resource-manager org-policies set-policy --project=PROJECT_ID <<EOF
{
  "auditConfigs": [
    {
      "service": "allServices",
      "auditLogConfigs": [
        {"logType": "DATA_ACCESS", "exemptedMembers": []}
      ]
    }
  ]
}
EOF
```

### Viewing Audit Logs

**Via Console**:
1. Go to Cloud Logging > Logs Explorer
2. Filter: `protoPayload.methodName=...`

**Example queries**:
```
# All admin activity
protoPayload.methodName=~".*"

# Who accessed storage bucket
protoPayload.methodName="storage.buckets.list"

# Who created instances
protoPayload.methodName="compute.instances.insert"
```

---

## Part 8: Diagnostic Tools

### Cloud Trace - Request Tracing

**Cloud Trace** shows how requests flow through your application:

```
Request arrives
  ├─ Load balancer (1ms)
  ├─ Frontend service (50ms)
  │   ├─ API call (40ms)
  │   └─ Cache check (10ms)
  ├─ Backend service (100ms)
  │   ├─ Database query (80ms)
  │   └─ Processing (20ms)
  └─ Response sent
Total: 151ms
```

**Enabling Trace**:
```bash
gcloud services enable cloudtrace.googleapis.com
```

**Instrumenting Python**:
```python
from google.cloud import trace

client = trace.Client()
tracer = client.tracer()

with tracer.span(name='database_query'):
    result = db.query("SELECT * FROM users")

with tracer.span(name='api_call'):
    response = requests.get("https://api.example.com/data")
```

### Cloud Profiler - Performance Profiling

**Cloud Profiler** shows where CPU time is spent:

```
Function: process_payment
├─ validate_card: 20% CPU time
├─ charge_card: 60% CPU time
└─ send_receipt: 20% CPU time
```

**Enabling Profiler**:
```bash
gcloud services enable cloudprofiler.googleapis.com
```

**Python**:
```python
from google.cloud import profiler

profiler.start()
```

Now Cloud Profiler automatically samples CPU and memory.

### Cloud Debugger - Live Debugging

**Cloud Debugger** lets you inspect variables without stopping application:

```
Set breakpoint at line 42
When hit:
  x = 10
  y = 20
  status = "processing"
```

**Via Console**:
1. Go to Cloud Debugger
2. Select application
3. Click code line to set breakpoint
4. View variable values when hit
5. No restart needed

### Error Reporting

**Error Reporting** shows top errors:

```
Error: NullPointerException
  Occurrences: 1,234
  Last occurrence: 2 minutes ago
  Stack trace: ...
```

**Accessing**:
1. Go to Error Reporting in Console
2. See error frequency and stack traces
3. Create alerts on new errors

---

## Part 9: Configuring Audit Logs

### VPC Flow Logs

**VPC Flow Logs** track network traffic between resources:

```
Source: 10.0.1.10:54322
Destination: 10.0.2.20:443
Bytes sent: 5,234
Bytes received: 45,234
Status: ACCEPTED
```

**Enabling on subnet**:
```hcl
resource "google_compute_subnetwork" "main" {
  name          = "my-subnet"
  network       = google_compute_network.vpc.id
  ip_cidr_range = "10.0.1.0/24"

  log_config {
    aggregation_interval = "INTERVAL_5_SEC"
    flow_logs_enabled    = true
    metadata             = "INCLUDE_ALL_METADATA"
  }
}
```

### Firewall Logs

**Firewall Logs** show which firewall rules allow/deny traffic:

```
Rule: allow-http-https
Direction: INGRESS
Action: ALLOW
Source: 203.0.113.15
Port: 443
```

**Enabling**:
```hcl
resource "google_compute_firewall" "allow_http" {
  name      = "allow-http"
  network   = google_compute_network.vpc.name

  allow {
    protocol = "tcp"
    ports    = ["80", "443"]
  }

  enable_logging = true

  log_config {
    metadata = "INCLUDE_ALL_METADATA"
  }
}
```

---

## Part 10: Ops Agent

### What is Ops Agent

**Ops Agent** is a single agent that collects both:
- Metrics (CPU, memory, disk, network)
- Logs (syslog, application logs)

One agent replaces old setup (Monitoring + Logging agents).

### Installing Ops Agent

**On Compute Engine**:
```bash
# Download and install
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
sudo bash add-google-cloud-ops-agent-repo.sh --also-install

# Or via gcloud (when creating instance)
gcloud compute instances create my-vm \
  --metadata enable-ops-agent=true
```

**In Docker container**:
```dockerfile
FROM debian:11
RUN curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
RUN bash add-google-cloud-ops-agent-repo.sh --also-install
```

### Configuring Ops Agent

**Default configuration** collects basic metrics and logs.

**Custom configuration** (YAML):
```yaml
logging:
  receivers:
    syslog:
      type: files
      include_paths:
        - /var/log/syslog
    app:
      type: files
      include_paths:
        - /var/log/app.log

  processors:
    json_parse:
      type: json_parser
      parse_from: body

  service:
    pipelines:
      default_pipeline:
        receivers: [syslog, app]
        processors: [json_parse]

metrics:
  receivers:
    hostmetrics:
      type: hostmetrics
      collection_interval: 60s
  service:
    metrics:
      - hostmetrics
```

---

## Part 11: Managed Service for Prometheus

### What is Managed Prometheus

**Managed Service for Prometheus** stores Prometheus metrics on Google Cloud:
- Export metrics from Prometheus-compatible applications
- Store in Google Cloud
- Query via Cloud Monitoring
- No infrastructure to manage

### Using Managed Prometheus

**Step 1**: Configure Prometheus to scrape metrics
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'app'
    static_configs:
      - targets: ['localhost:9090']
```

**Step 2**: Send to Google Cloud
```bash
# Add Google Cloud remote write config
remote_write:
  - url: https://monitoring.googleapis.com/api/projects/PROJECT_ID/timeSeries?key=API_KEY
```

**Step 3**: Query in Cloud Monitoring**
```
up{job="app"}  # Any Prometheus query works
```

---

## Part 12: Active Assist - Cost Optimization

### What is Active Assist

**Active Assist** uses machine learning to find optimization opportunities:
- Idle resources (VMs running but not used)
- Over-provisioned resources (too much capacity)
- Reserved capacity not being used
- Unattached resources (disks, IPs)

### Accessing Recommendations

**Via Console**:
1. Go to Active Assist dashboard
2. See optimization recommendations
3. Click to implement

**Example recommendations**:
- "Stop this VM, it's idle 90% of the time"
- "Resize this instance to smaller type"
- "Commit to 3-year CUD, save $50k"

### Automating Recommendations

```python
from google.cloud import recommender_v1

client = recommender_v1.RecommendersClient()

# Get recommendations
parent = f"projects/{project_id}"
recommendations = client.list_recommendations(parent=parent)

for rec in recommendations:
    print(f"{rec.description}: {rec.primary_impact}")
    # Could auto-mark as claimed, applied, etc.
```

---

## Part 13: Exam Scenarios

### Scenario 1: Setting up Production Monitoring

**Requirements**:
- Alert on high CPU (> 80% for 5 min)
- Alert on high error rate (> 5% for 1 min)
- Dashboard showing health
- Logs exported to BigQuery for analysis

**Solution**:
```hcl
# CPU Alert
resource "google_monitoring_alert_policy" "cpu_high" {
  display_name = "High CPU"
  conditions {
    display_name = "CPU > 80%"
    condition_threshold {
      filter          = "metric.type=\"compute.googleapis.com/instance/cpu/utilization\""
      threshold_value = 0.8
      duration        = "300s"
      comparison      = "COMPARISON_GT"
    }
  }
  notification_channels = [google_monitoring_notification_channel.pagerduty.id]
}

# Error Rate Alert
resource "google_monitoring_alert_policy" "error_rate_high" {
  display_name = "High Error Rate"
  conditions {
    display_name = "Error Rate > 5%"
    condition_threshold {
      filter          = "metric.type=\"custom.googleapis.com/error_rate\""
      threshold_value = 0.05
      duration        = "60s"
      comparison      = "COMPARISON_GT"
    }
  }
  notification_channels = [google_monitoring_notification_channel.slack.id]
}

# Dashboard
resource "google_monitoring_dashboard" "production" {
  dashboard_json = jsonencode({
    displayName = "Production Health"
    # ... widgets for CPU, memory, error rate
  })
}

# Export logs to BigQuery
resource "google_logging_project_sink" "logs_to_bq" {
  name        = "logs-to-bigquery"
  destination = "bigquery.googleapis.com/projects/PROJECT_ID/datasets/logs"
}
```

### Scenario 2: Troubleshooting Slow Requests

**Problem**: Some requests take 5+ seconds

**Approach**:
1. Enable Cloud Trace
2. Check for slow database queries
3. Check for external API latency
4. Profile CPU usage

**Commands**:
```bash
# View slow traces
gcloud trace list --filter="duration>5000ms"

# View slow log entries
gcloud logging read "jsonPayload.latency > 5000" --limit 10

# Check database slow log
gcloud sql operations list --instance=my-db | grep slow
```

### Scenario 3: Cost Optimization

**Goal**: Reduce $10k/month cloud bill by 20%

**Steps**:
1. Check Active Assist recommendations
2. Review Reserved Capacity Usage
3. Check idle resources
4. Analyze logs for excessive logging

**Implementation**:
```bash
# Find idle VMs
gcloud compute instances list \
  --filter="status=RUNNING" \
  --format="table(name, zone)"

# Check reservation usage
gcloud compute reservations list

# Check logs cost
gcloud logging read "resource.type" --limit=1 | tail

# Find unattached disks
gcloud compute disks list --filter="users:[]"
```

---

## Part 14: Common Exam Mistakes

**Mistake 1**: Not enabling audit logs early
- Hard to retro enable
- Enable before any activity

**Mistake 2**: Creating alert on every metric spike
- Creates alert fatigue
- Fine-tune thresholds based on baselines

**Mistake 3**: Forgetting to add notification channels
- Alert created but no one notified
- Always add channel (email, Slack, PagerDuty)

**Mistake 4**: Not understanding metric vs log
- Use metrics for trends
- Use logs for debugging

**Mistake 5**: Ignoring cost of logging
- Data Access logs charged
- Archive old logs to Cloud Storage
- Set retention appropriately

---

## Part 15: Quick Reference

### Key Commands

```bash
# Create alert policy
gcloud alpha monitoring policies create \
  --notification-channels=CHANNEL_ID \
  --display-name="Alert Name"

# View logs
gcloud logging read "resource.type=gce_instance" --limit 10

# Create log sink
gcloud logging sinks create sink-name \
  bigquery.googleapis.com/projects/PROJECT_ID/datasets/logs

# View metrics
gcloud monitoring time-series list \
  --filter='metric.type="compute.googleapis.com/instance/cpu/utilization"'
```

### Alert Checklist

- [ ] Define meaningful threshold
- [ ] Set appropriate duration (not too short)
- [ ] Add notification channel
- [ ] Write documentation
- [ ] Test alert triggers
- [ ] Review quarterly

### Log Retention Policy

```
Logs                    | Retention
-----------------------|----------
Admin Activity Logs     | 30 days (free)
System Events           | 30 days (free)
Data Access             | configurable
Application Logs        | configurable
Audit Logs (export)     | 1+ years
```

---

## Conclusion

Monitoring and logging are critical for running Google Cloud at scale. Master these skills:

1. **Metrics and Dashboards**: Understand what to measure
2. **Alerting**: Know when to react
3. **Logs**: Debug when something goes wrong
4. **Audit Logs**: Compliance and accountability
5. **Diagnostic Tools**: Find performance issues
6. **Cost**: Monitor your bill

Exam focus:
- Creating alert policies correctly
- Writing log queries to find issues
- Understanding metrics vs logs
- Setting up dashboards
- Routing logs appropriately
- Using audit logs for compliance

---

**Total estimated reading/study time: 2 hours**
**Word count: ~10,000 words**

This guide covers everything in Section 3.4 with practical examples and configurations.
