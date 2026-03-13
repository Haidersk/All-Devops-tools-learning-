# ☁️ Google Cloud Platform: Complete Mastery Guide
### From Absolute Beginner to All GCP Certifications

> **Taught by:** Senior GCP Cloud Architect & DevOps Engineer perspective | 15+ years in production
> **Goal:** Pass every GCP certification and design production-grade cloud architectures on Google Cloud

---

## 🎓 CERTIFICATION ROADMAP

```
FOUNDATIONAL              ASSOCIATE              PROFESSIONAL
────────────              ─────────              ────────────
Cloud Digital Leader  →  Associate Cloud    →   Cloud Architect
                         Engineer           →   Cloud Developer
                                            →   DevOps Engineer
                                            →   Cloud Security Engineer
                                            →   Cloud Network Engineer
                                            →   Data Engineer
                                            →   ML Engineer
```

---

## 📚 TABLE OF CONTENTS

**PART 1: BEGINNER — Digital Leader / Associate Level**
1. [What is Cloud Computing?](#1-what-is-cloud-computing)
2. [Cloud Deployment Models](#2-cloud-deployment-models)
3. [What is Google Cloud Platform?](#3-what-is-google-cloud-platform)
4. [GCP Global Infrastructure](#4-gcp-global-infrastructure)
5. [Google Cloud Free Tier](#5-google-cloud-free-tier)
6. [Shared Responsibility Model](#6-shared-responsibility-model)
7. [IAM Fundamentals](#7-iam-fundamentals)
8. [gcloud CLI Setup](#8-gcloud-cli-setup)
9. [Compute Engine Basics](#9-compute-engine-basics)
10. [Cloud Storage Basics](#10-cloud-storage-basics)
11. [VPC Fundamentals](#11-vpc-fundamentals)

**PART 2: INTERMEDIATE — Associate Cloud Engineer Level**
12. [Compute Engine Deep Dive](#12-compute-engine-deep-dive)
13. [Firewall Rules](#13-firewall-rules)
14. [VPC Architecture Deep Dive](#14-vpc-architecture-deep-dive)
15. [Persistent Disk vs Filestore](#15-persistent-disk-vs-filestore)
16. [Cloud Storage Classes & Lifecycle](#16-cloud-storage-classes--lifecycle)
17. [Cloud Load Balancing](#17-cloud-load-balancing)
18. [Managed Instance Groups](#18-managed-instance-groups)
19. [Cloud DNS](#19-cloud-dns)
20. [Cloud Monitoring & Logging](#20-cloud-monitoring--logging)
21. [Cloud SQL](#21-cloud-sql)
22. [Cloud CDN](#22-cloud-cdn)
23. [App Engine](#23-app-engine)
24. [Cloud Functions](#24-cloud-functions)
25. [API Gateway](#25-api-gateway)

**PART 3: DEVOPS / ENGINEER LEVEL**
26. [Infrastructure as Code: Terraform on GCP](#26-terraform-on-gcp)
27. [CI/CD with Cloud Build](#27-cicd-with-cloud-build)
28. [Artifact Registry](#28-artifact-registry)
29. [Cloud Run](#29-cloud-run)
30. [Google Kubernetes Engine (GKE)](#30-gke)
31. [Secret Manager](#31-secret-manager)
32. [Cloud Operations Suite](#32-cloud-operations-suite)
33. [Deployment Manager](#33-deployment-manager)
34. [Observability Architecture](#34-observability-architecture)

**PART 4: ADVANCED ARCHITECTURE — Professional Level**
35. [Highly Available Architecture Design](#35-highly-available-architecture)
36. [Multi-Region Architecture](#36-multi-region-architecture)
37. [Disaster Recovery Strategies](#37-disaster-recovery-strategies)
38. [Hybrid & Multi-Cloud Architecture](#38-hybrid--multi-cloud)
39. [VPC Peering & Shared VPC](#39-vpc-peering--shared-vpc)
40. [Performance Optimization](#40-performance-optimization)
41. [Cost Optimization](#41-cost-optimization)
42. [Scalable Distributed Systems](#42-scalable-distributed-systems)

**PART 5: EXPERT / SPECIALTY LEVEL**
43. [Advanced GCP Networking](#43-advanced-gcp-networking)
44. [Security Architecture & Compliance](#44-security-architecture--compliance)
45. [IAM Policy Deep Dive](#45-iam-policy-deep-dive)
46. [Encryption & Cloud KMS](#46-encryption--cloud-kms)
47. [Production Incident Debugging](#47-production-incident-debugging)
48. [Observability at Scale](#48-observability-at-scale)
49. [Large-Scale Troubleshooting](#49-large-scale-troubleshooting)
50. [Data Engineering & ML on GCP](#50-data-engineering--ml-on-gcp)

**APPENDIX**
- [GCP Architecture Best Practices](#best-practices)
- [Certification Exam Tips](#exam-tips)
- [Interview Questions Bank](#interview-questions)
- [Quick Reference Cheat Sheet](#cheat-sheet)

---

# ═══════════════════════════════════════
# PART 1: BEGINNER — DIGITAL LEADER / ASSOCIATE
# ═══════════════════════════════════════

---

## 1. What is Cloud Computing?

### Simple Explanation

Before the cloud existed, building software at scale looked like this:
- Buy physical servers ($5,000–$50,000 each)
- Rent or build data centers (power, cooling, physical security)
- Hire teams to manage hardware 24/7
- Wait **weeks or months** to add capacity
- Pay for servers whether they're busy or idle

**Cloud computing** flips this model completely: you rent computing resources over the internet and pay only for what you use, scaling up or down in seconds.

A useful analogy: electricity. You don't build your own power plant to power your home. You plug in and pay per kilowatt-hour. Cloud computing does the same for computing resources.

### NIST's 5 Essential Characteristics

```
1. ON-DEMAND SELF-SERVICE
   Provision compute, storage, network in seconds
   No human intervention from the provider
   "I need 50 VMs right now" → console click → done

2. BROAD NETWORK ACCESS
   Available over the internet from any device, anywhere

3. RESOURCE POOLING
   Provider serves many customers from shared infrastructure
   Multi-tenant (isolated but shared hardware)

4. RAPID ELASTICITY
   Scale up for traffic spikes, scale down when quiet
   Black Friday: auto-scale to 10× capacity, auto-shrink after

5. MEASURED SERVICE
   Pay per second, per GB, per request
   Like your water bill — metered and precise
```

### Cloud Service Models

```
┌──────────────────────────────────────────────────────────────┐
│          On-Premises    IaaS         PaaS         SaaS       │
│          ──────────     ────         ────         ────       │
│ YOU      Applications  Apps         Apps         Nothing    │
│ MANAGE   Data          Data         Data                     │
│          Runtime       Runtime                               │
│          Middleware    Middleware                             │
│          OS            OS                                    │
│          Virtualization                                      │
│          Servers                                             │
│          Storage                                             │
│          Networking                                          │
│                                                              │
│ GCP      Nothing       Compute      App Engine   Workspace  │
│ MANAGES  Nothing       Engine, VPC  Cloud Run    BigQuery   │
│                        GCS          Cloud SQL    Gmail      │
└──────────────────────────────────────────────────────────────┘
```

### 6 Key Benefits of Cloud (Exam Critical)

```
1. TRADE CAPITAL EXPENSE FOR VARIABLE EXPENSE
   Stop buying hardware → pay as you go

2. ECONOMIES OF SCALE
   Google buys millions of servers → you get lower prices

3. STOP GUESSING CAPACITY
   Provision what you need, when you need it

4. INCREASE SPEED AND AGILITY
   Deploy globally in minutes, experiment faster

5. STOP SPENDING ON DATA CENTERS
   Focus engineering on product, not infrastructure

6. GO GLOBAL IN MINUTES
   Google has regions on every continent
```

### Real-World Production Example: Spotify on GCP

Spotify migrated to GCP and:
- Scaled from 50M to 400M+ monthly active users
- Reduced infrastructure management overhead dramatically
- Used BigQuery for music recommendation analytics
- Auto-scales streaming infrastructure during peak hours globally

### 🎯 Practice Task
Before moving on, write down 3 problems your current or hypothetical company has with on-premises infrastructure. Keep these in mind — cloud solutions to each will appear throughout this guide.

### 🏆 Exam-Style Question

**Q:** A retail company wants to handle 10× traffic spikes during holiday seasons without over-provisioning infrastructure year-round. Which cloud characteristic solves this?

A) Broad network access
B) Resource pooling
C) Rapid elasticity ✅
D) Measured service

---

## 2. Cloud Deployment Models

### The Three Core Models

**Public Cloud**
```
Definition:  Cloud resources owned by provider (Google, AWS, Azure)
             Delivered over the public internet
             Multiple organizations share infrastructure (isolated)

Examples:    Google Cloud, AWS, Microsoft Azure

Pros:        No CapEx, instant scale, global reach
Cons:        Less control, potential compliance concerns

Best for:    Startups, SaaS companies, variable workloads
             "We want zero infrastructure management"
```

**Private Cloud**
```
Definition:  Cloud infrastructure dedicated to ONE organization
             Can be on-premises or hosted by provider

Examples:    VMware vSphere on-prem, Google Distributed Cloud

Pros:        Full control, data sovereignty, deep customization
Cons:        CapEx required, your team manages it

Best for:    Banks, defense, heavily regulated industries
             "Data cannot leave our premises"
```

**Hybrid Cloud**
```
Definition:  Mix of public + private cloud, connected securely
             Workloads can move between environments

Examples:    On-prem data warehouse + GCP analytics
             Legacy apps on-prem + new apps on GCP

Pros:        Flexibility, gradual migration, compliance
Cons:        Complexity, networking overhead

Best for:    Enterprises mid-migration, compliance-heavy sectors
             "Keep sensitive data on-prem, burst to cloud"
```

### Multi-Cloud

```
Definition:  Using multiple cloud providers simultaneously

Example:     GCP for ML (best AI/ML tooling)
             AWS for specific services
             Azure for Microsoft workloads
             
Why?:        Avoid vendor lock-in
             Best-of-breed services
             Regulatory requirements (some countries mandate it)
             
GCP tool:    Anthos (manage workloads across GCP, AWS, Azure, on-prem)
```

### Exam Tip — Deployment Model Scenarios

```
"Must comply with GDPR, data must stay in EU"      → Hybrid or Private
"Startup, no infra budget, need global scale"      → Public cloud
"Migrating from on-prem, still have legacy apps"  → Hybrid
"Need AWS S3 and GCP BigQuery in one workflow"    → Multi-cloud
"Defense contractor, classified workloads"         → Private cloud
```

---

## 3. What is Google Cloud Platform?

### GCP at a Glance

**Google Cloud Platform (GCP)** is Google's suite of cloud services, built on the same infrastructure that runs Google Search, Gmail, YouTube, and Google Maps. GCP launched publicly in 2008.

```
GCP Market Position (2024):
AWS:   31%  ████████████████████████████████
Azure: 25%  █████████████████████████
GCP:   11%  ███████████  ← Growing fastest

Revenue: $33+ billion annually and accelerating
Strength: AI/ML, Data Analytics, Kubernetes, Networking
```

### Why GCP is Unique

```
1. GOOGLE-BUILT INFRASTRUCTURE
   Runs on the same network as Search, YouTube, Gmail
   Private fiber network spans the globe (Jupiter network)
   Network handles >40% of the world's internet traffic

2. AI/ML LEADERSHIP
   TensorFlow invented at Google
   Best AI/ML services: Vertex AI, AutoML, TPUs
   Gemini models (most capable AI, built-in to GCP)

3. KUBERNETES INVENTED AT GOOGLE
   Google created Kubernetes, open-sourced it
   GKE (Google Kubernetes Engine) is the most mature managed K8s

4. BIGQUERY
   Serverless data warehouse — analyze petabytes in seconds
   No competitor matches its price/performance for analytics

5. NETWORK QUALITY
   Premium Tier network = fastest global routing (private backbone)
   Standard Tier = cheaper, uses public internet routing
   You choose the quality/cost tradeoff
```

### GCP Service Categories

```
COMPUTE:         Compute Engine, GKE, Cloud Run, App Engine, Cloud Functions
STORAGE:         Cloud Storage, Persistent Disk, Filestore
DATABASES:       Cloud SQL, Cloud Spanner, Bigtable, Firestore, Memorystore
NETWORKING:      VPC, Cloud Load Balancing, Cloud DNS, Cloud CDN, Interconnect
SECURITY:        Cloud IAM, KMS, Secret Manager, Security Command Center
OPERATIONS:      Cloud Monitoring, Cloud Logging, Cloud Trace, Cloud Profiler
DEVOPS:          Cloud Build, Artifact Registry, Cloud Deploy, Source Repositories
CONTAINERS:      GKE, Cloud Run, Artifact Registry
SERVERLESS:      Cloud Functions, Cloud Run, App Engine, Workflows
DATA/ANALYTICS:  BigQuery, Dataflow, Dataproc, Pub/Sub, Data Fusion
AI/ML:           Vertex AI, AutoML, Vision API, Speech API, Translation API
HYBRID:          Anthos, Google Distributed Cloud, Transfer Appliance
```

---

## 4. GCP Global Infrastructure

### CRITICAL — Tested on Every GCP Exam

Understanding GCP's global infrastructure is foundational to designing resilient, performant architectures.

### Geography Hierarchy

```
MULTI-REGION
  │  Broad geographic area containing multiple regions
  │  Examples: US, Europe, Asia-Pacific
  │  Used by: Cloud Storage multi-region, BigQuery
  │
  └── REGION
        │  Specific geographic location with multiple zones
        │  Examples: us-central1 (Iowa), europe-west1 (Belgium)
        │            asia-northeast1 (Tokyo), us-east1 (S. Carolina)
        │  35+ regions globally (more than any competitor)
        │  Most services are REGION-scoped
        │
        └── ZONE
              A single data center deployment area within a region
              Examples: us-central1-a, us-central1-b, us-central1-c
              Typically 3+ zones per region
              Independent power, cooling, networking
              Resources in zone are isolated from other zone failures
```

### Understanding Zones — The Core HA Concept

```
WHY ZONES MATTER:
  If you deploy all VMs in us-central1-a and that zone fails
  → Your entire application goes down

  If you spread VMs across us-central1-a, -b, -c
  → One zone fails → 2/3 of your VMs still running → app stays up

ZONE FAILURE IS RARE but does happen:
  - Power failure in a data center
  - Hardware issues in a specific facility
  - Natural events affecting one building

RULE: Any production workload must be multi-zone minimum
      Critical workloads should be multi-region
```

### GCP Regions — Know These for Exams

```
NORTH AMERICA:
  us-central1       Iowa (low latency to central US, cheapest US region)
  us-east1          South Carolina
  us-east4          Northern Virginia
  us-east5          Columbus, Ohio
  us-west1          Oregon
  us-west2          Los Angeles
  us-west3          Salt Lake City
  us-west4          Las Vegas
  northamerica-northeast1  Montreal

EUROPE:
  europe-west1      Belgium
  europe-west2      London
  europe-west3      Frankfurt
  europe-west4      Netherlands
  europe-north1     Finland (carbon-neutral!)

ASIA PACIFIC:
  asia-east1        Taiwan
  asia-northeast1   Tokyo
  asia-northeast3   Seoul
  asia-south1       Mumbai
  asia-southeast1   Singapore
  australia-southeast1  Sydney

CHOOSING A REGION:
  1. Latency     → Where are your users?
  2. Compliance  → Does data need to stay in a country/region?
  3. Price       → us-central1 is typically cheapest
  4. Services    → Some services only in certain regions
```

### Google's Network — The Differentiator

```
JUPITER NETWORK (inside data centers):
  Google's proprietary data center fabric
  Petabit-scale bandwidth
  Software-defined — programmable networking

B4 BACKBONE (between data centers):
  Private fiber connecting all Google data centers globally
  NOT the public internet
  When you send data between GCP regions, it travels on Google's private backbone

ANDROMEDA (Software Defined Networking):
  GCP's virtual network stack
  Runs on Google's servers, not customer hardware
  Enables features like Cloud NAT, Alias IPs, VPC peering

NETWORK TIERS:
  Premium Tier:  Google's private global backbone (fastest, most reliable)
                 Traffic stays on Google's network as long as possible
  Standard Tier: Uses public internet for parts of the journey
                 30-40% cheaper, acceptable for less-critical traffic

KEY POINT: GCP's network performance is a genuine competitive advantage
           Not marketing — measurably lower latency than competitors for global apps
```

### Points of Presence (PoPs) & Edge Nodes

```
EDGE LOCATIONS / POPS:
  GCP has 180+ edge nodes worldwide
  Used for: Cloud CDN caching, Cloud Load Balancing traffic
            Traffic enters Google's network as close to the user as possible

Example:
  User in Mumbai requests data from your app in us-central1
  
  WITHOUT EDGE:
    Mumbai user → public internet (high latency, variable) → Iowa → response

  WITH CLOUD CDN + PREMIUM TIER:
    Mumbai user → GCP edge node in Mumbai → Google's private backbone → Iowa
    Result: Faster, more consistent, more reliable
```

### Console Lab: Explore GCP Regions

```bash
# Install gcloud SDK first (next module)
# Then explore regions:

# List all available regions
gcloud compute regions list

# List zones in a specific region
gcloud compute zones list --filter="region:us-central1"

# Get details about a region
gcloud compute regions describe us-central1

# See which services are available in which regions
# → console.cloud.google.com → select a service → check region dropdown
```

### 🏆 Exam-Style Questions

**Q1:** A company needs their application to survive a single data center failure. What is the MINIMUM deployment strategy?

A) Deploy in multiple regions
B) Deploy in multiple zones within a region ✅
C) Use Cloud CDN edge nodes
D) Deploy in a multi-region storage bucket

**Q2:** A financial services company in Germany must ensure all customer data is processed and stored only within the EU. Which GCP feature should they use?

A) Premium Tier networking
B) Multi-region Cloud Storage
C) A europe-west region with Organization Policy constraints ✅
D) Private Google Access

**Q3:** What is the PRIMARY advantage of Google's Premium network tier over Standard tier?

A) Lower cost
B) Traffic routes over Google's private global backbone for lower latency ✅
C) More data center locations
D) Better SSL/TLS encryption

---

## 5. Google Cloud Free Tier

### What's Available for Free

```
ALWAYS FREE (no expiry):
  Compute Engine:
    1 f1-micro VM per month (us-central1, us-west1, us-east1 only)
    30 GB HDD persistent disk
    5 GB Cloud Storage (US regions)
    
  Cloud Functions:
    2 million invocations/month
    400,000 GB-seconds compute
    
  BigQuery:
    1 TB of queries/month
    10 GB active storage
    
  Cloud Run:
    2 million requests/month
    
  Firestore:
    1 GB storage
    50,000 reads, 20,000 writes, 20,000 deletes per day
    
  Pub/Sub:
    10 GB messages/month

90-DAY FREE TRIAL:
  $300 in free credits
  Access to all GCP services
  Cannot exceed free trial limits (credit card required but not charged)
```

### Setting Up Billing Alerts

```bash
# Create a budget alert to avoid surprise charges
# Console: Billing → Budgets & alerts → Create budget

# Via CLI (after setting up gcloud):
gcloud billing budgets create \
  --billing-account=BILLING_ACCOUNT_ID \
  --display-name="Monthly Budget Alert" \
  --budget-amount=10 \
  --threshold-rule=percent=50 \
  --threshold-rule=percent=90 \
  --threshold-rule=percent=100

# ⚠️ ALWAYS set a budget before experimenting
# Even with free tier, configuration mistakes can incur charges
```

---

## 6. Shared Responsibility Model

### Google's Approach to Shared Security

```
┌─────────────────────────────────────────────────────────────┐
│                  CUSTOMER RESPONSIBILITY                     │
│              "Security IN the cloud"                         │
│                                                              │
│  ✓ Your data (encryption, classification)                   │
│  ✓ Access control (who can do what in your project)         │
│  ✓ Application code and configuration                       │
│  ✓ Guest OS on Compute Engine (patching!)                   │
│  ✓ Network firewall rules you configure                     │
│  ✓ IAM policies and roles you assign                        │
│  ✓ Compliance with regulations (GDPR, HIPAA, PCI-DSS)       │
├─────────────────────────────────────────────────────────────┤
│                  GOOGLE'S RESPONSIBILITY                     │
│              "Security OF the cloud"                         │
│                                                              │
│  ✓ Physical data center security                            │
│  ✓ Hardware and infrastructure                              │
│  ✓ Google's network security                                │
│  ✓ Hypervisor and virtualization layer                      │
│  ✓ Managed service infrastructure (Cloud SQL's OS, etc.)   │
│  ✓ Google's software stack security                         │
└─────────────────────────────────────────────────────────────┘
```

### How Responsibility Shifts by Service

```
COMPUTE ENGINE (IaaS) — You manage most:
  Google: Physical hardware, hypervisor, network
  You:    OS patches, middleware, app, data, IAM, firewall rules

CLOUD SQL (Managed DB) — Shared:
  Google: Hardware, OS, database software patches
  You:    Data, DB users/passwords, IAM, network access

CLOUD RUN (Serverless) — Mostly Google:
  Google: Hardware, OS, runtime, scaling, maintenance
  You:    Container code, IAM, environment variables, data

APP ENGINE (PaaS) — Mostly Google:
  Google: Everything except your app code
  You:    Application code, IAM, service configuration

BIGQUERY (SaaS-like) — Mostly Google:
  Google: Infrastructure, scaling, maintenance, storage format
  You:    Data, IAM permissions, query costs, dataset structure
```

### Real Breach Example — Customer's Fault

```
2019: A major company exposed 100M+ customer records from GCP
  
  What happened: Customer misconfigured Cloud Storage bucket
                 Made sensitive data publicly accessible
                 Google's infrastructure was FINE
                 
  Who was at fault: The customer
  Lesson: Google secures the cloud infrastructure
          YOU secure how you USE the cloud
          
  Prevention: 
    - Never allow public bucket access unless intentional
    - Use Organization Policy to block public storage
    - Enable Security Command Center
    - Regular security audits
```

### 🏆 Exam-Style Questions

**Q1:** A company stores sensitive data in Cloud Storage. An auditor finds the bucket is publicly accessible. Who is responsible?

A) Google, because they manage Cloud Storage
B) The customer, because access control is customer responsibility ✅
C) Both Google and the customer equally
D) No one — public access is default and expected

**Q2:** A Compute Engine VM has critical security vulnerabilities in the operating system. Who must patch it?

A) Google automatically patches all VM operating systems
B) The customer must patch their own VM OS ✅
C) Google patches it during maintenance windows
D) It depends on the machine type

---

## 7. IAM Fundamentals

### What is GCP IAM?

IAM (Identity and Access Management) controls **WHO** can do **WHAT** on **WHICH** GCP resource. It is the most critical security mechanism in GCP — everything flows through it.

```
IAM = WHO (Identity) + CAN DO WHAT (Role) + ON WHICH RESOURCE (Resource)

Example:
  alice@company.com  (WHO)
  roles/storage.objectViewer  (WHAT)
  projects/my-project/buckets/my-bucket  (WHICH)
```

### GCP Resource Hierarchy

```
ORGANIZATION (google.com, yourcompany.com)
  │  Root node — the whole company
  │  Set org-wide policies here
  │
  ├── FOLDER (optional grouping)
  │     Departments, teams, environments
  │     Example: "Production", "Development", "Finance-Dept"
  │
  └── PROJECT
        │  Core unit of GCP — everything belongs to a project
        │  Billing, APIs, resources all tied to project
        │  Isolated from other projects
        │
        └── RESOURCES
              VMs, Buckets, Databases, etc.
              Belong to one project

INHERITANCE:
  Policies set at org level → inherited by all folders and projects
  Policies set at folder level → inherited by child projects
  Policies set at project level → apply to that project only

DENY:
  Lower level CANNOT grant more than parent allows
  (unlike AWS where explicit deny always wins)
```

### IAM Components

**Members (WHO)**
```
Google Account:          individual@gmail.com or company Google account
Service Account:         app@project-id.iam.gserviceaccount.com
Google Group:            eng-team@company.com (group of accounts)
Google Workspace Domain: company.com (all users in domain)
Cloud Identity Domain:   Non-Workspace orgs
allAuthenticatedUsers:   Any authenticated Google account (careful!)
allUsers:                Anyone on the internet, no auth needed (dangerous!)
```

**Roles (WHAT) — Three Types**

```
1. BASIC ROLES (legacy, avoid for production):
   roles/viewer  — Read-only access
   roles/editor  — Read + write, can't change IAM
   roles/owner   — Everything including IAM changes
   
   ⚠️ Too broad! Owner/Editor = full access to everything
      Never use basic roles in production

2. PREDEFINED ROLES (recommended):
   Google-managed roles for specific services
   Fine-grained, principle of least privilege
   
   Examples:
   roles/compute.instanceAdmin.v1  — Full Compute Engine control
   roles/storage.objectViewer      — Read objects in Cloud Storage
   roles/bigquery.dataEditor       — Read/write BigQuery data
   roles/cloudsql.client           — Connect to Cloud SQL databases

3. CUSTOM ROLES (when predefined isn't right):
   You define exactly which permissions are included
   Most granular control
   Higher maintenance burden (permissions change over time)
```

**Service Accounts (Critical for DevOps)**

```
A Service Account represents an APPLICATION or WORKLOAD, not a human.

Why needed:
  Your Compute Engine VM needs to read from Cloud Storage
  → Don't use your personal credentials!
  → Create a Service Account with only storage.objectViewer
  → Attach SA to the VM
  → VM inherits those permissions (no keys needed!)

Two types:
  Google-managed: Auto-created for some services, Google handles keys
  User-managed:   You create these for your apps

Service Account Best Practices:
  ✓ One SA per application/service (not one for everything)
  ✓ Least privilege — only what the app needs
  ✓ Prefer Workload Identity over key files
  ✓ Rotate keys regularly if you must use them
  ✗ Don't download SA keys unless absolutely necessary
  ✗ Never commit SA key files to Git
```

### IAM Hands-On Lab

```bash
# ─── View current project IAM policy ───
gcloud projects get-iam-policy YOUR_PROJECT_ID

# ─── Add a member to a role ───
gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
  --member="user:alice@example.com" \
  --role="roles/viewer"

# ─── Remove a member from a role ───
gcloud projects remove-iam-policy-binding YOUR_PROJECT_ID \
  --member="user:alice@example.com" \
  --role="roles/viewer"

# ─── Grant role on a specific resource (not whole project) ───
gcloud storage buckets add-iam-policy-binding gs://my-bucket \
  --member="user:bob@example.com" \
  --role="roles/storage.objectViewer"

# ─── Create a Service Account ───
gcloud iam service-accounts create my-app-sa \
  --display-name="My Application Service Account" \
  --description="SA for my-app to access Cloud Storage"

# ─── Grant role to Service Account ───
gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
  --member="serviceAccount:my-app-sa@YOUR_PROJECT_ID.iam.gserviceaccount.com" \
  --role="roles/storage.objectViewer"

# ─── List roles ───
gcloud iam roles list --filter="name:roles/storage"

# ─── Describe a role (see its permissions) ───
gcloud iam roles describe roles/storage.objectViewer

# ─── Create custom role ───
gcloud iam roles create CustomStorageReader \
  --project=YOUR_PROJECT_ID \
  --title="Custom Storage Reader" \
  --description="Read GCS objects and list buckets" \
  --permissions="storage.objects.get,storage.objects.list,storage.buckets.list" \
  --stage="GA"

# ─── Check what you can do (policy troubleshooting) ───
gcloud auth list
gcloud config get-value account
```

### IAM Best Practices

```
1. ✅ Use predefined roles over basic roles
2. ✅ Principle of least privilege — minimum permissions needed
3. ✅ Use Service Accounts for applications (not personal accounts)
4. ✅ Use groups for managing access (add/remove users from group)
5. ✅ Audit with Cloud Audit Logs — who did what, when
6. ✅ Use Organization Policies for guardrails
7. ✅ Enable 2FA/MFA for all human accounts
8. ✅ Prefer Workload Identity Federation over SA keys
9. ✗ Never use allUsers/allAuthenticatedUsers for sensitive resources
10. ✗ Never commit service account key files to source control
```

### 🏆 Exam-Style Questions

**Q1:** An application running on Compute Engine needs to read from Cloud Storage. What is the MOST secure approach?

A) Create an IAM user and store credentials in the VM
B) Attach a Service Account with storage.objectViewer role to the VM ✅
C) Use the default compute service account with editor role
D) Store application credentials in environment variables

**Q2:** A developer needs to CREATE and DELETE Cloud Storage buckets, but should not be able to modify IAM policies. Which role is MOST appropriate?

A) roles/owner
B) roles/editor
C) roles/storage.admin ✅
D) roles/storage.objectAdmin

**Q3:** A team of 20 developers all need the same access to a GCP project. What is the MOST efficient way to manage this?

A) Add each developer individually with their specific role
B) Create a Google Group, add all developers, grant the group the appropriate role ✅
C) Use the allAuthenticatedUsers member
D) Create a service account shared by all developers

---

## 8. gcloud CLI Setup

### Why the CLI Matters

Every DevOps and Cloud engineer lives in the CLI. The gcloud CLI lets you automate everything, script deployments, build CI/CD pipelines, and work far faster than clicking through the console.

```bash
# ─── INSTALLATION ───

# Linux (Debian/Ubuntu)
sudo apt-get install apt-transport-https ca-certificates gnupg
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | \
  sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | \
  sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt-get update && sudo apt-get install google-cloud-cli

# macOS (Homebrew)
brew install --cask google-cloud-sdk

# Windows
# Download installer from: https://cloud.google.com/sdk/docs/install

# ─── INITIAL SETUP ───
gcloud init
# Walks you through:
# 1. Login with your Google account (opens browser)
# 2. Select or create a project
# 3. Set default region/zone

# ─── AUTHENTICATION ───
# Login as yourself
gcloud auth login

# Application Default Credentials (for local development of apps)
gcloud auth application-default login

# Verify who you are
gcloud auth list
gcloud config get-value account

# ─── PROJECT CONFIGURATION ───
# List your projects
gcloud projects list

# Set default project
gcloud config set project YOUR_PROJECT_ID

# Create a new project
gcloud projects create my-new-project \
  --name="My New Project" \
  --organization=ORG_ID

# ─── CONFIGURATION PROFILES ───
# Useful for switching between dev/staging/prod
gcloud config configurations create production
gcloud config set project prod-project-id
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
gcloud config set account prod-user@company.com

gcloud config configurations create development
gcloud config set project dev-project-id
# etc.

# Switch between configs
gcloud config configurations activate production
gcloud config configurations activate development

# List configs
gcloud config configurations list

# ─── ESSENTIAL COMMANDS PATTERN ───
gcloud [GROUP] [SUBGROUP] [COMMAND] [FLAGS]
# Examples:
gcloud compute instances list
gcloud storage buckets list
gcloud container clusters list
gcloud iam service-accounts list
gcloud sql instances list

# ─── OUTPUT FORMATS ───
gcloud compute instances list --format=json
gcloud compute instances list --format=yaml
gcloud compute instances list --format=table
gcloud compute instances list --format="table(name,zone,status,machineType)"

# ─── FILTERING ───
gcloud compute instances list --filter="zone:us-central1-a AND status:RUNNING"
gcloud compute instances list --filter="tags.items:web-server"

# ─── GET HELP ───
gcloud help
gcloud compute --help
gcloud compute instances --help
gcloud compute instances create --help
```

### Useful gcloud Shortcuts for Daily Use

```bash
# ─── Morning health check script ───
#!/bin/bash
PROJECT=$(gcloud config get-value project)
echo "=== GCP Health Check for $PROJECT ==="

echo -e "\n--- Running Compute Instances ---"
gcloud compute instances list \
  --filter="status:RUNNING" \
  --format="table(name,zone,machineType,networkInterfaces[0].accessConfigs[0].natIP)"

echo -e "\n--- GKE Clusters ---"
gcloud container clusters list \
  --format="table(name,location,status,currentNodeCount)"

echo -e "\n--- Cloud SQL Instances ---"
gcloud sql instances list \
  --format="table(name,databaseVersion,state,ipAddresses[0].ipAddress)"

echo -e "\n--- Recent Error Logs ---"
gcloud logging read \
  'severity=ERROR AND timestamp>="2024-01-15T00:00:00Z"' \
  --limit=10 \
  --format="table(timestamp,resource.type,textPayload)"
```

---

## 9. Compute Engine Basics

### What is Compute Engine?

**Compute Engine** is GCP's Infrastructure-as-a-Service (IaaS) — virtual machines running in Google's data centers. It's the foundation of most GCP architectures.

```
Traditional Server:          Compute Engine VM:
──────────────────           ─────────────────────
Buy physical hardware    →   Launch in 30 seconds
Rack, cable, power up    →   Available immediately
Fixed capacity           →   Resize on the fly
Pay months in advance    →   Pay per second
Your data center         →   Google's data center (you manage OS up)
Weeks to provision       →   gcloud compute instances create → done
```

### Machine Types

```
GENERAL PURPOSE (most common):
  E2:   Cost-optimized, flexible vCPU/memory ratios
        Best for: Dev/test, web servers, small-medium DBs
        e2-micro (0.25-2 vCPU, 1 GB) — FREE TIER eligible!
        e2-standard-4 (4 vCPU, 16 GB)
        
  N2/N2D: Balanced performance, Intel/AMD
        Best for: Web servers, app servers, medium-large DBs
        n2-standard-8 (8 vCPU, 32 GB)
        
  N4:   Newest general purpose, best price/performance
        n4-standard-8 (8 vCPU, 32 GB)

COMPUTE OPTIMIZED:
  C2:   High-frequency Intel Xeon processors
        Best for: Compute-intensive (ML inference, gaming, HPC)
        c2-standard-60 (60 vCPU, 240 GB)
        
  C3:   Latest generation compute
        c3-standard-44 (44 vCPU, 176 GB)

MEMORY OPTIMIZED:
  M2/M3:  Ultra-high memory for in-memory databases
        Best for: SAP HANA, in-memory analytics, Redis
        m2-ultramem-208 (208 vCPU, 5.75 TB RAM!)
        
ACCELERATOR OPTIMIZED:
  A2/A3:  NVIDIA GPU-attached VMs
        Best for: ML training, HPC, graphics rendering
        a2-highgpu-8g (8× NVIDIA A100 GPUs!)
        
  TPU:  Tensor Processing Units (Google's custom AI chip)
        Best for: Large ML model training (JAX, TensorFlow)
```

### Compute Engine Pricing Models

```
ON-DEMAND:
  Pay per second (minimum 1 minute)
  No commitment, full price
  Best for: Unpredictable workloads, short tasks

COMMITTED USE DISCOUNTS (CUDs):
  1-year: up to 37% discount
  3-year: up to 55% discount
  Commit to specific vCPU + memory
  Best for: Stable production workloads
  
SUSTAINED USE DISCOUNTS (automatic!):
  GCP automatically discounts when you run VM for >25% of month
  40-60 minutes running → 0% discount
  25% of month → ~20% discount
  100% of month → ~30% discount
  No action needed — it's automatic!
  
SPOT VMs (formerly Preemptible):
  Up to 90% cheaper than on-demand
  Can be stopped by Google with 30 seconds notice
  Will be stopped if Google needs capacity
  Max run time: 24 hours
  Best for: Batch jobs, ML training, fault-tolerant workloads
  NOT for: Databases, production web servers
```

### Launching Your First VM

**Console Steps:**
```
1. Go to console.cloud.google.com
2. Compute Engine → VM Instances → Create Instance
3. Name: my-first-vm
4. Region: us-central1, Zone: us-central1-a
5. Machine type: e2-micro (free tier)
6. Boot disk: Debian GNU/Linux 12 (Bookworm) — 10 GB
7. Firewall: ✓ Allow HTTP traffic, ✓ Allow HTTPS
8. Click CREATE
```

**Via gcloud CLI:**
```bash
# Set defaults (do this once)
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a

# Create VM
gcloud compute instances create my-web-server \
  --machine-type=e2-medium \
  --image-project=debian-cloud \
  --image-family=debian-12 \
  --boot-disk-size=20GB \
  --boot-disk-type=pd-standard \
  --tags=http-server,https-server \
  --metadata=startup-script='#!/bin/bash
    apt-get update
    apt-get install -y nginx
    systemctl start nginx
    systemctl enable nginx
    echo "<h1>Hello from $(hostname)</h1>" > /var/www/html/index.html' \
  --zone=us-central1-a

# List your VMs
gcloud compute instances list

# SSH into VM (no SSH keys to manage!)
gcloud compute ssh my-web-server --zone=us-central1-a

# Stop VM (saves money — stops compute charges, keeps disk)
gcloud compute instances stop my-web-server

# Start VM again
gcloud compute instances start my-web-server

# Delete VM (removes compute + disk by default)
gcloud compute instances delete my-web-server

# Resize VM (change machine type)
gcloud compute instances stop my-web-server
gcloud compute instances set-machine-type my-web-server \
  --machine-type=e2-standard-4
gcloud compute instances start my-web-server
```

### VM Metadata and Startup Scripts

```bash
# Metadata = key-value store for VM configuration
# Available to VM at: http://metadata.google.internal/

# On the VM, access metadata:
curl http://metadata.google.internal/computeMetadata/v1/instance/name \
  -H "Metadata-Flavor: Google"

curl http://metadata.google.internal/computeMetadata/v1/instance/zone \
  -H "Metadata-Flavor: Google"

# Access service account credentials (for apps)
curl http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token \
  -H "Metadata-Flavor: Google"

# Set custom metadata
gcloud compute instances add-metadata my-vm \
  --metadata=app-version=1.2.3,environment=production

# Startup script runs on FIRST BOOT
gcloud compute instances create my-vm \
  --metadata-from-file=startup-script=./startup.sh

# Shutdown script runs on shutdown
gcloud compute instances add-metadata my-vm \
  --metadata-from-file=shutdown-script=./shutdown.sh
```

### 🏆 Exam-Style Questions

**Q1:** A data science team runs ML training jobs that take 8 hours and can be interrupted. Which pricing model is MOST cost-effective?

A) On-demand VMs
B) Committed Use Discounts
C) Spot VMs ✅
D) Reserved Instances

**Q2:** A company wants to run SAP HANA which requires 4 TB of RAM. Which Compute Engine machine family should they use?

A) E2 (General Purpose)
B) C2 (Compute Optimized)
C) N2 (General Purpose Balanced)
D) M2 (Memory Optimized) ✅

---

## 10. Cloud Storage Basics

### What is Cloud Storage?

Cloud Storage is GCP's object storage service — scalable, durable, globally accessible storage for any type of file.

```
Object Storage Concepts:
  Object  = File + Metadata + globally unique URL
  Bucket  = Container for objects (like a top-level folder)
  Key     = Object path (e.g., "images/2024/photo.jpg")

Properties:
  Durability:   99.999999999% (11 nines) — essentially never loses data
  Availability: 99.9% - 99.99% depending on storage class
  Scale:        No limit on objects or total storage
  Access:       HTTPS URL, gcloud, client libraries, JSON API
```

### Cloud Storage Operations

```bash
# ─── Create a bucket ───
gcloud storage buckets create gs://my-unique-bucket-name \
  --location=us-central1 \
  --storage-class=STANDARD \
  --uniform-bucket-level-access   # ← Security best practice!

# ─── Upload files ───
gcloud storage cp file.txt gs://my-bucket/
gcloud storage cp file.txt gs://my-bucket/folder/file.txt
gcloud storage cp -r ./localdir/ gs://my-bucket/remotedir/

# ─── Download files ───
gcloud storage cp gs://my-bucket/file.txt .
gcloud storage cp -r gs://my-bucket/folder/ ./local/

# ─── List objects ───
gcloud storage ls gs://my-bucket
gcloud storage ls -l gs://my-bucket/   # Detailed listing
gcloud storage ls -r gs://my-bucket/   # Recursive

# ─── Sync (like rsync) ───
gcloud storage rsync ./local/ gs://my-bucket/ --recursive
gcloud storage rsync gs://my-bucket/ ./local/ --recursive

# ─── Delete ───
gcloud storage rm gs://my-bucket/file.txt
gcloud storage rm -r gs://my-bucket/folder/   # Delete folder

# ─── Make a file publicly accessible ───
gcloud storage objects update gs://my-bucket/public-file.txt \
  --predefined-acl=publicRead
# URL: https://storage.googleapis.com/my-bucket/public-file.txt

# ─── Signed URLs (temporary access without credentials) ───
gcloud storage sign-url gs://my-bucket/private-doc.pdf \
  --duration=1h \
  --private-key-file=service-account-key.json
```

### Cloud Storage Security

```bash
# GCP's RECOMMENDED security model: Uniform bucket-level access
# (disables per-object ACLs, use IAM instead — simpler and more consistent)

# Enable uniform bucket-level access
gcloud storage buckets update gs://my-bucket \
  --uniform-bucket-level-access

# Grant access via IAM (not ACLs)
gcloud storage buckets add-iam-policy-binding gs://my-bucket \
  --member="user:alice@example.com" \
  --role="roles/storage.objectViewer"

gcloud storage buckets add-iam-policy-binding gs://my-bucket \
  --member="serviceAccount:my-app@project.iam.gserviceaccount.com" \
  --role="roles/storage.objectAdmin"

# Prevent public access at ORG level (Organization Policy)
# Policy: constraints/storage.publicAccessPrevention = enforced
gcloud org-policies set-policy org-policy.yaml

# Enable versioning (protect against accidental deletion)
gcloud storage buckets update gs://my-bucket --versioning

# List all versions of an object
gcloud storage ls -a gs://my-bucket/file.txt

# Restore deleted object
gcloud storage cp \
  gs://my-bucket/file.txt#VERSION_ID \
  gs://my-bucket/file.txt
```

### Storage Classes (Preview — Deep Dive in Module 16)

```
STANDARD:     Hot data, frequent access, highest cost/GB, no retrieval fee
NEARLINE:     Accessed < once per month, lower cost, retrieval fee
COLDLINE:     Accessed < once per quarter, lower cost, higher retrieval fee  
ARCHIVE:      Accessed < once per year, lowest cost, highest retrieval fee
              Minimum 365-day storage duration

Rule of thumb:
  Frequently accessed (hourly/daily)  → STANDARD
  Monthly backups                     → NEARLINE
  Quarterly compliance archives       → COLDLINE
  7-year legal retention              → ARCHIVE
```

### 🏆 Exam-Style Questions

**Q1:** A company needs to store 10 TB of database backups that are accessed only during disaster recovery (roughly once per year). Which storage class minimizes cost?

A) Standard
B) Nearline
C) Coldline
D) Archive ✅

**Q2:** A development team accidentally deletes a critical file from Cloud Storage. What feature would have allowed recovery?

A) Cloud Storage replication
B) Object versioning ✅
C) Bucket locking
D) Uniform bucket-level access

---

## 11. VPC Fundamentals

### What is a VPC in GCP?

A **Virtual Private Cloud (VPC)** is your own private, isolated network within GCP. Understanding GCP's VPC is essential for every certification.

```
KEY DIFFERENCE FROM AWS:
  AWS VPC:  Regional resource — must recreate per region
  GCP VPC:  GLOBAL resource — one VPC spans ALL regions!
            Subnets are regional, but the VPC itself is global
            
  Example:
    AWS: Create VPC in us-east-1, create separate VPC in eu-west-1
         VPC peering required to connect them
    
    GCP: Create ONE VPC, add subnets in us-central1 AND europe-west1
         VMs across the globe in ONE VPC communicate privately!
```

### GCP VPC Components

```
VPC NETWORK (Global)
  │  Your private network — spans all regions
  │  Subnets define IP ranges within regions
  │  Routes control traffic flow
  │
  ├── SUBNET (Regional)
  │     IP range (CIDR) within a specific region
  │     Examples:
  │       us-central1: 10.0.0.0/24
  │       europe-west1: 10.1.0.0/24
  │       asia-east1: 10.2.0.0/24
  │     VMs in the subnet get IPs from this range
  │
  ├── FIREWALL RULES
  │     Control ingress/egress traffic to VMs
  │     Based on: IP ranges, tags, service accounts
  │     Stateful: return traffic automatically allowed
  │
  ├── ROUTES
  │     Define how traffic flows
  │     Default route: 0.0.0.0/0 → internet (default internet gateway)
  │     Custom routes for VPN, Interconnect, etc.
  │
  └── CLOUD NAT (for private VMs)
        Allows VMs WITHOUT public IPs to reach internet
        Outbound only — internet cannot initiate connections
        Managed service — no NAT VM to manage
```

### Default vs Custom Mode VPCs

```
DEFAULT VPC (auto mode):
  Created automatically with every new project
  Subnets auto-created in EVERY region (25+ subnets!)
  Uses predefined IP ranges (10.128.0.0/9)
  Easy to start with
  ⚠️ Not recommended for production (overlapping IPs, too permissive)

CUSTOM VPC (custom mode):
  You define subnets manually in specific regions
  You control IP ranges (plan carefully for peering!)
  Better for production
  Best practice: Plan IP scheme before creating
  
RECOMMENDATION:
  Development: Default VPC is fine to experiment
  Production: Always use custom VPC with planned IP ranges
```

### Subnet Design for Production

```
IP PLANNING EXAMPLE — single VPC across regions:

VPC Network: (no IP range — VPC is global)
  ├── Subnet: us-central1
  │     Range: 10.10.0.0/20 (us-central1) — 4096 IPs
  │     Secondary ranges:
  │       pods: 10.100.0.0/16 (for GKE pods)
  │       services: 10.200.0.0/20 (for GKE services)
  │
  ├── Subnet: europe-west1
  │     Range: 10.20.0.0/20 (europe) — 4096 IPs
  │
  └── Subnet: asia-east1
        Range: 10.30.0.0/20 (asia) — 4096 IPs

VMs in us-central1 can talk to VMs in europe-west1
PRIVATELY (no public IPs needed) — same VPC!

GCP reserves 4 IPs in each subnet:
  .0   = Network address
  .1   = Default gateway
  .2   = Reserved (DNS)
  .3   = Reserved for future use
  Last = Broadcast (not used but reserved)
```

### Hands-On Lab: Create Custom VPC

```bash
# ─── Create a custom VPC ───
gcloud compute networks create production-vpc \
  --subnet-mode=custom \
  --mtu=1460 \
  --bgp-routing-mode=regional   # or "global" for global dynamic routing

# ─── Create subnets ───
gcloud compute networks subnets create us-subnet \
  --network=production-vpc \
  --range=10.10.0.0/20 \
  --region=us-central1 \
  --enable-private-ip-google-access   # Allow private VMs to reach Google APIs

gcloud compute networks subnets create eu-subnet \
  --network=production-vpc \
  --range=10.20.0.0/20 \
  --region=europe-west1 \
  --enable-private-ip-google-access

# ─── Create firewall rules ───
# Allow internal traffic between subnets
gcloud compute firewall-rules create allow-internal \
  --network=production-vpc \
  --allow=tcp,udp,icmp \
  --source-ranges=10.0.0.0/8 \
  --description="Allow all internal traffic"

# Allow SSH from corporate IP only
gcloud compute firewall-rules create allow-ssh \
  --network=production-vpc \
  --allow=tcp:22 \
  --source-ranges=YOUR_CORPORATE_IP/32 \
  --target-tags=ssh-allowed \
  --description="Allow SSH from corporate network"

# Allow HTTP/HTTPS from anywhere (for web servers)
gcloud compute firewall-rules create allow-web \
  --network=production-vpc \
  --allow=tcp:80,tcp:443 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=web-server \
  --description="Allow HTTP and HTTPS"

# ─── Set up Cloud NAT (for private VMs) ───
# First create a Cloud Router
gcloud compute routers create us-nat-router \
  --network=production-vpc \
  --region=us-central1

# Create Cloud NAT
gcloud compute routers nats create us-nat-config \
  --router=us-nat-router \
  --region=us-central1 \
  --auto-allocate-nat-external-ips \
  --nat-all-subnet-ip-ranges

# ─── Verify ───
gcloud compute networks describe production-vpc
gcloud compute networks subnets list --filter="network:production-vpc"
gcloud compute firewall-rules list --filter="network:production-vpc"
```

### Private Google Access

```bash
# When enabled on subnet, VMs WITHOUT public IPs can reach:
# - Google APIs (Cloud Storage, BigQuery, etc.)
# - WITHOUT going through the internet
# Via: private.googleapis.com or restricted.googleapis.com

# Enable on subnet (already done above with --enable-private-ip-google-access)
gcloud compute networks subnets update us-subnet \
  --enable-private-ip-google-access \
  --region=us-central1

# Verify
gcloud compute networks subnets describe us-subnet \
  --region=us-central1 \
  --format="value(privateIpGoogleAccess)"
```

### 🏆 Exam-Style Questions

**Q1:** A company creates a single VPC network and adds subnets in us-central1 and europe-west1. A VM in us-central1 needs to communicate privately with a VM in europe-west1. What is required?

A) VPC Peering between the two regions
B) A VPN connection between regions
C) Nothing — they're in the same VPC and can communicate privately ✅
D) Cloud Interconnect between regions

