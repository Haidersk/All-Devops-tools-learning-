# ☁️ AWS Cloud & DevOps: Complete Mastery Guide
### From Absolute Beginner to All AWS Certifications

> **Taught by:** Senior AWS Cloud Architect & DevOps Engineer perspective | 15+ years in production
> **Goal:** Pass every AWS certification and design production-grade cloud architectures

---

## 🎓 CERTIFICATION ROADMAP

```
FOUNDATIONAL          ASSOCIATE LEVEL              PROFESSIONAL / SPECIALTY
────────────          ───────────────              ────────────────────────
Cloud Practitioner →  Solutions Architect Assoc.→  Solutions Architect Pro
                   →  Developer Associate       →  DevOps Engineer Pro
                   →  SysOps Admin Associate    →  Security Specialty
                                                →  Advanced Networking Specialty
                                                →  Machine Learning Specialty
```

---

## 📚 TABLE OF CONTENTS

**PART 1: BEGINNER — Cloud Practitioner Level**
1. [What is Cloud Computing?](#1-what-is-cloud-computing)
2. [Cloud Deployment Models](#2-cloud-deployment-models)
3. [What is AWS?](#3-what-is-aws)
4. [AWS Global Infrastructure](#4-aws-global-infrastructure)
5. [AWS Free Tier](#5-aws-free-tier)
6. [Shared Responsibility Model](#6-shared-responsibility-model)
7. [IAM Fundamentals](#7-iam-fundamentals)
8. [AWS CLI Setup](#8-aws-cli-setup)
9. [EC2 Basics](#9-ec2-basics)
10. [S3 Basics](#10-s3-basics)
11. [VPC Fundamentals](#11-vpc-fundamentals)

**PART 2: INTERMEDIATE — Associate Level**
12. [EC2 Deep Dive](#12-ec2-deep-dive)
13. [Security Groups vs NACLs](#13-security-groups-vs-nacls)
14. [VPC Architecture Deep Dive](#14-vpc-architecture-deep-dive)
15. [EBS vs EFS Storage](#15-ebs-vs-efs)
16. [S3 Storage Classes & Lifecycle](#16-s3-storage-classes--lifecycle)
17. [Load Balancers: ALB & NLB](#17-load-balancers-alb--nlb)
18. [Auto Scaling](#18-auto-scaling)
19. [Route 53 DNS](#19-route-53-dns)
20. [CloudWatch Monitoring](#20-cloudwatch-monitoring)
21. [RDS & Database Services](#21-rds--database-services)
22. [CloudFront CDN](#22-cloudfront-cdn)
23. [Elastic Beanstalk](#23-elastic-beanstalk)
24. [Lambda & Serverless](#24-lambda--serverless)
25. [API Gateway](#25-api-gateway)

**PART 3: DEVOPS / ENGINEER LEVEL**
26. [Infrastructure as Code: Terraform on AWS](#26-infrastructure-as-code-terraform)
27. [AWS CodeBuild](#27-aws-codebuild)
28. [AWS CodePipeline](#28-aws-codepipeline)
29. [AWS CodeDeploy](#29-aws-codedeploy)
30. [ECR — Elastic Container Registry](#30-ecr)
31. [ECS — Elastic Container Service](#31-ecs)
32. [EKS — Kubernetes on AWS](#32-eks)
33. [Secrets Manager & SSM](#33-secrets-manager--ssm)
34. [Logging & Monitoring Architecture](#34-logging--monitoring-architecture)

**PART 4: ADVANCED ARCHITECTURE — Professional Level**
35. [Highly Available Architecture Design](#35-highly-available-architecture)
36. [Multi-Region Architecture](#36-multi-region-architecture)
37. [Disaster Recovery Strategies](#37-disaster-recovery-strategies)
38. [Hybrid Cloud Architecture](#38-hybrid-cloud-architecture)
39. [VPC Peering & Transit Gateway](#39-vpc-peering--transit-gateway)
40. [AWS PrivateLink](#40-aws-privatelink)
41. [Performance Optimization](#41-performance-optimization)
42. [Cost Optimization Strategies](#42-cost-optimization)

**PART 5: EXPERT / SPECIALTY LEVEL**
43. [Advanced Networking](#43-advanced-networking)
44. [Security Architecture & Compliance](#44-security-architecture--compliance)
45. [IAM Policy Deep Dive](#45-iam-policy-deep-dive)
46. [Encryption & KMS](#46-encryption--kms)
47. [Production Incident Debugging](#47-production-incident-debugging)
48. [Observability Architecture](#48-observability-architecture)
49. [Large-Scale System Troubleshooting](#49-large-scale-troubleshooting)
50. [ML on AWS: SageMaker & AI Services](#50-ml-on-aws)

**APPENDIX**
- [AWS Well-Architected Framework](#well-architected)
- [Certification Exam Tips](#exam-tips)
- [Interview Questions Bank](#interview-questions)
- [Architecture Cheat Sheet](#cheat-sheet)

---

# ═══════════════════════════════════════
# PART 1: BEGINNER — CLOUD PRACTITIONER
# ═══════════════════════════════════════

---

## 1. What is Cloud Computing?

### Simple Explanation

Before the cloud, if a company wanted to run software, they had to:
1. Buy physical servers (expensive — $10,000–$50,000 per server)
2. Build a data center (power, cooling, security, space)
3. Hire staff to manage hardware 24/7
4. Wait weeks/months to add capacity
5. Pay for capacity even when idle

**Cloud computing** replaces this with: **rent computing resources over the internet, pay only for what you use, scale in minutes.**

Think of it like electricity: you don't build your own power plant. You plug into the grid and pay for kilowatt-hours used.

### The 5 Characteristics of Cloud (NIST Definition)

```
1. ON-DEMAND SELF-SERVICE
   Provision resources without human interaction
   "I need 10 servers → click → done in 2 minutes"

2. BROAD NETWORK ACCESS
   Access via internet from any device, anywhere

3. RESOURCE POOLING
   Provider serves multiple customers (multi-tenant)
   Hardware is shared, but isolated

4. RAPID ELASTICITY
   Scale up or down quickly (or automatically)
   "Black Friday traffic spike → auto-scale → spike ends → scale back"

5. MEASURED SERVICE
   Pay per use (per hour, per GB, per request)
   Like your electricity or water bill
```

### Cloud Service Models

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  On-Premises         IaaS           PaaS         SaaS  │
│  ─────────           ────           ────         ────  │
│  YOU manage:         YOU manage:    YOU manage:  Provider│
│  Hardware            OS & above     App & Data   manages│
│  Network             Runtime        (not servers) all   │
│  Storage             Middleware                         │
│  OS                  Data                               │
│  Runtime             Application                        │
│  Middleware                                             │
│  Data                                                   │
│  Application                                            │
│                                                         │
│  Example:           EC2, VPC,      Elastic       Gmail, │
│  Your datacenter    EBS, S3        Beanstalk,    Salesf.│
│                                   RDS, Lambda           │
└─────────────────────────────────────────────────────────┘
```

### The 6 Advantages of Cloud (AWS Exam Critical!)

These are directly tested on the Cloud Practitioner exam:

```
1. TRADE CAPITAL EXPENSE FOR VARIABLE EXPENSE
   Stop buying hardware → pay for what you use

2. BENEFIT FROM MASSIVE ECONOMIES OF SCALE
   AWS buys millions of servers → you get lower prices

3. STOP GUESSING CAPACITY
   Scale up/down as needed → no over/under provisioning

4. INCREASE SPEED AND AGILITY
   Deploy globally in minutes → new features faster

5. STOP SPENDING MONEY ON DATA CENTERS
   Focus on your business, not infrastructure

6. GO GLOBAL IN MINUTES
   Deploy in multiple regions with a few clicks
```

### Real-World Production Example

**Netflix Migration to AWS:**
- Moved 100% of infrastructure to AWS (2016)
- Streams to 190+ countries simultaneously
- Auto-scales during peak hours (Friday nights, new show drops)
- Saved hundreds of millions vs. running own data centers
- Now uses 100,000+ EC2 instances during peak

### 🏆 Exam-Style Question

**Q:** A company wants to eliminate the cost of managing physical servers while still running their own applications. Which benefit of cloud computing does this represent?

A) Increased speed and agility
B) Trade capital expense for variable expense
C) Stop spending money on data centers ✅
D) Benefit from economies of scale

---

## 2. Cloud Deployment Models

### The Three Models

**1. Public Cloud**
```
Definition: Cloud resources owned and operated by third-party provider
            Delivered over the internet
            Multiple customers share the same infrastructure

Examples:   AWS, Microsoft Azure, Google Cloud

Best for:   Startups, variable workloads, non-sensitive data
            "We want zero infrastructure management"
```

**2. Private Cloud**
```
Definition: Cloud infrastructure dedicated to one organization
            Can be on-premises OR hosted by provider
            More control, more security

Examples:   VMware on-prem, AWS Outposts, Azure Stack

Best for:   Banking, healthcare, government
            "We need full control and compliance"
```

**3. Hybrid Cloud**
```
Definition: Mix of public + private cloud
            Connected via secure network (VPN or Direct Connect)
            Data flows between environments

Examples:   Hospital keeps patient records on-prem (compliance)
            but runs analytics workloads on AWS

Best for:   Migration phase, compliance requirements, burst capacity
            "Some workloads on-prem, some in cloud"
```

### Cloud vs. Edge

```
Cloud:  Centralized data centers (you send data TO AWS)
Edge:   Computing happens close to the user (AWS sends compute TO YOU)
        Examples: AWS Outposts, Wavelength, Local Zones
        Use case: Autonomous vehicles, 5G apps, gaming
```

### Exam Tip

The exam loves "which deployment model should this company use?" scenarios. Key hints:
- "Compliance requires data on-premises" → **Hybrid**
- "Startup with no budget for hardware" → **Public**
- "Government agency, classified data" → **Private**
- "Migrate slowly, keep some on-prem" → **Hybrid**

---

## 3. What is AWS?

### AWS at a Glance

**Amazon Web Services (AWS)** launched in 2006 with just 3 services: S3, SQS, and EC2. Today it has **200+ services** and captures **31% of the global cloud market** (the largest cloud provider).

```
AWS Market Share (2024):
AWS:    31%  ████████████████████████████████
Azure:  25%  █████████████████████████
GCP:    11%  ███████████

Revenue: $90+ billion annually
Customers: Millions (from startups to Fortune 500)
```

### Why AWS Dominates

```
1. FIRST MOVER ADVANTAGE — launched 5 years before Azure
2. BREADTH OF SERVICES — deepest and widest catalog
3. GLOBAL INFRASTRUCTURE — most regions worldwide
4. ECOSYSTEM — largest partner network, most documentation
5. PACE OF INNOVATION — launches 100s of new features yearly
```

### AWS Service Categories (You'll Use All of These)

```
COMPUTE:        EC2, Lambda, ECS, EKS, Fargate
STORAGE:        S3, EBS, EFS, Glacier
DATABASE:       RDS, DynamoDB, ElastiCache, Redshift
NETWORKING:     VPC, Route 53, CloudFront, ELB, Direct Connect
SECURITY:       IAM, KMS, WAF, Shield, GuardDuty, Security Hub
MONITORING:     CloudWatch, CloudTrail, X-Ray, AWS Config
DEVOPS:         CodePipeline, CodeBuild, CodeDeploy, CodeCommit
CONTAINERS:     ECR, ECS, EKS, Fargate
SERVERLESS:     Lambda, API Gateway, DynamoDB, SQS, SNS
ML/AI:          SageMaker, Rekognition, Comprehend, Polly
ANALYTICS:      Athena, EMR, Kinesis, Glue, QuickSight
INTEGRATION:    SQS, SNS, EventBridge, Step Functions
```

---

## 4. AWS Global Infrastructure

### This is CRITICAL for Every AWS Certification

Understanding global infrastructure is tested heavily on ALL AWS exams.

### Regions

A **Region** is a physical geographic area containing multiple data centers.

```
What is a Region?
─────────────────
- Cluster of data centers in a specific geographic area
- Examples: us-east-1 (N. Virginia), eu-west-1 (Ireland), ap-southeast-1 (Singapore)
- AWS has 33+ regions worldwide (growing)
- Most AWS services are Region-scoped (resources exist within one region)
- Data does NOT automatically replicate across regions

How to choose a Region?
────────────────────────
1. COMPLIANCE — "Data must stay in Germany" → eu-central-1 (Frankfurt)
2. LATENCY — Put compute close to your users → ap-southeast-1 for Asian users
3. PRICING — Prices vary 10-30% between regions
4. SERVICE AVAILABILITY — Not all services exist in all regions
   (New services launch in us-east-1 first)
```

### Availability Zones (AZs)

```
What is an AZ?
──────────────
- One or more discrete data centers within a Region
- Each AZ has independent power, cooling, networking
- Connected to other AZs via low-latency private fiber (<1ms RTT)
- 3–6 AZs per region (typically)
- Named: us-east-1a, us-east-1b, us-east-1c

WHY AZs MATTER: High Availability
───────────────────────────────────
If you deploy servers in ONE AZ and it fails → your app goes down
If you deploy across THREE AZs and one fails → still 2/3 running

This is called: Multi-AZ deployment (critical concept!)

Real example:
  App servers in us-east-1a, us-east-1b, us-east-1c
  AZ-a has power outage → 1/3 of capacity lost
  Load balancer routes to us-east-1b and us-east-1c
  Users experience no downtime ✅
```

### Edge Locations & CloudFront

```
What are Edge Locations?
────────────────────────
- Mini data centers closer to end users
- 400+ worldwide (more than Regions!)
- NOT for running your servers
- Used for: Content delivery (CloudFront CDN)
            DNS resolution (Route 53)
            DDoS protection (Shield)

How it works:
  User in Mumbai requests your website (hosted in us-east-1)
  Without Edge: Request travels 13,000km to Virginia → SLOW
  With Edge:   Request hits Mumbai Edge Location → served locally → FAST

Cache: Static content (images, JS, CSS) is cached at Edge Locations
       Only dynamic requests go to your origin server
```

### Local Zones & Wavelength

```
LOCAL ZONES:
- Extension of a Region, placed in large metro areas
- For workloads needing single-digit ms latency
- Example: Los Angeles Local Zone for Hollywood rendering farms

WAVELENGTH ZONES:
- AWS compute embedded in telecom 5G networks
- Ultra-low latency for mobile users
- Use case: Autonomous vehicles, real-time gaming, AR/VR
```

### Visual: AWS Global Infrastructure

```
REGION: us-east-1 (N. Virginia)
┌────────────────────────────────────────────┐
│                                            │
│  AZ: us-east-1a    AZ: us-east-1b    AZ: us-east-1c  │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐ │
│  │ Data Center │   │ Data Center │   │ Data Center │ │
│  │ Data Center │   │ Data Center │   │ Data Center │ │
│  └─────────────┘   └─────────────┘   └─────────────┘ │
│        │                 │                 │           │
│        └─────────────────┴─────────────────┘           │
│              Low-latency private fiber network         │
└────────────────────────────────────────────┘

      ↑ Connected to other Regions via AWS backbone network
      ↑ Connected to 400+ Edge Locations globally
```

### AWS CLI: Explore Infrastructure

```bash
# List all available regions
aws ec2 describe-regions --output table

# List availability zones in current region
aws ec2 describe-availability-zones --output table

# List all regions including opted-in status
aws ec2 describe-regions --all-regions --query 'Regions[*].[RegionName,OptInStatus]' --output table
```

### 🏆 Exam-Style Questions

**Q1:** A company needs to ensure their application is resilient to a single data center failure. What should they implement?

A) Deploy in multiple Regions
B) Deploy in multiple Availability Zones ✅
C) Deploy at Edge Locations
D) Use Local Zones

**Q2:** A media company wants to serve video content with low latency to users globally. Which AWS feature should they use?

A) Multiple EC2 instances in different Regions
B) Amazon CloudFront with Edge Locations ✅
C) Multiple VPCs
D) AWS Direct Connect

**Q3:** Which statement about AWS Regions is TRUE?

A) Data is automatically replicated between Regions for durability
B) All AWS services are available in all Regions
C) A Region consists of multiple Availability Zones ✅
D) Edge Locations are a type of AWS Region

---

## 5. AWS Free Tier

### What's Free (and for How Long)

```
THREE TYPES of Free Tier:

1. ALWAYS FREE (never expires)
   ─────────────────────────
   DynamoDB:  25 GB storage, 25 read/write capacity units
   Lambda:    1 million requests/month, 400,000 GB-seconds
   CloudWatch: 10 custom metrics, 10 alarms
   SQS:       1 million requests/month
   SNS:       1 million publishes/month

2. 12 MONTHS FREE (from account creation)
   ──────────────────────────────────────
   EC2:       750 hours/month of t2.micro (or t3.micro in some regions)
   S3:        5 GB standard storage
   RDS:       750 hours/month db.t2.micro
   CloudFront: 1 TB data transfer out/month
   ELB:       750 hours/month

3. TRIALS (short-term)
   ────────────────────
   Inspector: 15-day free trial
   Macie:     30-day free trial
   GuardDuty: 30-day free trial
```

### ⚠️ Free Tier Warnings

```bash
# Set up billing alerts BEFORE you start:

# 1. Enable Billing Alerts
# Console: Billing → Billing Preferences → Receive Billing Alerts ✓

# 2. Create CloudWatch Billing Alarm
aws cloudwatch put-metric-alarm \
  --alarm-name "billing-alert-10-dollars" \
  --alarm-description "Alert when charges exceed $10" \
  --metric-name EstimatedCharges \
  --namespace AWS/Billing \
  --statistic Maximum \
  --period 86400 \
  --evaluation-periods 1 \
  --threshold 10 \
  --comparison-operator GreaterThanThreshold \
  --dimensions Name=Currency,Value=USD \
  --alarm-actions arn:aws:sns:us-east-1:YOUR_ACCOUNT:billing-alerts

# 3. Set AWS Budget
aws budgets create-budget \
  --account-id YOUR_ACCOUNT_ID \
  --budget '{
    "BudgetName": "Monthly-Free-Tier-Budget",
    "BudgetLimit": {"Amount": "1", "Unit": "USD"},
    "TimeUnit": "MONTHLY",
    "BudgetType": "COST"
  }'
```

---

## 6. Shared Responsibility Model

### THE Most Important Concept for Security Exams

This concept appears on EVERY AWS certification exam. Master it completely.

```
┌──────────────────────────────────────────────────────────┐
│                  CUSTOMER RESPONSIBILITY                   │
│              "Security IN the cloud"                       │
│                                                            │
│  ✓ Customer data                                          │
│  ✓ Platform, applications, identity, access management    │
│  ✓ Operating system configuration                         │
│  ✓ Network & firewall configuration                       │
│  ✓ Client-side encryption                                 │
│  ✓ Server-side encryption                                 │
│  ✓ Network traffic protection                             │
├──────────────────────────────────────────────────────────┤
│                    AWS RESPONSIBILITY                      │
│              "Security OF the cloud"                       │
│                                                            │
│  ✓ Compute (hardware)                                     │
│  ✓ Storage (hardware)                                     │
│  ✓ Database (hardware)                                    │
│  ✓ Networking (hardware)                                  │
│  ✓ Regions, AZs, Edge Locations                           │
│  ✓ Physical security of data centers                      │
└──────────────────────────────────────────────────────────┘
```

### The Model Changes by Service Type

```
EC2 (IaaS) — MOST customer responsibility:
  AWS:      Physical hardware, hypervisor, network hardware
  YOU:      OS patches, middleware, app, data, firewall rules

RDS (PaaS) — SHARED:
  AWS:      Hardware + OS + database software patches
  YOU:      Data, access control, security groups

Lambda (SaaS/Serverless) — LEAST customer responsibility:
  AWS:      Hardware, OS, runtime environment, scaling
  YOU:      Your function code, permissions (IAM)

S3:
  AWS:      Infrastructure, hardware, durability
  YOU:      Bucket policies, ACLs, encryption, who can access
```

### Real-World Breach Analysis

```
2020 AWS Customer Data Breach (Capital One):
  - AWS infrastructure: FINE (not AWS's fault)
  - Customer's WAF misconfiguration: CUSTOMER'S FAULT
  - Result: $190 million fine for Capital One

Lesson: AWS secures the cloud infrastructure
        YOU secure what's IN the cloud
        (configurations, permissions, data)
```

### 🏆 Exam-Style Questions

**Q1:** A company is running an application on EC2. A security audit found that the operating system has unpatched vulnerabilities. Who is responsible?

A) AWS, because they manage EC2
B) The customer, because OS patches are customer responsibility ✅
C) Both AWS and the customer equally
D) Neither — this is a known EC2 limitation

**Q2:** AWS is responsible for which of the following? (Select TWO)

A) Patching the operating system on EC2 instances
B) Physical security of AWS data centers ✅
C) Configuring Security Groups correctly
D) Maintaining the AWS global network infrastructure ✅
E) Encrypting customer data at rest

---

## 7. IAM Fundamentals

### Why IAM Is Critical

IAM (Identity and Access Management) is the foundation of AWS security. Everything in AWS requires IAM permissions. Getting IAM wrong = security breaches.

```
IAM Controls:
WHO can do WHAT to WHICH resources
```

### IAM Components

**Users**
```
An IAM User represents a person or application that needs AWS access.

Properties:
- Has a name and credentials (password and/or access keys)
- Gets permissions via policies (directly or through groups)
- Best practice: One user per person (not shared accounts)

Root User vs IAM Users:
  Root:  Created with account, has UNLIMITED access
         → Enable MFA, create admin user, then NEVER use root again
  IAM:   Scoped permissions, used for daily work
```

**Groups**
```
A collection of IAM users with the same permissions.

Why groups?
  Instead of assigning policies to 50 individual developers,
  create a "Developers" group with the right policies.
  Add/remove users from the group.

Examples:
  "Developers"  → CodeBuild, CodePipeline, EC2 read access
  "DBAdmins"    → RDS full access, no EC2 terminate
  "Finance"     → Billing access only
  "SysAdmins"   → AdministratorAccess
```

**Roles**
```
An IAM Role is like a "temporary job title" with specific permissions.
NO credentials attached — it's assumed, not logged in to.

Key difference from Users:
  User: "I am Alice, I always have these permissions"
  Role: "This EC2 instance BECOMES the DBA role when needed"

Common role uses:
1. EC2 instance role: Let EC2 read from S3 (no access keys needed!)
2. Cross-account role: Allow Account B to access Account A's S3
3. Service roles: Allow Lambda to write to DynamoDB
4. Federated role: Allow company SSO users to access AWS

GOLDEN RULE: Always use roles instead of access keys on EC2/Lambda
```

**Policies**
```
A Policy is a JSON document that defines permissions.

Structure:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",          ← Allow or Deny
      "Action": [                 ← What action(s)
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [               ← Which resource(s)
        "arn:aws:s3:::my-bucket/*"
      ],
      "Condition": {              ← Optional: when does this apply?
        "IpAddress": {
          "aws:SourceIp": "192.168.0.0/24"
        }
      }
    }
  ]
}
```

### Types of Policies

```
1. AWS MANAGED POLICIES
   Created and maintained by AWS
   Examples: AdministratorAccess, ReadOnlyAccess, AmazonS3FullAccess
   Pros: Always up-to-date with new services
   Cons: Overly broad (least privilege violation)

2. CUSTOMER MANAGED POLICIES
   You create and maintain them
   Best practice: Create specific policies for your use case
   Example: "AllowEC2ReadAndS3BucketA" — very specific

3. INLINE POLICIES
   Embedded directly in a user/group/role
   Not reusable
   Use for: One-off strict permissions that shouldn't be shared
```

### IAM Policy Evaluation Logic

```
DENY ALWAYS WINS!

Evaluation order:
1. Explicit DENY? → DENY (end)
2. Explicit ALLOW? → ALLOW
3. No match? → IMPLICIT DENY (default)

Example:
  Policy A: Allow s3:* on *
  Policy B: Deny s3:DeleteObject on *
  → User CAN list, get, put — but CANNOT delete
    (Explicit deny overrides explicit allow)
```

### IAM Best Practices (Exam & Real World)

```
1. ✅ Enable MFA on root account (IMMEDIATELY)
2. ✅ Never use root for daily work
3. ✅ Create individual IAM users (no sharing)
4. ✅ Use groups to assign permissions
5. ✅ Follow least privilege principle
6. ✅ Use roles for applications (never access keys on EC2)
7. ✅ Rotate access keys regularly
8. ✅ Use IAM Access Analyzer to review permissions
9. ✅ Enable CloudTrail to audit all API calls
10. ✅ Use Service Control Policies (SCPs) in Organizations
```

### Hands-On Lab: IAM Setup

```bash
# ─── Step 1: Create an IAM user ───
aws iam create-user --user-name devops-engineer

# ─── Step 2: Create a group ───
aws iam create-group --group-name DevOps-Team

# ─── Step 3: Attach a policy to group ───
aws iam attach-group-policy \
  --group-name DevOps-Team \
  --policy-arn arn:aws:iam::aws:policy/PowerUserAccess

# ─── Step 4: Add user to group ───
aws iam add-user-to-group \
  --user-name devops-engineer \
  --group-name DevOps-Team

# ─── Step 5: Create access keys for user ───
aws iam create-access-key --user-name devops-engineer

# ─── Step 6: Create a custom policy ───
aws iam create-policy \
  --policy-name S3ReadOnlyBucketA \
  --policy-document '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Action": ["s3:GetObject", "s3:ListBucket"],
        "Resource": [
          "arn:aws:s3:::my-specific-bucket",
          "arn:aws:s3:::my-specific-bucket/*"
        ]
      }
    ]
  }'

# ─── Step 7: Create a role for EC2 ───
aws iam create-role \
  --role-name EC2-S3-ReadOnly \
  --assume-role-policy-document '{
    "Version": "2012-10-17",
    "Statement": [{
      "Effect": "Allow",
      "Principal": {"Service": "ec2.amazonaws.com"},
      "Action": "sts:AssumeRole"
    }]
  }'

aws iam attach-role-policy \
  --role-name EC2-S3-ReadOnly \
  --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# ─── Step 8: Check what permissions a user has ───
aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::ACCOUNT:user/devops-engineer \
  --action-names s3:GetObject \
  --resource-arns arn:aws:s3:::my-bucket/file.txt
```

### 🏆 Exam-Style Questions

**Q1:** An application running on EC2 needs to access objects in S3. What is the MOST secure approach?

A) Store AWS access keys in the application code
B) Store access keys in an environment variable on the EC2 instance
C) Create an IAM role with S3 permissions and attach it to the EC2 instance ✅
D) Create an IAM user with S3 permissions and store the credentials on EC2

**Q2:** A user has an explicit Allow policy for s3:DeleteObject AND an explicit Deny policy for s3:DeleteObject. What happens when they try to delete an S3 object?

A) The Allow wins (explicit allow overrides deny)
B) The Deny wins (explicit deny always wins) ✅
C) AWS throws an error — conflicting policies aren't allowed
D) The result is unpredictable

---

## 8. AWS CLI Setup

### What is the AWS CLI?

The AWS Command Line Interface is a unified tool to manage AWS services from your terminal. Every DevOps engineer uses this constantly.

### Installation

```bash
# ─── Linux ───
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version

# ─── macOS ───
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /

# ─── Windows ───
# Download from: https://awscli.amazonaws.com/AWSCLIV2.msi
```

### Configuration

```bash
# ─── Basic configuration ───
aws configure
# Prompts:
# AWS Access Key ID: [Your key]
# AWS Secret Access Key: [Your secret]
# Default region name: us-east-1
# Default output format: json

# ─── Multiple profiles (real-world setup) ───
aws configure --profile dev-account
aws configure --profile prod-account

# Use a specific profile:
aws s3 ls --profile prod-account
export AWS_PROFILE=dev-account    # Set default for session

# ─── Where credentials are stored ───
cat ~/.aws/credentials
cat ~/.aws/config

# ─── Verify your identity ───
aws sts get-caller-identity
# Returns your Account ID, User ARN, User ID

# ─── Set up environment variables (for CI/CD) ───
export AWS_ACCESS_KEY_ID=your_key
export AWS_SECRET_ACCESS_KEY=your_secret
export AWS_DEFAULT_REGION=us-east-1
```

### Essential CLI Commands

```bash
# ─── Output formats ───
aws ec2 describe-instances --output json   # Default
aws ec2 describe-instances --output table  # Human-readable table
aws ec2 describe-instances --output text   # Plain text

# ─── Filtering with --query (JMESPath) ───
# Get only instance IDs and states:
aws ec2 describe-instances \
  --query 'Reservations[*].Instances[*].[InstanceId,State.Name]' \
  --output table

# Get instances in "running" state:
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[*].Instances[*].InstanceId' \
  --output text

# ─── Pagination ───
aws s3api list-objects-v2 \
  --bucket my-bucket \
  --max-items 100    # Limit results

# ─── Get help ───
aws help
aws ec2 help
aws ec2 describe-instances help
```

### Real DevOps CLI Workflow

```bash
# Morning health check script (real DevOps usage):

#!/bin/bash
echo "=== AWS Health Check ==="
echo "Account: $(aws sts get-caller-identity --query Account --output text)"
echo ""
echo "Running EC2 Instances:"
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query 'Reservations[*].Instances[*].[InstanceId,InstanceType,PublicIpAddress,Tags[?Key==`Name`].Value|[0]]' \
  --output table

echo ""
echo "RDS Instances:"
aws rds describe-db-instances \
  --query 'DBInstances[*].[DBInstanceIdentifier,DBInstanceStatus,Endpoint.Address]' \
  --output table
```

---

## 9. EC2 Basics

### What is EC2?

**Amazon Elastic Compute Cloud (EC2)** is virtual servers in the cloud. It's the backbone of AWS — most architectures use EC2 somewhere.

```
Traditional Server:       EC2 Instance:
─────────────────         ────────────
Buy physical hardware  →  Launch in 60 seconds
Wait weeks to deploy   →  Available immediately
Fixed capacity         →  Scale up/down instantly
Pay whether used or not→  Pay per second (when running)
Your data center       →  AWS data center (you manage OS up)
```

### EC2 Instance Types

```
INSTANCE FAMILY NAMING:
  t3.medium  →  t = family, 3 = generation, medium = size
  m5.2xlarge →  m = family, 5 = generation, 2xlarge = size

FAMILIES:
  t = GENERAL PURPOSE (burstable) — t2, t3, t4g
      Best for: Web servers, dev environments, low traffic apps
      Special: CPU credits system (burst when needed)
      
  m = GENERAL PURPOSE (balanced) — m5, m6i, m7i
      Best for: Application servers, gaming, enterprise apps
      
  c = COMPUTE OPTIMIZED — c5, c6i, c7i
      Best for: Batch processing, video encoding, ML inference
      
  r = MEMORY OPTIMIZED — r5, r6i
      Best for: In-memory databases, real-time analytics, SAP HANA
      
  i = STORAGE OPTIMIZED — i3, i4i
      Best for: High I/O databases, data warehousing
      
  p/g = ACCELERATED (GPU) — p3, g4dn
      Best for: ML training, HPC, video rendering

SIZES:
  nano, micro, small, medium, large, xlarge, 2xlarge, 4xlarge...
  Each size roughly doubles vCPU and memory from previous
```

### EC2 Purchasing Options (CRITICAL for Cost Optimization Exams)

```
1. ON-DEMAND (default)
   ─────────────────────
   Pay per second (Linux) or per hour (Windows)
   No commitment, highest price
   Best for: Short-term, unpredictable, dev/test

2. RESERVED INSTANCES (1 or 3 year commitment)
   ─────────────────────────────────────────────
   Up to 75% savings vs On-Demand
   Three payment options:
     All Upfront:     Maximum discount
     Partial Upfront: Middle ground
     No Upfront:      Smallest discount, pay monthly
   Best for: Steady-state production workloads

3. SAVINGS PLANS (flexible Reserved)
   ────────────────────────────────────
   Commit to $/hour of compute spend
   More flexible than RIs (works across instance types)
   Up to 66% savings
   Best for: When you know your spend but not exact instances

4. SPOT INSTANCES
   ─────────────────
   Bid on spare AWS capacity
   Up to 90% savings!
   BUT: Can be terminated by AWS with 2-minute warning
   Best for: Fault-tolerant, batch, big data, ML training
   NOT for: Databases, production web servers (unless architected carefully)

5. DEDICATED HOSTS
   ─────────────────
   Physical server dedicated to YOU
   Most expensive
   Best for: BYOL (Bring Your Own License) - Windows Server, SQL Server
             Compliance requirements (no sharing hardware)

6. DEDICATED INSTANCES
   ──────────────────────
   Your instances on dedicated hardware, but you don't see the host
   Between Dedicated Hosts and regular in price/control

REMEMBER FOR EXAM:
  Stable, predictable = Reserved/Savings Plan
  Short-term/unpredictable = On-Demand
  Fault-tolerant batch = Spot
  Compliance/BYOL = Dedicated Host
```

### EC2 Storage: Instance Store vs EBS

```
INSTANCE STORE:
  Physical disk directly attached to the host hardware
  - Ultra-fast (local NVMe SSD)
  - EPHEMERAL: Data LOST if instance stops/terminates
  - Cannot detach and reattach
  Best for: Temporary data, buffer/cache, scratch data

EBS (Elastic Block Store):
  Network-attached persistent storage
  - Persists independently of instance lifecycle
  - Can detach and reattach to another instance
  - Can snapshot (backup to S3)
  Best for: OS volumes, databases, persistent data
```

### Launching Your First EC2 Instance

**Via Console:**
```
1. EC2 → Launch Instance
2. Name: my-first-server
3. AMI: Amazon Linux 2023 (free tier eligible)
4. Instance type: t2.micro (free tier)
5. Key pair: Create new → download .pem file (SAVE THIS!)
6. Network settings:
   - VPC: default
   - Security Group: Allow SSH (22) from your IP
   - Allow HTTP (80) from anywhere
7. Storage: 8 GB gp3 (free tier)
8. Launch!
```

**Via CLI:**
```bash
# Step 1: Get the latest Amazon Linux 2023 AMI ID
AMI_ID=$(aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=al2023-ami-2023*-x86_64" \
  --query 'sort_by(Images, &CreationDate)[-1].ImageId' \
  --output text)
echo "Using AMI: $AMI_ID"

# Step 2: Create a Security Group
SG_ID=$(aws ec2 create-security-group \
  --group-name my-web-sg \
  --description "Web server security group" \
  --query GroupId --output text)

# Allow SSH from your IP
MY_IP=$(curl -s https://checkip.amazonaws.com)
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp --port 22 --cidr "$MY_IP/32"

# Allow HTTP from anywhere
aws ec2 authorize-security-group-ingress \
  --group-id $SG_ID \
  --protocol tcp --port 80 --cidr "0.0.0.0/0"

# Step 3: Launch instance
INSTANCE_ID=$(aws ec2 run-instances \
  --image-id $AMI_ID \
  --instance-type t2.micro \
  --key-name my-key-pair \
  --security-group-ids $SG_ID \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=my-web-server}]' \
  --user-data '#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from $(hostname)</h1>" > /var/www/html/index.html' \
  --query 'Instances[0].InstanceId' \
  --output text)
echo "Instance: $INSTANCE_ID"

# Step 4: Wait for it to run
aws ec2 wait instance-running --instance-ids $INSTANCE_ID

# Step 5: Get public IP
PUBLIC_IP=$(aws ec2 describe-instances \
  --instance-ids $INSTANCE_ID \
  --query 'Reservations[0].Instances[0].PublicIpAddress' \
  --output text)
echo "Access at: http://$PUBLIC_IP"

# Step 6: SSH into it
ssh -i my-key-pair.pem ec2-user@$PUBLIC_IP

# Step 7: Stop instance when done (save costs!)
aws ec2 stop-instances --instance-ids $INSTANCE_ID

# Step 8: Terminate when done completely
aws ec2 terminate-instances --instance-ids $INSTANCE_ID
```

### 🏆 Exam-Style Questions

**Q1:** A company runs a big data analytics job every weekend. The job can tolerate interruptions. Which EC2 purchasing option is MOST cost-effective?

A) On-Demand Instances
B) Reserved Instances
C) Spot Instances ✅
D) Dedicated Hosts

**Q2:** An EC2 instance is terminated. Which storage will RETAIN data after termination?

A) Instance Store
B) EBS Root Volume (with DeleteOnTermination=false) ✅
C) Both A and B
D) Neither — all data is always deleted

---

## 10. S3 Basics

### What is S3?

**Amazon Simple Storage Service (S3)** is object storage. Unlike a file system (hierarchical) or block storage (raw disk), S3 stores data as objects in buckets.

```
Object Storage Basics:
  Object = File + Metadata + Unique Key
  Bucket = Container for objects (globally unique name!)
  Key    = Full path to object (e.g., "folder/subfolder/file.txt")

Limits:
  Object size: 0 bytes to 5 TB
  Single PUT: max 5 GB (use multipart for >100 MB)
  Bucket names: globally unique, 3-63 chars, lowercase, no underscores
  Objects per bucket: unlimited
  Buckets per account: default 100 (can increase)
```

### S3 Key Features

```
DURABILITY:    99.999999999% (11 nines!)
               Designed to NOT lose data
               Automatically stored across min. 3 AZs

AVAILABILITY:  99.99% (Standard)
               Your objects are accessible

SCALABILITY:   Unlimited storage — grows with you

ACCESS:        Via HTTPS URL, AWS Console, CLI, SDK
               URL format: https://bucket-name.s3.amazonaws.com/key
```

### S3 Operations

```bash
# ─── Create bucket ───
aws s3 mb s3://my-unique-bucket-name-2024
aws s3 mb s3://my-bucket --region us-west-2   # Specific region

# ─── Upload files ───
aws s3 cp file.txt s3://my-bucket/
aws s3 cp file.txt s3://my-bucket/folder/file.txt
aws s3 cp localdir/ s3://my-bucket/remotedir/ --recursive

# ─── Download files ───
aws s3 cp s3://my-bucket/file.txt .
aws s3 cp s3://my-bucket/ localdir/ --recursive

# ─── List objects ───
aws s3 ls s3://my-bucket
aws s3 ls s3://my-bucket --recursive --human-readable --summarize

# ─── Sync (like rsync) ───
aws s3 sync localdir/ s3://my-bucket/
aws s3 sync s3://my-bucket/ localdir/
aws s3 sync s3://bucket1/ s3://bucket2/   # Bucket to bucket

# ─── Delete ───
aws s3 rm s3://my-bucket/file.txt
aws s3 rm s3://my-bucket/ --recursive    # Delete all objects

# ─── Make bucket website ───
aws s3 website s3://my-bucket \
  --index-document index.html \
  --error-document error.html

# ─── Presigned URL (temporary access without credentials) ───
aws s3 presign s3://my-bucket/private-file.pdf --expires-in 3600
# Creates URL valid for 1 hour — share with anyone
```

### S3 Security

```
By default: ALL S3 buckets and objects are PRIVATE

Access control options:
1. BUCKET POLICIES (JSON — recommended)
   Applied to entire bucket and its objects

2. ACLs (Access Control Lists — legacy, avoid new use)
   Per-object or per-bucket level permissions

3. IAM POLICIES
   What IAM users/roles can do with S3

4. S3 BLOCK PUBLIC ACCESS (account and bucket level)
   Safety net to prevent accidental public exposure

Example bucket policy (allow public read for static website):
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-website-bucket/*"
  }]
}
```

### S3 Storage Classes (Preview — Deep Dive in Module 16)

```
STANDARD:          Frequently accessed data, low latency
INTELLIGENT-TIERING: Auto-moves between tiers based on access
STANDARD-IA:       Infrequent access, cheaper storage, retrieval fee
ONE ZONE-IA:       Infrequent, single AZ (less durable)
GLACIER INSTANT:   Archive, millisecond retrieval
GLACIER FLEXIBLE:  Archive, minutes-to-hours retrieval
GLACIER DEEP:      Long-term archive, 12-48hr retrieval, cheapest
```

### 🏆 Exam-Style Questions

**Q1:** A company needs to host a static website. Which AWS service is MOST cost-effective?

A) EC2 with Apache
B) Amazon S3 static website hosting ✅
C) Elastic Beanstalk
D) Amazon ECS

**Q2:** An S3 object must be accessible to 10 specific business partners via a URL for 24 hours without requiring AWS credentials. What should you use?

A) Make the bucket public
B) Create IAM users for each partner
C) Generate S3 presigned URLs ✅
D) Use S3 Transfer Acceleration

---

## 11. VPC Fundamentals

### What is a VPC?

**Virtual Private Cloud (VPC)** is your own isolated network within AWS. Think of it as building your own private data center networking inside AWS.

```
Without VPC understanding, you CANNOT:
- Secure your EC2 instances properly
- Design multi-tier architectures
- Pass Solutions Architect exams
- Work as a cloud engineer

VPC is the #1 most important networking concept in AWS
```

### VPC Core Components

```
VPC (Virtual Private Cloud)
│  Your private network — you define the IP range (CIDR)
│  Example: 10.0.0.0/16 → gives you 65,536 IP addresses
│
├── SUBNET
│   │  A subdivision of your VPC in ONE Availability Zone
│   │
│   ├── PUBLIC SUBNET
│   │   Has route to Internet Gateway
│   │   Resources CAN have public IPs
│   │   Example: 10.0.1.0/24 (in AZ-a)
│   │   Put here: Web servers, load balancers, bastion hosts
│   │
│   └── PRIVATE SUBNET
│       No route to Internet Gateway
│       Resources CANNOT be reached from internet (by default)
│       Example: 10.0.2.0/24 (in AZ-a)
│       Put here: Application servers, databases
│
├── INTERNET GATEWAY (IGW)
│   Allows VPC resources to communicate with the internet
│   One per VPC, horizontally scaled, redundant
│   Must attach to VPC AND add to route table
│
├── ROUTE TABLE
│   Rules that determine where network traffic is directed
│   Each subnet associated with one route table
│
│   Public subnet route table:
│     10.0.0.0/16 → local     (VPC traffic stays local)
│     0.0.0.0/0  → igw-xxx    (everything else → internet)
│
│   Private subnet route table:
│     10.0.0.0/16 → local     (VPC traffic stays local)
│     0.0.0.0/0  → nat-xxx    (everything else → NAT Gateway)
│
├── NAT GATEWAY
│   Allows PRIVATE subnet resources to reach internet
│   (for updates, downloads) WITHOUT being reachable FROM internet
│   One-way: outbound only
│   Placed in PUBLIC subnet, uses Elastic IP
│
├── SECURITY GROUPS (Stateful firewall)
│   Virtual firewall for EC2 instances
│   Controls inbound AND outbound traffic
│   Stateful: return traffic automatically allowed
│
└── NETWORK ACL (NACL) — Stateless firewall
    Virtual firewall for SUBNETS
    Applies to all resources in subnet
    Stateless: must explicitly allow return traffic
```

### Production VPC Architecture (Multi-AZ)

```
Region: us-east-1
VPC: 10.0.0.0/16
│
├── AZ: us-east-1a                    ├── AZ: us-east-1b
│   ├── Public Subnet: 10.0.1.0/24   │   ├── Public Subnet: 10.0.3.0/24
│   │   └── ALB (Application LB)     │   │   └── ALB (same load balancer)
│   │   └── NAT Gateway              │   │   └── NAT Gateway
│   │                                │   │
│   └── Private Subnet: 10.0.2.0/24 │   └── Private Subnet: 10.0.4.0/24
│       └── EC2 App Servers          │       └── EC2 App Servers
│       └── RDS Standby              │       └── RDS Primary
│
│                    Internet Gateway (IGW)
│                           ↕
│                      Internet
```

### Hands-On Lab: Create Your First VPC

```bash
# ─── Step 1: Create VPC ───
VPC_ID=$(aws ec2 create-vpc \
  --cidr-block 10.0.0.0/16 \
  --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=my-prod-vpc}]' \
  --query Vpc.VpcId --output text)
echo "VPC: $VPC_ID"

# Enable DNS hostnames (needed for EC2 to get DNS names)
aws ec2 modify-vpc-attribute \
  --vpc-id $VPC_ID \
  --enable-dns-hostnames

# ─── Step 2: Create Internet Gateway ───
IGW_ID=$(aws ec2 create-internet-gateway \
  --query InternetGateway.InternetGatewayId --output text)
aws ec2 attach-internet-gateway --vpc-id $VPC_ID --internet-gateway-id $IGW_ID

# ─── Step 3: Create subnets ───
# Public subnet in AZ-a
PUB_SUBNET_A=$(aws ec2 create-subnet \
  --vpc-id $VPC_ID \
  --cidr-block 10.0.1.0/24 \
  --availability-zone us-east-1a \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=public-1a}]' \
  --query Subnet.SubnetId --output text)

# Private subnet in AZ-a
PRV_SUBNET_A=$(aws ec2 create-subnet \
  --vpc-id $VPC_ID \
  --cidr-block 10.0.2.0/24 \
  --availability-zone us-east-1a \
  --tag-specifications 'ResourceType=subnet,Tags=[{Key=Name,Value=private-1a}]' \
  --query Subnet.SubnetId --output text)

# ─── Step 4: Create and configure route tables ───
# Public route table
PUB_RT=$(aws ec2 create-route-table \
  --vpc-id $VPC_ID \
  --query RouteTable.RouteTableId --output text)

# Add internet route to public RT
aws ec2 create-route \
  --route-table-id $PUB_RT \
  --destination-cidr-block 0.0.0.0/0 \
  --gateway-id $IGW_ID

# Associate public subnet with public RT
aws ec2 associate-route-table \
  --subnet-id $PUB_SUBNET_A \
  --route-table-id $PUB_RT

# Enable auto-assign public IP for public subnet
aws ec2 modify-subnet-attribute \
  --subnet-id $PUB_SUBNET_A \
  --map-public-ip-on-launch

echo "VPC Setup Complete!"
echo "VPC: $VPC_ID"
echo "Public Subnet: $PUB_SUBNET_A"
echo "Private Subnet: $PRV_SUBNET_A"
```

### 🏆 Exam-Style Questions

**Q1:** A company has EC2 instances in a private subnet that need to download software updates from the internet. What should they use?

A) Internet Gateway
B) VPC Peering
C) NAT Gateway ✅
D) Direct Connect

**Q2:** Which VPC component acts as a firewall at the SUBNET level?

A) Security Group
B) Network ACL ✅
C) Route Table
D) Internet Gateway

**Q3:** A web application needs a 3-tier architecture (web, app, database) that is highly available. How many subnets are needed at minimum?

A) 3 subnets (one per tier)
B) 6 subnets (web, app, DB in 2 AZs) ✅
C) 2 subnets
D) 9 subnets (one per tier per AZ)

---

### 🏆 BEGINNER LEVEL COMPREHENSIVE QUIZ

**Question 1 (Multiple Choice):** A startup wants to move to the cloud to avoid large upfront hardware purchases. Which cloud computing benefit does this represent?

A) Increased speed and agility
B) Go global in minutes
C) Trade capital expense for variable expense ✅
D) Stop spending money on data centers

**Question 2 (Scenario):** A banking application must keep all customer data within the European Union due to GDPR regulations. The bank also wants to leverage cloud services for non-sensitive workloads. What deployment model should they use?

A) Public cloud only
B) Private cloud only
C) Hybrid cloud ✅
D) Community cloud

**Question 3 (Multiple Choice):** Who is responsible for patching the operating system on an Amazon EC2 instance?

A) AWS
B) The customer ✅
C) Both AWS and the customer
D) The OS vendor (Microsoft/Red Hat)

**Question 4 (Scenario):** A company's website suddenly goes viral. Traffic increases 10x. Their on-premises servers crash. Which cloud characteristic would prevent this?

A) Broad network access
B) On-demand self-service
C) Rapid elasticity ✅
D) Measured service

**Question 5 (Multiple Choice):** An application runs on EC2 in a single Availability Zone. The company wants to improve availability. What is the SIMPLEST improvement?

A) Move to a different Region
B) Deploy in multiple Availability Zones ✅
C) Enable Enhanced Networking
D) Use a Dedicated Host

**Question 6 (IAM Scenario):** A developer needs read access to S3 and write access to DynamoDB. What is the BEST approach?

A) Give the developer the AdministratorAccess policy
B) Create an IAM user, create a custom policy with exactly these permissions, attach to user ✅
C) Give the developer root access
D) Create a service role

**Question 7 (EC2):** A company needs to run a memory-intensive database. Which instance family should they choose?

A) t3 (General Purpose Burstable)
B) c5 (Compute Optimized)
C) r5 (Memory Optimized) ✅
D) i3 (Storage Optimized)

**Question 8 (S3):** A company stores sensitive financial documents in S3. A new employee accidentally makes a bucket public. What feature would have PREVENTED this?

A) Bucket versioning
B) S3 Block Public Access ✅
C) S3 Transfer Acceleration
D) S3 lifecycle policy

---

# ═══════════════════════════════════════
# PART 2: INTERMEDIATE — ASSOCIATE LEVEL
# ═══════════════════════════════════════

---

## 12. EC2 Deep Dive

### AMIs (Amazon Machine Images)

```
An AMI is a template for your EC2 instance:
  OS + pre-installed software + configuration

Types:
  AWS-provided:  Amazon Linux, Ubuntu, Windows Server, RHEL
  Marketplace:   Vendor AMIs (Nginx pre-installed, etc.) — may cost extra
  Community:     Public AMIs from community — verify before using!
  Custom:        You create them (golden AMI pattern)

Golden AMI Pattern (Production Best Practice):
  1. Start with base AMI
  2. Install all required software and configure
  3. Create AMI from configured instance
  4. Use this "golden AMI" for all new instances
  → Benefit: Instances launch faster (no installation at boot)
              Consistent, tested configuration
              Part of immutable infrastructure pattern
```

```bash
# Create AMI from existing instance
aws ec2 create-image \
  --instance-id i-1234567890abcdef0 \
  --name "MyApp-GoldenAMI-$(date +%Y%m%d)" \
  --description "Production-ready app AMI" \
  --no-reboot \
  --tag-specifications 'ResourceType=image,Tags=[{Key=Environment,Value=production}]'

# Copy AMI to another region (for disaster recovery)
aws ec2 copy-image \
  --source-region us-east-1 \
  --source-image-id ami-12345678 \
  --name "MyApp-GoldenAMI-DR" \
  --region us-west-2
```

### EC2 User Data

```bash
# User Data = script that runs ONCE on first boot
# Used to bootstrap instances (install software, configure)

# Example: Full web server setup
USER_DATA='#!/bin/bash
set -e  # Exit on error

# Update system
yum update -y

# Install web server and tools
yum install -y nginx amazon-cloudwatch-agent jq aws-cli

# Configure nginx
cat > /etc/nginx/conf.d/app.conf << EOF
server {
    listen 80;
    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
EOF

# Pull application config from SSM Parameter Store
APP_ENV=$(aws ssm get-parameter --name "/myapp/env" --with-decryption --query Parameter.Value --output text)

# Write app environment
echo "$APP_ENV" > /etc/myapp/.env

# Start services
systemctl start nginx
systemctl enable nginx

# Signal CloudFormation/Auto Scaling that instance is healthy
/opt/aws/bin/cfn-signal -e $? --region us-east-1 --stack MyStack --resource WebServerGroup
'

# Launch with user data
aws ec2 run-instances \
  --image-id ami-0abcdef1234567890 \
  --instance-type t3.medium \
  --user-data "$USER_DATA" \
  --iam-instance-profile Name=EC2-SSM-Role \
  --key-name my-key
```

### EC2 Instance Metadata

```bash
# From INSIDE the EC2 instance:
# Instance Metadata Service (IMDS) — no credentials needed

# Get instance ID
curl http://169.254.169.254/latest/meta-data/instance-id

# Get availability zone
curl http://169.254.169.254/latest/meta-data/placement/availability-zone

# Get IAM role credentials (for apps without SDK)
curl http://169.254.169.254/latest/meta-data/iam/security-credentials/my-role

# Get instance type
curl http://169.254.169.254/latest/meta-data/instance-type

# Get public IP
curl http://169.254.169.254/latest/meta-data/public-ipv4

# IMDSv2 (more secure — use token first)
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id

# Force IMDSv2 only (security hardening):
aws ec2 modify-instance-metadata-options \
  --instance-id i-1234567890abcdef0 \
  --http-tokens required \
  --http-endpoint enabled
```

### Elastic IPs

```bash
# Elastic IP = Static public IP that persists even if instance restarts
# By default, EC2 public IPs CHANGE on stop/start

# Allocate Elastic IP
EIP_ALLOC=$(aws ec2 allocate-address \
  --domain vpc \
  --query AllocationId --output text)

# Associate with instance
aws ec2 associate-address \
  --instance-id i-1234567890abcdef0 \
  --allocation-id $EIP_ALLOC

# ⚠️ COST: Elastic IP is FREE when associated with running instance
#          CHARGED when not associated (wasted resource!)
# Always release unneeded Elastic IPs

# Disassociate and release
aws ec2 disassociate-address --association-id eipassoc-xxx
aws ec2 release-address --allocation-id $EIP_ALLOC
```

---

## 13. Security Groups vs NACLs

### Security Groups (Instance-Level Firewall)

```
PROPERTIES:
  ✓ Stateful — if inbound allowed, response automatically allowed
  ✓ Applied to instances (ENI level)
  ✓ Only ALLOW rules (no explicit deny)
  ✓ All rules evaluated before decision
  ✓ Default: deny all inbound, allow all outbound

EXAMPLE — Web Server Security Group:
  Inbound:
    Port 80  (HTTP)  from 0.0.0.0/0        → Allow web traffic
    Port 443 (HTTPS) from 0.0.0.0/0        → Allow HTTPS
    Port 22  (SSH)   from 10.0.0.0/8       → Allow SSH from VPN only
  
  Outbound:
    All traffic → 0.0.0.0/0              → Allow all outbound
```

### Security Group Referencing (Power Feature!)

```bash
# Instead of IPs, reference another security group!

# Create App-Tier Security Group:
APP_SG=$(aws ec2 create-security-group \
  --group-name app-tier-sg \
  --description "App tier security group" \
  --vpc-id $VPC_ID \
  --query GroupId --output text)

# Create DB-Tier Security Group:
DB_SG=$(aws ec2 create-security-group \
  --group-name db-tier-sg \
  --description "Database tier security group" \
  --vpc-id $VPC_ID \
  --query GroupId --output text)

# Allow DB tier to accept traffic ONLY from App tier SG:
aws ec2 authorize-security-group-ingress \
  --group-id $DB_SG \
  --protocol tcp \
  --port 3306 \
  --source-group $APP_SG

# Now:
# ✅ App servers can connect to MySQL (port 3306)
# ❌ NOTHING else can reach the database
# If a new app server is added and uses APP_SG → automatically allowed
# This is dynamic and scales automatically!
```

### Network ACLs (Subnet-Level Firewall)

```
PROPERTIES:
  ✗ Stateless — must explicitly allow return traffic
  ✓ Applied to subnets
  ✓ Has ALLOW and DENY rules
  ✓ Rules processed in order (lowest number wins)
  ✓ Default: allow all inbound and outbound

Rule numbers matter! Process lowest to highest, first match wins.

Example NACL for PUBLIC subnet:
  Rule  100: Allow HTTP (80) inbound from 0.0.0.0/0
  Rule  110: Allow HTTPS (443) inbound from 0.0.0.0/0
  Rule  120: Allow SSH (22) inbound from YOUR_IP
  Rule  130: Allow return traffic (1024-65535) inbound  ← REQUIRED for stateless!
  Rule *   : Deny all (default)
  
  (Outbound rules also needed!)
```

### Security Group vs NACL Comparison

```
┌─────────────────────────────────────────────────────────────┐
│                  Security Groups vs NACLs                   │
├─────────────────────────────┬───────────────────────────────┤
│      Security Groups        │          NACLs                │
├─────────────────────────────┼───────────────────────────────┤
│ Instance level (ENI)        │ Subnet level                  │
│ Stateful                    │ Stateless                     │
│ Allow rules only            │ Allow AND Deny rules          │
│ All rules evaluated         │ Rules processed in order      │
│ First line of defense       │ Second line of defense        │
│ Must be explicitly attached │ Auto-applies to all in subnet │
└─────────────────────────────┴───────────────────────────────┘

EXAM TIP: "Block a specific IP from accessing your instances"
→ Use NACL (Security Groups have no DENY rules!)
```

---

## 14. VPC Architecture Deep Dive

### CIDR Blocks & IP Planning

```
CIDR (Classless Inter-Domain Routing) notation:
  10.0.0.0/16 = 65,536 IP addresses (10.0.0.0 to 10.255.255.255)
  10.0.0.0/24 = 256 IP addresses (10.0.0.0 to 10.0.0.255)
  10.0.0.0/28 = 16 IP addresses

AWS reserves 5 IPs in each subnet:
  10.0.1.0   = Network address
  10.0.1.1   = AWS VPC Router
  10.0.1.2   = DNS server
  10.0.1.3   = Reserved for future use
  10.0.1.255 = Broadcast (not used in VPC)
  
So /24 gives you 256 - 5 = 251 usable IPs

IP Planning for Production:
  VPC:                10.0.0.0/16   (65,536 addresses)
  Public-1a:          10.0.1.0/24   (251 usable)
  Public-1b:          10.0.2.0/24
  Public-1c:          10.0.3.0/24
  Private-App-1a:     10.0.10.0/24
  Private-App-1b:     10.0.11.0/24
  Private-App-1c:     10.0.12.0/24
  Private-DB-1a:      10.0.20.0/24
  Private-DB-1b:      10.0.21.0/24
  Private-DB-1c:      10.0.22.0/24
```

### VPC Flow Logs

```bash
# VPC Flow Logs capture network traffic metadata
# Essential for: Security analysis, troubleshooting, compliance

# Create flow log to CloudWatch Logs
aws ec2 create-flow-logs \
  --resource-type VPC \
  --resource-ids $VPC_ID \
  --traffic-type ALL \
  --log-destination-type cloud-watch-logs \
  --log-group-name /aws/vpc/flowlogs \
  --deliver-logs-permission-arn arn:aws:iam::ACCOUNT:role/FlowLogsRole

# Create flow log to S3 (cheaper for long retention)
aws ec2 create-flow-logs \
  --resource-type VPC \
  --resource-ids $VPC_ID \
  --traffic-type ALL \
  --log-destination-type s3 \
  --log-destination arn:aws:s3:::my-vpc-flowlogs-bucket

# Flow log record format:
# version account-id interface-id srcaddr dstaddr srcport dstport 
# protocol packets bytes start end action log-status
# Example:
# 2 123456789 eni-abc REJECT: blocked SSH attempt from internet
```

### Bastion Host Pattern

```
PROBLEM: Private subnet EC2 instances are not accessible from internet
SOLUTION: Bastion Host (Jump Server)

Architecture:
  Internet → [Bastion Host in PUBLIC subnet] → SSH → [Private EC2]

Security rules:
  Bastion SG: Allow SSH (22) from YOUR_IP only
  Private EC2 SG: Allow SSH (22) from Bastion SG only
  
  NOBODY can SSH directly to private instances
  Only via bastion → audit trail

# Modern alternative: AWS Systems Manager Session Manager
# NO bastion host needed, NO open ports, fully audited
aws ssm start-session --target i-1234567890abcdef0
```

---

## 15. EBS vs EFS

### EBS (Elastic Block Store)

```
Think of it as: External hard drive for your EC2 instance

Properties:
  ✓ Block storage (raw disk — you format and mount it)
  ✓ Attached to ONE instance at a time (in same AZ!)
  ✓ Persists independently of instance
  ✓ Can snapshot to S3 (incremental backups)
  ✓ Can be encrypted

Volume Types:
  gp3 (General Purpose SSD) — DEFAULT
    - 3,000 IOPS baseline (up to 16,000)
    - 125 MB/s baseline (up to 1,000)
    - Best for: Most workloads, OS volumes, dev
    
  io2 Block Express (Provisioned IOPS SSD) — PERFORMANCE
    - Up to 256,000 IOPS
    - For: Mission-critical databases (Oracle, SQL Server)
    - Expensive
    
  st1 (Throughput Optimized HDD) — THROUGHPUT
    - Low IOPS, HIGH throughput (500 MB/s)
    - For: Big data, data warehouses, log processing
    - Cannot be boot volume
    
  sc1 (Cold HDD) — CHEAPEST
    - Lowest cost
    - For: Infrequently accessed data
    - Cannot be boot volume
```

```bash
# Create and attach EBS volume
VOLUME_ID=$(aws ec2 create-volume \
  --size 100 \
  --volume-type gp3 \
  --availability-zone us-east-1a \
  --iops 3000 \
  --throughput 125 \
  --encrypted \
  --tag-specifications 'ResourceType=volume,Tags=[{Key=Name,Value=my-data-volume}]' \
  --query VolumeId --output text)

# Attach to instance
aws ec2 attach-volume \
  --instance-id i-1234567890abcdef0 \
  --volume-id $VOLUME_ID \
  --device /dev/sdf

# On the instance (format and mount):
# lsblk                          # See the new volume
# mkfs.ext4 /dev/xvdf            # Format
# mkdir /data
# mount /dev/xvdf /data          # Mount
# echo "/dev/xvdf /data ext4 defaults 0 0" >> /etc/fstab  # Auto-mount

# Create snapshot (backup)
aws ec2 create-snapshot \
  --volume-id $VOLUME_ID \
  --description "Daily backup $(date +%Y-%m-%d)"
```

### EFS (Elastic File System)

```
Think of it as: Shared network drive (NFS) for multiple EC2 instances

Properties:
  ✓ Shared across MULTIPLE instances simultaneously
  ✓ Works across Multiple AZs
  ✓ Automatically grows and shrinks
  ✓ NFS protocol (mount like network drive)
  ✓ More expensive than EBS per GB

Use cases:
  - Shared content (CMS, media files)
  - Home directories for EC2 fleet
  - Container shared storage
  - Big data analytics
```

### EBS vs EFS vs S3 Comparison

```
┌─────────────────────────────────────────────────────────────────┐
│              When to Use Which Storage                          │
├────────────────┬────────────────┬─────────────────────────────┤
│      EBS       │      EFS       │            S3               │
├────────────────┼────────────────┼─────────────────────────────┤
│ Single instance│ Multiple inst. │ Any (via HTTPS)             │
│ Same AZ only   │ Multi-AZ       │ Regional (global reads)     │
│ Block storage  │ File storage   │ Object storage              │
│ OS/DB volumes  │ Shared data    │ Files, backups, archives    │
│ Low latency    │ Medium latency │ Higher latency              │
│ Middle price   │ More expensive │ Cheapest at scale           │
│ No auto-scale  │ Auto-scales    │ Unlimited                   │
└────────────────┴────────────────┴─────────────────────────────┘
```

---

## 16. S3 Storage Classes & Lifecycle

### Storage Classes Comparison

```
STANDARD:
  Use: Frequently accessed data
  Availability: 99.99%
  Durability: 99.999999999% (11 9s)
  Min storage: None
  Retrieval: Milliseconds
  Cost: Highest per GB

INTELLIGENT-TIERING:
  Use: Unknown or changing access patterns
  Auto-moves between Frequent and Infrequent tiers
  Small monthly monitoring fee
  No retrieval charges
  Best for: Long-lived data with unknown patterns

STANDARD-IA (Infrequent Access):
  Use: Accessed < once per month, needs fast retrieval
  Lower storage cost + retrieval fee
  Min storage: 30 days, 128KB per object
  Use: Disaster recovery files, backups

ONE ZONE-IA:
  Same as IA but only ONE AZ (cheaper, less resilient)
  20% cheaper than Standard-IA
  Use: Recreatable data, secondary backups

GLACIER INSTANT RETRIEVAL:
  Archival with millisecond retrieval
  Min storage: 90 days
  Cheaper than IA for rarely accessed

GLACIER FLEXIBLE RETRIEVAL:
  True archive, 3 retrieval options:
  - Expedited: 1-5 minutes (most expensive)
  - Standard: 3-5 hours
  - Bulk: 5-12 hours (cheapest)
  Min storage: 90 days

GLACIER DEEP ARCHIVE:
  Cheapest storage on AWS
  Retrieval: 12-48 hours
  Min storage: 180 days
  Use: Long-term compliance archives, 7-year retention
```

### S3 Lifecycle Policies

```bash
# Automatically transition objects between classes based on age

aws s3api put-bucket-lifecycle-configuration \
  --bucket my-data-bucket \
  --lifecycle-configuration '{
    "Rules": [
      {
        "ID": "ArchiveOldData",
        "Status": "Enabled",
        "Transitions": [
          {
            "Days": 30,
            "StorageClass": "STANDARD_IA"
          },
          {
            "Days": 90,
            "StorageClass": "GLACIER"
          },
          {
            "Days": 365,
            "StorageClass": "DEEP_ARCHIVE"
          }
        ],
        "Expiration": {
          "Days": 2555
        },
        "NoncurrentVersionTransitions": [
          {
            "NoncurrentDays": 30,
            "StorageClass": "GLACIER"
          }
        ]
      }
    ]
  }'
```

### S3 Versioning, Replication, and MFA Delete

```bash
# Enable versioning (protects against accidental deletion)
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled

# Enable Cross-Region Replication (CRR) for DR
aws s3api put-bucket-replication \
  --bucket source-bucket \
  --replication-configuration '{
    "Role": "arn:aws:iam::ACCOUNT:role/s3-replication-role",
    "Rules": [{
      "Status": "Enabled",
      "Destination": {
        "Bucket": "arn:aws:s3:::destination-bucket",
        "StorageClass": "STANDARD"
      }
    }]
  }'

# Enable MFA Delete (extra protection for versioned buckets)
# Requires MFA code to delete objects or disable versioning
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled,MFADelete=Enabled \
  --mfa "arn:aws:iam::ACCOUNT:mfa/root-device TOTP_CODE"
```

---

## 17. Load Balancers: ALB & NLB

### Why Load Balancers?

```
PROBLEM: Single server → single point of failure
         Traffic spikes → server overloaded

SOLUTION: Load Balancer distributes traffic across multiple servers
          + Health checks (removes failed servers automatically)
          + SSL termination
          + Single DNS endpoint for multiple servers
```

### Application Load Balancer (ALB) — Layer 7

```
Layer 7 = HTTP/HTTPS level routing
Smart routing based on URL, headers, query strings

Features:
  ✓ Host-based routing: api.example.com → API servers
                         app.example.com → App servers
  ✓ Path-based routing: /api/* → API servers
                         /images/* → Media servers
  ✓ SSL/TLS termination (HTTPS → HTTP backend)
  ✓ WebSockets support
  ✓ HTTP/2 and gRPC support
  ✓ User authentication via OIDC/SAML (Cognito)
  ✓ Fixed response (return 200 without hitting backend)
  ✓ Weighted routing (canary deployments!)
  
Target types:
  - EC2 instances
  - IP addresses (private)
  - Lambda functions
  - ECS containers
```

```bash
# Create ALB
ALB_ARN=$(aws elbv2 create-load-balancer \
  --name my-app-lb \
  --type application \
  --scheme internet-facing \
  --subnets subnet-pub-1a subnet-pub-1b subnet-pub-1c \
  --security-groups $ALB_SG_ID \
  --query 'LoadBalancers[0].LoadBalancerArn' \
  --output text)

# Create target group
TG_ARN=$(aws elbv2 create-target-group \
  --name web-servers \
  --protocol HTTP \
  --port 80 \
  --vpc-id $VPC_ID \
  --health-check-path /health \
  --health-check-interval-seconds 30 \
  --healthy-threshold-count 2 \
  --unhealthy-threshold-count 3 \
  --query 'TargetGroups[0].TargetGroupArn' \
  --output text)

# Register targets
aws elbv2 register-targets \
  --target-group-arn $TG_ARN \
  --targets Id=i-1234567890abcdef0 Id=i-0987654321abcdef

# Create HTTPS listener
aws elbv2 create-listener \
  --load-balancer-arn $ALB_ARN \
  --protocol HTTPS \
  --port 443 \
  --certificates CertificateArn=arn:aws:acm:us-east-1:ACCOUNT:certificate/cert-id \
  --default-actions Type=forward,TargetGroupArn=$TG_ARN

# Create routing rule — /api/* goes to API target group
aws elbv2 create-rule \
  --listener-arn $LISTENER_ARN \
  --conditions Field=path-pattern,Values='/api/*' \
  --priority 10 \
  --actions Type=forward,TargetGroupArn=$API_TG_ARN
```

### Network Load Balancer (NLB) — Layer 4

```
Layer 4 = TCP/UDP level routing
Extremely high performance — handles millions of requests/second

Features:
  ✓ Ultra-low latency (microseconds vs ALB milliseconds)
  ✓ Static IP addresses (one per AZ) — ALB has none
  ✓ Elastic IP support
  ✓ TCP, UDP, TLS protocols
  ✓ Source IP preservation (backend sees real client IP)
  ✓ Zonal isolation

Use NLB when:
  - Gaming (UDP, low latency)
  - IoT (MQTT)
  - Financial trading (ultra-low latency)
  - Need static IP for whitelisting
  - Non-HTTP protocols (SSH, SMTP, LDAP)
  
Use ALB when:
  - HTTP/HTTPS web apps (most use cases!)
  - Microservices routing
  - Content-based routing
```

### ALB vs NLB vs CLB Comparison

```
┌─────────────┬──────────────┬──────────────┬────────────────┐
│             │     ALB      │     NLB      │   CLB (Legacy) │
├─────────────┼──────────────┼──────────────┼────────────────┤
│ OSI Layer   │ 7 (HTTP)     │ 4 (TCP/UDP)  │ 4 & 7          │
│ Performance │ High         │ Ultra-high   │ Medium         │
│ Static IP   │ ❌           │ ✅           │ ❌             │
│ Host routing│ ✅           │ ❌           │ ❌             │
│ Path routing│ ✅           │ ❌           │ ❌             │
│ Websockets  │ ✅           │ ✅           │ ✅             │
│ Use case    │ Web apps     │ Gaming/IoT   │ Avoid (legacy) │
└─────────────┴──────────────┴──────────────┴────────────────┘
```

---

## 18. Auto Scaling

### What is Auto Scaling?

Auto Scaling automatically adjusts the number of EC2 instances based on demand.

```
LOW TRAFFIC:   Scale IN  → remove instances (save money)
HIGH TRAFFIC:  Scale OUT → add instances (handle load)
Instance dies: Replace   → maintain desired count
```

### Auto Scaling Components

```
1. LAUNCH TEMPLATE (what to launch)
   - Which AMI
   - Instance type
   - Security groups
   - User data
   - IAM role

2. AUTO SCALING GROUP (how many and where)
   - Min capacity: Never go below this
   - Desired capacity: Start with this many
   - Max capacity: Never exceed this
   - Which subnets (spread across AZs)
   - Which load balancer to attach to

3. SCALING POLICIES (when to scale)
   - Target Tracking: "Keep CPU at 50%"
   - Step Scaling: "If CPU > 70%, add 2 instances"
   - Scheduled: "Add 10 instances at 9 AM every weekday"
```

```bash
# Create Launch Template
LT_ID=$(aws ec2 create-launch-template \
  --launch-template-name my-app-template \
  --version-description "v1.0" \
  --launch-template-data '{
    "ImageId": "ami-0abcdef1234567890",
    "InstanceType": "t3.medium",
    "SecurityGroupIds": ["sg-1234567890abcdef0"],
    "IamInstanceProfile": {"Name": "EC2-App-Role"},
    "UserData": "'"$(base64 -w 0 user-data.sh)"'",
    "TagSpecifications": [{
      "ResourceType": "instance",
      "Tags": [{"Key": "Environment", "Value": "production"}]
    }]
  }' \
  --query 'LaunchTemplate.LaunchTemplateId' --output text)

# Create Auto Scaling Group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name my-app-asg \
  --launch-template LaunchTemplateId=$LT_ID,Version='$Latest' \
  --min-size 2 \
  --desired-capacity 4 \
  --max-size 20 \
  --vpc-zone-identifier "subnet-1a,subnet-1b,subnet-1c" \
  --target-group-arns $TG_ARN \
  --health-check-type ELB \
  --health-check-grace-period 300 \
  --default-cooldown 300

# Create Target Tracking Scaling Policy (most common)
aws autoscaling put-scaling-policy \
  --auto-scaling-group-name my-app-asg \
  --policy-name cpu-target-tracking \
  --policy-type TargetTrackingScaling \
  --target-tracking-configuration '{
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "ASGAverageCPUUtilization"
    },
    "TargetValue": 50.0
  }'

# Verify scaling activity
aws autoscaling describe-scaling-activities \
  --auto-scaling-group-name my-app-asg \
  --max-items 10
```

### Scaling Policies Deep Dive

```
TARGET TRACKING (Recommended):
  "Keep metric at X"
  Examples:
  - ASGAverageCPUUtilization at 50%
  - ALBRequestCountPerTarget at 1000
  - ASGAverageNetworkIn at 100MB/s
  AWS automatically creates CloudWatch alarms for you
  
STEP SCALING:
  "When metric crosses threshold, add/remove N instances"
  More control, useful for aggressive scaling
  Example:
    CPU 60-70%: Add 1 instance
    CPU 70-80%: Add 2 instances  
    CPU > 80%:  Add 4 instances
    CPU 40-50%: Remove 1 instance
    CPU < 40%:  Remove 2 instances

SCHEDULED SCALING:
  "At specific time, set capacity to N"
  Example:
    9:00 AM Mon-Fri: min=10, desired=20, max=40
    6:00 PM Mon-Fri: min=2,  desired=4,  max=10
    Black Friday: min=50, desired=100, max=200
```

---

## 19. Route 53 DNS

### What is Route 53?

Route 53 is AWS's DNS (Domain Name System) service. Named after TCP/UDP port 53 (DNS port).

```
DNS Function: Translate domain names to IP addresses
  "www.mycompany.com" → "54.239.17.6"

Route 53 also does:
  - Domain registration
  - Health checks (remove unhealthy endpoints)
  - Traffic routing (geo, weighted, latency-based)
  - Private DNS (within your VPC)
```

### Route 53 Record Types

```
A Record:      domain → IPv4 address
               example.com → 93.184.216.34

AAAA Record:   domain → IPv6 address

CNAME:         name → another name (cannot be used for zone apex!)
               www.example.com → myalb-123.us-east-1.elb.amazonaws.com
               
ALIAS:         AWS-specific: name → AWS resource
               Can be used for zone apex (example.com, not just www.)
               No additional DNS lookup cost
               Supports: ALB, ELB, CloudFront, S3 Website, API Gateway
               
MX Record:     Mail server records
TXT Record:    Text (DNS verification, SPF, DKIM)
NS Record:     Name server records for the domain
SOA Record:    Start of Authority — zone info
```

### Routing Policies

```
1. SIMPLE — Basic: domain → one or more IPs
   Random selection if multiple values
   No health checks

2. WEIGHTED — Route X% to one endpoint, Y% to another
   A/B testing: 90% to v1, 10% to v2
   Gradual migration: 99% old → 1% new

3. LATENCY-BASED — Route to lowest latency AWS Region
   User in Europe → eu-west-1
   User in Asia → ap-southeast-1

4. FAILOVER — Active/Passive high availability
   Primary: us-east-1 (active when healthy)
   Secondary: us-west-2 (standby, activated if primary fails)

5. GEOLOCATION — Route based on user's geographic location
   Users from Germany → eu-central-1 (legal/language reasons)
   Users from US → us-east-1
   
6. GEOPROXIMITY — Route based on location + bias
   Can expand/shrink traffic bias for regions

7. MULTIVALUE ANSWER — Like Simple but with health checks
   Returns up to 8 healthy records randomly
   NOT a replacement for load balancer!

8. IP-BASED — Route based on client's IP range (new)
```

```bash
# Create hosted zone
aws route53 create-hosted-zone \
  --name mycompany.com \
  --caller-reference $(date +%s) \
  --hosted-zone-config Comment="Production zone"

# Create A record pointing to ALB
ZONE_ID="Z1234567890ABCDE"
aws route53 change-resource-record-sets \
  --hosted-zone-id $ZONE_ID \
  --change-batch '{
    "Changes": [{
      "Action": "CREATE",
      "ResourceRecordSet": {
        "Name": "www.mycompany.com",
        "Type": "A",
        "AliasTarget": {
          "HostedZoneId": "Z35SXDOTRQ7X7K",
          "DNSName": "my-alb-123.us-east-1.elb.amazonaws.com",
          "EvaluateTargetHealth": true
        }
      }
    }]
  }'

# Create weighted routing (A/B test)
aws route53 change-resource-record-sets \
  --hosted-zone-id $ZONE_ID \
  --change-batch '{
    "Changes": [
      {
        "Action": "CREATE",
        "ResourceRecordSet": {
          "Name": "api.mycompany.com",
          "Type": "A",
          "SetIdentifier": "v1-production",
          "Weight": 90,
          "AliasTarget": {
            "HostedZoneId": "Z35SXDOTRQ7X7K",
            "DNSName": "v1-alb.us-east-1.elb.amazonaws.com",
            "EvaluateTargetHealth": true
          }
        }
      },
      {
        "Action": "CREATE",
        "ResourceRecordSet": {
          "Name": "api.mycompany.com",
          "Type": "A",
          "SetIdentifier": "v2-canary",
          "Weight": 10,
          "AliasTarget": {
            "HostedZoneId": "Z35SXDOTRQ7X7K",
            "DNSName": "v2-alb.us-east-1.elb.amazonaws.com",
            "EvaluateTargetHealth": true
          }
        }
      }
    ]
  }'
```

---

## 20. CloudWatch Monitoring

### What is CloudWatch?

CloudWatch is AWS's native monitoring and observability service. It's the nervous system of your AWS infrastructure.

```
CloudWatch provides:
  METRICS:    Numerical measurements (CPU%, request count, latency)
  LOGS:       Store and analyze log files
  ALARMS:     Alert when metrics cross thresholds
  DASHBOARDS: Visual monitoring panels
  EVENTS:     React to AWS state changes (EventBridge)
  INSIGHTS:   Log analytics with query language
```

### CloudWatch Metrics

```bash
# View EC2 CPU utilization
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=InstanceId,Value=i-1234567890abcdef0 \
  --start-time 2024-01-15T00:00:00Z \
  --end-time 2024-01-15T23:59:00Z \
  --period 3600 \
  --statistics Average,Maximum

# Custom metric (send your own data)
aws cloudwatch put-metric-data \
  --namespace "MyApp/Performance" \
  --metric-name "OrdersPerMinute" \
  --value 450 \
  --unit Count

# List available metrics for EC2
aws cloudwatch list-metrics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization
```

### CloudWatch Alarms

```bash
# Create alarm: CPU > 80% for 5 minutes → send SNS notification + Auto Scale
aws cloudwatch put-metric-alarm \
  --alarm-name "High-CPU-Alarm" \
  --alarm-description "Trigger alert when CPU > 80%" \
  --metric-name CPUUtilization \
  --namespace AWS/EC2 \
  --dimensions Name=AutoScalingGroupName,Value=my-app-asg \
  --statistic Average \
  --period 300 \
  --evaluation-periods 2 \
  --threshold 80 \
  --comparison-operator GreaterThanThreshold \
  --alarm-actions \
    arn:aws:sns:us-east-1:ACCOUNT:ops-alerts \
    arn:aws:autoscaling:us-east-1:ACCOUNT:scalingPolicy:xxx \
  --ok-actions arn:aws:sns:us-east-1:ACCOUNT:ops-alerts \
  --treat-missing-data breaching
```

### CloudWatch Logs & Insights

```bash
# Create log group
aws logs create-log-group --log-group-name /myapp/application

# Set retention (important for cost!)
aws logs put-retention-policy \
  --log-group-name /myapp/application \
  --retention-in-days 30

# CloudWatch Logs Insights query (SQL-like)
aws logs start-query \
  --log-group-name /myapp/application \
  --start-time $(date -d '1 hour ago' +%s) \
  --end-time $(date +%s) \
  --query-string '
    fields @timestamp, @message
    | filter @message like /ERROR/
    | stats count(*) as errorCount by bin(5m)
    | sort @timestamp desc
  '
```

### CloudWatch Agent (Custom Metrics from EC2)

```bash
# Install CloudWatch Agent on EC2
sudo yum install amazon-cloudwatch-agent -y

# Configure agent (collect memory, disk, custom app metrics)
sudo tee /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json << 'EOF'
{
  "metrics": {
    "metrics_collected": {
      "mem": {
        "measurement": ["mem_used_percent"],
        "metrics_collection_interval": 60
      },
      "disk": {
        "measurement": ["disk_used_percent"],
        "resources": ["/", "/data"],
        "metrics_collection_interval": 60
      },
      "cpu": {
        "measurement": ["cpu_usage_idle", "cpu_usage_user"],
        "metrics_collection_interval": 60
      }
    }
  },
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/myapp/app.log",
            "log_group_name": "/myapp/application",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
EOF

sudo systemctl start amazon-cloudwatch-agent
sudo systemctl enable amazon-cloudwatch-agent
```

---

## 21. RDS & Database Services

### Amazon RDS Overview

```
RDS = Managed relational database service
You choose: Engine + Instance type + Storage
AWS manages: Hardware, OS, patches, backups, replication

Supported engines:
  MySQL         (most popular, free engine)
  PostgreSQL    (advanced features)
  MariaDB       (MySQL fork)
  Oracle        (enterprise, bring license or license-included)
  SQL Server    (Windows shops, licensing options)
  Amazon Aurora (AWS-built, MySQL/PostgreSQL compatible, 5x faster)
```

### RDS vs Self-Managed DB on EC2

```
RDS:                              EC2 + DB:
────                              ─────────
AWS patches OS/DB                 You patch everything
Automated backups                 You configure backups
Multi-AZ built-in                 You build replication
Read replicas easy                You set up read replicas
Performance Insights              You build monitoring
35 backup retention max           Unlimited retention
Can't SSH to OS                   Full OS access
Less flexible                     More flexible
Pay more for management           Pay less + your time
```

### RDS High Availability: Multi-AZ

```
Multi-AZ Deployment:
  PRIMARY DB: us-east-1a (active, handles all traffic)
  STANDBY DB: us-east-1b (synchronous replication, no traffic)
  
  Failover scenarios:
    - Primary AZ failure → Failover in 1-2 minutes automatically
    - DB crashes → Failover to standby
    - DB maintenance → Failover to standby
  
  Key points:
    - Standby is NOT used for reads (it's standby-only)
    - DNS CNAME automatically updates to standby during failover
    - Multi-AZ = high availability (HA), NOT performance
    - For performance → Read Replicas
```

### RDS Read Replicas

```
Read Replicas:
  - Asynchronous replication from primary
  - Can read from replicas (scale read traffic)
  - Can be in different Region (Cross-Region Read Replica)
  - Can be promoted to standalone DB (for migrations)
  - Up to 15 replicas (Aurora) or 5 (MySQL/PostgreSQL)
  
Use cases:
  - Scale read-heavy workloads (reporting, analytics)
  - Cross-region DR (replica in different region)
  - Database migration (promote replica)
```

```bash
# Create RDS MySQL instance
aws rds create-db-instance \
  --db-instance-identifier prod-mysql-db \
  --db-instance-class db.t3.medium \
  --engine mysql \
  --engine-version "8.0" \
  --master-username admin \
  --master-user-password 'Super$ecret123' \
  --allocated-storage 100 \
  --storage-type gp3 \
  --storage-encrypted \
  --vpc-security-group-ids $DB_SG_ID \
  --db-subnet-group-name my-db-subnet-group \
  --multi-az \
  --backup-retention-period 7 \
  --preferred-backup-window "02:00-03:00" \
  --preferred-maintenance-window "Mon:03:00-Mon:04:00" \
  --deletion-protection \
  --tags Key=Environment,Value=production

# Create read replica
aws rds create-db-instance-read-replica \
  --db-instance-identifier prod-mysql-read-1 \
  --source-db-instance-identifier prod-mysql-db \
  --db-instance-class db.t3.medium

# Create automated snapshot
aws rds create-db-snapshot \
  --db-instance-identifier prod-mysql-db \
  --db-snapshot-identifier manual-backup-$(date +%Y%m%d)
```

### Amazon Aurora

```
Aurora is AWS's cloud-native database:
  - Compatible with MySQL and PostgreSQL (drop-in replacement)
  - 5x faster than MySQL, 3x faster than PostgreSQL
  - Storage auto-scales from 10GB to 128TB
  - 6 copies of data across 3 AZs (built-in)
  - Up to 15 read replicas
  - Aurora Serverless: auto-scale compute (pay per second)
  
Aurora storage:
  - Shared storage across all instances
  - Not replicated per-instance — shared cluster volume
  - Much faster replication (in-memory, not disk-based)
  
Aurora Global Database:
  - Primary cluster in one Region
  - Up to 5 secondary clusters in other Regions
  - < 1 second lag globally
  - For: global apps, disaster recovery across regions
```

---

## 22. CloudFront CDN

### What is CloudFront?

CloudFront is AWS's Content Delivery Network (CDN). It caches content at 400+ edge locations globally.

```
WITHOUT CloudFront:
  User in India → Request → S3/EC2 in US (13,000 km) → 500ms latency

WITH CloudFront:
  User in India → CloudFront edge in Mumbai → <10ms latency
  (If cached, otherwise → origin, then cached for next user)
```

### CloudFront Architecture

```
ORIGIN (your content source):
  - S3 bucket (static websites, files)
  - ALB (dynamic web apps)
  - EC2 (custom HTTP servers)
  - Any HTTP server (even on-premises!)

DISTRIBUTION:
  - Global deployment of your content config
  - Gets a *.cloudfront.net domain
  - You configure custom domain + SSL

BEHAVIORS:
  Rules for how CloudFront handles requests
  Example:
    /images/*   → Cache for 86400 seconds (1 day)
    /api/*      → Never cache (TTL=0), pass through
    /*.html     → Cache for 300 seconds (5 minutes)

CACHE:
  TTL: How long to cache (time-to-live)
  Invalidation: Remove items from cache (/images/* → clear all images)
```

```bash
# Create CloudFront distribution (S3 origin)
aws cloudfront create-distribution \
  --distribution-config '{
    "CallerReference": "my-distribution-001",
    "Comment": "My website distribution",
    "DefaultCacheBehavior": {
      "TargetOriginId": "S3-my-website-bucket",
      "ViewerProtocolPolicy": "redirect-to-https",
      "CachePolicyId": "658327ea-f89d-4fab-a63d-7e88639e58f6",
      "Compress": true
    },
    "Origins": {
      "Quantity": 1,
      "Items": [{
        "Id": "S3-my-website-bucket",
        "DomainName": "my-website-bucket.s3.amazonaws.com",
        "S3OriginConfig": {"OriginAccessIdentity": ""}
      }]
    },
    "Enabled": true,
    "DefaultRootObject": "index.html",
    "HttpVersion": "http2"
  }'

# Invalidate cache (force refresh)
aws cloudfront create-invalidation \
  --distribution-id EDFDVBD6EXAMPLE \
  --paths "/images/*" "/index.html"
```

### CloudFront Security

```
OAC (Origin Access Control) — CURRENT recommended method
  - CloudFront only can access S3 bucket
  - S3 bucket is private, NO public access
  - Requests to S3 directly are denied
  - Only CloudFront edge → S3 works

WAF (Web Application Firewall) integration:
  - Block SQL injection, XSS
  - Rate limiting (DDoS protection)
  - IP blocking
  - Geo-restriction (block specific countries)

Signed URLs and Signed Cookies:
  - Control access to premium content
  - Time-limited access
  - IP-restricted access
  - Example: Netflix-style "play this video for 24 hours"
```

---

## 23. Elastic Beanstalk

### What is Elastic Beanstalk?

```
Elastic Beanstalk = AWS managed PaaS

You provide: Your application code
AWS provides: Everything else (EC2, LB, ASG, RDS, monitoring)

Supported platforms:
  Java, .NET, Node.js, PHP, Python, Ruby, Go, Docker
  
Under the hood, it creates:
  - EC2 instances (your app servers)
  - Auto Scaling Group
  - Load Balancer (ALB)
  - Security Groups
  - CloudWatch alarms
  - S3 (artifact storage)

You still CONTROL the underlying resources
You can SSH in, modify security groups, etc.
```

```bash
# Install EB CLI
pip install awsebcli

# Initialize application
cd my-app
eb init my-application \
  --platform "Python 3.9 running on 64bit Amazon Linux 2" \
  --region us-east-1

# Create environment
eb create production \
  --instance-type t3.medium \
  --min-instances 2 \
  --max-instances 10 \
  --elb-type application

# Deploy new version
eb deploy production

# View logs
eb logs

# Open app URL
eb open

# Monitor health
eb health
eb status

# Scale manually
eb scale 5 --environment production
```

---

## 24. Lambda & Serverless

### What is Lambda?

```
Lambda = Run code without managing servers

You provide:  Function code + handler
AWS provides: Servers, scaling, HA, patching, monitoring

Properties:
  - Event-driven: runs in response to events
  - Auto-scales: 0 to 10,000+ concurrent executions
  - Pay per use: charged per 1ms of execution time + requests
  - Stateless: no persistent state between invocations
  - Max timeout: 15 minutes per execution
  - Max memory: 10,240 MB (10 GB)

Supported runtimes:
  Node.js, Python, Java, Go, .NET, Ruby, C++
  Custom Runtime: run ANY language!
```

### Lambda Triggers (Event Sources)

```
API Gateway    → REST API backend
S3             → Process uploaded files (resize images, parse CSV)
DynamoDB       → React to database changes (streams)
SQS/SNS        → Process messages
EventBridge    → Scheduled jobs (cron), AWS events
Kinesis        → Real-time stream processing
Cognito        → User authentication hooks
CloudWatch     → Automated operations
ALB            → HTTP endpoint (alternative to API Gateway)
```

```python
# Python Lambda function example
import json
import boto3
import os

s3 = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ['TABLE_NAME'])

def lambda_handler(event, context):
    """
    Triggered by S3 upload event.
    Saves file metadata to DynamoDB.
    """
    try:
        # Parse S3 event
        for record in event['Records']:
            bucket = record['s3']['bucket']['name']
            key = record['s3']['object']['key']
            size = record['s3']['object']['size']
            
            # Get object metadata
            response = s3.head_object(Bucket=bucket, Key=key)
            content_type = response['ContentType']
            
            # Save to DynamoDB
            table.put_item(Item={
                'file_key': key,
                'bucket': bucket,
                'size': size,
                'content_type': content_type,
                'uploaded_at': record['eventTime'],
                'status': 'PROCESSING'
            })
            
            print(f"Processed: s3://{bucket}/{key}")
        
        return {
            'statusCode': 200,
            'body': json.dumps({'message': 'Success'})
        }
    
    except Exception as e:
        print(f"Error: {str(e)}")
        raise  # Re-raise so Lambda marks as failed
```

```bash
# Package and deploy Lambda
cd lambda-function

# Create deployment package
zip -r function.zip . -x "*.git*"

# Create function
aws lambda create-function \
  --function-name process-uploads \
  --runtime python3.11 \
  --role arn:aws:iam::ACCOUNT:role/lambda-execution-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip \
  --environment Variables='{
    "TABLE_NAME": "file-metadata",
    "ENVIRONMENT": "production"
  }' \
  --timeout 30 \
  --memory-size 512 \
  --reserved-concurrency 100

# Update function code
aws lambda update-function-code \
  --function-name process-uploads \
  --zip-file fileb://function.zip

# Add S3 trigger
aws lambda add-permission \
  --function-name process-uploads \
  --statement-id S3Trigger \
  --action lambda:InvokeFunction \
  --principal s3.amazonaws.com \
  --source-arn arn:aws:s3:::my-upload-bucket

aws s3api put-bucket-notification-configuration \
  --bucket my-upload-bucket \
  --notification-configuration '{
    "LambdaFunctionConfigurations": [{
      "LambdaFunctionArn": "arn:aws:lambda:us-east-1:ACCOUNT:function:process-uploads",
      "Events": ["s3:ObjectCreated:*"]
    }]
  }'

# Test function manually
aws lambda invoke \
  --function-name process-uploads \
  --payload '{"key": "test", "value": "test"}' \
  --log-type Tail \
  output.json \
  --query 'LogResult' --output text | base64 -d
```

### Lambda Layers

```bash
# Layers = shared code/dependencies for multiple functions
# Package common libraries once, reference in functions

# Create a layer with Python dependencies
mkdir python
pip install requests boto3 -t python/
zip -r layer.zip python/

aws lambda publish-layer-version \
  --layer-name common-dependencies \
  --description "Common Python dependencies" \
  --zip-file fileb://layer.zip \
  --compatible-runtimes python3.11

# Add layer to function
aws lambda update-function-configuration \
  --function-name my-function \
  --layers arn:aws:lambda:us-east-1:ACCOUNT:layer:common-dependencies:1
```

---

## 25. API Gateway

### What is API Gateway?

```
API Gateway = Fully managed API service

Creates:
  - REST APIs
  - HTTP APIs (cheaper, faster for proxy use cases)
  - WebSocket APIs (real-time two-way communication)

Features:
  - Request/response transformation
  - Authentication (IAM, Cognito, Lambda authorizers)
  - Rate limiting and throttling
  - API versioning
  - SDK generation
  - CORS handling
  - Caching
```

### Serverless API Architecture

```
Client (Browser/Mobile)
    ↓ HTTPS
API Gateway
    ↓ Invoke (IAM auth internally)
Lambda Function
    ↓ SDK calls
DynamoDB / RDS / S3

Benefits:
  - Zero server management
  - Pay per request (not per hour)
  - Auto-scales to millions of requests
  - Built-in TLS/SSL
```

```bash
# Create REST API
API_ID=$(aws apigateway create-rest-api \
  --name my-api \
  --description "My REST API" \
  --endpoint-configuration types=REGIONAL \
  --query id --output text)

# Get root resource ID
ROOT_ID=$(aws apigateway get-resources \
  --rest-api-id $API_ID \
  --query 'items[?path==`/`].id' --output text)

# Create /users resource
USERS_ID=$(aws apigateway create-resource \
  --rest-api-id $API_ID \
  --parent-id $ROOT_ID \
  --path-part "users" \
  --query id --output text)

# Create GET method
aws apigateway put-method \
  --rest-api-id $API_ID \
  --resource-id $USERS_ID \
  --http-method GET \
  --authorization-type NONE

# Connect to Lambda
aws apigateway put-integration \
  --rest-api-id $API_ID \
  --resource-id $USERS_ID \
  --http-method GET \
  --type AWS_PROXY \
  --integration-http-method POST \
  --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:ACCOUNT:function:get-users/invocations"

# Deploy API
aws apigateway create-deployment \
  --rest-api-id $API_ID \
  --stage-name production \
  --stage-description "Production deployment"

# API URL: https://API_ID.execute-api.us-east-1.amazonaws.com/production/users
```

---

### 🏆 ASSOCIATE LEVEL COMPREHENSIVE EXAM

**Question 1 (Scenario):** A company's EC2-based web application receives unpredictable traffic spikes. During low traffic, they want to minimize costs. What combination of services provides the BEST solution?

A) Reserved EC2 instances + static count
B) Auto Scaling Group + On-Demand instances + ALB ✅
C) Spot instances + Auto Scaling + ALB
D) Lambda + API Gateway

**Question 2:** A company has a web tier (public) and database tier (private). The database must initiate outbound connections to download software patches but must NOT be accessible from the internet. What solution achieves this?

A) Attach an Internet Gateway to the private subnet
B) Deploy a NAT Gateway in the public subnet and add a route from private to NAT ✅
C) Use a VPC Endpoint
D) Use Direct Connect

**Question 3:** An RDS MySQL instance has one Multi-AZ standby. A developer wants to offload reporting queries from the primary. What should they recommend?

A) Create a second Multi-AZ deployment
B) Create a Read Replica ✅
C) Enable Multi-AZ — standby handles reads automatically
D) Use ElastiCache

**Question 4 (Scenario):** A mobile app uploads photos to S3. For each upload, the app must: resize the image, extract metadata, update a database record. What is the MOST cost-effective serverless architecture?

A) EC2 instance polling S3 for new uploads
B) S3 Event Notification → SQS → Lambda → DynamoDB ✅
C) CloudWatch Events → Lambda
D) S3 Event → SNS → EC2

**Question 5:** A company wants to route 10% of traffic to a new version of their application for testing. Which Route 53 routing policy should they use?

A) Failover routing
B) Latency-based routing
C) Weighted routing ✅
D) Geolocation routing

**Question 6:** Which EBS volume type is BEST for a mission-critical Oracle database requiring 50,000 IOPS?

A) gp3 (General Purpose SSD)
B) st1 (Throughput Optimized HDD)
C) io2 (Provisioned IOPS SSD) ✅
D) sc1 (Cold HDD)

**Question 7:** A company stores log files that are accessed frequently for the first 30 days, occasionally for the next 60 days, and then must be retained for 7 years for compliance. What is the MOST cost-effective S3 configuration?

A) Standard for all
B) Standard → Standard-IA at day 30 → Glacier Flexible at day 90 → Deep Archive at year 1 ✅
C) Intelligent-Tiering only
D) Standard-IA for all

---

## 26. Infrastructure as Code: Terraform

### Why IaC?

```
PROBLEM with manual AWS Console:
  - Not repeatable (different each time)
  - Can't version control your infrastructure
  - Can't review changes before applying
  - Prone to human error
  - Can't diff between environments

SOLUTION: Infrastructure as Code
  - Infrastructure defined in files (like software code)
  - Version controlled in Git
  - Repeatable (same code = same infrastructure)
  - Reviewable (PR process)
  - Automated (CI/CD applies changes)
```

### Terraform vs CloudFormation

```
TERRAFORM:                          CLOUDFORMATION:
  Multi-cloud (AWS, GCP, Azure)       AWS-only
  HCL language (readable)            YAML/JSON (verbose)
  State file management needed       AWS manages state
  Larger ecosystem                   Tighter AWS integration
  Industry standard for multi-cloud  Better for AWS-native shops
  
Both are excellent — know both for interviews!
```

### Terraform Core Workflow

```bash
terraform init    # Initialize: download providers, setup backend
terraform plan    # Preview changes ("what will change?")
terraform apply   # Apply changes ("make it so")
terraform destroy # Destroy all resources ("tear it down")
```

### Complete Terraform AWS Example

```hcl
# main.tf — Production VPC + EC2 + RDS setup

terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  # Remote state in S3 (team collaboration)
  backend "s3" {
    bucket         = "my-terraform-state-bucket"
    key            = "production/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"    # State locking!
  }
}

provider "aws" {
  region = var.aws_region
  
  default_tags {
    tags = {
      Project     = var.project_name
      Environment = var.environment
      ManagedBy   = "Terraform"
    }
  }
}

# ─── Variables ───
variable "aws_region"    { default = "us-east-1" }
variable "project_name"  { default = "myapp" }
variable "environment"   { default = "production" }
variable "vpc_cidr"      { default = "10.0.0.0/16" }
variable "db_password"   { sensitive = true }

# ─── VPC ───
resource "aws_vpc" "main" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = { Name = "${var.project_name}-vpc" }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  tags   = { Name = "${var.project_name}-igw" }
}

# ─── Subnets across 3 AZs ───
locals {
  azs = ["us-east-1a", "us-east-1b", "us-east-1c"]
  
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  private_subnets = ["10.0.10.0/24", "10.0.11.0/24", "10.0.12.0/24"]
  db_subnets      = ["10.0.20.0/24", "10.0.21.0/24", "10.0.22.0/24"]
}

resource "aws_subnet" "public" {
  count             = length(local.azs)
  vpc_id            = aws_vpc.main.id
  cidr_block        = local.public_subnets[count.index]
  availability_zone = local.azs[count.index]
  map_public_ip_on_launch = true
  tags = { Name = "${var.project_name}-public-${local.azs[count.index]}" }
}

resource "aws_subnet" "private" {
  count             = length(local.azs)
  vpc_id            = aws_vpc.main.id
  cidr_block        = local.private_subnets[count.index]
  availability_zone = local.azs[count.index]
  tags = { Name = "${var.project_name}-private-${local.azs[count.index]}" }
}

# ─── NAT Gateways (one per AZ for HA) ───
resource "aws_eip" "nat" {
  count  = length(local.azs)
  domain = "vpc"
}

resource "aws_nat_gateway" "main" {
  count         = length(local.azs)
  allocation_id = aws_eip.nat[count.index].id
  subnet_id     = aws_subnet.public[count.index].id
  depends_on    = [aws_internet_gateway.main]
}

# ─── Route Tables ───
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  tags = { Name = "${var.project_name}-public-rt" }
}

resource "aws_route_table_association" "public" {
  count          = length(local.azs)
  subnet_id      = aws_subnet.public[count.index].id
  route_table_id = aws_route_table.public.id
}

resource "aws_route_table" "private" {
  count  = length(local.azs)
  vpc_id = aws_vpc.main.id
  route {
    cidr_block     = "0.0.0.0/0"
    nat_gateway_id = aws_nat_gateway.main[count.index].id
  }
  tags = { Name = "${var.project_name}-private-rt-${local.azs[count.index]}" }
}

# ─── Security Groups ───
resource "aws_security_group" "alb" {
  name        = "${var.project_name}-alb-sg"
  vpc_id      = aws_vpc.main.id
  description = "ALB Security Group"
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_security_group" "app" {
  name        = "${var.project_name}-app-sg"
  vpc_id      = aws_vpc.main.id
  description = "App Server Security Group"
  
  ingress {
    from_port       = 8080
    to_port         = 8080
    protocol        = "tcp"
    security_groups = [aws_security_group.alb.id]  # Only from ALB!
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_security_group" "rds" {
  name        = "${var.project_name}-rds-sg"
  vpc_id      = aws_vpc.main.id
  description = "RDS Security Group"
  
  ingress {
    from_port       = 5432
    to_port         = 5432
    protocol        = "tcp"
    security_groups = [aws_security_group.app.id]  # Only from App!
  }
}

# ─── ALB ───
resource "aws_lb" "main" {
  name               = "${var.project_name}-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.alb.id]
  subnets            = aws_subnet.public[*].id
  
  enable_deletion_protection = true
  enable_http2               = true
}

resource "aws_lb_target_group" "app" {
  name     = "${var.project_name}-tg"
  port     = 8080
  protocol = "HTTP"
  vpc_id   = aws_vpc.main.id
  
  health_check {
    path                = "/health"
    interval            = 30
    timeout             = 5
    healthy_threshold   = 2
    unhealthy_threshold = 3
  }
}

# ─── Auto Scaling ───
resource "aws_launch_template" "app" {
  name_prefix   = "${var.project_name}-lt"
  image_id      = data.aws_ami.amazon_linux.id
  instance_type = "t3.medium"
  
  vpc_security_group_ids = [aws_security_group.app.id]
  
  iam_instance_profile {
    name = aws_iam_instance_profile.app.name
  }
  
  user_data = base64encode(<<-EOF
    #!/bin/bash
    yum update -y
    yum install -y amazon-cloudwatch-agent
    # Start your application
    systemctl start myapp
    systemctl enable myapp
  EOF
  )
  
  tag_specifications {
    resource_type = "instance"
    tags = { Name = "${var.project_name}-app" }
  }
}

resource "aws_autoscaling_group" "app" {
  name                = "${var.project_name}-asg"
  min_size            = 2
  desired_capacity    = 4
  max_size            = 20
  vpc_zone_identifier = aws_subnet.private[*].id
  target_group_arns   = [aws_lb_target_group.app.arn]
  health_check_type   = "ELB"
  health_check_grace_period = 300
  
  launch_template {
    id      = aws_launch_template.app.id
    version = "$Latest"
  }
}

# ─── RDS ───
resource "aws_db_subnet_group" "main" {
  name       = "${var.project_name}-db-subnet-group"
  subnet_ids = aws_subnet.private[*].id
}

resource "aws_db_instance" "main" {
  identifier             = "${var.project_name}-postgres"
  engine                 = "postgres"
  engine_version         = "15.4"
  instance_class         = "db.t3.medium"
  allocated_storage      = 100
  storage_type           = "gp3"
  storage_encrypted      = true
  
  db_name  = "appdb"
  username = "dbadmin"
  password = var.db_password
  
  multi_az               = true
  db_subnet_group_name   = aws_db_subnet_group.main.name
  vpc_security_group_ids = [aws_security_group.rds.id]
  
  backup_retention_period = 7
  deletion_protection     = true
  skip_final_snapshot     = false
  final_snapshot_identifier = "${var.project_name}-final-snapshot"
}

# ─── Data Sources ───
data "aws_ami" "amazon_linux" {
  most_recent = true
  owners      = ["amazon"]
  filter {
    name   = "name"
    values = ["al2023-ami-2023*-x86_64"]
  }
}

# ─── Outputs ───
output "alb_dns_name" {
  value       = aws_lb.main.dns_name
  description = "Application Load Balancer DNS name"
}

output "rds_endpoint" {
  value       = aws_db_instance.main.endpoint
  sensitive   = true
  description = "RDS instance endpoint"
}
```

---

## 27. AWS CodeBuild

### What is CodeBuild?

```
CodeBuild = Fully managed continuous integration service
  - Compiles source code
  - Runs tests
  - Produces deployable artifacts
  - No servers to manage
  - Pay per build minute
```

### buildspec.yml — The Heart of CodeBuild

```yaml
# buildspec.yml — placed in root of your repository

version: 0.2

env:
  parameter-store:                          # Pull from SSM Parameter Store
    DB_HOST: /myapp/production/db_host
  secrets-manager:                          # Pull from Secrets Manager
    DB_PASSWORD: myapp/production/db-password:password
  variables:                                # Static env vars
    ENVIRONMENT: production
    APP_NAME: myapp

phases:
  install:
    runtime-versions:
      nodejs: 20
      python: 3.11
    commands:
      - echo "Installing dependencies..."
      - npm ci --production=false
      - pip install -r requirements.txt

  pre_build:
    commands:
      - echo "Running pre-build checks..."
      # Login to ECR
      - aws ecr get-login-password --region $AWS_DEFAULT_REGION | 
          docker login --username AWS 
          --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com
      - IMAGE_TAG=$(echo $CODEBUILD_RESOLVED_SOURCE_VERSION | cut -c 1-7)
      - REPOSITORY_URI=$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$APP_NAME

  build:
    commands:
      - echo "Running tests..."
      - npm test
      - echo "Test coverage check..."
      - npm run coverage
      
      - echo "Running security scan..."
      - npm audit --audit-level=high
      
      - echo "Building Docker image..."
      - docker build -t $REPOSITORY_URI:$IMAGE_TAG .
      - docker tag $REPOSITORY_URI:$IMAGE_TAG $REPOSITORY_URI:latest

  post_build:
    commands:
      - echo "Pushing Docker image to ECR..."
      - docker push $REPOSITORY_URI:$IMAGE_TAG
      - docker push $REPOSITORY_URI:latest
      
      # Write image definitions for CodeDeploy/ECS
      - printf '[{"name":"app","imageUri":"%s"}]' 
          $REPOSITORY_URI:$IMAGE_TAG > imagedefinitions.json
      
      - echo "Build completed successfully!"

artifacts:
  files:
    - imagedefinitions.json
    - appspec.yml
    - task-definition.json
    - "**/*"
  discard-paths: no

cache:
  paths:
    - node_modules/**/*       # Cache npm packages between builds
    - .npm/**/*               # npm cache

reports:
  unit-test-reports:
    files:
      - "coverage/junit.xml"
    file-format: JUNITXML
  coverage-report:
    files:
      - "coverage/lcov.info"
    file-format: LCOV
```

```bash
# Create CodeBuild project via CLI
aws codebuild create-project \
  --name my-app-build \
  --description "Build and test my application" \
  --source type=GITHUB,location=https://github.com/myorg/myapp \
  --artifacts type=S3,location=my-artifacts-bucket,packaging=ZIP \
  --environment '{
    "type": "LINUX_CONTAINER",
    "image": "aws/codebuild/standard:7.0",
    "computeType": "BUILD_GENERAL1_MEDIUM",
    "privilegedMode": true,
    "environmentVariables": [
      {"name": "AWS_ACCOUNT_ID", "value": "123456789012"},
      {"name": "APP_NAME", "value": "myapp"}
    ]
  }' \
  --service-role arn:aws:iam::ACCOUNT:role/codebuild-role \
  --vpc-config '{
    "vpcId": "vpc-xxx",
    "subnets": ["subnet-xxx"],
    "securityGroupIds": ["sg-xxx"]
  }'

# Start a build manually
aws codebuild start-build \
  --project-name my-app-build \
  --source-version main

# View build logs
aws codebuild batch-get-builds \
  --ids my-app-build:$(aws codebuild list-builds-for-project \
    --project-name my-app-build \
    --query 'ids[0]' --output text)
```

---

## 28. AWS CodePipeline

### What is CodePipeline?

```
CodePipeline = Fully managed continuous delivery orchestrator
  - Automates your release process end-to-end
  - Connects: Source → Build → Test → Deploy
  - Every code push automatically flows through pipeline
  - Visual pipeline view in console
  - Integration with GitHub, CodeCommit, S3, Jenkins
```

### Full CI/CD Pipeline Architecture

```
Developer pushes code to GitHub
         ↓
STAGE 1: SOURCE
  CodePipeline detects GitHub push
  Downloads source code
  Stores in S3 artifact store
         ↓
STAGE 2: BUILD & TEST
  CodeBuild: run unit tests, lint, security scan
  CodeBuild: build Docker image
  CodeBuild: push image to ECR
  Fail? → Pipeline stops, notifications sent
         ↓
STAGE 3: DEPLOY TO STAGING
  CodeDeploy: Deploy new version to staging ECS cluster
  Wait for health checks
  Run integration tests against staging
         ↓
STAGE 4: MANUAL APPROVAL (optional)
  SNS notification to team lead
  Human reviews staging, clicks "Approve" or "Reject"
  ← This is the gate to production
         ↓
STAGE 5: DEPLOY TO PRODUCTION
  CodeDeploy: Blue/Green deployment to production
  CloudWatch alarms monitor for errors
  Automatic rollback if error rate spikes
```

```json
{
  "pipeline": {
    "name": "my-app-pipeline",
    "roleArn": "arn:aws:iam::ACCOUNT:role/codepipeline-role",
    "artifactStore": {
      "type": "S3",
      "location": "my-pipeline-artifacts"
    },
    "stages": [
      {
        "name": "Source",
        "actions": [{
          "name": "GitHub-Source",
          "actionTypeId": {
            "category": "Source",
            "owner": "ThirdParty",
            "provider": "GitHub",
            "version": "1"
          },
          "configuration": {
            "Owner": "myorg",
            "Repo": "myapp",
            "Branch": "main",
            "OAuthToken": "{{resolve:secretsmanager:github-token}}"
          },
          "outputArtifacts": [{"name": "SourceCode"}]
        }]
      },
      {
        "name": "Build",
        "actions": [{
          "name": "CodeBuild",
          "actionTypeId": {
            "category": "Build",
            "owner": "AWS",
            "provider": "CodeBuild",
            "version": "1"
          },
          "configuration": {
            "ProjectName": "my-app-build"
          },
          "inputArtifacts": [{"name": "SourceCode"}],
          "outputArtifacts": [{"name": "BuildOutput"}]
        }]
      },
      {
        "name": "Deploy-Staging",
        "actions": [{
          "name": "ECS-Deploy-Staging",
          "actionTypeId": {
            "category": "Deploy",
            "owner": "AWS",
            "provider": "ECS",
            "version": "1"
          },
          "configuration": {
            "ClusterName": "staging-cluster",
            "ServiceName": "my-app-staging",
            "FileName": "imagedefinitions.json"
          },
          "inputArtifacts": [{"name": "BuildOutput"}]
        }]
      },
      {
        "name": "Manual-Approval",
        "actions": [{
          "name": "Approve-Production",
          "actionTypeId": {
            "category": "Approval",
            "owner": "AWS",
            "provider": "Manual",
            "version": "1"
          },
          "configuration": {
            "NotificationArn": "arn:aws:sns:us-east-1:ACCOUNT:deploy-approvals",
            "CustomData": "Review staging: https://staging.myapp.com"
          }
        }]
      },
      {
        "name": "Deploy-Production",
        "actions": [{
          "name": "ECS-Deploy-Production",
          "actionTypeId": {
            "category": "Deploy",
            "owner": "AWS",
            "provider": "CodeDeployToECS",
            "version": "1"
          },
          "configuration": {
            "ApplicationName": "myapp-ecs",
            "DeploymentGroupName": "production",
            "TaskDefinitionTemplateArtifact": "BuildOutput",
            "AppSpecTemplateArtifact": "BuildOutput"
          },
          "inputArtifacts": [{"name": "BuildOutput"}]
        }]
      }
    ]
  }
}
```

---

## 29. AWS CodeDeploy

### Deployment Strategies

```
IN-PLACE (Rolling):
  - Stop half servers, deploy, test, continue
  - Downtime possible
  - Good for: Dev/Test environments
  
BLUE/GREEN:
  - Create entirely NEW environment ("green")
  - Test new environment while old ("blue") serves traffic
  - Switch traffic to green (instant cutover at LB)
  - Keep blue for quick rollback
  - NO downtime
  - Best practice for production

LINEAR:
  - Route X% of traffic every N minutes to new version
  - Example: 10% more every 10 minutes until 100%
  - Slow rollout with automatic rollback on errors

CANARY:
  - Route small % (e.g., 10%) to new version
  - Monitor for X minutes
  - Route rest (90%) if healthy
  - Perfect for risk-averse production deployments
```

```yaml
# appspec.yml — CodeDeploy configuration for ECS Blue/Green
version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: <TASK_DEFINITION>
        LoadBalancerInfo:
          ContainerName: app
          ContainerPort: 8080
        PlatformVersion: LATEST

Hooks:
  - BeforeInstall: "arn:aws:lambda:us-east-1:ACCOUNT:function:validate-pre-deploy"
  - AfterInstall: "arn:aws:lambda:us-east-1:ACCOUNT:function:run-smoke-tests"
  - AfterAllowTestTraffic: "arn:aws:lambda:us-east-1:ACCOUNT:function:integration-tests"
  - BeforeAllowTraffic: "arn:aws:lambda:us-east-1:ACCOUNT:function:final-validation"
  - AfterAllowTraffic: "arn:aws:lambda:us-east-1:ACCOUNT:function:monitor-deployment"
```

---

## 30. ECR — Elastic Container Registry

### What is ECR?

```
ECR = Private Docker container registry on AWS
  - Store, manage, deploy Docker container images
  - Integrated with ECS, EKS, Lambda
  - Image scanning (security vulnerability detection)
  - Lifecycle policies (auto-delete old images)
  - Cross-account and cross-region replication
```

```bash
# Create ECR repository
aws ecr create-repository \
  --repository-name myapp/api \
  --image-scanning-configuration scanOnPush=true \
  --encryption-configuration encryptionType=AES256 \
  --tags Key=Project,Value=myapp

# Authenticate Docker to ECR
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS \
  --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

# Build and push image
docker build -t myapp/api:1.2.3 .
docker tag myapp/api:1.2.3 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp/api:1.2.3
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp/api:1.2.3

# Add lifecycle policy (keep only 10 latest images)
aws ecr put-lifecycle-policy \
  --repository-name myapp/api \
  --lifecycle-policy '{
    "rules": [{
      "rulePriority": 1,
      "description": "Keep only 10 images",
      "selection": {
        "tagStatus": "any",
        "countType": "imageCountMoreThan",
        "countNumber": 10
      },
      "action": {"type": "expire"}
    }]
  }'

# Scan image for vulnerabilities
aws ecr start-image-scan \
  --repository-name myapp/api \
  --image-id imageTag=1.2.3

# Get scan findings
aws ecr describe-image-scan-findings \
  --repository-name myapp/api \
  --image-id imageTag=1.2.3 \
  --query 'imageScanFindings.findings[?severity==`CRITICAL`]'
```

---

## 31. ECS — Elastic Container Service

### ECS Core Concepts

```
CLUSTER:
  Logical grouping of tasks/services and infrastructure
  Can run on EC2 instances OR Fargate

TASK DEFINITION:
  Blueprint for your container(s)
  Defines: Docker image, CPU, memory, ports, env vars, volumes, IAM role

TASK:
  A running instance of a task definition
  Can be standalone (run once) or part of a service

SERVICE:
  Long-running tasks with desired count
  Integrated with ALB for traffic distribution
  Maintains desired number of healthy tasks
  
LAUNCH TYPES:
  EC2:     You manage the EC2 instances in the cluster
  Fargate: Serverless — AWS manages the infrastructure
           You define CPU/memory, pay per task-second
```

```json
// Task Definition example (JSON)
{
  "family": "myapp-api",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "512",
  "memory": "1024",
  "executionRoleArn": "arn:aws:iam::ACCOUNT:role/ecsTaskExecutionRole",
  "taskRoleArn": "arn:aws:iam::ACCOUNT:role/ecsTaskRole",
  
  "containerDefinitions": [
    {
      "name": "app",
      "image": "123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp/api:1.2.3",
      "portMappings": [
        {"containerPort": 8080, "protocol": "tcp"}
      ],
      "environment": [
        {"name": "ENVIRONMENT", "value": "production"},
        {"name": "PORT", "value": "8080"}
      ],
      "secrets": [
        {
          "name": "DB_PASSWORD",
          "valueFrom": "arn:aws:secretsmanager:us-east-1:ACCOUNT:secret:myapp/db-password"
        },
        {
          "name": "API_KEY",
          "valueFrom": "/myapp/production/api-key"
        }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/myapp-api",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "ecs"
        }
      },
      "healthCheck": {
        "command": ["CMD-SHELL", "curl -f http://localhost:8080/health || exit 1"],
        "interval": 30,
        "timeout": 5,
        "retries": 3,
        "startPeriod": 60
      },
      "cpu": 512,
      "memory": 1024,
      "essential": true
    }
  ]
}
```

```bash
# Register task definition
aws ecs register-task-definition \
  --cli-input-json file://task-definition.json

# Create ECS Service
aws ecs create-service \
  --cluster production \
  --service-name myapp-api \
  --task-definition myapp-api:5 \
  --desired-count 4 \
  --launch-type FARGATE \
  --network-configuration '{
    "awsvpcConfiguration": {
      "subnets": ["subnet-private-1a", "subnet-private-1b"],
      "securityGroups": ["sg-app"],
      "assignPublicIp": "DISABLED"
    }
  }' \
  --load-balancers '[{
    "targetGroupArn": "arn:aws:elasticloadbalancing:...:targetgroup/myapp",
    "containerName": "app",
    "containerPort": 8080
  }]' \
  --deployment-configuration '{
    "deploymentCircuitBreaker": {
      "enable": true,
      "rollback": true
    },
    "maximumPercent": 200,
    "minimumHealthyPercent": 100
  }'

# Update service with new image (rolling update)
aws ecs update-service \
  --cluster production \
  --service myapp-api \
  --task-definition myapp-api:6   # New task def version

# Force new deployment (same task def, useful for pulling latest)
aws ecs update-service \
  --cluster production \
  --service myapp-api \
  --force-new-deployment
```

---

## 32. EKS — Kubernetes on AWS

### EKS Overview

```
EKS = Managed Kubernetes control plane on AWS

AWS manages:
  - Kubernetes API servers
  - etcd (cluster state database)
  - Control plane HA across multiple AZs
  - Control plane upgrades

You manage:
  - Worker nodes (EC2 or Fargate)
  - Node groups
  - Your applications (pods, services, deployments)
  - Kubernetes version upgrades for nodes
```

### EKS Architecture

```
EKS CLUSTER
├── Control Plane (AWS Managed, Multi-AZ)
│   ├── kube-apiserver
│   ├── etcd
│   ├── kube-scheduler
│   └── kube-controller-manager
│
└── Data Plane (Your worker nodes)
    ├── Managed Node Group (EC2, AWS manages patching)
    ├── Self-Managed Nodes (more control)
    └── Fargate Profile (serverless, no nodes)
```

```bash
# Install eksctl (EKS CLI)
curl --location "https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin/

# Create EKS cluster
eksctl create cluster \
  --name production-cluster \
  --version 1.28 \
  --region us-east-1 \
  --nodegroup-name app-nodes \
  --node-type m5.large \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 10 \
  --managed \
  --asg-access \
  --full-ecr-access \
  --alb-ingress-access \
  --vpc-private-subnets subnet-1a,subnet-1b,subnet-1c \
  --vpc-public-subnets subnet-pub-1a,subnet-pub-1b,subnet-pub-1c

# Configure kubectl
aws eks update-kubeconfig --name production-cluster --region us-east-1

# Deploy application
kubectl apply -f - << 'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
  namespace: production
spec:
  replicas: 4
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: app
        image: 123456789012.dkr.ecr.us-east-1.amazonaws.com/myapp/api:1.2.3
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        env:
        - name: ENVIRONMENT
          value: production
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: db-password
        readinessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 10
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 60
          periodSeconds: 30
EOF

# Install Cluster Autoscaler
kubectl apply -f https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml

# Install AWS Load Balancer Controller (for ALB ingress)
helm repo add eks https://aws.github.io/eks-charts
helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=production-cluster \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```

---

## 33. Secrets Manager & SSM

### AWS Secrets Manager

```
For storing: Database passwords, API keys, OAuth tokens
Features:
  - Automatic secret rotation
  - Version control of secrets
  - Fine-grained IAM access control
  - Audit trail via CloudTrail
  - Cross-account access
```

```bash
# Create a secret
aws secretsmanager create-secret \
  --name myapp/production/database \
  --description "Production database credentials" \
  --secret-string '{
    "username": "dbadmin",
    "password": "SuperSecret123!",
    "host": "prod-db.cluster-xxx.us-east-1.rds.amazonaws.com",
    "port": "5432",
    "dbname": "appdb"
  }' \
  --tags Key=Environment,Value=production

# Get secret value
aws secretsmanager get-secret-value \
  --secret-id myapp/production/database \
  --query SecretString --output text | python3 -m json.tool

# In your application (Python):
import boto3, json

def get_db_credentials():
    client = boto3.client('secretsmanager', region_name='us-east-1')
    response = client.get_secret_value(SecretId='myapp/production/database')
    return json.loads(response['SecretString'])

# Enable automatic rotation (for RDS)
aws secretsmanager rotate-secret \
  --secret-id myapp/production/database \
  --rotation-lambda-arn arn:aws:lambda:us-east-1:ACCOUNT:function:SecretsManagerRotation \
  --rotation-rules AutomaticallyAfterDays=30
```

### AWS Systems Manager (SSM)

```
SSM is a suite of tools for managing EC2 instances at scale:

PARAMETER STORE:
  Store configuration data and non-sensitive secrets
  FREE (Standard Tier) or paid (Advanced)
  Use for: Config values, feature flags, connection strings
  
SESSION MANAGER:
  SSH to EC2 without open port 22!
  No bastion host needed
  Full audit trail in CloudTrail
  
RUN COMMAND:
  Run commands on fleets of EC2 instances
  No SSH needed
  
PATCH MANAGER:
  Automate OS patching across your fleet
  Compliance reporting
  
INVENTORY:
  Collect software/hardware inventory from instances
  
STATE MANAGER:
  Ensure instances stay in desired state (configuration drift prevention)
```

```bash
# SSM Parameter Store
# Create parameter
aws ssm put-parameter \
  --name "/myapp/production/database_host" \
  --value "prod-db.cluster.us-east-1.rds.amazonaws.com" \
  --type String

aws ssm put-parameter \
  --name "/myapp/production/api_key" \
  --value "sk-1234567890abcdef" \
  --type SecureString \
  --key-id alias/aws/ssm    # Encrypted with KMS

# Get parameter
aws ssm get-parameter \
  --name "/myapp/production/database_host" \
  --query Parameter.Value \
  --output text

aws ssm get-parameter \
  --name "/myapp/production/api_key" \
  --with-decryption \
  --query Parameter.Value \
  --output text

# Get all parameters by path
aws ssm get-parameters-by-path \
  --path "/myapp/production/" \
  --recursive \
  --with-decryption

# SSM Session Manager — SSH without port 22
aws ssm start-session --target i-1234567890abcdef0

# Run command on multiple instances
aws ssm send-command \
  --targets Key=tag:Environment,Values=production \
  --document-name "AWS-RunShellScript" \
  --parameters commands="systemctl status myapp && journalctl -u myapp --since '5 minutes ago' -n 50" \
  --comment "Health check all production instances"
```

---

## 34. Logging & Monitoring Architecture

### Production Observability Stack

```
THREE PILLARS OF OBSERVABILITY:

1. METRICS (What is happening?)
   - CloudWatch Metrics (AWS services)
   - Custom metrics via CloudWatch Agent
   - Container metrics (Container Insights)
   - Application metrics (custom namespace)

2. LOGS (Why is it happening?)
   - CloudWatch Logs (centralized log aggregation)
   - Log Insights (query and analyze)
   - Application logs → CloudWatch Agent
   - VPC Flow Logs → S3 / CloudWatch
   - CloudTrail → API audit logs → S3

3. TRACES (Where is it happening?)
   - AWS X-Ray (distributed tracing)
   - See request flow across microservices
   - Identify bottlenecks, errors, latencies
```

### CloudTrail — Who Did What?

```bash
# CloudTrail logs EVERY AWS API call
# Who: Which IAM user/role
# What: Which API action (e.g., ec2:TerminateInstances)
# When: Timestamp
# Where: Source IP, Region
# What succeeded/failed

# Enable CloudTrail (should be on by default in new accounts)
aws cloudtrail create-trail \
  --name production-audit-trail \
  --s3-bucket-name my-audit-logs \
  --is-multi-region-trail \
  --include-global-service-events \
  --enable-log-file-validation    # Detect tampering!

aws cloudtrail start-logging --name production-audit-trail

# Search CloudTrail events
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=DeleteBucket \
  --start-time 2024-01-01T00:00:00Z

# Who terminated EC2 instances in last 24 hours?
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=EventName,AttributeValue=TerminateInstances \
  --max-results 20
```

### AWS X-Ray Distributed Tracing

```python
# Add X-Ray to your Python application
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.core import patch_all

# Patch AWS SDK calls (boto3, etc.)
patch_all()

xray_recorder.configure(service='my-api')

@app.route('/api/orders')
def get_orders():
    with xray_recorder.in_segment('GetOrders'):
        with xray_recorder.in_subsegment('database-query'):
            orders = db.query("SELECT * FROM orders WHERE user_id = ?", user_id)
        
        with xray_recorder.in_subsegment('enrich-data'):
            for order in orders:
                order['items'] = get_order_items(order['id'])
        
        return jsonify(orders)
```

### Centralized Logging Architecture

```
All EC2 instances
     ↓ CloudWatch Agent
CloudWatch Logs Groups:
  /myapp/production/application
  /myapp/production/nginx-access
  /myapp/production/nginx-error
  /aws/lambda/my-function
  /aws/ecs/my-cluster
     ↓ Subscription Filter (real-time)
Lambda Function → Elasticsearch/OpenSearch
     OR
     ↓ Export Task
S3 (long-term storage, cheap)
     ↓ Athena
SQL queries on log data

REAL-TIME ALERTING:
CloudWatch Logs → Metric Filter → CloudWatch Alarm → SNS → PagerDuty
Example: Count "CRITICAL" in logs > 0 → page on-call engineer
```

---

# ═══════════════════════════════════════
# PART 4: ADVANCED ARCHITECTURE — PROFESSIONAL
# ═══════════════════════════════════════

---

## 35. Highly Available Architecture

### The Well-Architected Framework (5 Pillars)

```
1. OPERATIONAL EXCELLENCE
   Automate everything, make frequent small changes
   Prepare for and learn from failures
   Tools: Systems Manager, CodePipeline, CloudFormation

2. SECURITY
   Implement security at all layers
   Least privilege, encryption everywhere
   Tools: IAM, KMS, WAF, GuardDuty, Security Hub

3. RELIABILITY
   Recover from failures automatically
   Scale horizontally, test recovery procedures
   Tools: Multi-AZ RDS, Auto Scaling, Route53 failover

4. PERFORMANCE EFFICIENCY
   Use the right resource types and sizes
   Experiment with new technologies
   Tools: CloudFront, ElastiCache, Auto Scaling

5. COST OPTIMIZATION
   Adopt a consumption model
   Measure efficiency, stop spending on undifferentiated work
   Tools: Cost Explorer, Trusted Advisor, Savings Plans
```

### Production HA Architecture

```
INTERNET
    │
    ↓ Port 443 (HTTPS)
Route 53 (DNS with health checks, failover)
    │
    ↓ (weighted routing to multiple regions)
CloudFront (CDN, WAF, SSL termination)
    │ Cache miss
    ↓
Application Load Balancer (Multi-AZ)
    │
    ├── Target Group (Auto Scaling Group)
    │       ├── EC2: us-east-1a (app servers)
    │       ├── EC2: us-east-1b (app servers)
    │       └── EC2: us-east-1c (app servers)
    │
    ├── ElastiCache (Redis, Multi-AZ)
    │   └── Session store, caching
    │
    └── RDS Aurora (Multi-AZ, Read Replicas)
            ├── Primary Writer (us-east-1a)
            ├── Read Replica 1 (us-east-1b)
            └── Read Replica 2 (us-east-1c)

ALL LAYERS:
  ✓ Multiple AZs
  ✓ Auto Scaling
  ✓ Health checks + automatic failover
  ✓ CloudWatch monitoring + alerting
  ✓ CloudTrail auditing
```

---

## 36. Multi-Region Architecture

### Why Multi-Region?

```
REASONS TO GO MULTI-REGION:
1. Disaster Recovery (entire region failure — rare but happens)
2. Compliance (data sovereignty requirements)
3. Latency (serve users closer to their location)
4. Business continuity requirements
5. Regulatory requirements

COST: 2-3x cost of single region
      Only do it if you actually need it

COMPLEXITY: DNS routing, data replication, consistency challenges
```

### Multi-Region Active-Active vs Active-Passive

```
ACTIVE-PASSIVE:
  Primary: us-east-1 (all traffic)
  Secondary: us-west-2 (warm standby, no traffic unless failover)
  Route 53: Failover routing (health check on primary)
  RDS: Cross-region read replica (promote on failover)
  S3: Cross-region replication
  
  RPO: Minutes (replication lag)
  RTO: Minutes (promotion time)
  Cost: 50% extra (standby runs but doesn't serve traffic)

ACTIVE-ACTIVE:
  Both regions serve traffic simultaneously
  Route 53: Latency-based or geolocation routing
  DynamoDB Global Tables (built for this!)
  Aurora Global Database
  S3 Cross-Region Replication (bidirectional)
  
  RPO: Near-zero (sub-second replication)
  RTO: Near-zero (instant failover)
  Cost: 100% extra (both regions fully active)
  Complexity: Conflict resolution, global consistency
```

---

## 37. Disaster Recovery Strategies

### The 4 DR Strategies (Exam Critical!)

```
1. BACKUP & RESTORE (Cheapest, Slowest)
   RTO: Hours          RPO: Hours
   Cost: $             Just backups stored in S3/Glacier
   How: Restore from backup when disaster occurs
   Good for: Non-critical systems

2. PILOT LIGHT (Low cost, Moderate speed)
   RTO: ~10 minutes    RPO: Minutes
   Cost: $$            Core infrastructure always running (DB)
   How: Keep DB replica running in DR region
        Scale out compute on failover
   Good for: Important systems, moderate DR budget

3. WARM STANDBY (Moderate cost, Fast)
   RTO: Minutes        RPO: Seconds
   Cost: $$$           Scaled-down full system always running
   How: Full system in DR but smaller scale
        Scale up on failover
   Good for: Business-critical systems

4. MULTI-SITE ACTIVE-ACTIVE (Expensive, Fastest)
   RTO: Zero           RPO: Zero
   Cost: $$$$          Full system in multiple regions, both active
   How: Traffic goes to both regions always
        Remove failed region from Route 53
   Good for: Mission-critical (banking, healthcare)
```

### DR Key Metrics

```
RPO (Recovery Point Objective):
  Maximum acceptable data LOSS
  "We can lose up to 1 hour of data"
  Lower RPO = more frequent backups/replication = more cost

RTO (Recovery Time Objective):
  Maximum acceptable DOWNTIME
  "We must be back online within 30 minutes"
  Lower RTO = more pre-provisioned resources = more cost
```

---

## 38. Hybrid Cloud Architecture

### AWS Direct Connect

```
Direct Connect = Dedicated private network connection between 
                 your data center and AWS
                 
Benefits vs VPN:
  - Consistent network performance (not internet-based)
  - Up to 100 Gbps bandwidth
  - Lower latency
  - More reliable
  - Better for large data transfers
  
Use cases:
  - Migrating large datasets to AWS
  - Hybrid applications (on-prem + cloud)
  - Compliance (data shouldn't traverse public internet)
  
Cost: Significant (dedicated circuit + port hour charges)
Alternative: Site-to-Site VPN (cheaper but internet-based)
```

```bash
# Create Site-to-Site VPN (quicker to set up than Direct Connect)
# Create Virtual Private Gateway
VGW_ID=$(aws ec2 create-vpn-gateway \
  --type ipsec.1 \
  --query VpnGateway.VpnGatewayId \
  --output text)

aws ec2 attach-vpn-gateway \
  --vpn-gateway-id $VGW_ID \
  --vpc-id $VPC_ID

# Create Customer Gateway (your on-prem router)
CGW_ID=$(aws ec2 create-customer-gateway \
  --type ipsec.1 \
  --public-ip YOUR_ON_PREM_ROUTER_IP \
  --bgp-asn 65000 \
  --query CustomerGateway.CustomerGatewayId \
  --output text)

# Create VPN Connection
aws ec2 create-vpn-connection \
  --type ipsec.1 \
  --customer-gateway-id $CGW_ID \
  --vpn-gateway-id $VGW_ID \
  --options StaticRoutesOnly=false
```

### AWS Storage Gateway

```
Storage Gateway = Hybrid storage (connect on-prem to AWS storage)

Types:
  S3 File Gateway:    Map NFS/SMB shares to S3 buckets
  Volume Gateway:     iSCSI block storage backed by S3 + EBS
  Tape Gateway:       Virtual tape library backed by Glacier
  
Use case: Gradually move backup/archive workloads to AWS
          Keep frequently accessed data on-prem (cached)
          Move older data to S3/Glacier automatically
```

---

## 39. VPC Peering & Transit Gateway

### VPC Peering

```
Connects TWO VPCs (same or different account/region)
Private traffic flows between them

Limitations:
  - 1:1 only (not transitive!)
  - VPC A peered with B, B peered with C
    → A CANNOT reach C via B!
  
  - CIDR blocks must not overlap
  - Max 125 peering connections per VPC

Good for: Small number of VPCs that need to connect
```

```bash
# Create VPC peering
PEER_ID=$(aws ec2 create-vpc-peering-connection \
  --vpc-id vpc-requester \
  --peer-vpc-id vpc-accepter \
  --peer-region us-west-2 \       # Cross-region
  --peer-owner-id 987654321098 \  # Cross-account
  --query VpcPeeringConnection.VpcPeeringConnectionId \
  --output text)

# Accept the peering (from accepter account)
aws ec2 accept-vpc-peering-connection \
  --vpc-peering-connection-id $PEER_ID

# Update route tables in BOTH VPCs
aws ec2 create-route \
  --route-table-id rtb-requester \
  --destination-cidr-block 10.1.0.0/16 \
  --vpc-peering-connection-id $PEER_ID
```

### Transit Gateway

```
Transit Gateway = Hub-and-spoke network for multiple VPCs

Benefits over VPC Peering:
  ✓ Connects 1000s of VPCs through one gateway (hub)
  ✓ Transitive routing (A → TGW → B → TGW → C works!)
  ✓ Connects on-premises (via VPN or Direct Connect)
  ✓ Cross-account, cross-region (TGW peering)
  ✓ One route table to manage
  
Use cases:
  - Large enterprise with many VPCs
  - Hub-and-spoke network architecture
  - Replacing VPN concentrators
  - Centralized security inspection (firewall VPC)
```

```bash
# Create Transit Gateway
TGW_ID=$(aws ec2 create-transit-gateway \
  --description "Company Hub Transit Gateway" \
  --options '{
    "DefaultRouteTableAssociation": "enable",
    "DefaultRouteTablePropagation": "enable",
    "AutoAcceptSharedAttachments": "disable",
    "DnsSupport": "enable"
  }' \
  --query TransitGateway.TransitGatewayId \
  --output text)

# Attach VPCs to Transit Gateway
aws ec2 create-transit-gateway-vpc-attachment \
  --transit-gateway-id $TGW_ID \
  --vpc-id vpc-production \
  --subnet-ids subnet-1a subnet-1b subnet-1c

# Share Transit Gateway with other accounts (RAM)
aws ram create-resource-share \
  --name tgw-share \
  --resource-arns arn:aws:ec2:us-east-1:ACCOUNT:transit-gateway/$TGW_ID \
  --principals 987654321098   # Account to share with
```

---

## 40. AWS PrivateLink

```
PrivateLink = Expose your service privately to other VPCs/accounts
              WITHOUT VPC peering, VPN, or internet exposure

Architecture:
  Service Provider:
    Application (behind NLB)
    → Create Endpoint Service from NLB
    → Share service with specific AWS accounts

  Service Consumer:
    Create Interface VPC Endpoint
    → Private IP in their VPC
    → Traffic NEVER leaves AWS network
    
Use cases:
  - SaaS provider exposing APIs to customers
  - Centralized shared services (Active Directory, monitoring)
  - AWS services accessed privately (S3, SQS, etc. via Gateway Endpoints)
```

---

## 41. Performance Optimization

### ElastiCache (In-Memory Caching)

```
ElastiCache = Managed Redis or Memcached
Purpose: Cache frequently accessed data in memory
Result: Microsecond response times vs milliseconds for database

Redis vs Memcached:
  Redis:      Rich data types, persistence, replication, pub/sub
              Multi-AZ with automatic failover
              Backup and restore
  Memcached:  Simple key-value, multi-threading, horizontal scaling
              No persistence, no replication
              
Use Redis unless you specifically need Memcached's thread model.

Common caching patterns:
  Cache-Aside:   App checks cache → miss → load from DB → cache it
  Write-Through: Write to cache AND database simultaneously
  Write-Behind:  Write to cache, async write to DB (risk of loss)
  Read-Through:  Cache handles DB reads automatically
```

```bash
# Create Redis cluster (ElastiCache)
aws elasticache create-replication-group \
  --replication-group-id prod-redis \
  --description "Production Redis cluster" \
  --node-type cache.r6g.large \
  --num-cache-clusters 3 \
  --cache-parameter-group-name default.redis7 \
  --engine redis \
  --engine-version 7.0 \
  --automatic-failover-enabled \
  --multi-az-enabled \
  --at-rest-encryption-enabled \
  --transit-encryption-enabled \
  --cache-subnet-group-name my-redis-subnet-group
```

### Database Performance

```
READ REPLICAS: Scale read traffic
  - Use for reporting queries (don't hit primary)
  - Set read_preference in app to use replicas

CACHING LAYER: Cache query results
  - ElastiCache in front of RDS
  - Dramatically reduce DB load

CONNECTION POOLING: RDS Proxy
  - Lambda → RDS Proxy → RDS (handles connection limits!)
  - Lambda can create thousands of connections
  - RDS Proxy pools and reuses connections
  - Reduces DB load, improves performance

INDEXES: Most impactful for query performance
  - Analyze slow query log
  - Add indexes for frequent WHERE clauses
  - Composite indexes for multi-column queries
```

---

## 42. Cost Optimization

### AWS Cost Optimization Strategies

```
1. RIGHT SIZING
   Identify over-provisioned resources
   Tool: AWS Compute Optimizer → recommends right instance type
   Action: Downsize if CPU < 20% average

2. PURCHASE OPTIMIZATION
   Reserved Instances for stable workloads (up to 75% savings)
   Savings Plans for flexible compute commitments
   Spot Instances for fault-tolerant batch jobs (up to 90% savings)

3. STORAGE TIERING
   S3 Lifecycle Policies → auto-move to Glacier
   EBS volume type right-sizing
   Delete unused EBS volumes, snapshots

4. TURN OFF WHAT YOU DON'T NEED
   Stop dev/test instances nights and weekends
   Instance Scheduler (AWS solution)
   Delete unattached Elastic IPs (charged when not associated)
   Remove old load balancers

5. MONITORING AND ALERTS
   AWS Cost Explorer → visualize spending
   AWS Budgets → alert at $X threshold
   Trusted Advisor → free recommendations
   Cost Anomaly Detection → ML-based unusual spend alerts
```

```bash
# Cost Explorer CLI
aws ce get-cost-and-usage \
  --time-period Start=2024-01-01,End=2024-01-31 \
  --granularity MONTHLY \
  --metrics BlendedCost \
  --group-by Type=SERVICE,Key=SERVICE \
  --query 'ResultsByTime[0].Groups[*].[Keys[0],Metrics.BlendedCost.Amount]' \
  --output table | sort -k2 -rn | head -20

# Find unused EBS volumes
aws ec2 describe-volumes \
  --filters Name=status,Values=available \
  --query 'Volumes[*].[VolumeId,Size,CreateTime]' \
  --output table

# Find unassociated Elastic IPs
aws ec2 describe-addresses \
  --query 'Addresses[?AssociationId==null].[PublicIp,AllocationId]' \
  --output table
```

---

# ═══════════════════════════════════════
# PART 5: EXPERT / SPECIALTY LEVEL
# ═══════════════════════════════════════

---

## 43. Advanced Networking

### AWS Network Architecture Patterns

```
INTERNET EDGE ARCHITECTURE:
Internet → Route53 → CloudFront (WAF, Shield) → ALB → App

VPN CONNECTIVITY:
On-Premises → Site-to-Site VPN → VGW → VPC

PRIVATE CONNECTIVITY:
On-Premises → Direct Connect → DX Location → VGW → VPC
              (Dedicated 1-100Gbps circuit)

HUB AND SPOKE:
Multiple VPCs → Transit Gateway ← On-Premises (VPN/DX)

INSPECTION ARCHITECTURE:
Traffic → TGW → Inspection VPC (Network Firewall) → TGW → Destination VPC
         (All traffic inspected before reaching destination)
```

### AWS Network Firewall

```bash
# Create Network Firewall (stateful, managed)
aws network-firewall create-firewall \
  --firewall-name production-firewall \
  --firewall-policy-arn arn:aws:network-firewall:us-east-1:ACCOUNT:firewall-policy/strict-policy \
  --vpc-id $VPC_ID \
  --subnet-mappings SubnetId=subnet-firewall-1a SubnetId=subnet-firewall-1b

# Create stateful rule group
aws network-firewall create-rule-group \
  --rule-group-name block-bad-domains \
  --type STATEFUL \
  --capacity 100 \
  --rule-group '{
    "RulesSource": {
      "StatefulRules": [
        {
          "Action": "DROP",
          "Header": {
            "Protocol": "TCP",
            "Source": "ANY",
            "Destination": "ANY",
            "DestinationPort": "ANY",
            "SourcePort": "ANY",
            "Direction": "ANY"
          },
          "RuleOptions": [{
            "Keyword": "dns.query",
            "Settings": ["malware.example.com"]
          }]
        }
      ]
    }
  }'
```

### BGP and Direct Connect Deep Dive

```
BGP (Border Gateway Protocol):
  Used in: Direct Connect, Transit Gateway, VPN connections
  Purpose: Advertise routes between networks
  
  Direct Connect + BGP:
    Your router BGP AS: 65001 (your ASN)
    AWS BGP AS: 64512 (private) or Amazon public ASNs
    
    Your router advertises: 192.168.0.0/16 (your on-prem CIDR)
    AWS advertises: 10.0.0.0/16 (your VPC CIDRs)
    
    Traffic flows based on BGP routing table

Link Aggregation Groups (LAGs):
  Bundle multiple Direct Connect connections for:
  - Higher bandwidth (multiple × 10Gbps)
  - Redundancy (if one link fails, others carry traffic)
```

---

## 44. Security Architecture & Compliance

### AWS Security Services Ecosystem

```
THREAT DETECTION:
  GuardDuty      → ML-based threat detection (analyzes CloudTrail, DNS, Flow Logs)
  Macie          → Discover and protect sensitive data in S3 (PII, credentials)
  Inspector      → Automated vulnerability assessment for EC2, Lambda, ECR

IDENTITY & ACCESS:
  IAM            → Users, roles, policies
  AWS SSO/IAC    → Single sign-on for multiple accounts
  Cognito        → User pools for your applications (OAuth2/OIDC)
  IAM Identity Center → Centralized SSO for AWS Organizations

INFRASTRUCTURE PROTECTION:
  WAF            → Web Application Firewall (SQLi, XSS, etc.)
  Shield         → DDoS protection (Standard=free, Advanced=paid)
  Network Firewall → Stateful managed firewall for VPCs
  Security Groups, NACLs → Layer 3/4 controls

DATA PROTECTION:
  KMS            → Managed encryption keys
  CloudHSM       → Hardware Security Module (your own HSM)
  ACM            → Free SSL/TLS certificates
  Secrets Manager → Secret rotation and storage

COMPLIANCE & GOVERNANCE:
  Config         → Configuration change tracking and compliance rules
  Security Hub   → Unified security dashboard, aggregates all findings
  Audit Manager  → Continuous compliance evidence collection
  Artifact       → AWS compliance reports (SOC, ISO, PCI)
  Organizations  → Multi-account governance with SCPs
```

### GuardDuty — Threat Detection

```bash
# Enable GuardDuty
aws guardduty create-detector \
  --enable \
  --data-sources S3Logs={Enable=true},Kubernetes={AuditLogs={Enable=true}} \
  --finding-publishing-frequency FIFTEEN_MINUTES

# List findings (security threats detected)
DETECTOR_ID=$(aws guardduty list-detectors --query DetectorIds[0] --output text)

aws guardduty list-findings \
  --detector-id $DETECTOR_ID \
  --finding-criteria '{
    "Criterion": {
      "severity": {"Gte": 7}
    }
  }'

# Common GuardDuty findings:
# UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B (login from unusual country)
# CryptoCurrency:EC2/BitcoinTool.B!DNS (EC2 mining crypto!)
# Recon:EC2/PortProbeUnprotectedPort (port scanning)
# Trojan:EC2/BlackholeTraffic (EC2 infected, calling C2 server)
```

### AWS Config — Compliance Rules

```bash
# Enable AWS Config
aws configservice put-configuration-recorder \
  --configuration-recorder name=default,roleARN=arn:aws:iam::ACCOUNT:role/config-role \
  --recording-group allSupported=true,includeGlobalResourceTypes=true

aws configservice put-delivery-channel \
  --delivery-channel name=default,s3BucketName=my-config-bucket

aws configservice start-configuration-recorder --configuration-recorder-name default

# Create compliance rule (no unencrypted S3 buckets)
aws configservice put-config-rule \
  --config-rule '{
    "ConfigRuleName": "s3-bucket-server-side-encryption-enabled",
    "Source": {
      "Owner": "AWS",
      "SourceIdentifier": "S3_BUCKET_SERVER_SIDE_ENCRYPTION_ENABLED"
    }
  }'

# Check compliance
aws configservice get-compliance-summary-by-resource-type \
  --query 'ComplianceSummariesByResourceType[*].[ResourceType,ComplianceSummary]'
```

---

## 45. IAM Policy Deep Dive

### Advanced Policy Conditions

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "aws:RequestedRegion": "us-east-1"     // Only in this region
        },
        "Bool": {
          "aws:SecureTransport": "true"          // HTTPS only!
        },
        "IpAddress": {
          "aws:SourceIp": "10.0.0.0/8"          // Only from corporate network
        },
        "DateGreaterThan": {
          "aws:CurrentTime": "2024-01-01T00:00:00Z"
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "BoolIfExists": {
          "aws:MultiFactorAuthPresent": "false"  // Require MFA for all actions
        }
      }
    }
  ]
}
```

### Service Control Policies (SCPs) — AWS Organizations

```json
// SCP applied to all accounts in org unit
// Even administrator accounts can't bypass SCPs!
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": [
        "ec2:*",
        "rds:*"
      ],
      "Resource": "*",
      "Condition": {
        "StringNotEquals": {
          "aws:RequestedRegion": [
            "us-east-1",
            "us-west-2"
          ]
        }
      }
    },
    {
      "Effect": "Deny",
      "Action": [
        "organizations:LeaveOrganization",
        "cloudtrail:DeleteTrail",
        "cloudtrail:StopLogging",
        "config:DeleteConfigurationRecorder"
      ],
      "Resource": "*"
    }
  ]
}
```

### Permission Boundaries

```json
// Permission Boundary: maximum permissions a role can ever have
// Even if you grant AdministratorAccess, PB limits actual permissions
// Use case: Let developers create IAM roles, but limit scope

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:*",
        "dynamodb:*",
        "lambda:*"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "Action": [
        "iam:*",
        "organizations:*",
        "account:*"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 46. Encryption & KMS

### AWS KMS (Key Management Service)

```
KMS manages encryption keys for AWS services and your applications

Key types:
  AWS Managed Keys:   AWS creates and manages (free, you can't control rotation)
                      Used by default in S3, EBS, RDS
                      Format: aws/s3, aws/ebs, aws/rds
                      
  Customer Managed:   You create and control
                      Manual or automatic rotation (annual)
                      Key policies control access
                      $1/month per key
                      
  CloudHSM Backed:    Keys stored in dedicated hardware
                      FIPS 140-2 Level 3 compliance
                      Most expensive, highest security
```

```bash
# Create KMS key
KEY_ID=$(aws kms create-key \
  --description "Production application key" \
  --key-usage ENCRYPT_DECRYPT \
  --policy '{
    "Version": "2012-10-17",
    "Statement": [
      {
        "Effect": "Allow",
        "Principal": {"AWS": "arn:aws:iam::ACCOUNT:root"},
        "Action": "kms:*",
        "Resource": "*"
      },
      {
        "Effect": "Allow",
        "Principal": {"AWS": "arn:aws:iam::ACCOUNT:role/app-role"},
        "Action": ["kms:Decrypt", "kms:GenerateDataKey"],
        "Resource": "*"
      }
    ]
  }' \
  --query KeyMetadata.KeyId --output text)

# Create human-readable alias
aws kms create-alias \
  --alias-name alias/myapp-production \
  --target-key-id $KEY_ID

# Encrypt data
aws kms encrypt \
  --key-id alias/myapp-production \
  --plaintext "my secret data" \
  --query CiphertextBlob --output text

# Decrypt data
aws kms decrypt \
  --ciphertext-blob fileb://encrypted.bin \
  --query Plaintext --output text | base64 -d

# Enable automatic rotation (annual)
aws kms enable-key-rotation --key-id $KEY_ID
```

### Envelope Encryption

```
KMS uses envelope encryption:

1. Generate Data Key (DEK) from KMS
   DEK = actual key that encrypts your data
   
2. Use DEK to encrypt your data (locally, fast)
   Encrypted data output
   
3. KMS encrypts the DEK with your CMK
   Encrypted DEK stored alongside encrypted data
   
4. Delete plaintext DEK from memory

WHY: 
  KMS can't encrypt large data directly (max 4KB)
  Envelope encryption = unlimited data size
  DEK encryption is local (fast)
  
DECRYPTION:
  1. Send encrypted DEK to KMS → get plaintext DEK
  2. Use DEK to decrypt your data locally
```

---

## 47. Production Incident Debugging

### AWS Incident Response Runbook

```
SEVERITY LEVELS:
  SEV-1: Complete service outage (all users affected)
         → Wake everyone up, start war room NOW
  SEV-2: Partial outage or significant degradation
         → Immediate response, senior engineers
  SEV-3: Minor degradation, workaround available
         → Business hours response
  SEV-4: Non-urgent issues
         → Scheduled fix

INCIDENT RESPONSE STEPS:
1. DETECT (< 2 minutes)
   CloudWatch Alarm fires → SNS → PagerDuty → Engineer paged

2. DECLARE (< 5 minutes)
   Create incident in Slack/PagerDuty
   Assign incident commander
   Open war room channel

3. TRIAGE (< 10 minutes)
   What is broken? What is the impact?
   Quick investigation of symptoms

4. MITIGATE (varies)
   Roll back to last known good (fastest!)
   OR fix forward if rollback not possible
   Goal: restore service, root cause later

5. RESOLVE
   Verify service restored
   Monitor for recurrence

6. POST-MORTEM (within 48 hours)
   What happened?
   Why did it happen?
   What prevented us from detecting/fixing faster?
   Action items to prevent recurrence
   BLAMELESS — focus on systems, not people
```

### Production Debugging Commands

```bash
# ─── Quick health check dashboard ───
#!/bin/bash
REGION=us-east-1
ENV=production

echo "=== ALB Health ==="
aws elbv2 describe-target-health \
  --target-group-arn $TG_ARN \
  --query 'TargetHealthDescriptions[*].[Target.Id,TargetHealth.State,TargetHealth.Description]' \
  --output table

echo "=== ASG Status ==="
aws autoscaling describe-auto-scaling-groups \
  --auto-scaling-group-names $ASG_NAME \
  --query 'AutoScalingGroups[0].[MinSize,MaxSize,DesiredCapacity,Instances[*].HealthStatus]' \
  --output json

echo "=== RDS Status ==="
aws rds describe-db-instances \
  --db-instance-identifier $RDS_ID \
  --query 'DBInstances[0].[DBInstanceStatus,Endpoint.Address,MultiAZ]' \
  --output table

echo "=== Recent Errors in Logs ==="
aws logs filter-log-events \
  --log-group-name /myapp/production/application \
  --start-time $(date -d '15 minutes ago' +%s)000 \
  --filter-pattern "ERROR" \
  --query 'events[*].[timestamp,message]' \
  --output text | tail -20

echo "=== EC2 Metrics (Last 5 min) ==="
aws cloudwatch get-metric-statistics \
  --namespace AWS/EC2 \
  --metric-name CPUUtilization \
  --dimensions Name=AutoScalingGroupName,Value=$ASG_NAME \
  --start-time $(date -d '10 minutes ago' -u +%Y-%m-%dT%H:%M:%S) \
  --end-time $(date -u +%Y-%m-%dT%H:%M:%S) \
  --period 60 \
  --statistics Average,Maximum \
  --output table
```

---

## 48. Observability Architecture

### The Complete Observability Stack

```
DATA SOURCES:
  EC2/ECS/EKS → CloudWatch Agent → CloudWatch Metrics + Logs
  Lambda       → CloudWatch Metrics + Logs (automatic)
  RDS          → Performance Insights + Enhanced Monitoring
  ALB          → Access Logs → S3 → Athena queries
  API Gateway  → CloudWatch Logs + X-Ray
  
AGGREGATION:
  CloudWatch → Dashboards
  CloudWatch Logs → Log Insights
  X-Ray → Service Map (distributed trace visualization)
  
ALERTING:
  CloudWatch Alarms → SNS → PagerDuty/Slack/Email
  EventBridge → Lambda → Automated remediation
  
ANALYSIS:
  CloudWatch Logs Insights → Ad-hoc queries
  Athena → SQL queries on S3 logs
  OpenSearch → Full-text search on logs
```

### Building Effective Dashboards

```bash
# Create CloudWatch Dashboard
aws cloudwatch put-dashboard \
  --dashboard-name production-overview \
  --dashboard-body '{
    "widgets": [
      {
        "type": "metric",
        "properties": {
          "metrics": [
            ["AWS/ApplicationELB", "RequestCount", "LoadBalancer", "app/my-alb/1234"],
            ["AWS/ApplicationELB", "HTTPCode_Target_5XX_Count", "LoadBalancer", "app/my-alb/1234"]
          ],
          "period": 60,
          "stat": "Sum",
          "title": "ALB Requests & Errors"
        }
      },
      {
        "type": "metric",
        "properties": {
          "metrics": [
            ["AWS/EC2", "CPUUtilization", "AutoScalingGroupName", "my-app-asg"]
          ],
          "period": 60,
          "stat": "Average",
          "title": "Application CPU"
        }
      },
      {
        "type": "log",
        "properties": {
          "query": "fields @timestamp, @message | filter @message like /ERROR/ | sort @timestamp desc | limit 20",
          "logGroupNames": ["/myapp/production/application"],
          "title": "Recent Errors"
        }
      }
    ]
  }'
```

---

## 49. Large-Scale Troubleshooting

### Common Production Issues & Solutions

**Issue 1: High latency on API**
```bash
# Investigation steps:
# 1. Check X-Ray service map → which service is slow?
aws xray get-service-graph \
  --start-time $(date -d '1 hour ago' +%s) \
  --end-time $(date +%s)

# 2. Check ALB target response times
aws cloudwatch get-metric-statistics \
  --namespace AWS/ApplicationELB \
  --metric-name TargetResponseTime \
  --statistics Average,p95,p99 \
  --period 60 \
  --start-time ... --end-time ...

# 3. Check RDS metrics
aws cloudwatch get-metric-statistics \
  --namespace AWS/RDS \
  --metric-name DatabaseConnections \
  --period 60 ...

# 4. Check for DB slow queries
aws logs filter-log-events \
  --log-group-name /aws/rds/instance/prod-db/slowquery \
  --filter-pattern "Query_time"
```

**Issue 2: EC2 instances failing health checks**
```bash
# Check system logs
aws ec2 get-console-output --instance-id i-1234567890abcdef0

# Connect via SSM (no SSH needed)
aws ssm start-session --target i-1234567890abcdef0

# Check if app is running
systemctl status myapp
journalctl -u myapp -n 100

# Check disk space
df -h

# Check memory
free -h

# Check for OOM kills
dmesg | grep -i "oom\|killed"
```

**Issue 3: Lambda timeout/memory errors**
```bash
# View Lambda logs
aws logs tail /aws/lambda/my-function --follow

# Get Lambda configuration
aws lambda get-function-configuration \
  --function-name my-function \
  --query '[Timeout,MemorySize,Environment]'

# Update timeout and memory
aws lambda update-function-configuration \
  --function-name my-function \
  --timeout 60 \
  --memory-size 1024
```

---

## 50. ML on AWS: SageMaker & AI Services

### AWS AI/ML Services Overview

```
LEVEL 1 — AI SERVICES (No ML knowledge needed):
  Rekognition:    Image/video analysis (faces, objects, text)
  Comprehend:     NLP (sentiment, entities, topics)
  Translate:      Language translation (75+ languages)
  Polly:          Text-to-speech (multiple voices/languages)
  Transcribe:     Speech-to-text
  Lex:            Build chatbots (powers Alexa!)
  Textract:       Extract text from documents/forms
  Fraud Detector: ML-based fraud detection
  Personalize:    Recommendation engine (like Netflix)

LEVEL 2 — ML PLATFORM (Data scientists):
  SageMaker:      End-to-end ML platform
                  Studio (IDE), Training, Endpoints, Pipelines

LEVEL 3 — CUSTOM ML (ML engineers):
  EC2 P-series:   GPU instances for custom training
  Trainium:       AWS custom ML training chip
  Inferentia:     AWS custom ML inference chip (cheapest inference)
```

### SageMaker Workflow

```
1. DATA PREPARATION
   S3 (data storage) → SageMaker Data Wrangler (visual prep)
   
2. TRAINING
   SageMaker Training Job (managed training cluster)
   Built-in algorithms OR custom Docker containers
   Spot instances for training (70% cost reduction)
   
3. MODEL EVALUATION
   SageMaker Experiments (track runs, metrics)
   SageMaker Clarify (bias detection, explainability)
   
4. DEPLOYMENT
   SageMaker Endpoint (real-time inference)
   SageMaker Batch Transform (batch inference)
   
5. MONITORING
   SageMaker Model Monitor (detect data drift)
   SageMaker Clarify (ongoing bias monitoring)
```

```python
# SageMaker SDK example — Train and deploy a model
import sagemaker
from sagemaker.sklearn import SKLearn

session = sagemaker.Session()
role = sagemaker.get_execution_role()

# Upload training data to S3
train_data_path = session.upload_data('train.csv', key_prefix='mymodel/data')

# Train with built-in SKLearn
estimator = SKLearn(
    entry_point='train.py',
    framework_version='1.0-1',
    instance_type='ml.m5.large',
    instance_count=1,
    role=role,
    use_spot_instances=True,       # 70% cost savings!
    max_wait=7200,
    hyperparameters={
        'n-estimators': 100,
        'max-depth': 6
    }
)

estimator.fit({'train': train_data_path})

# Deploy to real-time endpoint
predictor = estimator.deploy(
    initial_instance_count=2,
    instance_type='ml.t3.medium',
    endpoint_name='my-model-production'
)

# Make predictions
result = predictor.predict([[1.2, 3.4, 5.6, 7.8]])
print(f"Prediction: {result}")

# Cleanup
predictor.delete_endpoint()
```

---

# ═══════════════════════════════════════
# APPENDIX
# ═══════════════════════════════════════

---

<a id="well-architected"></a>
## AWS Well-Architected Framework Checklist

```
OPERATIONAL EXCELLENCE:
□ All infrastructure as code (CloudFormation/Terraform)
□ CI/CD pipeline for all deployments
□ Runbooks for all operational procedures
□ Regular game days (simulate failures)

SECURITY:
□ MFA on all accounts
□ Least privilege IAM (no AdministratorAccess on production roles)
□ All data encrypted at rest and in transit
□ GuardDuty enabled in all regions
□ Security Hub enabled
□ CloudTrail enabled multi-region
□ Config rules for compliance

RELIABILITY:
□ Multi-AZ for all stateful resources
□ Auto Scaling for compute
□ RDS Multi-AZ
□ Backup and restore tested
□ Health checks on all endpoints
□ Circuit breakers in application code

PERFORMANCE EFFICIENCY:
□ CloudFront for content delivery
□ ElastiCache for frequently accessed data
□ Right-sized instances (Compute Optimizer)
□ Appropriate storage class for each use case

COST OPTIMIZATION:
□ Budgets and billing alerts
□ Reserved Instances for stable workloads
□ Spot for fault-tolerant workloads
□ Turn off non-production resources nights/weekends
□ S3 lifecycle policies
□ Delete unused resources (EBS, EIP, LBs)
```

---

<a id="exam-tips"></a>
## Certification Exam Tips

### Cloud Practitioner (CLF-C02)
```
Focus areas:
- Cloud benefits and economics (trade CapEx for OpEx)
- Shared responsibility model
- IAM basics
- Core services: EC2, S3, VPC, RDS, Lambda
- Support plans
- Billing and pricing

Tips:
- 90 questions, 90 minutes
- Most questions are conceptual (not technical commands)
- If unsure: think "which option is most cost-effective" or "most operationally simple"
```

### Solutions Architect Associate (SAA-C03)
```
Focus areas:
- VPC in depth (subnets, SGs, NACLs, peering)
- EC2 purchasing options and types
- Storage (S3, EBS, EFS, Glacier)
- High availability (Multi-AZ, ASG, ALB)
- Database services (RDS, DynamoDB, Aurora)
- Serverless (Lambda, API Gateway, DynamoDB)
- Security (IAM, KMS, WAF)

Tips:
- 65 questions, 130 minutes
- Lots of scenario-based questions
- Think: "highly available", "cost-effective", "secure"
- ALWAYS consider multi-AZ for HA
- Read each option carefully — wrong answers are designed to look right
```

### DevOps Engineer Professional (DOP-C02)
```
Focus areas:
- CI/CD with CodePipeline, CodeBuild, CodeDeploy
- Infrastructure as Code (CloudFormation in depth)
- ECS, EKS, Fargate
- Monitoring and observability
- Incident management
- Configuration management (SSM)
- Blue/Green, Canary deployments

Tips:
- 75 questions, 180 minutes
- Deep technical knowledge required
- Scenario questions are complex (multiple correct-looking answers)
- Know CodePipeline workflow deeply
- Know deployment strategies (Blue/Green, Canary, Rolling)
```

---

<a id="interview-questions"></a>
## AWS Interview Questions Bank

### Junior/Mid Level

**Q1:** Explain the difference between vertical and horizontal scaling.
```
Vertical: Make one server bigger (more CPU, RAM) — has limits, single point of failure
Horizontal: Add more servers — scales infinitely, resilient to failures
AWS favors horizontal scaling with Auto Scaling Groups
```

**Q2:** How would you secure an S3 bucket containing sensitive customer data?
```
1. Block Public Access (account + bucket level)
2. Enable default encryption (SSE-KMS)
3. Enable versioning
4. Enable MFA Delete
5. Bucket policy denying non-HTTPS access
6. VPC Endpoint for private access
7. Enable S3 server access logging
8. Enable Macie to detect PII
9. Use presigned URLs for controlled access
```

**Q3:** A company wants to reduce database load. What strategies would you recommend?
```
1. ElastiCache (Redis) in front of RDS for frequently read data
2. Read Replicas for read-heavy queries (reporting)
3. RDS Proxy for connection pooling (especially with Lambda)
4. Query optimization + appropriate indexes
5. Database right-sizing (Performance Insights)
```

### Senior/Lead Level

**Q4:** Design a multi-region DR strategy with RTO < 5 minutes and RPO < 1 minute.
```
Solution: Warm Standby or Active-Active with Aurora Global Database
- Aurora Global Database: < 1 second replication lag globally
- Route 53 health checks + failover routing
- Automated failover Lambda triggered by health check failure
- RTO: DNS propagation (~60s) + Aurora promote (~60s) = ~2 minutes
- RPO: < 1 second (Aurora Global replication)
- Cost: ~2x single region
```

**Q5:** How do you handle secret rotation in a production microservices environment?
```
1. Secrets Manager with automatic rotation (Lambda rotator)
2. Application reads secret at startup AND caches with TTL
3. Rotation happens: new secret created → app uses new → old deleted
4. Rolling restarts during rotation window (Kubernetes, ECS rolling update)
5. Monitor rotation with CloudWatch Events
6. Test rotation procedure monthly
```

---

<a id="cheat-sheet"></a>
## AWS Architecture Cheat Sheet

### Choosing the Right Database
```
Relational (SQL):          RDS (MySQL, PostgreSQL, Aurora)
NoSQL Key-Value:           DynamoDB
NoSQL Document:            DocumentDB (MongoDB-compatible)
In-Memory Cache:           ElastiCache (Redis or Memcached)
Data Warehouse:            Redshift
Search:                    OpenSearch Service
Time-Series:               Timestream
Graph:                     Neptune
Ledger:                    QLDB
```

### Choosing the Right Compute
```
Long-running servers:      EC2
Auto-scaling web apps:     EC2 + ASG + ALB
PaaS (simple deploy):      Elastic Beanstalk
Event-driven:              Lambda
Containers (managed EC2):  ECS on EC2
Containers (serverless):   ECS on Fargate
Kubernetes:                EKS
Batch jobs:                Batch
```

### Choosing the Right Storage
```
Block (OS, DB volumes):    EBS
Shared file system:        EFS
Object storage:            S3
Archive:                   S3 Glacier / Deep Archive
Local (temporary):         Instance Store
Hybrid (on-prem + cloud):  Storage Gateway
CDN:                       CloudFront
```

### Disaster Recovery Decision Matrix
```
Cost budget LOW + RTO hours OK:        Backup & Restore
Cost budget LOW + RTO 10 min OK:       Pilot Light
Cost budget MEDIUM + RTO minutes OK:   Warm Standby
Cost budget HIGH + RTO seconds OK:     Multi-Site Active-Active
```

---

## 🗺️ YOUR CERTIFICATION ROADMAP

```
MONTH 1-2: CLOUD PRACTITIONER
  □ Parts 1 of this guide
  □ Practice: AWS Console exploration
  □ Free: AWS Skill Builder courses
  □ Practice tests: Tutorials Dojo
  □ Take exam! (entry-level, build confidence)

MONTH 3-5: SOLUTIONS ARCHITECT ASSOCIATE
  □ Parts 1-2 of this guide
  □ Lab: A Cloud Guru or Linux Academy hands-on labs
  □ Build: 3-tier web app from scratch
  □ Practice tests: Tutorials Dojo (all practice exams)
  
MONTH 6-8: DEVELOPER OR SYSOPS ASSOCIATE
  □ Part 2-3 of this guide
  □ Lab: Deploy containerized app on ECS/EKS
  □ Build: Full CI/CD pipeline
  
MONTH 9-12: DEVOPS PROFESSIONAL
  □ All parts of this guide
  □ Real project: production-grade pipeline
  □ Study: CloudFormation in depth
  □ Hardest AWS exam — allow extra time
  
YEAR 2: SPECIALTIES
  □ Security Specialty (career-defining)
  □ Advanced Networking Specialty
  □ ML Specialty (high demand)

RESOURCES:
  □ AWS Documentation (best source)
  □ Tutorials Dojo (best practice tests)
  □ Adrian Cantrill (best courses, deep content)
  □ AWS re:Invent YouTube (free talks by builders)
  □ AWS Well-Architected Labs (free hands-on)
  □ Cloud Quest (AWS gamified learning)
```

---

*You now hold a complete AWS mastery guide. From Cloud Practitioner foundations to Expert-level architecture and debugging. The cloud rewards those who build, break, and learn. Create real projects. Earn real certifications. Design real systems. 

Welcome to the cloud. ☁️*
