# LOAD BALANCING IN GCP COMPUTE ENGINE
## Complete Learning Guide for Google Cloud Engineer Certification

---

## TABLE OF CONTENTS

1. Introduction to Load Balancing
2. Types of Load Balancers
3. Load Balancer Architecture
4. Routing & Traffic Management
5. Health Checks & Session Affinity
6. Global vs Regional Load Balancing
7. Configuration & Best Practices
8. Real-World Scenarios

---

## 1. INTRODUCTION TO LOAD BALANCING

### What is Load Balancing?

Load balancing distributes incoming traffic across multiple backend instances to:

- Improve application availability and reliability
- Increase throughput and reduce latency
- Enable horizontal scaling
- Handle traffic spikes
- Perform zero-downtime deployments
- Route traffic intelligently based on multiple criteria

### Why Load Balancing Matters

**Without load balancing:**
- A single instance receiving all traffic becomes a bottleneck
- Instance failure causes complete service outage
- No way to scale horizontally
- Difficult to perform updates without downtime

**With load balancing:**
- Traffic is distributed across instances
- Traffic can be rerouted if an instance fails
- Easy to add/remove instances without disruption
- Updates can happen gradually with zero downtime

---

## 2. TYPES OF LOAD BALANCERS

GCP provides several types of load balancers for different use cases:

### HTTP(S) LOAD BALANCER (LAYER 7 - APPLICATION)

**Global, managed service for HTTP and HTTPS traffic**

**Characteristics:**
- Operates at Layer 7 (Application)
- Understands HTTP headers, paths, hostnames
- Global distribution with intelligent routing
- Can route based on URL paths, hostnames, HTTP methods
- SSL/TLS termination at load balancer
- Connection multiplexing for efficiency

**Use Cases:**
- Web applications and websites
- REST APIs
- Need content-based routing
- Multi-region deployments
- Cross-region failover

**Example:**
- Route `/api/*` to API backend
- Route `/static/*` to static content backend
- Route based on hostname
- Rate limiting and DDOS protection

### NETWORK LOAD BALANCER (LAYER 4 - TRANSPORT)

**Ultra-high performance load balancer for extreme throughput**

**Characteristics:**
- Operates at Layer 4 (Transport)
- Handles TCP and UDP traffic
- Ultra-high performance (millions of packets/sec)
- Single IP, not global (regional)
- Preserves source IP
- Minimal latency
- Can handle any protocol (not just HTTP)

**Use Cases:**
- Non-HTTP protocols (gaming, IoT, databases)
- Extreme performance requirements
- Non-terminated connections (pass-through)
- Real-time applications
- VPN and firewall services

**Example:**
- Gaming servers needing 60+ FPS
- Database replication
- Custom protocols on port 5000+
- High-frequency trading systems

### INTERNAL TCP/UDP LOAD BALANCER

**Load balancer for internal traffic within VPC**

**Characteristics:**
- Regional, not global
- Only accessible from VPC network
- Private IP address
- Handles TCP and UDP
- High performance for internal services
- No external connectivity

**Use Cases:**
- Internal microservices
- Database sharding
- Message queues
- Internal API services
- Multi-tier applications

**Example:**
- Frontend (external LB) → Backend (internal LB) → Database

### SSL PROXY LOAD BALANCER

**For non-HTTP traffic requiring SSL/TLS**

**Characteristics:**
- Global load balancer
- Handles SSL/TLS for non-HTTP protocols
- Terminates SSL/TLS at load balancer
- Useful for custom protocols over SSL

**Use Cases:**
- Custom protocols requiring encryption
- Legacy applications
- Specific port-based services

### Load Balancer Comparison

| Feature | HTTP(S) | Network LB | Internal LB |
|---------|---------|-----------|------------|
| Layer | 7 (Application) | 4 (Transport) | 4 (Transport) |
| Protocol | HTTP/HTTPS only | Any (TCP/UDP) | TCP/UDP |
| Scope | Global | Regional | Regional |
| Access | External (public) | External | Internal |
| Content Routing | Yes (path, host) | No | No |
| Performance | High | Ultra-high | High |
| SSL/TLS | Supported | Not supported | Not supported |

---

## 3. LOAD BALANCER ARCHITECTURE

### HTTP(S) Load Balancer Components

#### 1. FRONTEND
- Forwarding Rule: Routes traffic to load balancer
- Target HTTPS/HTTP Proxy: Handles SSL/TLS and HTTP
- Listens on specific IP and port(s)
- Example: `35.184.0.1:443`

#### 2. URL MAP
- Defines routing rules
- Routes based on paths, hostnames, methods
- Example:
  - `/api/*` → backend-api
  - `/static/*` → backend-cdn
  - Default → backend-web