**Q2:** Private VMs in a subnet need to download OS updates from the internet but should not have public IP addresses. What is the BEST solution?

A) Assign public IPs to all VMs
B) Use Cloud NAT ✅
C) Create a NAT VM in the subnet
D) Enable Private Google Access

---

### 🏆 BEGINNER COMPREHENSIVE CHECKPOINT

**Q1 (Scenario):** A startup wants to host their application globally with automatic scaling and no infrastructure management. They have no capital budget. Which cloud characteristic BEST addresses this?

A) Measured service
B) Rapid elasticity + On-demand self-service ✅
C) Resource pooling
D) Broad network access

**Q2:** Which of the following is the CUSTOMER'S responsibility when running Compute Engine?

A) Physical security of Google's data centers
B) Patching the hypervisor
C) Patching the VM operating system ✅
D) Ensuring the Google network hardware is secure

**Q3:** A GCP organization wants to ensure no Cloud Storage buckets in any project can be made publicly accessible. What should they configure?

A) Bucket-level ACLs on each bucket
B) An IAM policy denying allUsers
C) An Organization Policy constraint on storage.publicAccessPrevention ✅
D) A firewall rule blocking port 443

**Q4:** What makes GCP's VPC architecture different from most other cloud providers?

A) GCP VPCs are more expensive
B) GCP VPCs are GLOBAL resources — one VPC can span all regions ✅
C) GCP VPCs cannot be peered
D) GCP VPCs do not support custom IP ranges

