<div align="center">

# 🌐 GCP for DevOps 
### Complete Reference Guide — GCP ACE to DevOps Engineer

**Author:** Manish Deore
**Level:** Beginner → Intermediate → DevOps Engineer

</div>

---

## 📚 Table of Contents

| # | Topic |
|---|-------|
| 1 | [What is GCP?](#-what-is-gcp) |
| 2 | [Cloud Computing Concepts](#-cloud-computing-concepts) |
| 3 | [GCP Global Infrastructure](#-gcp-global-infrastructure) |
| 4 | [GCP Management Tools](#-gcp-management-tools) |
| 5 | [GCP Compute Services](#-gcp-compute-services) |
| 6 | [GCP Networking](#-gcp-networking) |
| 7 | [GCP Storage Services](#-gcp-storage-services) |
| 8 | [GCP Databases](#-gcp-databases) |
| 9 | [GCP Security & Identity (IAM)](#-gcp-security--identity-iam) |
| 10 | [GCP DevOps — CI/CD](#-gcp-devops--cicd) |
| 11 | [Google Kubernetes Engine (GKE)](#-google-kubernetes-engine-gke) |
| 12 | [Artifact Registry & Container Registry](#-artifact-registry--container-registry) |
| 13 | [GCP Monitoring & Logging (Cloud Operations)](#-gcp-monitoring--logging-cloud-operations) |
| 14 | [Infrastructure as Code on GCP](#-infrastructure-as-code-on-gcp) |
| 15 | [GCP Pricing & Cost Management](#-gcp-pricing--cost-management) |
| 16 | [gcloud CLI Cheat Sheet](#-gcloud-cli-cheat-sheet) |

---

## 🌐 What is GCP?

Google Cloud Platform (GCP) is Google's suite of **cloud computing services** running on the same infrastructure Google uses for its own products (Search, Gmail, YouTube).

```
On-Premises                         Google Cloud (GCP)
────────────────────────            ──────────────────────────────
You manage everything         vs    Google manages hardware
High CapEx (buy servers)            Pay-as-you-go (OpEx)
Long provisioning (weeks)           Provision in seconds
Limited scalability                 Planet-scale infrastructure
Single datacenter risk              Multi-region resilience
Manual patching                     Managed, auto-patched services
```

### Why GCP for DevOps?

```
GCP DevOps Strengths:
├── Best-in-class Kubernetes (GKE) — Google invented Kubernetes
├── Cloud Build     — Fast, serverless CI/CD
├── Artifact Registry — Private container + package registry
├── Cloud Run       — Serverless containers (no cluster needed)
├── Cloud Operations— Logging, monitoring, tracing, profiling
├── BigQuery        — Fastest analytics warehouse
└── Global network  — Lowest latency backbone on the planet
```

---

## 💡 Cloud Computing Concepts

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **High Availability** | 99.99%+ SLAs with multi-zone/region deployments |
| **Scalability** | Scale from 0 to millions of users instantly |
| **Elasticity** | Auto-scale based on real-time demand |
| **Agility** | Deploy infrastructure in seconds with gcloud |
| **Geo-Distribution** | 40+ regions worldwide |
| **Disaster Recovery** | Multi-region replication built-in |
| **CapEx → OpEx** | No upfront hardware cost, pay per second |

### Cloud Service Models

```
                  IaaS          PaaS          SaaS
                ────────       ──────        ──────
GCP Example:   Compute VM   App Engine    Google Workspace
               (you manage  (you manage   (Google manages
                OS + above)  app + data)   everything)

Responsibility:
Application      You           You          Google
Data             You           You          Google
Runtime          You          Google        Google
OS               You          Google        Google
Servers         Google        Google        Google
Network         Google        Google        Google
```

### Consumption-Based Model
- Pay per second (Compute Engine)
- Pay per request (Cloud Functions, Cloud Run)
- Pay per GB (Storage, Network egress)
- Sustained use discounts apply automatically (no commitment needed)

---

## 🌍 GCP Global Infrastructure

```
GCP Infrastructure
├── Multi-Regions      (e.g., US, EU, ASIA)
│   └── Regions        (e.g., us-central1, asia-south1)
│       └── Zones      (e.g., us-central1-a, us-central1-b, us-central1-c)
│           └── Clusters of datacenters
└── Edge Locations     (CDN, Cloud Armor, load balancing PoPs)
```

### Regions & Zones

```
Region: asia-south1 (Mumbai)
├── Zone: asia-south1-a
├── Zone: asia-south1-b
└── Zone: asia-south1-c

Each zone = independent power, cooling, networking
Deploy across zones → high availability
Deploy across regions → disaster recovery
```

### Key GCP Regions

| Region | Location |
|--------|---------|
| `us-central1` | Iowa, USA |
| `us-east1` | South Carolina, USA |
| `europe-west1` | Belgium |
| `asia-south1` | Mumbai, India |
| `asia-east1` | Taiwan |
| `australia-southeast1` | Sydney |

### GCP Resource Hierarchy

```
Organization          (your-company.com)
└── Folders           (Dev, Staging, Production)
    └── Projects      (my-app-dev, my-app-prod)
        └── Resources (VMs, Buckets, Clusters, etc.)

IAM policies inherit downward:
Policy on Org → applies to all Folders, Projects, Resources
Policy on Project → applies to all Resources in project
```

```bash
# List projects
gcloud projects list

# Set active project
gcloud config set project my-project-id

# Create project
gcloud projects create my-new-project --name="My New Project"

# List folders
gcloud resource-manager folders list --organization=ORG_ID
```

---

## 🛠️ GCP Management Tools

### Google Cloud Console
- Web UI at [console.cloud.google.com](https://console.cloud.google.com)
- Visual management of all GCP resources
- Cloud Shell available directly in browser

### gcloud CLI

```bash
# Install gcloud SDK
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init

# Authentication
gcloud auth login
gcloud auth application-default login     # for SDKs/apps
gcloud auth list                          # list accounts
gcloud auth activate-service-account \
  --key-file=service-account-key.json     # for automation

# Configuration
gcloud config set project my-project
gcloud config set compute/region asia-south1
gcloud config set compute/zone asia-south1-a
gcloud config list                        # view all settings

# Config profiles (switch between projects easily)
gcloud config configurations create prod-config
gcloud config configurations activate prod-config
```

### Cloud Shell
- Free VM (Debian Linux) in your browser
- Pre-installed: gcloud, kubectl, terraform, docker, git
- 5 GB persistent home directory
- Access at [shell.cloud.google.com](https://shell.cloud.google.com)

### Client Libraries (SDK)

```python
# Python — install
pip install google-cloud-storage google-cloud-bigquery

# Use Application Default Credentials (ADC)
from google.cloud import storage

client = storage.Client()   # auto-authenticates using ADC
bucket = client.bucket('my-bucket')
blob = bucket.blob('myfile.txt')
blob.upload_from_filename('localfile.txt')
```

### Service Accounts
Non-human identities for apps and automation:

```bash
# Create service account
gcloud iam service-accounts create my-sa \
  --display-name="My Service Account"

# Grant role to service account
gcloud projects add-iam-policy-binding my-project \
  --member="serviceAccount:my-sa@my-project.iam.gserviceaccount.com" \
  --role="roles/storage.objectViewer"

# Create and download key (use Workload Identity instead when possible)
gcloud iam service-accounts keys create key.json \
  --iam-account=my-sa@my-project.iam.gserviceaccount.com

# Use key for auth
gcloud auth activate-service-account --key-file=key.json
export GOOGLE_APPLICATION_CREDENTIALS="key.json"
```

---

## 💻 GCP Compute Services

### Compute Engine (IaaS)
Full control over VMs — GCP's equivalent of EC2/Azure VMs.

**Machine Types:**

| Family | Series | Optimized For |
|--------|--------|--------------|
| General Purpose | E2, N2, N2D | Balanced CPU/memory |
| Compute Optimized | C2, C2D | High CPU performance |
| Memory Optimized | M2, M3 | Large in-memory workloads |
| Accelerator Optimized | A2, G2 | GPU/ML workloads |
| Storage Optimized | Z3 | High disk throughput |

**Pricing Models:**

| Model | Discount | How |
|-------|---------|-----|
| On-demand | Baseline | Pay per second |
| Sustained Use Discount | Up to 30% | Automatic, no commitment |
| Committed Use (1yr) | Up to 37% | 1-year commitment |
| Committed Use (3yr) | Up to 55% | 3-year commitment |
| Spot VMs | Up to 91% | Preemptible, can be stopped |
| Free tier | Free | f1-micro in us-regions (1/month) |

```bash
# Create VM
gcloud compute instances create my-vm \
  --zone=asia-south1-a \
  --machine-type=e2-medium \
  --image-family=ubuntu-2204-lts \
  --image-project=ubuntu-os-cloud \
  --boot-disk-size=20GB \
  --tags=http-server,https-server

# SSH into VM
gcloud compute ssh my-vm --zone=asia-south1-a

# Start / Stop / Delete
gcloud compute instances start  my-vm --zone=asia-south1-a
gcloud compute instances stop   my-vm --zone=asia-south1-a
gcloud compute instances delete my-vm --zone=asia-south1-a --quiet

# List VMs
gcloud compute instances list

# Create VM from snapshot
gcloud compute instances create my-vm-from-snap \
  --zone=asia-south1-a \
  --source-snapshot=my-snapshot
```

**Instance Templates & Managed Instance Groups (MIGs):**

```bash
# Create instance template
gcloud compute instance-templates create my-template \
  --machine-type=e2-medium \
  --image-family=ubuntu-2204-lts \
  --image-project=ubuntu-os-cloud

# Create Managed Instance Group (auto-scaling)
gcloud compute instance-groups managed create my-mig \
  --template=my-template \
  --size=2 \
  --zone=asia-south1-a

# Set autoscaling (CPU > 60% → scale out)
gcloud compute instance-groups managed set-autoscaling my-mig \
  --zone=asia-south1-a \
  --max-num-replicas=10 \
  --min-num-replicas=2 \
  --target-cpu-utilization=0.60
```

### App Engine (PaaS)
Fully managed platform — just deploy your code, Google manages the rest.

```
App Engine Environments:
├── Standard  → Runs in sandboxed env, scales to 0, very fast scale
│   Languages: Python, Java, Go, PHP, Node.js, Ruby
└── Flexible  → Docker container, more control, min 1 instance
    Languages: Any (bring your own Dockerfile)
```

```yaml
# app.yaml (Standard environment)
runtime: python311

handlers:
- url: /.*
  script: auto

automatic_scaling:
  min_instances: 0
  max_instances: 10
  target_cpu_utilization: 0.65
```

```bash
# Deploy to App Engine
gcloud app deploy app.yaml

# View app
gcloud app browse

# List services and versions
gcloud app services list
gcloud app versions list

# Traffic splitting (canary deployment)
gcloud app services set-traffic my-service \
  --splits v1=90,v2=10
```

### Cloud Run (Serverless Containers)
Run stateless containers without managing infrastructure.

```
You → docker push image → Cloud Run → Scales 0 to N automatically
      (any language,                  Pay only when handling requests
       any framework)
```

```bash
# Deploy from container image
gcloud run deploy my-service \
  --image gcr.io/my-project/my-app:latest \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated \
  --memory 512Mi \
  --cpu 1 \
  --min-instances 0 \
  --max-instances 100 \
  --concurrency 80

# Deploy from source code (builds automatically)
gcloud run deploy my-service \
  --source . \
  --region asia-south1 \
  --allow-unauthenticated

# List services
gcloud run services list --region asia-south1

# View logs
gcloud run services logs tail my-service --region asia-south1

# Update environment variables
gcloud run services update my-service \
  --region asia-south1 \
  --set-env-vars DB_HOST=localhost,DB_PORT=5432
```

### Cloud Functions (Serverless)
Event-driven functions — pay per invocation.

```
Triggers:
├── HTTP          → REST endpoint
├── Pub/Sub       → Message on a topic
├── Cloud Storage → File uploaded/deleted
├── Firestore     → Document changed
├── Cloud Scheduler → Cron job
└── Eventarc      → Any GCP service event
```

```python
# functions/main.py
import functions_framework
from flask import Request

@functions_framework.http
def hello_world(request: Request):
    name = request.args.get('name', 'World')
    return f'Hello, {name}!'
```

```bash
# Deploy Cloud Function
gcloud functions deploy hello-world \
  --runtime python311 \
  --trigger-http \
  --allow-unauthenticated \
  --region asia-south1 \
  --entry-point hello_world \
  --memory 256MB

# Deploy Pub/Sub triggered function
gcloud functions deploy process-message \
  --runtime python311 \
  --trigger-topic my-topic \
  --region asia-south1 \
  --entry-point process_message

# List functions
gcloud functions list
gcloud functions logs read hello-world --region asia-south1
```

### Compute Comparison

| Service | Control | Scaling | Best For |
|---------|---------|---------|---------|
| Compute Engine | Full OS | Manual / MIG | Lift-and-shift, custom OS |
| App Engine | App only | Automatic | Simple web apps, APIs |
| Cloud Run | Container | 0 to N | Stateless microservices |
| Cloud Functions | Function | 0 to N | Event-driven, lightweight |
| GKE | K8s pods | HPA / CA | Complex microservices |

---

## 🌐 GCP Networking

### Virtual Private Cloud (VPC)

```
GCP VPC (Global — spans all regions!)
├── Subnet: asia-south1  (10.0.1.0/24)  → VMs in Mumbai
├── Subnet: us-central1  (10.0.2.0/24)  → VMs in Iowa
└── Subnet: europe-west1 (10.0.3.0/24)  → VMs in Belgium

Key difference from AWS/Azure: GCP VPC is GLOBAL
VMs in different regions can communicate privately
```

```bash
# Create custom VPC
gcloud compute networks create my-vpc \
  --subnet-mode=custom \
  --bgp-routing-mode=regional

# Create subnet
gcloud compute networks subnets create my-subnet \
  --network=my-vpc \
  --region=asia-south1 \
  --range=10.0.1.0/24 \
  --enable-private-ip-google-access

# List VPCs and subnets
gcloud compute networks list
gcloud compute networks subnets list
```

### Firewall Rules

```
GCP Firewall Rules (applied at VPC level, filter by tags/SA):

Priority | Direction | Action | Protocol | Ports | Target Tag
───────────────────────────────────────────────────────────────
1000     | INGRESS   | ALLOW  | TCP      | 80    | http-server
1000     | INGRESS   | ALLOW  | TCP      | 443   | https-server
1000     | INGRESS   | ALLOW  | TCP      | 22    | ssh-access
65534    | INGRESS   | DENY   | all      | all   | (default deny)
```

```bash
# Allow HTTP traffic to VMs with tag 'http-server'
gcloud compute firewall-rules create allow-http \
  --network=my-vpc \
  --allow=tcp:80 \
  --target-tags=http-server \
  --direction=INGRESS \
  --priority=1000

# Allow SSH from specific IP
gcloud compute firewall-rules create allow-ssh \
  --network=my-vpc \
  --allow=tcp:22 \
  --source-ranges=203.0.113.0/32 \
  --direction=INGRESS

# List firewall rules
gcloud compute firewall-rules list
```

### Load Balancing

| Type | Layer | Scope | Use Case |
|------|-------|-------|---------|
| **External HTTP(S) LB** | 7 | Global | Web apps, APIs — global anycast |
| **Internal HTTP(S) LB** | 7 | Regional | Internal microservices |
| **External TCP/UDP LB** | 4 | Regional | TCP/UDP — non-HTTP traffic |
| **Internal TCP/UDP LB** | 4 | Regional | Internal TCP/UDP |
| **Cloud CDN** | 7 | Global | Static content caching |

```bash
# Create HTTP(S) Load Balancer (simplified steps)

# 1. Create health check
gcloud compute health-checks create http my-health-check \
  --port=80 --request-path=/health

# 2. Create backend service
gcloud compute backend-services create my-backend \
  --protocol=HTTP \
  --health-checks=my-health-check \
  --global

# 3. Add instance group to backend
gcloud compute backend-services add-backend my-backend \
  --instance-group=my-mig \
  --instance-group-zone=asia-south1-a \
  --global

# 4. Create URL map
gcloud compute url-maps create my-url-map \
  --default-service=my-backend

# 5. Create target HTTP proxy
gcloud compute target-http-proxies create my-proxy \
  --url-map=my-url-map

# 6. Create forwarding rule (reserve IP)
gcloud compute global-forwarding-rules create my-lb \
  --target-http-proxy=my-proxy \
  --ports=80 \
  --global
```

### VPC Peering & Shared VPC

```bash
# VPC Peering — connect two VPCs
gcloud compute networks peerings create peer-a-to-b \
  --network=vpc-a \
  --peer-project=project-b \
  --peer-network=vpc-b \
  --auto-create-routes

# Shared VPC — share one VPC across multiple projects
# Host project: owns the VPC
# Service projects: use subnets from host VPC
gcloud compute shared-vpc enable host-project-id
gcloud compute shared-vpc associated-projects add service-project-id \
  --host-project=host-project-id
```

### Cloud DNS

```bash
# Create DNS zone
gcloud dns managed-zones create my-zone \
  --dns-name=mydomain.com. \
  --description="My domain"

# Add A record
gcloud dns record-sets create www.mydomain.com. \
  --zone=my-zone \
  --type=A \
  --ttl=300 \
  --rrdatas=1.2.3.4

# List records
gcloud dns record-sets list --zone=my-zone
```

### Cloud NAT
Allow VMs without external IPs to access the internet:

```bash
# Create Cloud Router (required for NAT)
gcloud compute routers create my-router \
  --network=my-vpc \
  --region=asia-south1

# Create NAT gateway
gcloud compute routers nats create my-nat \
  --router=my-router \
  --region=asia-south1 \
  --auto-allocate-nat-external-ips \
  --nat-all-subnet-ip-ranges
```

### Private Google Access
Allow VMs without public IPs to access Google APIs (Storage, BigQuery, etc.):

```bash
# Enable on subnet
gcloud compute networks subnets update my-subnet \
  --region=asia-south1 \
  --enable-private-ip-google-access
```

### Cloud Armor (DDoS + WAF)

```bash
# Create security policy
gcloud compute security-policies create my-policy \
  --description="WAF policy"

# Block a specific IP
gcloud compute security-policies rules create 1000 \
  --security-policy=my-policy \
  --expression="inIpRange(origin.ip, '203.0.113.0/24')" \
  --action=deny-403

# Rate limiting
gcloud compute security-policies rules create 2000 \
  --security-policy=my-policy \
  --expression="true" \
  --action=rate-based-ban \
  --rate-limit-threshold-count=100 \
  --rate-limit-threshold-interval-sec=60

# Attach to backend service
gcloud compute backend-services update my-backend \
  --security-policy=my-policy \
  --global
```

---

## 💾 GCP Storage Services

### Cloud Storage (Object Storage)

```
Cloud Storage
├── Buckets          → Top-level containers (globally unique names)
│   └── Objects      → Files of any size (up to 5TB each)

Storage Classes:
├── Standard    → Frequently accessed, no min duration
├── Nearline    → Once a month access, 30-day min
├── Coldline    → Once a quarter access, 90-day min
└── Archive     → Once a year access, 365-day min, lowest cost
```

```bash
# Create bucket
gcloud storage buckets create gs://my-unique-bucket \
  --location=asia-south1 \
  --storage-class=STANDARD \
  --uniform-bucket-level-access

# Upload files
gcloud storage cp localfile.txt gs://my-unique-bucket/
gcloud storage cp -r ./mydir gs://my-unique-bucket/mydir/

# Download
gcloud storage cp gs://my-unique-bucket/file.txt ./localfile.txt

# List objects
gcloud storage ls gs://my-unique-bucket/

# Sync directory (like rsync)
gcloud storage rsync -r ./localdir gs://my-unique-bucket/

# Make object public
gcloud storage objects add-iam-policy-binding gs://my-unique-bucket/file.txt \
  --member=allUsers --role=roles/storage.objectViewer

# Delete
gcloud storage rm gs://my-unique-bucket/file.txt
gcloud storage rm -r gs://my-unique-bucket/mydir/

# Set lifecycle policy (auto-delete after 30 days)
cat > lifecycle.json << EOF
{
  "lifecycle": {
    "rule": [
      {
        "action": {"type": "Delete"},
        "condition": {"age": 30}
      }
    ]
  }
}
EOF
gcloud storage buckets update gs://my-unique-bucket \
  --lifecycle-file=lifecycle.json
```

### Persistent Disk (Block Storage)

```bash
# Create persistent disk
gcloud compute disks create my-disk \
  --size=100GB \
  --type=pd-ssd \
  --zone=asia-south1-a

# Attach to VM
gcloud compute instances attach-disk my-vm \
  --disk=my-disk \
  --zone=asia-south1-a

# Create snapshot
gcloud compute snapshots create my-snapshot \
  --source-disk=my-disk \
  --source-disk-zone=asia-south1-a

# Disk types
# pd-standard → HDD, cheapest
# pd-balanced  → SSD, balanced price/performance
# pd-ssd       → SSD, high IOPS
# pd-extreme   → SSD, highest IOPS (up to 120,000)
```

### Filestore (Managed NFS)
Managed NFS file shares for GKE and Compute Engine:

```bash
# Create Filestore instance
gcloud filestore instances create my-filestore \
  --zone=asia-south1-a \
  --tier=STANDARD \
  --file-share=name=myshare,capacity=1TB \
  --network=name=my-vpc

# Mount on Linux VM
sudo mount -t nfs \
  FILESTORE_IP:/myshare \
  /mnt/filestore
```

### Cloud Storage Transfer Service
Move data into GCP from S3, Azure Blob, or HTTP sources:

```bash
gcloud transfer jobs create \
  s3://my-aws-bucket gs://my-gcp-bucket \
  --source-creds-file=aws-creds.json \
  --schedule-repeats-every=24h
```

---

## 🗄️ GCP Databases

### Database Services Overview

| Service | Type | Best For |
|---------|------|---------|
| **Cloud SQL** | Relational (MySQL/PostgreSQL/SQL Server) | Traditional apps, web backends |
| **Cloud Spanner** | Globally distributed relational | Planet-scale, ACID, 99.999% SLA |
| **Firestore** | NoSQL document | Mobile/web apps, real-time sync |
| **Bigtable** | NoSQL wide-column | Time-series, IoT, analytics |
| **BigQuery** | Data warehouse | Analytics, ML, massive datasets |
| **Memorystore** | In-memory (Redis/Memcached) | Caching, session storage |
| **AlloyDB** | PostgreSQL-compatible | High-performance PostgreSQL |

### Cloud SQL

```bash
# Create Cloud SQL instance (PostgreSQL)
gcloud sql instances create my-db \
  --database-version=POSTGRES_15 \
  --tier=db-g1-small \
  --region=asia-south1 \
  --storage-type=SSD \
  --storage-size=20GB \
  --availability-type=REGIONAL   # HA — failover to standby

# Create database
gcloud sql databases create mydb --instance=my-db

# Create user
gcloud sql users create myuser \
  --instance=my-db \
  --password=MyP@ssword!

# Connect via Cloud SQL Proxy (secure)
cloud_sql_proxy --instances=my-project:asia-south1:my-db=tcp:5432 &
psql -h 127.0.0.1 -U myuser -d mydb

# Backup
gcloud sql backups create --instance=my-db

# List instances
gcloud sql instances list
```

### BigQuery

```bash
# Create dataset
bq mk --dataset my-project:my_dataset

# Load data
bq load --source_format=CSV \
  my_dataset.my_table \
  gs://my-bucket/data.csv \
  schema.json

# Run query
bq query --use_legacy_sql=false \
  'SELECT country, COUNT(*) as cnt
   FROM my_dataset.my_table
   GROUP BY country
   ORDER BY cnt DESC
   LIMIT 10'

# Export results to GCS
bq extract my_dataset.my_table \
  gs://my-bucket/export-*.csv
```

```sql
-- BigQuery SQL example
SELECT
  DATE(created_at) as date,
  COUNT(*) as orders,
  SUM(amount) as revenue
FROM `my-project.ecommerce.orders`
WHERE created_at >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
GROUP BY date
ORDER BY date DESC
```

### Firestore

```python
from google.cloud import firestore

db = firestore.Client()

# Add document
db.collection('users').document('user123').set({
    'name': 'Manish Deore',
    'email': 'manish@example.com',
    'city': 'Mumbai'
})

# Get document
doc = db.collection('users').document('user123').get()
print(doc.to_dict())

# Query
users = db.collection('users').where('city', '==', 'Mumbai').stream()
for user in users:
    print(user.to_dict())
```

### Memorystore (Redis)

```bash
# Create Redis instance
gcloud redis instances create my-redis \
  --size=1 \
  --region=asia-south1 \
  --redis-version=redis_7_0 \
  --tier=STANDARD_HA

# Get connection details
gcloud redis instances describe my-redis --region=asia-south1

# Connect (from VM in same VPC)
redis-cli -h REDIS_IP -p 6379
```

---

## 🔐 GCP Security & Identity (IAM)

### IAM — Identity and Access Management

```
Who (Principal/Member) + What (Role) + Resource = Policy

Principal types:
├── Google Account         → user:name@gmail.com
├── Service Account        → serviceAccount:sa@project.iam.gserviceaccount.com
├── Google Group           → group:team@company.com
├── Google Workspace Domain→ domain:company.com
└── allUsers / allAuthenticatedUsers → public access
```

### IAM Roles

| Type | Description | Example |
|------|-------------|---------|
| **Basic** | Coarse-grained, project-level | Owner, Editor, Viewer |
| **Predefined** | Fine-grained, service-specific | roles/storage.objectViewer |
| **Custom** | You define exact permissions | Custom DevOps role |

**Common Predefined Roles:**

```
Compute:
  roles/compute.admin           → Full compute control
  roles/compute.instanceAdmin   → VM management only
  roles/compute.viewer          → Read-only

Storage:
  roles/storage.admin           → Full storage control
  roles/storage.objectViewer    → Read objects only
  roles/storage.objectCreator   → Upload only

GKE:
  roles/container.admin         → Full GKE control
  roles/container.developer     → Deploy workloads
  roles/container.viewer        → Read-only

Project:
  roles/owner                   → Full project control
  roles/editor                  → Edit (not IAM)
  roles/viewer                  → Read-only
```

```bash
# Grant role to user
gcloud projects add-iam-policy-binding my-project \
  --member="user:manish@example.com" \
  --role="roles/compute.instanceAdmin"

# Grant role to service account
gcloud projects add-iam-policy-binding my-project \
  --member="serviceAccount:my-sa@my-project.iam.gserviceaccount.com" \
  --role="roles/storage.objectViewer"

# View IAM policy
gcloud projects get-iam-policy my-project

# Remove role
gcloud projects remove-iam-policy-binding my-project \
  --member="user:manish@example.com" \
  --role="roles/compute.instanceAdmin"

# Create custom role
gcloud iam roles create DevOpsRole \
  --project=my-project \
  --title="DevOps Role" \
  --permissions=compute.instances.list,container.clusters.get
```

### Workload Identity Federation
Let GKE workloads access GCP services without service account keys:

```bash
# Enable Workload Identity on GKE cluster
gcloud container clusters update my-cluster \
  --workload-pool=my-project.svc.id.goog \
  --region=asia-south1

# Create K8s service account
kubectl create serviceaccount my-ksa -n my-namespace

# Bind to GCP service account
gcloud iam service-accounts add-iam-policy-binding \
  my-gcp-sa@my-project.iam.gserviceaccount.com \
  --role=roles/iam.workloadIdentityUser \
  --member="serviceAccount:my-project.svc.id.goog[my-namespace/my-ksa]"

# Annotate K8s service account
kubectl annotate serviceaccount my-ksa \
  -n my-namespace \
  iam.gke.io/gcp-service-account=my-gcp-sa@my-project.iam.gserviceaccount.com
```

### Secret Manager
Store and access secrets securely:

```bash
# Create secret
echo -n "MySecretPassword" | gcloud secrets create db-password \
  --data-file=- \
  --replication-policy=automatic

# Add new version
echo -n "NewPassword123" | gcloud secrets versions add db-password \
  --data-file=-

# Access secret
gcloud secrets versions access latest --secret=db-password

# Grant access to service account
gcloud secrets add-iam-policy-binding db-password \
  --member="serviceAccount:my-sa@my-project.iam.gserviceaccount.com" \
  --role="roles/secretmanager.secretAccessor"
```

```python
# Access from Python app
from google.cloud import secretmanager

client = secretmanager.SecretManagerServiceClient()
name = "projects/my-project/secrets/db-password/versions/latest"
response = client.access_secret_version(request={"name": name})
secret_value = response.payload.data.decode("UTF-8")
```

### VPC Service Controls
Create security perimeters around GCP services:

```bash
# Create access policy
gcloud access-context-manager policies create \
  --organization=ORG_ID \
  --title="My Policy"

# Create service perimeter (restrict BigQuery/Storage to VPC)
gcloud access-context-manager perimeters create my-perimeter \
  --policy=POLICY_ID \
  --title="Data Perimeter" \
  --resources=projects/PROJECT_NUMBER \
  --restricted-services=bigquery.googleapis.com,storage.googleapis.com
```

### Security Best Practices for GCP DevOps

```
✅ Use Workload Identity         — no service account keys in pods
✅ Store secrets in Secret Manager — never in code or env vars
✅ Apply least-privilege IAM    — minimum required permissions
✅ Use VPC Service Controls     — protect sensitive data services
✅ Enable Cloud Armor           — DDoS protection + WAF
✅ Use Private Google Access    — no external IPs on VMs
✅ Enable audit logs            — Cloud Audit Logs for all APIs
✅ Scan containers              — use Artifact Registry scanning
✅ Use Binary Authorization     — only trusted images in GKE
✅ Enable org policies          — enforce compliance at scale
✅ Use shielded VMs             — protect VM boot process
✅ Rotate service account keys  — or better: avoid keys entirely
```

---

## 🚀 GCP DevOps — CI/CD

### Cloud Build — Serverless CI/CD

```
Developer pushes code
        ↓
Cloud Build triggers (GitHub, Cloud Source Repos, Bitbucket)
        ↓
cloudbuild.yaml executed on Google's infrastructure
        ↓
Build → Test → Push to Artifact Registry → Deploy
```

### Complete cloudbuild.yaml

```yaml
# cloudbuild.yaml
substitutions:
  _REGION: asia-south1
  _SERVICE: my-app
  _IMAGE: $_REGION-docker.pkg.dev/$PROJECT_ID/my-repo/$_SERVICE

steps:

# ─── STEP 1: UNIT TESTS ─────────────────────────────────
- name: 'python:3.11'
  id: 'unit-tests'
  entrypoint: 'bash'
  args:
    - '-c'
    - |
      pip install -r requirements.txt
      pip install pytest pytest-cov
      pytest tests/ --junitxml=test-results.xml \
        --cov=app --cov-report=xml:coverage.xml
  env:
    - 'PYTHONDONTWRITEBYTECODE=1'

# ─── STEP 2: BUILD DOCKER IMAGE ─────────────────────────
- name: 'gcr.io/cloud-builders/docker'
  id: 'build-image'
  waitFor: ['unit-tests']
  args:
    - 'build'
    - '-t'
    - '$_IMAGE:$SHORT_SHA'
    - '-t'
    - '$_IMAGE:latest'
    - '.'

# ─── STEP 3: PUSH TO ARTIFACT REGISTRY ──────────────────
- name: 'gcr.io/cloud-builders/docker'
  id: 'push-image'
  waitFor: ['build-image']
  args:
    - 'push'
    - '--all-tags'
    - '$_IMAGE'

# ─── STEP 4: SECURITY SCAN ──────────────────────────────
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  id: 'scan-image'
  waitFor: ['push-image']
  entrypoint: 'bash'
  args:
    - '-c'
    - |
      gcloud artifacts docker images scan $_IMAGE:$SHORT_SHA \
        --format='value(response.scan)' > scan_id.txt
      gcloud artifacts docker images list-vulnerabilities \
        $(cat scan_id.txt) --format=json | \
        python3 -c "
      import json, sys
      vulns = json.load(sys.stdin)
      critical = [v for v in vulns if v.get('vulnerability',{}).get('effectiveSeverity') == 'CRITICAL']
      if critical:
          print(f'FAIL: {len(critical)} critical vulnerabilities found!')
          sys.exit(1)
      print('PASS: No critical vulnerabilities')
      "

# ─── STEP 5: DEPLOY TO STAGING ──────────────────────────
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  id: 'deploy-staging'
  waitFor: ['scan-image']
  args:
    - 'run'
    - 'deploy'
    - '$_SERVICE-staging'
    - '--image=$_IMAGE:$SHORT_SHA'
    - '--region=$_REGION'
    - '--platform=managed'
    - '--no-allow-unauthenticated'
    - '--set-env-vars=ENVIRONMENT=staging'

# ─── STEP 6: INTEGRATION TESTS ──────────────────────────
- name: 'python:3.11'
  id: 'integration-tests'
  waitFor: ['deploy-staging']
  entrypoint: 'bash'
  args:
    - '-c'
    - |
      STAGING_URL=$(gcloud run services describe $_SERVICE-staging \
        --region=$_REGION --format='value(status.url)')
      pip install requests pytest
      STAGING_URL=$STAGING_URL pytest tests/integration/

# ─── STEP 7: DEPLOY TO PRODUCTION ───────────────────────
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  id: 'deploy-production'
  waitFor: ['integration-tests']
  args:
    - 'run'
    - 'deploy'
    - '$_SERVICE'
    - '--image=$_IMAGE:$SHORT_SHA'
    - '--region=$_REGION'
    - '--platform=managed'
    - '--allow-unauthenticated'
    - '--set-env-vars=ENVIRONMENT=production'
    - '--traffic=100'

# ─── STEP 8: TAG RELEASE ────────────────────────────────
- name: 'gcr.io/cloud-builders/git'
  id: 'tag-release'
  waitFor: ['deploy-production']
  args:
    - 'tag'
    - 'release-$SHORT_SHA'

# Artifacts to store
artifacts:
  objects:
    location: 'gs://my-build-artifacts/$BUILD_ID'
    paths:
      - 'test-results.xml'
      - 'coverage.xml'

options:
  logging: CLOUD_LOGGING_ONLY
  machineType: 'E2_HIGHCPU_8'

timeout: '1800s'
```

### Trigger Cloud Build

```bash
# Create trigger from GitHub repo
gcloud builds triggers create github \
  --repo-owner=my-org \
  --repo-name=my-repo \
  --branch-pattern='^main$' \
  --build-config=cloudbuild.yaml \
  --name=main-trigger

# Create trigger for PRs
gcloud builds triggers create github \
  --repo-owner=my-org \
  --repo-name=my-repo \
  --pull-request-pattern='^main$' \
  --build-config=cloudbuild-pr.yaml \
  --name=pr-trigger

# Manual trigger
gcloud builds submit \
  --config=cloudbuild.yaml \
  --substitutions=SHORT_SHA=$(git rev-parse --short HEAD) \
  .

# List builds
gcloud builds list --limit=10
gcloud builds log BUILD_ID
```

### Cloud Deploy — Managed Continuous Delivery

```
Cloud Deploy pipeline:
Developer pushes → Cloud Build → Artifact Registry
                                         ↓
                              Cloud Deploy Pipeline
                              ├── Target: staging     → auto-deploy
                              ├── Target: QA          → auto-deploy after tests
                              └── Target: production  → manual approval required
```

```yaml
# clouddeploy.yaml
apiVersion: deploy.cloud.google.com/v1
kind: DeliveryPipeline
metadata:
  name: my-pipeline
  location: asia-south1
description: "My app delivery pipeline"
serialPipeline:
  stages:
  - targetId: staging
    profiles: [staging]
  - targetId: production
    profiles: [production]
    strategy:
      canary:
        runtimeConfig:
          cloudRun:
            automaticTrafficControl: true
        canaryDeployment:
          percentages: [25, 50, 75]
          verify: true
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: staging
  location: asia-south1
run:
  location: projects/my-project/locations/asia-south1
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: production
  location: asia-south1
requireApproval: true
run:
  location: projects/my-project/locations/asia-south1
```

```bash
# Apply pipeline config
gcloud deploy apply --file=clouddeploy.yaml \
  --region=asia-south1 \
  --project=my-project

# Create release (triggers deployment to first target)
gcloud deploy releases create release-$SHORT_SHA \
  --delivery-pipeline=my-pipeline \
  --region=asia-south1 \
  --images=my-app=asia-south1-docker.pkg.dev/my-project/my-repo/my-app:$SHORT_SHA

# Approve production deployment
gcloud deploy rollouts approve release-$SHORT_SHA-to-production-0001 \
  --delivery-pipeline=my-pipeline \
  --release=release-$SHORT_SHA \
  --region=asia-south1

# List rollouts
gcloud deploy rollouts list \
  --delivery-pipeline=my-pipeline \
  --release=release-$SHORT_SHA \
  --region=asia-south1
```

### Cloud Source Repositories

```bash
# Create repo
gcloud source repos create my-repo

# Clone
gcloud source repos clone my-repo

# Push code
git push origin main

# Mirror GitHub repo to Cloud Source Repos
gcloud source repos create my-mirrored-repo
# Then connect in Console: Source Repositories → Add Repo → Mirror from GitHub
```

### Pub/Sub (Async Messaging)

```bash
# Create topic
gcloud pubsub topics create my-topic

# Create subscription
gcloud pubsub subscriptions create my-sub --topic=my-topic

# Publish message
gcloud pubsub topics publish my-topic --message='{"event": "deploy", "status": "success"}'

# Pull messages
gcloud pubsub subscriptions pull my-sub --auto-ack --limit=10
```

---

## ☸️ Google Kubernetes Engine (GKE)

### Cluster Types

| Type | Control Plane | Scaling | Best For |
|------|--------------|---------|---------|
| **Autopilot** | Google managed | Node auto-provisioning | Most workloads, less ops |
| **Standard** | Google managed | Manual / Cluster Autoscaler | Full control over nodes |

```bash
# Create Autopilot cluster (recommended)
gcloud container clusters create-auto my-cluster \
  --region=asia-south1

# Create Standard cluster
gcloud container clusters create my-cluster \
  --zone=asia-south1-a \
  --num-nodes=3 \
  --machine-type=e2-standard-4 \
  --enable-autoscaling \
  --min-nodes=1 \
  --max-nodes=10 \
  --enable-autorepair \
  --enable-autoupgrade \
  --workload-pool=my-project.svc.id.goog \
  --enable-ip-alias \
  --network=my-vpc \
  --subnetwork=my-subnet

# Get credentials
gcloud container clusters get-credentials my-cluster \
  --zone=asia-south1-a

# List clusters
gcloud container clusters list
```

### Node Pools

```bash
# Add GPU node pool
gcloud container node-pools create gpu-pool \
  --cluster=my-cluster \
  --zone=asia-south1-a \
  --machine-type=n1-standard-4 \
  --accelerator=type=nvidia-tesla-t4,count=1 \
  --num-nodes=2

# Add spot/preemptible node pool (cheap!)
gcloud container node-pools create spot-pool \
  --cluster=my-cluster \
  --zone=asia-south1-a \
  --machine-type=e2-standard-4 \
  --spot \
  --num-nodes=0 \
  --enable-autoscaling \
  --min-nodes=0 \
  --max-nodes=20
```

### GKE Deployments

```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: my-app
    spec:
      serviceAccountName: my-ksa    # Workload Identity SA
      containers:
      - name: my-app
        image: asia-south1-docker.pkg.dev/my-project/my-repo/my-app:v1
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: "250m"
            memory: "256Mi"
          limits:
            cpu: "500m"
            memory: "512Mi"
        env:
        - name: DB_HOST
          valueFrom:
            secretKeyRef:
              name: db-secrets
              key: host
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
  namespace: production
spec:
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-ingress
  annotations:
    kubernetes.io/ingress.class: "gce"
    kubernetes.io/ingress.global-static-ip-name: "my-static-ip"
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-service
            port:
              number: 80
```

### HPA — Horizontal Pod Autoscaler

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

### GKE Useful Commands

```bash
# Cluster operations
kubectl get nodes -o wide
kubectl get pods --all-namespaces
kubectl get services --all-namespaces
kubectl describe pod my-pod -n my-namespace
kubectl logs my-pod -n my-namespace -f --tail=100
kubectl exec -it my-pod -n my-namespace -- /bin/bash

# Deployments
kubectl apply -f deployment.yaml
kubectl rollout status deployment/my-app -n production
kubectl rollout history deployment/my-app -n production
kubectl rollout undo deployment/my-app -n production  # rollback
kubectl scale deployment my-app --replicas=5 -n production
kubectl set image deployment/my-app my-app=my-image:v2 -n production

# Secrets
kubectl create secret generic db-secrets \
  --from-literal=host=localhost \
  --from-literal=password=MyP@ss! \
  -n production

# Config Maps
kubectl create configmap app-config \
  --from-file=config.yaml \
  -n production

# Port forward (local testing)
kubectl port-forward deployment/my-app 8080:8080 -n production

# Resource usage
kubectl top nodes
kubectl top pods -n production
```

### GKE Security

```bash
# Enable Binary Authorization (only signed images allowed)
gcloud container clusters update my-cluster \
  --enable-binauthz \
  --zone=asia-south1-a

# Enable Vulnerability Scanning on Artifact Registry
gcloud services enable containerscanning.googleapis.com

# Network Policy (deny all traffic, allow only necessary)
```

```yaml
# network-policy.yaml — default deny all
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
---
# Allow frontend to backend only
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-frontend-to-backend
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - port: 8080
```

---

## 📦 Artifact Registry & Container Registry

Artifact Registry is the **next-generation** private registry for containers and packages.

```
Artifact Registry
├── Docker images    → Container images for GKE, Cloud Run, etc.
├── Maven            → Java packages
├── npm              → Node.js packages
├── Python (PyPI)    → Python packages
├── Apt              → Debian/Ubuntu packages
└── Helm charts      → Kubernetes charts
```

```bash
# Create repository
gcloud artifacts repositories create my-repo \
  --repository-format=docker \
  --location=asia-south1 \
  --description="My Docker repo" \
  --immutable-tags

# Configure Docker auth
gcloud auth configure-docker asia-south1-docker.pkg.dev

# Build and push image
docker build -t asia-south1-docker.pkg.dev/my-project/my-repo/my-app:v1 .
docker push asia-south1-docker.pkg.dev/my-project/my-repo/my-app:v1

# Or build and push with Cloud Build
gcloud builds submit \
  --tag asia-south1-docker.pkg.dev/my-project/my-repo/my-app:v1 .

# List images
gcloud artifacts docker images list asia-south1-docker.pkg.dev/my-project/my-repo

# Scan for vulnerabilities
gcloud artifacts docker images scan \
  asia-south1-docker.pkg.dev/my-project/my-repo/my-app:v1

# Set up cleanup policy (delete old images)
gcloud artifacts repositories set-cleanup-policies my-repo \
  --location=asia-south1 \
  --policy=cleanup-policy.json

# Python package repo
gcloud artifacts repositories create python-repo \
  --repository-format=python \
  --location=asia-south1

# Push Python package
pip install twine
twine upload \
  --repository-url https://asia-south1-python.pkg.dev/my-project/python-repo/ \
  dist/*
```

---

## 📊 GCP Monitoring & Logging (Cloud Operations)

### Cloud Operations Suite Overview

```
Cloud Operations (formerly Stackdriver)
├── Cloud Monitoring    → Metrics, dashboards, uptime checks, alerts
├── Cloud Logging       → Log ingestion, search, export, sinks
├── Cloud Trace         → Distributed request tracing
├── Cloud Profiler      → CPU/memory profiling of live apps
├── Cloud Debugger      → Debug live apps without stopping them
└── Error Reporting     → Aggregate and alert on app errors
```

### Cloud Monitoring

```bash
# Create uptime check
gcloud monitoring uptime-check-configs create \
  --display-name="My App Health Check" \
  --resource-type=uptime_url \
  --monitored-resource=project_id=my-project,host=myapp.example.com \
  --http-check-path=/health \
  --period=60s \
  --timeout=10s

# Create alerting policy (CPU > 80%)
gcloud alpha monitoring policies create \
  --notification-channels=my-channel \
  --display-name="High CPU Alert" \
  --condition-display-name="CPU > 80%" \
  --condition-filter='resource.type="gce_instance" AND metric.type="compute.googleapis.com/instance/cpu/utilization"' \
  --condition-threshold-value=0.8 \
  --condition-threshold-duration=300s \
  --condition-comparison=COMPARISON_GT

# Create notification channel (email)
gcloud beta monitoring channels create \
  --display-name="DevOps Email" \
  --type=email \
  --channel-labels=email_address=devops@company.com
```

**Custom Metrics:**

```python
from google.cloud import monitoring_v3
import time

client = monitoring_v3.MetricServiceClient()
project_name = f"projects/my-project"

series = monitoring_v3.TimeSeries()
series.metric.type = "custom.googleapis.com/my_app/request_count"
series.metric.labels["environment"] = "production"
series.resource.type = "global"

now = time.time()
interval = monitoring_v3.TimeInterval(
    {"end_time": {"seconds": int(now), "nanos": 0}}
)
point = monitoring_v3.Point(
    {"interval": interval, "value": {"int64_value": 42}}
)
series.points = [point]

client.create_time_series(name=project_name, time_series=[series])
```

### Cloud Logging

```bash
# View logs in terminal
gcloud logging read "resource.type=gce_instance" \
  --limit=50 \
  --format=json

# Filter logs by severity
gcloud logging read \
  'resource.type="cloud_run_revision" AND severity>=ERROR' \
  --limit=20

# Stream live logs
gcloud logging tail "resource.type=gce_instance AND resource.labels.instance_id=INSTANCE_ID"

# Create log sink (export to BigQuery)
gcloud logging sinks create my-bq-sink \
  bigquery.googleapis.com/projects/my-project/datasets/my_logs_dataset \
  --log-filter='resource.type="gce_instance"'

# Create log sink (export to Cloud Storage)
gcloud logging sinks create my-gcs-sink \
  storage.googleapis.com/my-logs-bucket \
  --log-filter='severity>=WARNING'

# Create log-based metric
gcloud logging metrics create error-count \
  --description="Count of ERROR logs" \
  --log-filter='severity=ERROR'
```

**Log Query Examples:**

```
# All errors in last hour
severity>=ERROR
timestamp>="2024-01-01T00:00:00Z"

# Specific Cloud Run service errors
resource.type="cloud_run_revision"
resource.labels.service_name="my-service"
severity>=ERROR

# HTTP 5xx errors
httpRequest.status>=500

# Slow requests (> 2 seconds)
httpRequest.latency>"2s"

# Specific user activity
protoPayload.authenticationInfo.principalEmail="user@company.com"

# IAM policy changes (audit log)
protoPayload.methodName="SetIamPolicy"
```

### Cloud Trace

```python
from opentelemetry import trace
from opentelemetry.exporter.cloud_trace import CloudTraceSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

# Set up tracing
tracer_provider = TracerProvider()
cloud_trace_exporter = CloudTraceSpanExporter()
tracer_provider.add_span_processor(BatchSpanProcessor(cloud_trace_exporter))
trace.set_tracer_provider(tracer_provider)

tracer = trace.get_tracer(__name__)

def handle_request():
    with tracer.start_as_current_span("handle-request") as span:
        span.set_attribute("user.id", "123")
        with tracer.start_as_current_span("db-query"):
            result = db.query("SELECT ...")
        return result
```

### SLO Monitoring

```yaml
# slo.yaml
apiVersion: monitoring.googleapis.com/v1
kind: ServiceLevelObjective
metadata:
  name: my-app-availability-slo
spec:
  displayName: "99.9% Availability SLO"
  goal: 0.999
  rollingPeriod: 30d
  serviceLevelIndicator:
    requestBased:
      goodTotalRatio:
        goodServiceFilter: |
          metric.type="loadbalancing.googleapis.com/https/request_count"
          metric.labels.response_code_class="2xx"
        totalServiceFilter: |
          metric.type="loadbalancing.googleapis.com/https/request_count"
```

---

## 🏗️ Infrastructure as Code on GCP

### Terraform on GCP

```hcl
# main.tf
terraform {
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
  }
  backend "gcs" {
    bucket = "my-tfstate-bucket"
    prefix = "terraform/state"
  }
}

provider "google" {
  project = var.project_id
  region  = var.region
}

# Variables
variable "project_id" { type = string }
variable "region"     { type = string  default = "asia-south1" }
variable "zone"       { type = string  default = "asia-south1-a" }

# VPC
resource "google_compute_network" "main" {
  name                    = "my-vpc"
  auto_create_subnetworks = false
}

resource "google_compute_subnetwork" "main" {
  name          = "my-subnet"
  ip_cidr_range = "10.0.1.0/24"
  region        = var.region
  network       = google_compute_network.main.id
  private_ip_google_access = true
}

# GKE Cluster
resource "google_container_cluster" "main" {
  name     = "my-cluster"
  location = var.zone

  # Use separately managed node pools
  remove_default_node_pool = true
  initial_node_count       = 1

  network    = google_compute_network.main.name
  subnetwork = google_compute_subnetwork.main.name

  workload_identity_config {
    workload_pool = "${var.project_id}.svc.id.goog"
  }
}

resource "google_container_node_pool" "main" {
  name       = "main-pool"
  location   = var.zone
  cluster    = google_container_cluster.main.name
  node_count = 2

  autoscaling {
    min_node_count = 1
    max_node_count = 10
  }

  node_config {
    machine_type = "e2-standard-4"
    disk_size_gb = 50
    disk_type    = "pd-ssd"

    workload_metadata_config {
      mode = "GKE_METADATA"
    }

    oauth_scopes = [
      "https://www.googleapis.com/auth/cloud-platform"
    ]
  }

  management {
    auto_repair  = true
    auto_upgrade = true
  }
}

# Cloud Storage bucket
resource "google_storage_bucket" "artifacts" {
  name          = "my-artifacts-${var.project_id}"
  location      = var.region
  force_destroy = false

  uniform_bucket_level_access = true

  versioning {
    enabled = true
  }

  lifecycle_rule {
    action { type = "Delete" }
    condition { age = 90 }
  }
}

# Outputs
output "cluster_endpoint" {
  value     = google_container_cluster.main.endpoint
  sensitive = true
}

output "bucket_url" {
  value = google_storage_bucket.artifacts.url
}
```

```bash
# Initialize and deploy
terraform init
terraform plan -var="project_id=my-project"
terraform apply -var="project_id=my-project" -auto-approve
terraform destroy -var="project_id=my-project"

# Remote state
terraform init \
  -backend-config="bucket=my-tfstate-bucket" \
  -backend-config="prefix=production/terraform.tfstate"
```

### Deployment Manager (Native GCP IaC)

```yaml
# deployment.yaml
resources:
- name: my-vm
  type: compute.v1.instance
  properties:
    zone: asia-south1-a
    machineType: zones/asia-south1-a/machineTypes/e2-medium
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/ubuntu-os-cloud/global/images/family/ubuntu-2204-lts
    networkInterfaces:
    - network: global/networks/default
```

```bash
gcloud deployment-manager deployments create my-deployment \
  --config=deployment.yaml

gcloud deployment-manager deployments list
gcloud deployment-manager deployments delete my-deployment
```

---

## 💰 GCP Pricing & Cost Management

### Pricing Models

| Model | How it Works | Savings |
|-------|-------------|---------|
| On-demand | Pay per second | Baseline |
| Sustained Use Discount | Auto after 25% month usage | Up to 30% |
| Committed Use (1yr) | 1-year resource commitment | Up to 37% |
| Committed Use (3yr) | 3-year resource commitment | Up to 55% |
| Spot VMs | Interruptible, spare capacity | Up to 91% |
| Free tier | Always-free products | Free |

### Always-Free Products

```
Compute:
  1 f1-micro VM per month (us regions only)
  30 GB HDD persistent disk

Storage:
  5 GB Cloud Storage (US regions)

Databases:
  1 GB Firestore storage
  10 GB BigQuery storage + 1 TB queries/month

Networking:
  1 GB egress to most destinations/month

Operations:
  Cloud Logging: first 50 GB/month
  Cloud Monitoring: basic features
```

### GCP Pricing Calculator
Estimate costs: [cloud.google.com/products/calculator](https://cloud.google.com/products/calculator)

### Cloud Billing & Budget Alerts

```bash
# Create budget alert
gcloud billing budgets create \
  --billing-account=BILLING_ACCOUNT_ID \
  --display-name="Monthly Budget" \
  --budget-amount=1000USD \
  --threshold-rule=percent=50,basis=CURRENT_SPEND \
  --threshold-rule=percent=90,basis=CURRENT_SPEND \
  --threshold-rule=percent=100,basis=CURRENT_SPEND \
  --notifications-rule-pubsub-topic=projects/my-project/topics/budget-alerts

# Export billing to BigQuery (for analysis)
# Enable in Console: Billing → Billing export → BigQuery export
```

### Cost Optimization Tools

```bash
# View recommendations (right-sizing, idle resources)
gcloud recommender recommendations list \
  --project=my-project \
  --location=asia-south1-a \
  --recommender=google.compute.instance.MachineTypeRecommender

# View active VMs to find idle ones
gcloud compute instances list --format="table(name,machineType,status,lastStartTimestamp)"

# Use Labels for cost allocation
gcloud compute instances create my-vm \
  --labels=env=production,team=devops,project=myapp \
  --zone=asia-south1-a

# View label-based costs in Billing → Cost table (after BigQuery export)
```

### Committed Use Discounts

```bash
# Create 1-year committed use discount for N2 VMs
gcloud compute commitments create my-commitment \
  --plan=12-month \
  --region=asia-south1 \
  --resources=vcpu=8,memory=32GB
```

---

## 🖥️ gcloud CLI Cheat Sheet

```bash
# ── AUTH & CONFIG ──────────────────────────────────────────
gcloud auth login
gcloud auth application-default login
gcloud auth list
gcloud config set project my-project
gcloud config set compute/region asia-south1
gcloud config set compute/zone asia-south1-a
gcloud config list
gcloud config configurations create dev-profile
gcloud config configurations activate dev-profile

# ── COMPUTE ENGINE ────────────────────────────────────────
gcloud compute instances create my-vm --zone=asia-south1-a --machine-type=e2-medium --image-family=ubuntu-2204-lts --image-project=ubuntu-os-cloud
gcloud compute instances start  my-vm --zone=asia-south1-a
gcloud compute instances stop   my-vm --zone=asia-south1-a
gcloud compute instances delete my-vm --zone=asia-south1-a --quiet
gcloud compute instances list
gcloud compute ssh my-vm --zone=asia-south1-a
gcloud compute instances create-with-container my-vm --zone=asia-south1-a --container-image=nginx

# ── CLOUD STORAGE ─────────────────────────────────────────
gcloud storage buckets create gs://my-bucket --location=asia-south1
gcloud storage cp localfile.txt gs://my-bucket/
gcloud storage cp gs://my-bucket/file.txt ./localfile.txt
gcloud storage ls gs://my-bucket/
gcloud storage rm gs://my-bucket/file.txt
gcloud storage rsync -r ./localdir gs://my-bucket/dir/
gcloud storage buckets delete gs://my-bucket

# ── GKE ───────────────────────────────────────────────────
gcloud container clusters create-auto my-cluster --region=asia-south1
gcloud container clusters create my-cluster --zone=asia-south1-a --num-nodes=3
gcloud container clusters get-credentials my-cluster --zone=asia-south1-a
gcloud container clusters delete my-cluster --zone=asia-south1-a --quiet
gcloud container clusters list
gcloud container clusters upgrade my-cluster --master --cluster-version=1.28 --zone=asia-south1-a

# ── CLOUD RUN ─────────────────────────────────────────────
gcloud run deploy my-service --image gcr.io/my-project/my-app:latest --region=asia-south1 --allow-unauthenticated
gcloud run services list --region=asia-south1
gcloud run services describe my-service --region=asia-south1
gcloud run services delete my-service --region=asia-south1 --quiet
gcloud run revisions list --service=my-service --region=asia-south1
gcloud run services update-traffic my-service --to-latest --region=asia-south1

# ── CLOUD BUILD ───────────────────────────────────────────
gcloud builds submit --tag gcr.io/my-project/my-app:latest .
gcloud builds submit --config=cloudbuild.yaml .
gcloud builds list --limit=10
gcloud builds log BUILD_ID
gcloud builds triggers list
gcloud builds triggers run TRIGGER_ID

# ── ARTIFACT REGISTRY ─────────────────────────────────────
gcloud artifacts repositories create my-repo --repository-format=docker --location=asia-south1
gcloud artifacts docker images list asia-south1-docker.pkg.dev/my-project/my-repo
gcloud auth configure-docker asia-south1-docker.pkg.dev

# ── IAM ───────────────────────────────────────────────────
gcloud iam service-accounts create my-sa --display-name="My SA"
gcloud projects add-iam-policy-binding my-project --member="serviceAccount:my-sa@my-project.iam.gserviceaccount.com" --role="roles/storage.objectViewer"
gcloud projects get-iam-policy my-project
gcloud iam roles list --project=my-project

# ── SECRET MANAGER ────────────────────────────────────────
gcloud secrets create my-secret --data-file=-
gcloud secrets versions access latest --secret=my-secret
gcloud secrets list

# ── SQL ───────────────────────────────────────────────────
gcloud sql instances create my-db --database-version=POSTGRES_15 --tier=db-g1-small --region=asia-south1
gcloud sql databases create mydb --instance=my-db
gcloud sql users create myuser --instance=my-db --password=MyP@ss!
gcloud sql instances list

# ── NETWORKING ────────────────────────────────────────────
gcloud compute networks create my-vpc --subnet-mode=custom
gcloud compute networks subnets create my-subnet --network=my-vpc --region=asia-south1 --range=10.0.1.0/24
gcloud compute firewall-rules create allow-http --network=my-vpc --allow=tcp:80 --target-tags=http-server
gcloud compute firewall-rules list

# ── MONITORING ────────────────────────────────────────────
gcloud logging read 'severity>=ERROR' --limit=20
gcloud logging tail 'resource.type="cloud_run_revision"'
gcloud monitoring dashboards list
```

---

## 📖 Study Resources

| Resource | Link |
|----------|------|
| GCP Documentation | [cloud.google.com/docs](https://cloud.google.com/docs) |
| ACE Exam (Cloud Engineer) | [cloud.google.com/certification/cloud-engineer](https://cloud.google.com/certification/cloud-engineer) |
| PCA Exam (Cloud Architect) | [cloud.google.com/certification/cloud-architect](https://cloud.google.com/certification/cloud-architect) |
| DevOps Engineer Exam | [cloud.google.com/certification/cloud-devops-engineer](https://cloud.google.com/certification/cloud-devops-engineer) |
| Google Cloud Skills Boost | [cloudskillsboost.google](https://cloudskillsboost.google) |
| GCP Pricing Calculator | [cloud.google.com/products/calculator](https://cloud.google.com/products/calculator) |
| Cloud Architecture Center | [cloud.google.com/architecture](https://cloud.google.com/architecture) |
| GCP GitHub Samples | [github.com/GoogleCloudPlatform](https://github.com/GoogleCloudPlatform) |
| GCP Free Tier | [cloud.google.com/free](https://cloud.google.com/free) |

---

<div align="center">

**Made by Manish Deore**

*Star ⭐ this repo if it helps you!*
*Feel free to contribute, raise issues, or suggest improvements!*

</div>