#### 3. BACKEND SERVICE
- Group of backend instances
- Health check configuration
- Load balancing policy (Round-robin, etc.)
- Session affinity settings
- Connection draining settings
- Timeout settings

#### 4. BACKEND INSTANCES
- Actual servers serving requests
- Typically in Managed Instance Groups
- Must pass health checks
- Can be in different zones/regions

#### 5. HEALTH CHECK
- Monitors backend health
- Removes unhealthy instances from rotation
- Automatically repairs/replaces unhealthy instances (with MIG)
- Example: `GET /health` returning HTTP 200

### Request Flow Through HTTP(S) Load Balancer

1. Client sends HTTPS request to load balancer IP
2. Forwarding Rule accepts traffic on port 443
3. HTTPS Proxy terminates SSL/TLS
4. URL Map examines request path/hostname
5. Selects appropriate Backend Service
6. Backend Service checks health of instances
7. Selects healthy instance using load balancing algorithm
8. Establishes connection to backend instance
9. Backend processes request and sends response
10. Response flows back through load balancer to client
11. Connection multiplexing may reuse connections

---

## 4. ROUTING & TRAFFIC MANAGEMENT

### URL Map Routing Rules

HTTP(S) load balancers use URL Maps to route traffic:

#### PATH-BASED ROUTING
- Route based on URL path
- Example: `/api/*` → API backend, `/images/*` → CDN backend
- Evaluated from most specific to least specific
- Most common routing type

#### HOSTNAME-BASED ROUTING
- Route based on Host header
- Example: `api.example.com` → API backend
- Example: `www.example.com` → Web backend
- Allows multiple domains on single IP

#### HTTP METHOD ROUTING
- Route based on HTTP method
- Example: `GET /resource` → read backend
- Example: `POST /resource` → write backend
- Less common but useful for separating read/write

#### COMBINED ROUTING
- Can combine path, hostname, and method
- Evaluated in order of specificity
- Default route catches unmatched traffic

**EXAMPLE URL MAP:**
```
Rule 1: www.example.com/api/* → backend-api
Rule 2: www.example.com/static/* → backend-cdn
Rule 3: api.example.com/* → backend-api
Rule 4: * (default) → backend-web
```

### Load Balancing Algorithms

#### ROUND ROBIN (Default)
- Distributes requests equally across instances
- Simple and fair
- Example: 1st request → Instance 1, 2nd → Instance 2, 3rd → Instance 1...
- Good for stateless applications

#### LEAST REQUEST
- Sends new requests to instance with fewest requests
- Adapts to varying server capacity
- Better for heterogeneous backend capacity
- Available in some load balancer configurations

#### IP HASH / SOURCE IP
- Uses source IP to determine backend
- Same client always goes to same backend
- Useful with session affinity
- Enables session persistence

### Traffic Splitting & Canary Deployments

#### TRAFFIC SPLITTING
- Send percentage of traffic to different backends
- Example: 90% to v1.0, 10% to v2.0 (canary)
- Gradually increase traffic to new version
- Allows testing in production

#### BENEFITS
- Test new code with real traffic
- Detect issues before full rollout
- Rollback by adjusting traffic split
- A/B testing capabilities

**EXAMPLE DEPLOYMENT:**
```
Day 1: 5% traffic to v2.0
Day 2: 10% traffic to v2.0
Day 3: 25% traffic to v2.0
Day 4: 50% traffic to v2.0
Day 5: 100% traffic to v2.0 (v1.0 retired)
```

---

## 5. HEALTH CHECKS & SESSION AFFINITY

### Health Checks

#### PURPOSE
- Verify backend instances are healthy and responding
- Remove unhealthy instances from rotation
- Restore instances when they become healthy again
- Trigger automatic repairs with MIGs

#### HEALTH CHECK TYPES

**HTTP/HTTPS**
- Send HTTP request to backend
- Check HTTP status code (200-299 is healthy)
- Example: `GET http://instance:8080/health`
- Most common for web applications

**TCP**
- Try to establish TCP connection
- Connection success = healthy
- No actual data sent
- Good for low-level verification

**SSL**
- Establish SSL connection
- Verify SSL handshake succeeds
- For backends requiring encryption

#### CONFIGURATION PARAMETERS
- **Check interval:** How often to check (default 30 seconds)
- **Timeout:** How long to wait for response (default 10 seconds)
- **Healthy threshold:** Consecutive successes before marking healthy (default 2)
- **Unhealthy threshold:** Consecutive failures before marking unhealthy (default 2)