**Q5:** A batch processing job runs for 6 hours and can tolerate interruption. Which Compute Engine pricing model provides the HIGHEST cost savings?

A) Committed Use Discounts
B) On-demand with sustained use discounts
C) Spot VMs ✅
D) E2 machine type

---

# ═══════════════════════════════════════
# PART 2: INTERMEDIATE — ASSOCIATE CLOUD ENGINEER
# ═══════════════════════════════════════

---

## 12. Compute Engine Deep Dive

### Custom Machine Types

```bash
# Not finding the right size? Create EXACTLY what you need
gcloud compute instances create custom-vm \
  --custom-cpu=6 \
  --custom-memory=20GB \
  --custom-vm-type=n2

# Extended memory (more RAM per CPU than standard)
gcloud compute instances create memory-vm \
  --custom-cpu=4 \
  --custom-memory=32GB \
  --custom-extensions   # Enable extended memory

# This is a major GCP differentiator:
# AWS/Azure: Pick from fixed sizes (2, 4, 8 vCPU with fixed RAM)
# GCP: Specify EXACT vCPU and memory combination you need
# Result: No over-provisioning, lower cost
```

### Sole-Tenant Nodes

```bash
# Sole-tenant: Your VMs run on physical hardware DEDICATED to you
# Use cases:
#   - Compliance (no sharing hardware with other customers)
#   - BYOL (Bring Your Own License) for Windows/SQL Server
#   - Performance isolation

gcloud compute sole-tenancy node-templates create my-template \
  --node-type=n1-node-96-624 \
  --region=us-central1

gcloud compute sole-tenancy node-groups create my-group \
  --node-template=my-template \
  --target-size=1 \
  --zone=us-central1-a

# Deploy VM to sole-tenant node
gcloud compute instances create licensed-vm \
  --node-group=my-group \
  --zone=us-central1-a
```

### Instance Templates

```bash
# Instance templates: reusable VM configuration
# Required for Managed Instance Groups (auto-scaling)
# Cannot be edited after creation — create new version

gcloud compute instance-templates create web-server-template \
  --machine-type=n2-standard-4 \
  --image-project=debian-cloud \
  --image-family=debian-12 \
  --boot-disk-size=50GB \
  --boot-disk-type=pd-ssd \
  --tags=http-server,https-server \
  --service-account=web-server-sa@PROJECT.iam.gserviceaccount.com \
  --scopes=cloud-platform \
  --metadata-from-file=startup-script=startup.sh \
  --region=us-central1

# List templates
gcloud compute instance-templates list

# Create instance from template
gcloud compute instances create my-vm \
  --source-instance-template=web-server-template \
  --zone=us-central1-a

# Update: create new template version
gcloud compute instance-templates create web-server-template-v2 \
  --source-instance-template=web-server-template \
  --machine-type=n2-standard-8   # Override specific field
```

### Snapshots and Images

```bash
# SNAPSHOT: Point-in-time backup of a persistent disk
# Incremental after first snapshot (efficient!)
gcloud compute disks snapshot my-vm-disk \
  --snapshot-names=my-vm-backup-$(date +%Y%m%d) \
  --zone=us-central1-a \
  --description="Pre-upgrade backup"

# Create disk from snapshot (restore)
gcloud compute disks create restored-disk \
  --source-snapshot=my-vm-backup-20240115 \
  --zone=us-central1-a

# CUSTOM IMAGE: Snapshot packaged for launching new VMs
# Use for: "Golden image" with all software pre-installed

# 1. Configure a VM exactly how you want it
# 2. Stop the VM
gcloud compute instances stop my-configured-vm --zone=us-central1-a

# 3. Create image from the VM's disk
gcloud compute images create golden-image-v1 \
  --source-disk=my-configured-vm \
  --source-disk-zone=us-central1-a \
  --family=my-app-image-family \
  --description="Production-ready app image with nginx, nodejs"

# 4. Use image in instance templates for all new VMs
gcloud compute instance-templates create app-template \
  --image=golden-image-v1 \
  --image-project=YOUR_PROJECT_ID \
  --machine-type=n2-standard-4

# List images
gcloud compute images list --filter="family:my-app-image-family"
```