**EXAMPLE:**
Check every 15 seconds, timeout 5 seconds, must pass 3 checks to be healthy

### Session Affinity

#### PURPOSE
- Ensure all requests from same client go to same backend
- Maintain session state on backend
- Improve application performance by leveraging server cache

#### WHEN NEEDED
- Applications with in-memory session state
- Sticky sessions required for protocol
- Performance optimization with server-side caching

#### AFFINITY OPTIONS

**CLIENT IP**
- Use source IP to determine backend
- Works for most clients
- May not work if multiple users share IP (NAT)

**COOKIE-BASED**
- Use HTTP cookie to track sessions
- Load balancer can generate cookie
- Or application can set affinity hint
- More reliable than IP-based

**GENERATED COOKIE**
- Load balancer generates unique cookie
- Client must support cookies
- Survives client IP changes
- RECOMMENDED APPROACH

**EXAMPLE:**
After first request to backend-1:
- Load balancer sets cookie: `LB_COOKIE=backend-1`
- All subsequent requests from client sent to backend-1
- Even if load balancing would normally send to backend-2

### Connection Draining

#### PURPOSE
- Gracefully shut down instances
- Allow existing connections to complete
- Prevent abrupt disconnection of clients

#### HOW IT WORKS
1. Instance is marked for shutdown
2. Load balancer stops sending NEW connections to instance
3. Existing connections are allowed to complete
4. After timeout, forcefully close remaining connections
5. Instance is removed

#### CONFIGURATION
- **Connection draining timeout:** Maximum time to wait (default 300 seconds)
- Can be tuned based on expected request duration

**EXAMPLE:**
With 300 second drain timeout:
- Long-running requests (< 300s) can complete
- New requests go to other backends
- After 300s, remaining connections forcefully closed
- Instance can safely terminate

---

## 6. GLOBAL VS REGIONAL LOAD BALANCING

### Global Load Balancers

#### CHARACTERISTICS
- Single global IP address
- Route traffic to nearest backend region
- Automatic failover between regions
- Ultra-low latency via Anycast routing
- HTTP(S) load balancer
- SSL Proxy load balancer
- TCP Proxy load balancer

#### BENEFITS
- Multi-region high availability
- Automatic geo-routing to nearest region
- Seamless failover if region fails
- Better user experience (lower latency)
- Single DNS entry for global service

#### USE CASES
- Global applications
- Multi-region redundancy
- Serving users worldwide
- Disaster recovery across regions

### Regional Load Balancers

#### CHARACTERISTICS
- Regional IP address
- Cannot route across regions
- Network load balancer (TCP/UDP)
- Internal TCP/UDP load balancer

#### BENEFITS
- High performance for regional traffic
- Lower latency within region
- Ultra-high throughput capability
- Cost-effective for single-region deployments

#### USE CASES
- Single-region deployments
- Non-HTTP protocols
- High-performance requirements
- Internal service mesh

---

## 7. CONFIGURATION & BEST PRACTICES

### Setting Up HTTP(S) Load Balancer: Step-by-Step

**Step 1: Create Health Check**
- Define health check endpoint
- Set check interval and timeout
- Example: `HTTP GET /health port 8080`

**Step 2: Create Backend Service**
- Select backend instances (MIG or instance group)
- Attach health check
- Configure timeout
- Set connection draining (300 seconds recommended)
- Configure session affinity (if needed)

**Step 3: Create URL Map**
- Define routing rules
- Specify default backend service
- Add custom routing rules (paths, hostnames)
- Example: `/api/*` → api-backend

**Step 4: Create HTTPS Proxy**
- Attach URL map
- Configure SSL certificate
- Redirect HTTP to HTTPS (recommended)

**Step 5: Create Forwarding Rule**
- Select proxy (HTTPS or HTTP)
- Assign static external IP
- Configure ports (80 and/or 443)
- Global (for global LB)

**Step 6: Configure DNS**
- Update DNS to point to load balancer IP
- Example: `www.example.com` → `35.184.0.1`

### Best Practices

#### 1. USE MANAGED INSTANCE GROUPS
- Automatic health checking and repair
- Easy scaling with autoscaling
- Zero-downtime deployments