---

## 13. Firewall Rules

### GCP Firewall Deep Dive

```
GCP FIREWALL PROPERTIES:
  ✓ Applied at NETWORK level (not subnet level)
  ✓ Stateful — return traffic automatically allowed
  ✓ INGRESS and EGRESS rules (both directions)
  ✓ Applied to VMs via TAGS or SERVICE ACCOUNTS
  ✓ Priority: 0-65535 (lower = higher priority, default=1000)
  ✓ Default: implied DENY ALL ingress, ALLOW ALL egress

TARGETING MECHANISMS:
  Network tags:       Apply rule to VMs with specific tag
  Service accounts:   Apply rule to VMs using specific SA (better!)
  IP ranges:          Apply rule to/from specific CIDRs
```

```bash
# ─── Creating firewall rules ───

# Allow HTTP/HTTPS from internet to VMs tagged "web-server"
gcloud compute firewall-rules create allow-web-ingress \
  --network=production-vpc \
  --direction=INGRESS \
  --priority=1000 \
  --action=ALLOW \
  --rules=tcp:80,tcp:443 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=web-server

# Allow VMs with "app-server" SA to connect to DB on port 5432
# (Service account-based — more secure than tags!)
gcloud compute firewall-rules create allow-app-to-db \
  --network=production-vpc \
  --direction=INGRESS \
  --priority=1000 \
  --action=ALLOW \
  --rules=tcp:5432 \
  --source-service-accounts=app-server-sa@PROJECT.iam.gserviceaccount.com \
  --target-service-accounts=db-server-sa@PROJECT.iam.gserviceaccount.com

# Block specific IP range (explicit deny)
gcloud compute firewall-rules create block-bad-ips \
  --network=production-vpc \
  --direction=INGRESS \
  --priority=500 \
  --action=DENY \
  --rules=all \
  --source-ranges=192.0.2.0/24

# Allow internal health check from Load Balancer
# (Required for GCP Load Balancer health checks!)
gcloud compute firewall-rules create allow-health-check \
  --network=production-vpc \
  --direction=INGRESS \
  --priority=1000 \
  --action=ALLOW \
  --rules=tcp:80,tcp:8080 \
  --source-ranges=130.211.0.0/22,35.191.0.0/16 \   # Google's LB health check IPs
  --target-tags=web-server

# ─── Hierarchical Firewall Policies (Org-level) ───
# Apply consistent firewall rules across ALL projects in org/folder

gcloud compute firewall-policies create org-policy \
  --description="Organization-wide firewall policy" \
  --global-organization

gcloud compute firewall-policies rules create 10000 \
  --firewall-policy=org-policy \
  --action=deny \
  --direction=INGRESS \
  --priority=10000 \
  --dest-ranges=10.0.0.0/8 \
  --description="Block all traffic from known bad ranges"

# Associate with organization
gcloud compute firewall-policies associations create \
  --firewall-policy=org-policy \
  --organization=ORG_ID

# ─── Verify rules ───
gcloud compute firewall-rules list --filter="network:production-vpc"
gcloud compute firewall-rules describe allow-web-ingress

# Test connectivity
gcloud compute instances create test-vm --zone=us-central1-a
gcloud compute ssh test-vm -- "curl -I http://TARGET_IP"
```

### Firewall Logging

```bash
# Enable logging on a rule to debug connectivity issues
gcloud compute firewall-rules update allow-web-ingress \
  --enable-logging

# View logs in Cloud Logging
gcloud logging read \
  'resource.type="gce_subnetwork" AND jsonPayload.rule_details.reference="compute.googleapis.com/projects/PROJECT/global/firewalls/allow-web-ingress"' \
  --limit=20
```

---

## 14. VPC Architecture Deep Dive

### VPC Modes and IP Addressing

```bash
# CIDR Planning for Enterprise:
# VPC has no IP range — it's global
# Each subnet has a primary IP range + optional secondary ranges

# Subnet secondary ranges (required for GKE!)
gcloud compute networks subnets create gke-subnet \
  --network=production-vpc \
  --region=us-central1 \
  --range=10.10.0.0/20 \              # Node IPs
  --secondary-range=pod-range=10.100.0.0/16 \   # Pod IPs
  --secondary-range=svc-range=10.200.0.0/20     # Service IPs

# List subnets with secondary ranges
gcloud compute networks subnets list \
  --filter="network:production-vpc" \
  --format="table(name,region,ipCidrRange,secondaryIpRanges)"
```

### Cloud Router and Dynamic Routing

```bash
# Cloud Router enables dynamic BGP routing
# Used with: Cloud VPN, Dedicated Interconnect, Partner Interconnect

gcloud compute routers create production-router \
  --network=production-vpc \
  --region=us-central1 \
  --asn=64512   # Your private BGP ASN

# Add BGP interface for VPN
gcloud compute routers add-interface production-router \
  --interface-name=vpn-interface \
  --vpn-tunnel=my-vpn-tunnel \
  --region=us-central1

# View router and BGP state
gcloud compute routers get-status production-router \
  --region=us-central1
```

### VPC Flow Logs

```bash
# Flow Logs: capture network traffic metadata for a subnet
# Use for: Security analysis, network debugging, compliance

gcloud compute networks subnets update us-subnet \
  --region=us-central1 \
  --enable-flow-logs \
  --logging-flow-sampling=0.5 \    # Sample 50% of flows (cost tradeoff)
  --logging-metadata=INCLUDE_ALL_METADATA

# Query flow logs
gcloud logging read \
  'resource.type="gce_subnetwork" AND logName="projects/PROJECT/logs/compute.googleapis.com%2Fvpc_flows"' \
  --format="json" | python3 -m json.tool | head -50

# Flow log record includes:
# - Source/destination IP and port
# - Protocol
# - Bytes and packets
# - Direction (ingress/egress)
# - Whether blocked by firewall
```

---

## 15. Persistent Disk vs Filestore

### Persistent Disk

```
GCP's block storage for Compute Engine VMs
Like an external hard drive attached to your VM
Can survive VM deletion (if configured)
Can be detached and attached to different VM

Types:
  pd-standard:   HDD — cheapest, good throughput, higher latency
                 Best for: sequential reads (backup, batch)
  pd-balanced:   SSD balanced — good balance of price and performance
                 Best for: most workloads
  pd-ssd:        SSD — fastest, most expensive
                 Best for: databases, low-latency requirements
  pd-extreme:    Extreme IOPS SSD — highest performance, configurable IOPS
                 Best for: mission-critical databases (Oracle, SAP HANA)
  hyperdisk-*:   New generation, decoupled IOPS/throughput from size
```

```bash
# Create persistent disk
gcloud compute disks create my-data-disk \
  --size=500GB \
  --type=pd-ssd \
  --zone=us-central1-a \
  --labels=env=production,team=data

# Attach to running VM
gcloud compute instances attach-disk my-vm \
  --disk=my-data-disk \
  --zone=us-central1-a \
  --mode=rw   # or ro for read-only

# ON THE VM: format and mount
# lsblk                          # See new disk
# sudo mkfs.ext4 /dev/sdb        # Format (first time only!)
# sudo mkdir /mnt/data
# sudo mount /dev/sdb /mnt/data  # Mount
# echo "/dev/sdb /mnt/data ext4 defaults 0 0" | sudo tee -a /etc/fstab

# Regional persistent disk (replicated across 2 zones for HA)
gcloud compute disks create ha-disk \
  --size=100GB \
  --type=pd-ssd \
  --region=us-central1 \
  --replica-zones=us-central1-a,us-central1-b

# Take snapshot (backup)
gcloud compute disks snapshot my-data-disk \
  --snapshot-names=data-backup-$(date +%Y%m%d) \
  --zone=us-central1-a

# Resize disk (online — no downtime!)
gcloud compute disks resize my-data-disk \
  --size=1000GB \
  --zone=us-central1-a
# Then on the VM: sudo resize2fs /dev/sdb
```

### Cloud Filestore

```
Filestore = Managed NFS file server
Shared file storage accessible from multiple VMs simultaneously
(EFS equivalent for GCP)

When to use Filestore vs Persistent Disk:
  Persistent Disk: One VM at a time (read-write)
  Filestore:       Multiple VMs simultaneously (shared NFS)

Tiers:
  Basic HDD:      1-63.9 TB, cost-effective
  Basic SSD:      2.5-63.9 TB, higher performance
  Enterprise:     1-10 TB per share, HA with regional replication
  High Scale SSD: 10 TB+, highest performance (HPC workloads)
```

```bash
# Create Filestore instance
gcloud filestore instances create nfs-server \
  --zone=us-central1-a \
  --tier=BASIC_SSD \
  --file-share=name="data",capacity=2.5TB \
  --network=name=production-vpc

# Get NFS IP
gcloud filestore instances describe nfs-server \
  --zone=us-central1-a \
  --format="value(networks[0].ipAddresses[0])"

# Mount on VM
# sudo apt-get install nfs-common
# sudo mkdir /mnt/shared
# sudo mount NFS_IP:/data /mnt/shared
# All VMs can now read/write to /mnt/shared simultaneously
```

### Storage Comparison

```
┌──────────────────────────────────────────────────────────────┐
│         Cloud Storage vs Persistent Disk vs Filestore        │
├────────────────┬─────────────┬──────────────┬───────────────┤
│                │Cloud Storage│Persistent Disk│  Filestore   │
├────────────────┼─────────────┼──────────────┼───────────────┤
│ Type           │ Object      │ Block         │ File (NFS)   │
│ Accessed by    │ Any (HTTP)  │ One VM (rw)   │ Multiple VMs │
│ Protocol       │ HTTPS/API   │ Local block   │ NFS          │
│ Latency        │ Medium      │ Low           │ Low-Medium   │
│ Best for       │ Backups,    │ OS, DB,       │ Shared home  │
│                │ files, media│ local storage │ dirs, CMS    │
│ Scaling        │ Unlimited   │ Up to 257 TB  │ Up to 100 TB │
└────────────────┴─────────────┴──────────────┴───────────────┘
```

---

## 16. Cloud Storage Classes & Lifecycle

### Storage Classes In Depth

```bash
# Create bucket with specific storage class
gcloud storage buckets create gs://my-archive-bucket \
  --location=us \
  --storage-class=ARCHIVE \
  --uniform-bucket-level-access

# Change storage class of existing object
gcloud storage objects update gs://my-bucket/old-file.zip \
  --storage-class=COLDLINE

# Check storage class of objects
gcloud storage ls -L gs://my-bucket/ | grep "Storage class"
```

### Lifecycle Management

```bash
# Lifecycle policy = automatically manage object storage classes and deletion

# Create lifecycle config file
cat > lifecycle.json << 'EOF'
{
  "lifecycle": {
    "rule": [
      {
        "action": {"type": "SetStorageClass", "storageClass": "NEARLINE"},
        "condition": {"age": 30, "matchesStorageClass": ["STANDARD"]}
      },
      {
        "action": {"type": "SetStorageClass", "storageClass": "COLDLINE"},
        "condition": {"age": 90, "matchesStorageClass": ["NEARLINE"]}
      },
      {
        "action": {"type": "SetStorageClass", "storageClass": "ARCHIVE"},
        "condition": {"age": 365, "matchesStorageClass": ["COLDLINE"]}
      },
      {
        "action": {"type": "Delete"},
        "condition": {"age": 2555}
      },
      {
        "action": {"type": "Delete"},
        "condition": {"numNewerVersions": 3, "isLive": false}
      }
    ]
  }
}
EOF

# Apply lifecycle policy
gcloud storage buckets update gs://my-bucket \
  --lifecycle-file=lifecycle.json

# View lifecycle policy
gcloud storage buckets describe gs://my-bucket \
  --format="json(lifecycle)"
```

### Object Lock and Retention

```bash
# Retention policy: objects cannot be deleted before retention period
# Use for: Compliance, WORM (Write Once Read Many) storage

# Set bucket retention policy (objects kept for at least 365 days)
gcloud storage buckets update gs://compliance-bucket \
  --retention-period=365d

# Lock the retention policy (CANNOT be shortened after this!)
gcloud storage buckets update gs://compliance-bucket \
  --lock-retention-policy

# Object holds: prevent deletion of specific objects
gcloud storage objects update gs://compliance-bucket/important-doc.pdf \
  --event-based-hold
```

---

## 17. Cloud Load Balancing

### GCP Load Balancer Types

```
GCP's load balancing is SOFTWARE-DEFINED and GLOBAL
No instances to manage, no IP to worry about at edge
Traffic enters Google's network at the closest edge location

LOAD BALANCER TYPES:
┌────────────────────────────────────────────────────────────────┐
│ Type              │ Layer │ Protocol   │ Scope  │ Use Case     │
├───────────────────┼───────┼────────────┼────────┼──────────────┤
│ Global HTTPS LB   │  7    │ HTTP/HTTPS │ Global │ Web apps     │
│ Global TCP Proxy  │  4    │ TCP        │ Global │ Non-HTTP TCP │
│ Global SSL Proxy  │  4    │ SSL/TLS    │ Global │ SSL offload  │
│ Regional HTTPS LB │  7    │ HTTP/HTTPS │ Region │ Internal web │
│ Regional Internal │  7    │ HTTP/HTTPS │ Region │ Microservices│
│ Network TCP/UDP   │  4    │ TCP/UDP    │ Region │ Gaming, IoT  │
│ Internal TCP/UDP  │  4    │ TCP/UDP    │ Region │ Internal svc │
└───────────────────┴───────┴────────────┴────────┴──────────────┘

MOST COMMONLY USED:
  External HTTPS LB (global): Public web applications, APIs
  Internal HTTPS LB (regional): Microservice-to-microservice (within VPC)
  Network LB: UDP, non-HTTP TCP, static IP needs
```

### External HTTPS Load Balancer Setup