#### 2. CONFIGURE APPROPRIATE HEALTH CHECKS
- Create meaningful health check endpoints
- Check application readiness, not just connectivity
- Example: `/health` checks database connection
- Set realistic thresholds (don't mark unhealthy too quickly)

#### 3. IMPLEMENT GRACEFUL SHUTDOWN
- Handlers for SIGTERM signals
- Drain in-flight requests
- Set connection draining timeout based on typical request duration

#### 4. USE SESSION AFFINITY CAREFULLY
- Only when necessary (session state needed)
- Prefer stateless design when possible
- Cookie-based affinity more reliable than IP-based

#### 5. MONITOR LOAD BALANCER
- Set up Cloud Monitoring dashboards
- Alert on health check failures
- Monitor latency and request rates
- Track backend capacity

#### 6. TEST LOAD BALANCING
- Verify traffic distribution
- Test health check behavior
- Simulate instance failures
- Verify session affinity works

#### 7. USE TRAFFIC SPLITTING FOR DEPLOYMENTS
- Canary deployments for new versions
- A/B testing capabilities
- Automatic rollback if issues detected

#### 8. IMPLEMENT DDoS PROTECTION
- Use Cloud Armor
- Create security policies
- Rate limiting
- Geographic restrictions

---

## 8. REAL-WORLD SCENARIOS

### Scenario 1: High-Traffic Website

**Requirements:**
- Handle traffic spikes
- Global user base
- High availability
- Multiple content types

**Solution:**
- Global HTTP(S) load balancer
- Regional MIGs in multiple regions (us, europe, asia)
- URL routing:
  - `/api/*` → api-backend-service
  - `/static/*` → cdn-backend-service
  - Default → web-backend-service
- Health checks: `GET /health port 8080`
- Session affinity: Cookie-based
- Connection draining: 300 seconds
- Autoscaling: CPU > 70%

**Benefits:**
- Users routed to nearest region
- Traffic distributed across instances
- Automatic failover if region fails
- Zero-downtime deployments with canary traffic

### Scenario 2: Microservices Architecture

**Requirements:**
- Multiple internal services
- Service-to-service communication
- Single region (but multiple zones for HA)
- Fast internal communication

**Solution:**
- Internal TCP/UDP load balancer for each service
- Regional MIGs for each service
- No internet-facing load balancer for internal services
- Private IPs for internal services
- Health checks on service endpoints
- Example:
  - auth-service-lb → auth-service-mig
  - user-service-lb → user-service-mig
  - payment-service-lb → payment-service-mig
- High performance without unnecessary external routing

**Benefits:**
- Fast inter-service communication
- Decoupled services
- Independent scaling
- Secure (internal only)

### Scenario 3: API Service with Version Management

**Requirements:**
- Support multiple API versions
- Gradual migration to new version
- No downtime for clients
- A/B testing capability

**Solution:**
- Single global HTTP(S) load balancer
- URL routing by path:
  - `/v1/*` → v1-backend-service (stable)
  - `/v2/*` → v2-backend-service (new)
  - `/latest/*` → Uses traffic splitting (80% v1, 20% v2)
- Traffic splitting allows gradual rollout
- Health checks ensure backward compatibility
- Canary deployment strategy

**Deployment Process:**
- Day 1: `/latest` routes 10% to v2
- Day 2: `/latest` routes 25% to v2
- Day 3: `/latest` routes 50% to v2
- Day 4: `/latest` routes 100% to v2
- Clients on `/v1` unaffected during migration
- Easy rollback if issues detected

---

## LOAD BALANCING EXAM QUESTIONS

### Q1: Multi-Region Traffic Distribution

**Question:** You need to distribute traffic between instances in us-central1 and europe-west1. Which load balancer supports this?

- A) Network load balancer
- B) Global HTTP(S) load balancer
- C) Internal TCP/UDP load balancer
- D) Regional network load balancer

**✓ Answer: B** - Only HTTP(S) load balancer is global and can route across regions

---

### Q2: Session Affinity with Cookie-Based Tracking

**Question:** You configured session affinity with cookie-based tracking. What happens when load balancer adds a new instance?

- A) All sessions are rerouted to new instance
- B) Existing sessions continue to route to original instance
- C) Sessions are reset
- D) New instance is not used until old sessions expire

**✓ Answer: B** - Cookie-based affinity ensures clients continue to same backend

---

### Q3: Instance Health Check Failure

**Question:** An instance starts failing health checks. What happens to traffic?

- A) No change; traffic continues
- B) Traffic is gradually drained (connection draining)
- C) Traffic is immediately redirected to other instances
- D) Load balancer is automatically removed

**✓ Answer: C** - Unhealthy instances are removed from rotation immediately; existing connections may use connection draining

---

### Q4: URL Path Routing

**Question:** You need to route `/api/*` to backend-api and `/web/*` to backend-web. Which feature do you use?

- A) Traffic splitting
- B) URL map with path-based routing
- C) Session affinity
- D) Health checks

**✓ Answer: B** - URL maps with path-based rules handle this routing

---

**End of Guide**