```bash
# The HTTPS LB has multiple components to create:
# 1. Health check
# 2. Backend service
# 3. URL map (routing rules)
# 4. Target HTTPS proxy
# 5. Forwarding rule (the "front door")

# ─── Step 1: Create health check ───
gcloud compute health-checks create http app-health-check \
  --port=8080 \
  --request-path=/health \
  --check-interval=30s \
  --timeout=10s \
  --healthy-threshold=2 \
  --unhealthy-threshold=3

# ─── Step 2: Create backend service ───
gcloud compute backend-services create app-backend \
  --protocol=HTTP \
  --port-name=http \
  --health-checks=app-health-check \
  --global \
  --enable-logging \
  --logging-sample-rate=1.0   # Log all requests

# ─── Step 3: Add Managed Instance Group to backend ───
gcloud compute backend-services add-backend app-backend \
  --instance-group=us-app-mig \
  --instance-group-zone=us-central1-a \
  --balancing-mode=UTILIZATION \
  --max-utilization=0.8 \
  --global

gcloud compute backend-services add-backend app-backend \
  --instance-group=eu-app-mig \
  --instance-group-zone=europe-west1-b \
  --balancing-mode=UTILIZATION \
  --max-utilization=0.8 \
  --global

# ─── Step 4: URL map (routing rules) ───
gcloud compute url-maps create app-url-map \
  --default-service=app-backend

# Add path-based routing
gcloud compute url-maps import app-url-map --global << 'EOF'
defaultService: projects/PROJECT/global/backendServices/app-backend
hostRules:
- hosts:
  - www.myapp.com
  pathMatcher: main-paths
- hosts:
  - api.myapp.com
  pathMatcher: api-paths
pathMatchers:
- name: main-paths
  defaultService: projects/PROJECT/global/backendServices/app-backend
  pathRules:
  - paths: ["/images/*", "/static/*"]
    service: projects/PROJECT/global/backendBuckets/static-assets
- name: api-paths
  defaultService: projects/PROJECT/global/backendServices/api-backend
EOF

# ─── Step 5: SSL certificate ───
# Option A: Google-managed cert (automatic renewal!)
gcloud compute ssl-certificates create myapp-cert \
  --domains=www.myapp.com,api.myapp.com \
  --global

# ─── Step 6: HTTPS proxy ───
gcloud compute target-https-proxies create app-https-proxy \
  --url-map=app-url-map \
  --ssl-certificates=myapp-cert \
  --global

# ─── Step 7: Forwarding rule (global static IP) ───
gcloud compute addresses create myapp-ip \
  --global \
  --ip-version=IPV4

gcloud compute forwarding-rules create app-forwarding-rule \
  --global \
  --target-https-proxy=app-https-proxy \
  --ports=443 \
  --address=myapp-ip

# Get your global IP
gcloud compute addresses describe myapp-ip --global \
  --format="value(address)"

# ─── Verify ───
gcloud compute backend-services get-health app-backend --global
```

### Cloud Armor (DDoS + WAF)

```bash
# Create Cloud Armor security policy
gcloud compute security-policies create app-security-policy \
  --description="WAF and DDoS protection"

# Block specific countries (geo-restriction)
gcloud compute security-policies rules create 1000 \
  --security-policy=app-security-policy \
  --expression="origin.region_code == 'KP' || origin.region_code == 'IR'" \
  --action=deny-403

# Block SQL injection
gcloud compute security-policies rules create 2000 \
  --security-policy=app-security-policy \
  --expression="evaluatePreconfiguredExpr('sqli-stable')" \
  --action=deny-403

# Rate limiting (100 requests per minute from one IP)
gcloud compute security-policies rules create 3000 \
  --security-policy=app-security-policy \
  --expression="true" \
  --action=rate-based-ban \
  --rate-limit-threshold-count=100 \
  --rate-limit-threshold-interval-sec=60 \
  --ban-duration-sec=300

# Attach policy to backend service
gcloud compute backend-services update app-backend \
  --security-policy=app-security-policy \
  --global
```

---

## 18. Managed Instance Groups

### What are MIGs?

```
Managed Instance Group = Group of IDENTICAL VMs
  - Created from an instance template
  - Autoscaling (add/remove VMs based on load)
  - Auto-healing (replace unhealthy VMs automatically)
  - Rolling updates (update all VMs with zero downtime)
  - Multi-zone (spread across zones for HA)
  - Load balancer integration

Types:
  Zonal MIG:    All VMs in one zone (single zone failure risk)
  Regional MIG: VMs spread across multiple zones (recommended!)
```

```bash
# Create Regional MIG (highly available)
gcloud compute instance-groups managed create app-mig \
  --template=web-server-template \
  --size=3 \
  --region=us-central1 \
  --distribution-policy-zones=us-central1-a,us-central1-b,us-central1-c

# ─── Autoscaling ───
gcloud compute instance-groups managed set-autoscaling app-mig \
  --region=us-central1 \
  --min-num-replicas=2 \
  --max-num-replicas=20 \
  --target-cpu-utilization=0.6 \
  --cool-down-period=120

# Scale based on Load Balancer capacity instead of CPU
gcloud compute instance-groups managed set-autoscaling app-mig \
  --region=us-central1 \
  --min-num-replicas=2 \
  --max-num-replicas=20 \
  --target-load-balancing-utilization=0.8

# Scale based on custom Cloud Monitoring metric
gcloud compute instance-groups managed set-autoscaling app-mig \
  --region=us-central1 \
  --min-num-replicas=2 \
  --max-num-replicas=20 \
  --custom-metric-utilization=metric=custom.googleapis.com/myapp/queue_depth,utilization-target=100

# ─── Auto-healing ───
gcloud compute instance-groups managed update app-mig \
  --region=us-central1 \
  --health-check=app-health-check \
  --initial-delay=300   # Give new VMs 5 min to start before health check

# ─── Rolling Updates ───
# Update to new template with zero downtime
gcloud compute instance-groups managed rolling-action start-update app-mig \
  --version=template=web-server-template-v2 \
  --region=us-central1 \
  --max-surge=3 \           # Add 3 new VMs before removing old ones
  --max-unavailable=0       # Never have fewer than desired count

# Canary deployment (10% new version)
gcloud compute instance-groups managed rolling-action start-update app-mig \
  --region=us-central1 \
  --version=template=web-server-template-v1,name=stable \
  --canary-version=template=web-server-template-v2,target-size=10%,name=canary

# Monitor update
gcloud compute instance-groups managed describe app-mig \
  --region=us-central1 \
  --format="value(status)"

# ─── Monitor MIG ───
gcloud compute instance-groups managed list-instances app-mig \
  --region=us-central1 \
  --format="table(name,zone,status,lastAttempt.errors)"
```

---

## 19. Cloud DNS

### What is Cloud DNS?

```
Fully managed, authoritative DNS service
100% uptime SLA
Anycast DNS — responses from nearest PoP globally
Used for: Public domains, Private DNS in VPC, Split-horizon DNS
```

```bash
# ─── Create DNS zone ───
# Public zone (accessible from internet)
gcloud dns managed-zones create myapp-zone \
  --dns-name=myapp.com. \
  --description="Main application zone" \
  --visibility=public

# Private zone (only within VPC)
gcloud dns managed-zones create internal-zone \
  --dns-name=internal.myapp.com. \
  --description="Internal service discovery" \
  --visibility=private \
  --networks=production-vpc

# ─── Create records ───
# A record pointing to LB IP
gcloud dns record-sets create www.myapp.com. \
  --type=A \
  --ttl=300 \
  --rrdatas=34.102.236.194 \
  --zone=myapp-zone

# CNAME record
gcloud dns record-sets create api.myapp.com. \
  --type=CNAME \
  --ttl=300 \
  --rrdatas=www.myapp.com. \
  --zone=myapp-zone

# MX records for email
gcloud dns record-sets create myapp.com. \
  --type=MX \
  --ttl=3600 \
  --rrdatas="10 mail.myapp.com.,20 mail2.myapp.com." \
  --zone=myapp-zone

# ─── Routing policies (advanced) ───
# Weighted routing (A/B testing or gradual migration)
gcloud dns record-sets create weighted.myapp.com. \
  --type=A \
  --ttl=30 \
  --zone=myapp-zone \
  --routing-policy-type=WRR \
  --routing-policy-data="90=10.0.1.1;10=10.0.1.2"   # 90% v1, 10% v2

# Geo routing
gcloud dns record-sets create geo.myapp.com. \
  --type=A \
  --ttl=30 \
  --zone=myapp-zone \
  --routing-policy-type=GEO \
  --routing-policy-data="us-central1=10.0.1.1;europe-west1=10.0.2.1"

# ─── Verify DNS ───
gcloud dns record-sets list --zone=myapp-zone
# From a VM:
# dig www.myapp.com
# nslookup www.myapp.com
```

---

## 20. Cloud Monitoring & Logging

### Cloud Monitoring

```bash
# ─── Custom metrics ───
# Send custom metrics from your application
gcloud monitoring metrics-scopes create \
  --project=monitoring-project \
  --scoped-project=app-project-1 \
  --scoped-project=app-project-2

# Create alerting policy via CLI
gcloud alpha monitoring policies create --policy-from-file=alert-policy.yaml
```

```yaml
# alert-policy.yaml — Alert when CPU > 80% for 5 minutes
displayName: "High CPU Alert"
combiner: OR
conditions:
  - displayName: "CPU utilization > 80%"
    conditionThreshold:
      filter: >
        resource.type = "gce_instance"
        AND metric.type = "compute.googleapis.com/instance/cpu/utilization"
      comparison: COMPARISON_GT
      thresholdValue: 0.8
      duration: 300s   # 5 minutes
      aggregations:
        - alignmentPeriod: 60s
          perSeriesAligner: ALIGN_MEAN
notificationChannels:
  - projects/PROJECT/notificationChannels/CHANNEL_ID
alertStrategy:
  autoClose: 1800s  # Auto-close after 30 min of not firing
```

```bash
# ─── Create notification channel ───
gcloud alpha monitoring channels create \
  --display-name="DevOps Team Email" \
  --type=email \
  --channel-labels=email_address=devops@company.com

# ─── Create uptime check ───
gcloud monitoring uptime create \
  --display-name="App Homepage Check" \
  --resource-type=uptime-url \
  --monitored-resource="host=www.myapp.com" \
  --check-interval=60 \
  --http-check-path="/" \
  --port=443 \
  --use-ssl

# ─── Dashboards ───
# Console: Monitoring → Dashboards → Create Dashboard
# Add charts for: CPU, memory, request count, error rate, latency

# ─── Install Ops Agent (for VM memory/disk metrics) ───
# On the VM:
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
sudo bash add-google-cloud-ops-agent-repo.sh --also-install

# Configure Ops Agent for application logs
sudo tee /etc/google-cloud-ops-agent/config.yaml << 'EOF'
logging:
  receivers:
    app-logs:
      type: files
      include_paths:
        - /var/log/myapp/*.log
  service:
    pipelines:
      app-pipeline:
        receivers: [app-logs]
metrics:
  receivers:
    host:
      type: hostmetrics
  service:
    pipelines:
      host-metrics:
        receivers: [host]
EOF

sudo systemctl restart google-cloud-ops-agent
```

### Cloud Logging

```bash
# ─── View logs ───
# Recent errors across all resources
gcloud logging read 'severity=ERROR' --limit=20 --freshness=1h

# Application logs
gcloud logging read \
  'resource.type="gce_instance" AND logName="projects/PROJECT/logs/myapp"' \
  --limit=50 \
  --format="table(timestamp,jsonPayload.message)"

# Security audit logs
gcloud logging read \
  'logName="projects/PROJECT/logs/cloudaudit.googleapis.com%2Factivity"' \
  --limit=20

# ─── Log-based metrics (count occurrences) ───
gcloud logging metrics create error-rate \
  --description="Count of application errors" \
  --log-filter='resource.type="gce_instance" AND severity=ERROR'

# ─── Log exports (sinks) ───
# Export logs to BigQuery for long-term analysis
gcloud logging sinks create bigquery-sink \
  bigquery.googleapis.com/projects/PROJECT/datasets/logs \
  --log-filter='resource.type="gce_instance"'

# Export ALL logs to Cloud Storage
gcloud logging sinks create storage-sink \
  storage.googleapis.com/my-log-archive-bucket \
  --log-filter=""   # Empty = all logs

# ─── Log retention ───
# Default: 30 days for most logs, 400 days for admin activity
# Extend with _Default log bucket update:
gcloud logging buckets update _Default \
  --location=global \
  --retention-days=365

# ─── Useful log queries (Cloud Logging UI) ───
# HTTP 5xx errors on Load Balancer:
# resource.type="http_load_balancer"
# httpRequest.status >= 500

# SSH login attempts:
# resource.type="gce_instance"
# protoPayload.methodName="v1.compute.instances.setMetadata"

# Cloud SQL slow queries:
# resource.type="cloudsql_database"
# logName:"cloudsql.googleapis.com/postgres.log"
# textPayload:"duration"
```

---

## 21. Cloud SQL

### What is Cloud SQL?

```
Cloud SQL = Fully managed relational database service
Engines:  MySQL (5.7, 8.0), PostgreSQL (up to 15), SQL Server

CLOUD SQL MANAGES:
  ✓ Database software patching
  ✓ Backups (automated daily, on-demand)
  ✓ High availability (regional, automatic failover)
  ✓ Read replicas
  ✓ Failover promotion
  ✓ SSL/TLS for connections
  ✓ Encryption at rest

YOU MANAGE:
  ✓ Database users and passwords
  ✓ Database schema
  ✓ Query optimization
  ✓ Connection management
  ✓ IAM database authentication (optional)
```

```bash
# ─── Create Cloud SQL instance ───
gcloud sql instances create prod-postgres \
  --database-version=POSTGRES_15 \
  --cpu=4 \
  --memory=16384MB \
  --region=us-central1 \
  --availability-type=REGIONAL \   # HA with failover replica
  --storage-type=SSD \
  --storage-size=100GB \
  --storage-auto-increase \
  --backup-start-time=02:00 \
  --enable-bin-log \               # For point-in-time recovery
  --retained-backups-count=7 \
  --retained-transaction-log-days=7 \
  --deletion-protection \
  --database-flags=max_connections=500

# ─── Create database ───
gcloud sql databases create myapp-db \
  --instance=prod-postgres

# ─── Create user ───
gcloud sql users create appuser \
  --instance=prod-postgres \
  --password=SuperSecret123!

# ─── Connect to Cloud SQL ───
# Option 1: Cloud SQL Auth Proxy (recommended for apps)
# Handles IAM auth, SSL, no direct DB exposure
wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy
chmod +x cloud_sql_proxy
./cloud_sql_proxy -instances=PROJECT:us-central1:prod-postgres=tcp:5432 &
# Now connect: psql -h 127.0.0.1 -U appuser myapp-db

# Option 2: Direct (use Private IP for production)
gcloud sql connect prod-postgres --user=appuser

# Option 3: Private IP (recommended for production VMs)
gcloud sql instances patch prod-postgres \
  --network=production-vpc \
  --no-assign-ip   # Disable public IP

# ─── Read Replica ───
gcloud sql instances create prod-postgres-replica \
  --master-instance-name=prod-postgres \
  --database-version=POSTGRES_15 \
  --region=us-west1 \              # Cross-region replica for DR
  --replica-type=READ

# ─── Backups ───
# Manual backup
gcloud sql backups create --instance=prod-postgres

# List backups
gcloud sql backups list --instance=prod-postgres

# Restore from backup
gcloud sql backups restore BACKUP_ID \
  --restore-instance=prod-postgres

# Point-in-time recovery
gcloud sql instances clone prod-postgres restored-db \
  --point-in-time="2024-01-15T14:30:00Z"
```

---

## 22. Cloud CDN

```bash
# Cloud CDN works WITH the HTTPS Load Balancer
# Cache static content at Google's edge locations globally

# Enable Cloud CDN on a backend service
gcloud compute backend-services update app-backend \
  --enable-cdn \
  --global

# Configure cache mode
gcloud compute backend-services update app-backend \
  --cache-mode=CACHE_ALL_STATIC \   # Auto-cache based on content type
  --global

# Or: FORCE_CACHE_ALL (cache everything including dynamic)
# Or: USE_ORIGIN_HEADERS (follow Cache-Control headers from origin)

# Custom cache keys (cache per region, user, etc.)
gcloud compute backend-services update app-backend \
  --global \
  --cache-key-policy-include-host \
  --cache-key-policy-include-protocol \
  --cache-key-policy-include-query-string

# Invalidate cached content
gcloud compute url-maps invalidate-cdn-cache app-url-map \
  --path="/images/*" \
  --global

# Check CDN cache status
# In response headers: X-Cache: HIT or MISS
# In Cloud Monitoring: loadbalancing.googleapis.com/https/response_count
# Filter by cache_result=HIT vs MISS
```

---

## 23. App Engine

### What is App Engine?

```
App Engine = Google's original PaaS (Platform as a Service)
You provide: Application code
Google provides: Web serving, scaling, load balancing, OS, runtime

Two environments:
  STANDARD:   Language-specific sandboxed runtime
              Scales to zero (no traffic = no cost)
              Faster scaling (seconds)
              Languages: Python, Java, Node.js, Go, PHP, Ruby
              
  FLEXIBLE:   Docker container in a managed VM
              Always has at least 1 instance running
              Slower scaling
              Any language (via Docker)
              More control over environment
```

```bash
# ─── Deploy to App Engine ───
# Create app.yaml (Standard environment example)
cat > app.yaml << 'EOF'
runtime: python311
service: default

env_variables:
  DB_CONNECTION: /cloudsql/PROJECT:us-central1:prod-postgres
  ENVIRONMENT: production

instance_class: F2   # F1=128MB, F2=256MB, F4=512MB, F4_1G=1GB

automatic_scaling:
  min_instances: 0
  max_instances: 20
  target_cpu_utilization: 0.6
  max_concurrent_requests: 80

handlers:
- url: /static
  static_dir: static
  
- url: /.*
  script: auto
  
beta_settings:
  cloud_sql_instances: PROJECT:us-central1:prod-postgres
EOF

# Deploy
gcloud app deploy app.yaml --version=v20240115 --no-promote

# Manage traffic between versions
gcloud app services set-traffic default \
  --splits=v20240115=0.1,v20240110=0.9   # 10% canary

# Promote new version
gcloud app services set-traffic default \
  --splits=v20240115=1.0

# View logs
gcloud app logs tail -s default

# Browse app
gcloud app browse
```

---

## 24. Cloud Functions

### What is Cloud Functions?

```
Cloud Functions = Serverless, event-driven functions
No servers, no containers — just code
Scales automatically from 0 to thousands of instances
Supports: Node.js, Python, Go, Java, Ruby, PHP, .NET

Triggers:
  HTTP:       HTTPS endpoint — invoked by web requests
  Cloud Storage: File upload, delete, metadata change
  Pub/Sub:    Message published to a topic
  Cloud Firestore: Database document changes
  Cloud Tasks: Task queue
  EventArc:   Any GCP event or custom event
```

```python
# main.py — Process uploaded images

import functions_framework
from google.cloud import storage, vision
import json

storage_client = storage.Client()
vision_client = vision.ImageAnnotatorClient()

@functions_framework.cloud_event
def process_image(cloud_event):
    """Triggered by Cloud Storage upload"""
    data = cloud_event.data
    bucket_name = data["bucket"]
    file_name = data["name"]
    
    print(f"Processing: gs://{bucket_name}/{file_name}")
    
    # Skip non-image files
    if not file_name.lower().endswith(('.jpg', '.jpeg', '.png', '.gif')):
        print(f"Skipping non-image: {file_name}")
        return
    
    # Analyze with Vision API
    image = vision.Image()
    image.source.image_uri = f"gs://{bucket_name}/{file_name}"
    
    response = vision_client.label_detection(image=image)
    labels = [label.description for label in response.label_annotations[:5]]
    
    # Save metadata
    metadata_bucket = storage_client.bucket(f"{bucket_name}-metadata")
    blob = metadata_bucket.blob(f"{file_name}.json")
    blob.upload_from_string(json.dumps({
        "file": file_name,
        "labels": labels,
        "processed": True
    }))
    
    print(f"Labels found: {labels}")
```

```bash
# requirements.txt
cat > requirements.txt << 'EOF'
functions-framework==3.*
google-cloud-storage==2.*
google-cloud-vision==3.*
EOF

# ─── Deploy Cloud Function ───
gcloud functions deploy process-image \
  --gen2 \
  --runtime=python311 \
  --region=us-central1 \
  --source=. \
  --entry-point=process_image \
  --trigger-event-filters="type=google.cloud.storage.object.v1.finalized" \
  --trigger-event-filters="bucket=my-upload-bucket" \
  --service-account=function-sa@PROJECT.iam.gserviceaccount.com \
  --max-instances=100 \
  --memory=512MB \
  --timeout=300s \
  --set-env-vars=ENVIRONMENT=production

# ─── HTTP Function ───
gcloud functions deploy hello-api \
  --gen2 \
  --runtime=python311 \
  --region=us-central1 \
  --source=. \
  --entry-point=hello \
  --trigger-http \
  --allow-unauthenticated \
  --max-instances=50 \
  --min-instances=1   # Keep warm — no cold starts

# ─── Test function ───
gcloud functions call hello-api \
  --data='{"name": "World"}' \
  --region=us-central1

# ─── View logs ───
gcloud functions logs read process-image \
  --region=us-central1 \
  --limit=20
```

---

## 25. API Gateway

```bash
# API Gateway: Managed API proxy
# Handles: Auth, rate limiting, SSL, CORS, API versioning

# Create API config
cat > openapi.yaml << 'EOF'
swagger: "2.0"
info:
  title: My App API
  version: "1.0"
paths:
  /users:
    get:
      operationId: getUsers
      x-google-backend:
        address: https://us-central1-PROJECT.cloudfunctions.net/get-users
      security:
        - api_key: []
      responses:
        200:
          description: Success
  /users/{id}:
    get:
      operationId: getUser
      x-google-backend:
        address: https://us-central1-PROJECT.cloudfunctions.net/get-user
      parameters:
        - name: id
          in: path
          required: true
          type: string
securityDefinitions:
  api_key:
    type: apiKey
    name: key
    in: query
EOF

# Create API
gcloud api-gateway apis create my-api \
  --project=PROJECT

# Create API config
gcloud api-gateway api-configs create v1 \
  --api=my-api \
  --openapi-spec=openapi.yaml \
  --project=PROJECT \
  --backend-auth-service-account=api-gateway-sa@PROJECT.iam.gserviceaccount.com

# Deploy to gateway
gcloud api-gateway gateways create my-gateway \
  --api=my-api \
  --api-config=v1 \
  --location=us-central1 \
  --project=PROJECT

# Get gateway URL
gcloud api-gateway gateways describe my-gateway \
  --location=us-central1 \
  --format="value(defaultHostname)"
```

---

### 🏆 ASSOCIATE LEVEL COMPREHENSIVE EXAM

**Q1 (Scenario):** A web application needs to serve users globally with low latency. The app has static assets and dynamic API endpoints. What is the BEST architecture?

A) One Compute Engine VM with a public IP
B) Global HTTPS Load Balancer + Cloud CDN for static, backend MIG for dynamic ✅
C) App Engine with CDN
D) Cloud Functions for all requests

**Q2:** A company's Cloud SQL instance is in us-central1. They need a read replica for analytics queries in the same region. What type of replica should they create?

A) Failover replica
B) Cross-region replica
C) Regional read replica ✅
D) External read replica

**Q3:** A firewall rule allows HTTP (port 80) with priority 1000. Another rule denies all traffic with priority 500. What happens to an HTTP request?

A) It is allowed (allow wins)
B) It is denied (lower priority number wins, 500 < 1000) ✅
C) It depends on the rule order
D) Both rules are applied

**Q4:** Which Cloud Storage class is MOST cost-effective for data accessed approximately once per month?

A) Standard
B) Nearline ✅
C) Coldline
D) Archive

**Q5:** A microservices application has 10 services that need to call each other. They are all in the same VPC. What load balancer type should be used for internal service-to-service communication?

A) External HTTPS Load Balancer
B) Internal HTTPS Load Balancer ✅
C) Network Load Balancer
D) No load balancer — use DNS directly

---
