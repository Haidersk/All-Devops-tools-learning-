# GCP Cloud & DevOps Mastery Guide — Part 2
### DevOps, Advanced Architecture, Expert & Specialty Levels

---

# ═══════════════════════════════════════
# PART 3: DEVOPS / ENGINEER LEVEL
# ═══════════════════════════════════════

---

## 26. Terraform on GCP

### Why Infrastructure as Code?

```
PROBLEM with manual Cloud Console:
  - Not reproducible (different config each time)
  - Can't version control infrastructure
  - Can't review changes before applying (code review)
  - Error-prone (human mistakes)
  - Can't diff environments (prod vs staging)

SOLUTION — Infrastructure as Code:
  - Infrastructure in files → version controlled in Git
  - Repeatable: same code = same infrastructure every time
  - Reviewable: team reviews changes via PR before applying
  - Auditable: history of every change
  - Self-documenting: code IS the documentation
```

### Terraform GCP Provider

```hcl
# main.tf — Complete production GCP setup with Terraform

terraform {
  required_version = ">= 1.5"
  required_providers {
    google = {
      source  = "hashicorp/google"
      version = "~> 5.0"
    }
    google-beta = {
      source  = "hashicorp/google-beta"
      version = "~> 5.0"
    }
  }

  # Remote state in Cloud Storage (critical for teams!)
  backend "gcs" {
    bucket = "my-terraform-state-bucket"
    prefix = "production/terraform.tfstate"
  }
}

provider "google" {
  project = var.project_id
  region  = var.region
}

# ─── Variables ───
variable "project_id"   { default = "my-production-project" }
variable "region"       { default = "us-central1" }
variable "environment"  { default = "production" }
variable "db_password"  { sensitive = true }

locals {
  common_labels = {
    environment = var.environment
    managed_by  = "terraform"
    project     = "myapp"
  }
}

# ─── VPC ───
resource "google_compute_network" "main" {
  name                    = "${var.environment}-vpc"
  auto_create_subnetworks = false
  routing_mode            = "GLOBAL"
}

resource "google_compute_subnetwork" "us" {
  name                     = "${var.environment}-us-subnet"
  ip_cidr_range            = "10.10.0.0/20"
  region                   = "us-central1"
  network                  = google_compute_network.main.id
  private_ip_google_access = true

  secondary_ip_range {
    range_name    = "pods"
    ip_cidr_range = "10.100.0.0/16"
  }

  secondary_ip_range {
    range_name    = "services"
    ip_cidr_range = "10.200.0.0/20"
  }

  log_config {
    aggregation_interval = "INTERVAL_10_MIN"
    flow_sampling        = 0.5
    metadata             = "INCLUDE_ALL_METADATA"
  }
}

resource "google_compute_subnetwork" "eu" {
  name                     = "${var.environment}-eu-subnet"
  ip_cidr_range            = "10.20.0.0/20"
  region                   = "europe-west1"
  network                  = google_compute_network.main.id
  private_ip_google_access = true
}

# ─── Firewall Rules ───
resource "google_compute_firewall" "allow_internal" {
  name    = "${var.environment}-allow-internal"
  network = google_compute_network.main.id

  allow {
    protocol = "tcp"
  }
  allow {
    protocol = "udp"
  }
  allow {
    protocol = "icmp"
  }

  source_ranges = ["10.0.0.0/8"]
}

resource "google_compute_firewall" "allow_web" {
  name    = "${var.environment}-allow-web"
  network = google_compute_network.main.id

  allow {
    protocol = "tcp"
    ports    = ["80", "443"]
  }

  source_ranges = ["0.0.0.0/0"]
  target_tags   = ["web-server"]
}

resource "google_compute_firewall" "allow_health_check" {
  name    = "${var.environment}-allow-health-check"
  network = google_compute_network.main.id

  allow {
    protocol = "tcp"
    ports    = ["80", "8080"]
  }

  # Google's Load Balancer health check IPs (required!)
  source_ranges = ["130.211.0.0/22", "35.191.0.0/16"]
  target_tags   = ["web-server"]
}

# ─── Cloud NAT ───
resource "google_compute_router" "main" {
  name    = "${var.environment}-router"
  region  = var.region
  network = google_compute_network.main.id
}

resource "google_compute_router_nat" "main" {
  name                               = "${var.environment}-nat"
  router                             = google_compute_router.main.name
  region                             = var.region
  nat_ip_allocate_option             = "AUTO_ONLY"
  source_subnetwork_ip_ranges_to_nat = "ALL_SUBNETWORKS_ALL_IP_RANGES"

  log_config {
    enable = true
    filter = "ERRORS_ONLY"
  }
}

# ─── Service Account for app VMs ───
resource "google_service_account" "app" {
  account_id   = "app-server-sa"
  display_name = "App Server Service Account"
}

resource "google_project_iam_member" "app_storage_reader" {
  project = var.project_id
  role    = "roles/storage.objectViewer"
  member  = "serviceAccount:${google_service_account.app.email}"
}

resource "google_project_iam_member" "app_secret_accessor" {
  project = var.project_id
  role    = "roles/secretmanager.secretAccessor"
  member  = "serviceAccount:${google_service_account.app.email}"
}

# ─── Instance Template ───
resource "google_compute_instance_template" "app" {
  name_prefix  = "${var.environment}-app-template-"
  machine_type = "n2-standard-4"
  region       = var.region
  tags         = ["web-server"]
  labels       = local.common_labels

  disk {
    source_image = "projects/debian-cloud/global/images/family/debian-12"
    auto_delete  = true
    boot         = true
    disk_type    = "pd-ssd"
    disk_size_gb = 50
  }

  network_interface {
    network    = google_compute_network.main.id
    subnetwork = google_compute_subnetwork.us.id
    # No access_config = no public IP (private only, use Cloud NAT for egress)
  }

  service_account {
    email  = google_service_account.app.email
    scopes = ["cloud-platform"]
  }

  metadata = {
    startup-script = <<-EOF
      #!/bin/bash
      apt-get update -y
      apt-get install -y nginx google-cloud-ops-agent
      
      # Get secrets from Secret Manager
      DB_PASS=$(gcloud secrets versions access latest --secret="db-password")
      
      systemctl start nginx
      systemctl enable nginx
      echo "<h1>Production App on $(hostname)</h1>" > /var/www/html/index.html
      
      # Start ops agent for monitoring
      systemctl restart google-cloud-ops-agent
    EOF
  }

  lifecycle {
    create_before_destroy = true  # Zero-downtime updates!
  }
}

# ─── Managed Instance Group ───
resource "google_compute_region_instance_group_manager" "app" {
  name               = "${var.environment}-app-mig"
  region             = var.region
  base_instance_name = "app"

  version {
    name              = "stable"
    instance_template = google_compute_instance_template.app.id
  }

  target_size = 3

  named_port {
    name = "http"
    port = 80
  }

  auto_healing_policies {
    health_check      = google_compute_health_check.app.id
    initial_delay_sec = 300
  }

  update_policy {
    type                  = "PROACTIVE"
    minimal_action        = "REPLACE"
    max_surge_fixed       = 3
    max_unavailable_fixed = 0    # Zero downtime updates
  }
}

# ─── Autoscaling ───
resource "google_compute_region_autoscaler" "app" {
  name   = "${var.environment}-app-autoscaler"
  region = var.region
  target = google_compute_region_instance_group_manager.app.id

  autoscaling_policy {
    max_replicas    = 20
    min_replicas    = 2
    cooldown_period = 120

    cpu_utilization {
      target = 0.6
    }
  }
}

# ─── Health Check ───
resource "google_compute_health_check" "app" {
  name               = "${var.environment}-app-health-check"
  check_interval_sec = 30
  timeout_sec        = 10
  healthy_threshold  = 2
  unhealthy_threshold = 3

  http_health_check {
    port         = 80
    request_path = "/health"
  }
}

# ─── Cloud SQL ───
resource "google_sql_database_instance" "main" {
  name             = "${var.environment}-postgres"
  database_version = "POSTGRES_15"
  region           = var.region

  settings {
    tier              = "db-n1-standard-4"
    availability_type = "REGIONAL"   # HA!
    disk_type         = "PD_SSD"
    disk_size         = 100
    disk_autoresize   = true

    backup_configuration {
      enabled                        = true
      start_time                     = "02:00"
      point_in_time_recovery_enabled = true
      transaction_log_retention_days = 7
      backup_retention_settings {
        retained_backups = 7
      }
    }

    ip_configuration {
      ipv4_enabled    = false    # No public IP!
      private_network = google_compute_network.main.id
    }

    database_flags {
      name  = "max_connections"
      value = "500"
    }
  }

  deletion_protection = true
}

resource "google_sql_database" "app" {
  name     = "myapp"
  instance = google_sql_database_instance.main.name
}

# ─── Secret Manager ───
resource "google_secret_manager_secret" "db_password" {
  secret_id = "db-password"
  replication {
    auto {}
  }
}

resource "google_secret_manager_secret_version" "db_password" {
  secret      = google_secret_manager_secret.db_password.id
  secret_data = var.db_password
}

# ─── Cloud Storage ───
resource "google_storage_bucket" "assets" {
  name          = "${var.project_id}-assets"
  location      = "US"
  storage_class = "STANDARD"

  uniform_bucket_level_access = true

  versioning {
    enabled = true
  }

  lifecycle_rule {
    condition { age = 90 }
    action {
      type          = "SetStorageClass"
      storage_class = "NEARLINE"
    }
  }

  lifecycle_rule {
    condition { age = 365 }
    action {
      type          = "SetStorageClass"
      storage_class = "COLDLINE"
    }
  }
}

# ─── Outputs ───
output "mig_name" {
  value = google_compute_region_instance_group_manager.app.name
}

output "db_private_ip" {
  value     = google_sql_database_instance.main.private_ip_address
  sensitive = true
}
```

```bash
# ─── Terraform workflow ───
# Initialize (download providers, set up backend)
terraform init

# Preview changes
terraform plan -var="db_password=SuperSecret123" -out=tfplan

# Apply changes
terraform apply tfplan

# Destroy (tear down all)
terraform destroy -var="db_password=SuperSecret123"

# Import existing resource into Terraform state
terraform import google_compute_instance.existing projects/PROJECT/zones/us-central1-a/instances/my-vm

# Terraform workspaces (manage dev/staging/prod)
terraform workspace new staging
terraform workspace select production
terraform workspace list
```

---

## 27. CI/CD with Cloud Build

### What is Cloud Build?

```
Cloud Build = Fully managed CI/CD service on GCP
No servers to manage, scales automatically
Integrates with: GitHub, GitLab, Bitbucket, Cloud Source Repos
Runs in isolated Docker containers
Free tier: 120 build-minutes/day
```

### cloudbuild.yaml — The Build Definition

```yaml
# cloudbuild.yaml — Complete CI/CD pipeline

steps:
  # ─── Step 1: Install dependencies ───
  - name: 'node:20'
    id: 'install'
    entrypoint: 'npm'
    args: ['ci']
    dir: '/workspace'

  # ─── Step 2: Run linting ───
  - name: 'node:20'
    id: 'lint'
    entrypoint: 'npm'
    args: ['run', 'lint']
    waitFor: ['install']

  # ─── Step 3: Run unit tests ───
  - name: 'node:20'
    id: 'test'
    entrypoint: 'npm'
    args: ['run', 'test:coverage']
    waitFor: ['install']
    env:
      - 'NODE_ENV=test'
      - 'CI=true'

  # ─── Step 4: Security scanning ───
  - name: 'gcr.io/cloud-builders/gcloud'
    id: 'security-scan'
    entrypoint: 'bash'
    args:
      - '-c'
      - |
        npm audit --audit-level=high
        echo "Security scan passed"
    waitFor: ['install']

  # ─── Step 5: Build Docker image ───
  - name: 'gcr.io/cloud-builders/docker'
    id: 'build-image'
    args:
      - 'build'
      - '-t'
      - '${_REGION}-docker.pkg.dev/${PROJECT_ID}/${_REPO}/${_SERVICE}:${SHORT_SHA}'
      - '-t'
      - '${_REGION}-docker.pkg.dev/${PROJECT_ID}/${_REPO}/${_SERVICE}:latest'
      - '--build-arg'
      - 'BUILD_DATE=${_BUILD_DATE}'
      - '--cache-from'
      - '${_REGION}-docker.pkg.dev/${PROJECT_ID}/${_REPO}/${_SERVICE}:latest'
      - '.'
    waitFor: ['lint', 'test', 'security-scan']

  # ─── Step 6: Push to Artifact Registry ───
  - name: 'gcr.io/cloud-builders/docker'
    id: 'push-image'
    args:
      - 'push'
      - '--all-tags'
      - '${_REGION}-docker.pkg.dev/${PROJECT_ID}/${_REPO}/${_SERVICE}'
    waitFor: ['build-image']

  # ─── Step 7: Deploy to Cloud Run (staging) ───
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    id: 'deploy-staging'
    args:
      - 'run'
      - 'deploy'
      - '${_SERVICE}-staging'
      - '--image=${_REGION}-docker.pkg.dev/${PROJECT_ID}/${_REPO}/${_SERVICE}:${SHORT_SHA}'
      - '--region=${_REGION}'
      - '--platform=managed'
      - '--no-allow-unauthenticated'
      - '--set-env-vars=ENVIRONMENT=staging'
    waitFor: ['push-image']

  # ─── Step 8: Run integration tests against staging ───
  - name: 'node:20'
    id: 'integration-tests'
    entrypoint: 'npm'
    args: ['run', 'test:integration']
    env:
      - 'BASE_URL=https://${_SERVICE}-staging-${PROJECT_NUMBER}.${_REGION}.run.app'
    waitFor: ['deploy-staging']

  # ─── Step 9: Deploy to production (with traffic split) ───
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    id: 'deploy-production'
    args:
      - 'run'
      - 'deploy'
      - '${_SERVICE}'
      - '--image=${_REGION}-docker.pkg.dev/${PROJECT_ID}/${_REPO}/${_SERVICE}:${SHORT_SHA}'
      - '--region=${_REGION}'
      - '--platform=managed'
      - '--no-traffic'   # Deploy without routing traffic yet
    waitFor: ['integration-tests']

  # ─── Step 10: Canary — send 10% to new version ───
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    id: 'canary-release'
    entrypoint: 'bash'
    args:
      - '-c'
      - |
        gcloud run services update-traffic ${_SERVICE} \
          --region=${_REGION} \
          --to-revisions=${_SERVICE}-${SHORT_SHA}=10

        echo "Canary deployed: 10% traffic to new version"
        echo "Monitor for 5 minutes..."
        sleep 300

        # Check error rate
        ERROR_RATE=$(gcloud monitoring time-series list \
          'metric.type="run.googleapis.com/request_count" AND metric.labels.response_code_class="5xx"' \
          --format="value(points[0].value.int64Value)" 2>/dev/null || echo "0")
        
        if [ "$ERROR_RATE" -gt "5" ]; then
          echo "High error rate detected! Rolling back..."
          gcloud run services update-traffic ${_SERVICE} \
            --region=${_REGION} \
            --to-latest
          exit 1
        fi

        echo "Canary healthy. Promoting to 100%..."
        gcloud run services update-traffic ${_SERVICE} \
          --region=${_REGION} \
          --to-latest
    waitFor: ['deploy-production']

# ─── Substitution variables ───
substitutions:
  _REGION: us-central1
  _REPO: my-app-repo
  _SERVICE: my-app
  _BUILD_DATE: $(date -u +%Y-%m-%dT%H:%M:%SZ)

# ─── Build options ───
options:
  machineType: 'E2_HIGHCPU_8'   # Faster build machine
  logging: CLOUD_LOGGING_ONLY

# ─── Artifacts to store ───
artifacts:
  objects:
    location: 'gs://${PROJECT_ID}-build-artifacts'
    paths:
      - 'coverage/**'
      - 'test-results/**'
```

```bash
# ─── Trigger build manually ───
gcloud builds submit \
  --config=cloudbuild.yaml \
  --substitutions=SHORT_SHA=$(git rev-parse --short HEAD)

# ─── Create a trigger (auto-build on push) ───
gcloud builds triggers create github \
  --repo-name=my-app \
  --repo-owner=myorg \
  --branch-pattern="^main$" \
  --build-config=cloudbuild.yaml \
  --description="Deploy to production on push to main"

# ─── View builds ───
gcloud builds list --limit=10

# ─── Stream build logs ───
gcloud builds log BUILD_ID --stream

# ─── Cancel a build ───
gcloud builds cancel BUILD_ID
```

---

## 28. Artifact Registry

### What is Artifact Registry?

```
Artifact Registry = Private package and container registry on GCP
Replaces Google Container Registry (GCR) — use AR instead!

Supported formats:
  Docker:    Container images
  Maven:     Java packages
  npm:       Node.js packages
  Python:    PyPI packages
  Apt/Yum:  OS packages
  Helm:     Kubernetes charts
  Go:       Go modules
```

```bash
# ─── Create repository ───
gcloud artifacts repositories create my-app-repo \
  --repository-format=docker \
  --location=us-central1 \
  --description="Production container images" \
  --immutable-tags   # Prevent tag overwrites!

# ─── Authenticate Docker ───
gcloud auth configure-docker us-central1-docker.pkg.dev

# ─── Build and push ───
IMAGE="us-central1-docker.pkg.dev/PROJECT/my-app-repo/api:1.2.3"
docker build -t $IMAGE .
docker push $IMAGE

# ─── Scan for vulnerabilities ───
# Automatic on push if enabled:
gcloud artifacts repositories update my-app-repo \
  --location=us-central1 \
  --enable-vulnerability-scanning

# View scan results
gcloud artifacts docker images list \
  us-central1-docker.pkg.dev/PROJECT/my-app-repo \
  --show-occurrences \
  --occurrence-filter="kind=VULNERABILITY AND vulnerability.severity=CRITICAL"

# ─── Lifecycle policies ───
gcloud artifacts repositories set-cleanup-policies my-app-repo \
  --location=us-central1 \
  --policy='{
    "name": "delete-old-images",
    "action": {"type": "Delete"},
    "condition": {
      "tagState": "UNTAGGED",
      "olderThan": "2592000s"
    }
  }'

# ─── npm repository ───
gcloud artifacts repositories create npm-packages \
  --repository-format=npm \
  --location=us-central1

# Configure npm to use Artifact Registry
gcloud artifacts print-settings npm \
  --repository=npm-packages \
  --location=us-central1 \
  --scope=@mycompany   # Only @mycompany scoped packages use this registry
```

---

## 29. Cloud Run

### What is Cloud Run?

```
Cloud Run = Fully managed serverless container platform
  Deploy any container → Cloud Run handles everything else
  Auto-scales from 0 → thousands of instances in seconds
  Pay only when serving requests (billing per 100ms)

WHY CLOUD RUN?
  vs Lambda/Cloud Functions:
    No language/framework restrictions
    Longer timeouts (up to 60 minutes)
    More memory (up to 32 GB)
    
  vs GKE:
    No cluster to manage
    No Kubernetes YAML to write
    Much simpler operations
    
  Best for:
    APIs and microservices
    Event-driven processing
    Batch jobs
    Web backends
    Any containerized HTTP workload
```

```bash
# ─── Deploy to Cloud Run ───
gcloud run deploy my-api \
  --image=us-central1-docker.pkg.dev/PROJECT/my-repo/api:1.2.3 \
  --region=us-central1 \
  --platform=managed \
  --allow-unauthenticated \      # Public endpoint
  --concurrency=80 \             # Requests per container instance
  --cpu=2 \
  --memory=1Gi \
  --min-instances=1 \            # Keep 1 warm (no cold starts!)
  --max-instances=100 \
  --timeout=60 \                 # 60 second request timeout
  --set-env-vars=ENVIRONMENT=production,PORT=8080 \
  --set-secrets=DB_PASSWORD=db-password:latest \
  --service-account=run-sa@PROJECT.iam.gserviceaccount.com \
  --vpc-connector=my-vpc-connector \    # Connect to VPC for private resources
  --vpc-egress=private-ranges-only

# ─── Traffic management ───
# Deploy new version without traffic
gcloud run deploy my-api \
  --image=us-central1-docker.pkg.dev/PROJECT/my-repo/api:1.3.0 \
  --region=us-central1 \
  --no-traffic \
  --tag=v1-3-0

# Gradual traffic split
gcloud run services update-traffic my-api \
  --region=us-central1 \
  --to-tags=v1-3-0=10   # 10% to new version

# Promote to 100%
gcloud run services update-traffic my-api \
  --region=us-central1 \
  --to-latest

# ─── View service ───
gcloud run services describe my-api \
  --region=us-central1 \
  --format="table(status.url,spec.template.spec.containers[0].image)"

# ─── Invoke (authenticated) ───
gcloud run services proxy my-api --region=us-central1 &
curl http://localhost:8080/api/users

# ─── View logs ───
gcloud logging read \
  'resource.type=cloud_run_revision AND resource.labels.service_name=my-api' \
  --limit=20

# ─── Scale to zero when not needed ───
gcloud run services update my-api \
  --region=us-central1 \
  --min-instances=0

# ─── Cloud Run Jobs (for batch processing) ───
gcloud run jobs create data-processor \
  --image=us-central1-docker.pkg.dev/PROJECT/my-repo/processor:latest \
  --region=us-central1 \
  --tasks=10 \          # Run 10 parallel tasks
  --max-retries=3 \
  --set-env-vars=BATCH_SIZE=1000

gcloud run jobs execute data-processor --region=us-central1
```

### Cloud Run with VPC Connector

```bash
# Connect Cloud Run to your VPC (for private Cloud SQL, Redis, etc.)
gcloud compute networks vpc-access connectors create my-vpc-connector \
  --region=us-central1 \
  --network=production-vpc \
  --range=10.8.0.0/28   # Small /28 subnet just for connector

# Update Cloud Run service to use connector
gcloud run services update my-api \
  --vpc-connector=my-vpc-connector \
  --region=us-central1
```

---

## 30. GKE — Google Kubernetes Engine

### GKE Overview

```
GKE = Managed Kubernetes on Google Cloud
Google manages: Control plane (kube-apiserver, etcd, scheduler)
You manage:     Nodes, workloads, namespaces

GKE MODES:
  STANDARD:   You configure and manage node pools
              More control, more responsibility
              
  AUTOPILOT:  Google manages everything including nodes
              Pay per Pod (not per node)
              Recommended for most workloads
              
  Benefits of GKE over self-managed K8s:
    - Auto-upgrades (control plane and nodes)
    - Auto-repair (replaces unhealthy nodes)
    - Integrated load balancing, persistent volumes
    - Integrated Cloud Logging/Monitoring
    - Binary Authorization (container security)
```

```bash
# ─── Create GKE cluster (Autopilot — recommended) ───
gcloud container clusters create-auto production-cluster \
  --region=us-central1 \
  --network=production-vpc \
  --subnetwork=us-subnet \
  --cluster-secondary-range-name=pods \
  --services-secondary-range-name=services \
  --enable-private-nodes \
  --enable-master-authorized-networks \
  --master-authorized-networks=10.0.0.0/8

# ─── Create GKE cluster (Standard) ───
gcloud container clusters create production-cluster \
  --region=us-central1 \
  --node-locations=us-central1-a,us-central1-b,us-central1-c \
  --num-nodes=2 \               # 2 nodes PER zone = 6 total
  --machine-type=n2-standard-4 \
  --disk-type=pd-ssd \
  --disk-size=100 \
  --enable-autoscaling \
  --min-nodes=1 \
  --max-nodes=10 \
  --enable-autorepair \
  --enable-autoupgrade \
  --network=production-vpc \
  --subnetwork=us-subnet \
  --cluster-secondary-range-name=pods \
  --services-secondary-range-name=services \
  --enable-private-nodes \
  --master-ipv4-cidr=172.16.0.0/28 \
  --workload-pool=PROJECT.svc.id.goog   # Workload Identity!

# ─── Configure kubectl ───
gcloud container clusters get-credentials production-cluster \
  --region=us-central1

# Verify
kubectl get nodes
kubectl cluster-info

# ─── Deploy application ───
kubectl apply -f - << 'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-api
  namespace: production
  labels:
    app: my-api
    version: "1.2.3"
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-api
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0    # Zero downtime
  template:
    metadata:
      labels:
        app: my-api
        version: "1.2.3"
    spec:
      serviceAccountName: my-api-ksa    # For Workload Identity
      containers:
      - name: api
        image: us-central1-docker.pkg.dev/PROJECT/my-repo/api:1.2.3
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
        - name: PORT
          value: "8080"
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secrets
              key: db-password
        readinessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 10
        livenessProbe:
          httpGet:
            path: /health
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 30
        lifecycle:
          preStop:
            exec:
              command: ["/bin/sh", "-c", "sleep 5"]    # Graceful shutdown
EOF

# ─── Horizontal Pod Autoscaler ───
kubectl autoscale deployment my-api \
  --min=3 \
  --max=50 \
  --cpu-percent=60

# ─── Service (internal load balancer) ───
kubectl apply -f - << 'EOF'
apiVersion: v1
kind: Service
metadata:
  name: my-api-svc
  namespace: production
spec:
  selector:
    app: my-api
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP
EOF

# ─── Ingress (external HTTPS) ───
kubectl apply -f - << 'EOF'
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-api-ingress
  namespace: production
  annotations:
    kubernetes.io/ingress.class: "gce"
    kubernetes.io/ingress.global-static-ip-name: "myapp-ip"
    networking.gke.io/managed-certificates: "myapp-cert"
    kubernetes.io/ingress.allow-http: "false"
spec:
  rules:
  - host: api.myapp.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-api-svc
            port:
              number: 80
EOF

# ─── Managed Certificate ───
kubectl apply -f - << 'EOF'
apiVersion: networking.gke.io/v1
kind: ManagedCertificate
metadata:
  name: myapp-cert
  namespace: production
spec:
  domains:
    - api.myapp.com
EOF
```

### Workload Identity (GKE → GCP Services)

```bash
# Workload Identity: Pods can access GCP services using K8s Service Accounts
# No downloaded key files! Uses GKE's built-in identity federation

# 1. Create GCP Service Account
gcloud iam service-accounts create my-api-gsa \
  --display-name="My API GCP Service Account"

# 2. Grant permissions
gcloud projects add-iam-policy-binding PROJECT \
  --member="serviceAccount:my-api-gsa@PROJECT.iam.gserviceaccount.com" \
  --role="roles/cloudsql.client"

gcloud projects add-iam-policy-binding PROJECT \
  --member="serviceAccount:my-api-gsa@PROJECT.iam.gserviceaccount.com" \
  --role="roles/secretmanager.secretAccessor"

# 3. Create Kubernetes Service Account
kubectl create serviceaccount my-api-ksa \
  --namespace=production

# 4. Link KSA to GSA (the magic binding!)
gcloud iam service-accounts add-iam-policy-binding my-api-gsa@PROJECT.iam.gserviceaccount.com \
  --role="roles/iam.workloadIdentityUser" \
  --member="serviceAccount:PROJECT.svc.id.goog[production/my-api-ksa]"

# 5. Annotate KSA
kubectl annotate serviceaccount my-api-ksa \
  --namespace=production \
  iam.gke.io/gcp-service-account=my-api-gsa@PROJECT.iam.gserviceaccount.com

# Now Pods using my-api-ksa can access GCP services without any key files!
```

---

## 31. Secret Manager

### What is Secret Manager?

```
Secret Manager = Managed secret storage for GCP
Store: API keys, passwords, certificates, tokens
Features:
  - Version control (keep old versions, rotate safely)
  - Automatic replication (regional or global)
  - Fine-grained IAM access
  - Audit logging
  - Secret rotation (with Pub/Sub notifications)
  - Encryption with Cloud KMS
```

```bash
# ─── Create secrets ───
# From string
echo -n "SuperSecret123!" | gcloud secrets create db-password \
  --data-file=- \
  --replication-policy=automatic

# From file
gcloud secrets create ssl-cert \
  --data-file=./server.crt \
  --replication-policy=user-managed \
  --locations=us-central1,europe-west1

# ─── Add new version ───
echo -n "NewSecret456!" | gcloud secrets versions add db-password \
  --data-file=-

# ─── Access secret ───
gcloud secrets versions access latest --secret=db-password

# Access specific version
gcloud secrets versions access 3 --secret=db-password

# ─── List versions ───
gcloud secrets versions list db-password

# ─── Disable/enable versions ───
gcloud secrets versions disable 1 --secret=db-password
gcloud secrets versions enable 1 --secret=db-password

# ─── Delete old versions ───
gcloud secrets versions destroy 1 --secret=db-password

# ─── Grant access ───
gcloud secrets add-iam-policy-binding db-password \
  --member="serviceAccount:my-app-sa@PROJECT.iam.gserviceaccount.com" \
  --role="roles/secretmanager.secretAccessor"

# ─── Secret rotation with Pub/Sub ───
# Get notified when a secret is rotated
gcloud secrets update db-password \
  --next-rotation-time="2024-04-01T00:00:00Z" \
  --rotation-period="30d" \
  --topics=projects/PROJECT/topics/secret-rotation

# ─── Access from code (Python) ───
python3 << 'EOF'
from google.cloud import secretmanager

def get_secret(project_id: str, secret_id: str, version: str = "latest") -> str:
    client = secretmanager.SecretManagerServiceClient()
    name = f"projects/{project_id}/secrets/{secret_id}/versions/{version}"
    response = client.access_secret_version(request={"name": name})
    return response.payload.data.decode("UTF-8")

db_password = get_secret("my-project", "db-password")
print(f"Got secret: {'*' * len(db_password)}")
EOF
```

---

## 32. Cloud Operations Suite

### What is Cloud Operations Suite?

```
Cloud Operations (formerly Stackdriver) = Full observability platform

Components:
  Cloud Monitoring:   Metrics, dashboards, alerting
  Cloud Logging:      Log storage, search, routing
  Cloud Trace:        Distributed request tracing
  Cloud Profiler:     Continuous application profiling
  Cloud Debugger:     Debug live production apps
  Error Reporting:    Track and alert on errors
```

### Cloud Monitoring Deep Dive

```bash
# ─── Create custom metric descriptor ───
gcloud monitoring metric-descriptors create \
  --type=custom.googleapis.com/myapp/orders_processed \
  --description="Number of orders processed" \
  --metric-kind=GAUGE \
  --value-type=INT64 \
  --labels=payment_method,region

# ─── Create Monitoring workspace (Metrics Scope) ───
# Combine metrics from multiple projects into one view
# Console: Monitoring → Settings → Add GCP Projects

# ─── Create alerting policy via gcloud ───
gcloud alpha monitoring policies create \
  --notification-channels=CHANNEL_ID \
  --aggregation='{"alignmentPeriod": "60s", "perSeriesAligner": "ALIGN_MEAN"}' \
  --condition-display-name="High error rate" \
  --condition-filter='metric.type="run.googleapis.com/request_count" AND metric.labels.response_code_class="5xx"' \
  --condition-threshold-value=0.05 \
  --condition-threshold-comparison=COMPARISON_GT \
  --display-name="Cloud Run 5xx error rate alert"

# ─── Create notification channel ───
gcloud alpha monitoring channels create \
  --display-name="PagerDuty On-Call" \
  --type=pagerduty \
  --channel-labels=service_key=YOUR_PAGERDUTY_KEY

gcloud alpha monitoring channels create \
  --display-name="Slack #alerts" \
  --type=slack \
  --channel-labels=channel_name=#production-alerts
```

### Cloud Trace — Distributed Tracing

```python
# Add tracing to your Python app
from opentelemetry import trace
from opentelemetry.exporter.cloud_trace import CloudTraceSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

# Initialize tracer
provider = TracerProvider()
exporter = CloudTraceSpanExporter(project_id="my-project")
provider.add_span_processor(BatchSpanProcessor(exporter))
trace.set_tracer_provider(provider)

tracer = trace.get_tracer("my-service")

def process_order(order_id: str):
    with tracer.start_as_current_span("process_order") as span:
        span.set_attribute("order.id", order_id)
        span.set_attribute("service.name", "order-processor")
        
        # Nested span for DB operation
        with tracer.start_as_current_span("db_query"):
            result = db.query(f"SELECT * FROM orders WHERE id = '{order_id}'")
        
        # Nested span for payment processing
        with tracer.start_as_current_span("payment_processing"):
            payment_result = process_payment(result)
        
        span.set_attribute("payment.status", payment_result["status"])
        return payment_result
```

---

## 33. Cloud Deploy (CD for GKE/Cloud Run)

```bash
# Cloud Deploy = Managed CD service for progressive delivery

# Create delivery pipeline
gcloud deploy apply --file=pipeline.yaml --region=us-central1

# pipeline.yaml
cat > pipeline.yaml << 'EOF'
apiVersion: deploy.cloud.google.com/v1
kind: DeliveryPipeline
metadata:
  name: my-app-pipeline
description: Production delivery pipeline
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
          percentages: [10, 25, 50]
          verify: true
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: staging
description: Staging environment
run:
  location: projects/PROJECT/locations/us-central1
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: production
description: Production environment
requireApproval: true   # Manual approval gate!
run:
  location: projects/PROJECT/locations/us-central1
EOF

# Create a release (new version to deploy)
gcloud deploy releases create release-$(date +%Y%m%d-%H%M%S) \
  --delivery-pipeline=my-app-pipeline \
  --region=us-central1 \
  --images=my-app=us-central1-docker.pkg.dev/PROJECT/repo/app:SHA

# Approve production deployment
gcloud deploy rollouts approve ROLLOUT_NAME \
  --delivery-pipeline=my-app-pipeline \
  --release=RELEASE_NAME \
  --region=us-central1
```

---

## 34. Observability Architecture

### Complete Observability Stack

```
APPLICATION INSTRUMENTATION:
  Your code emits:
    Logs     → Cloud Logging (structured JSON logs)
    Metrics  → Cloud Monitoring (OpenTelemetry or Ops Agent)
    Traces   → Cloud Trace (OpenTelemetry)

INFRASTRUCTURE METRICS (automatic):
  Compute Engine → Ops Agent → Cloud Monitoring
  GKE            → Cloud Monitoring (auto-enabled)
  Cloud Run      → Cloud Monitoring (auto)
  Cloud SQL      → Cloud Monitoring (auto)

ALERTING PIPELINE:
  Cloud Monitoring Alarm → Notification Channel
  Channels: Email, SMS, Slack, PagerDuty, Pub/Sub, Webhook
  Pub/Sub → Cloud Function → Custom automation

LOG ROUTING ARCHITECTURE:
  All Logs → _Default log bucket (30 days)
           → BigQuery (SQL queries, long retention, analytics)
           → Cloud Storage (cheap long-term archive)
           → Pub/Sub → External SIEM (Splunk, Datadog)
```

```bash
# ─── Log-based alerting ───
# Alert when application has > 10 errors in 5 minutes
gcloud logging metrics create high-error-rate \
  --description="Count of ERROR severity log entries" \
  --log-filter='severity="ERROR" AND resource.type="cloud_run_revision"'

# Then create alerting policy on this metric in Cloud Monitoring

# ─── SLI/SLO configuration ───
# SLO: "99.9% of requests respond in < 200ms"
gcloud alpha monitoring slos create \
  --service=my-service \
  --display-name="Availability SLO 99.9%" \
  --request-based-sli-good-total-ratio \
  --good-service-filter='metric.type="run.googleapis.com/request_count" AND metric.labels.response_code_class!="5xx"' \
  --total-service-filter='metric.type="run.googleapis.com/request_count"' \
  --goal=0.999 \
  --rolling-period-days=30
```

---

# ═══════════════════════════════════════
# PART 4: ADVANCED ARCHITECTURE
# ═══════════════════════════════════════

---

## 35. Highly Available Architecture

### GCP HA Architecture Blueprint

```
INTERNET
    │
    ↓ (any GCP edge — enters Google's network immediately)
Cloud Armor (WAF + DDoS)
    │
Global HTTPS Load Balancer (Premium Tier, anycast IP)
    │
    ├── Backend: us-central1 (Managed Instance Group)
    │     ├── VM in us-central1-a
    │     ├── VM in us-central1-b
    │     └── VM in us-central1-c
    │
    └── Backend: europe-west1 (Managed Instance Group)
          ├── VM in europe-west1-b
          ├── VM in europe-west1-c
          └── VM in europe-west1-d

CLOUD MEMORYSTORE (Redis, HA mode)
  Primary + Replica across zones

CLOUD SQL (Regional — HA)
  Primary + Failover replica
  Automatic failover in < 60 seconds

CLOUD STORAGE (Multi-region)
  Data replicated across regions automatically

GLOBAL LOAD BALANCER ROUTING:
  User in Chicago → us-central1 (closest backend)
  User in London  → europe-west1 (closest backend)
  us-central1 MIG fails → LB routes all to europe-west1 automatically!
```

### Multi-Zone vs Multi-Region

```
MULTI-ZONE (within one region):
  Protects against: Zone failures, hardware failures in one DC
  Latency: Sub-millisecond between zones
  Cost: Minimal (no egress charges within region)
  Setup: Regional MIGs, Regional Cloud SQL, multi-zone GKE
  
MULTI-REGION:
  Protects against: Full region failures (rare but real)
  Also: Serves users in different continents with low latency
  Latency: Add 50-200ms cross-region
  Cost: Egress charges between regions ($0.08/GB)
  Setup: Global LB, cross-region replicas, multi-region Storage
  
RECOMMENDATION:
  All production: Multi-zone mandatory
  Business-critical: Multi-region
  Global user base: Multi-region for latency
```

---

## 36. Multi-Region Architecture

### Global HTTPS LB for Multi-Region

```bash
# The Global HTTPS LB automatically routes to nearest healthy backend
# No DNS changes needed — one anycast IP, Google handles routing

# Create backend services in multiple regions
gcloud compute backend-services create global-app-backend \
  --protocol=HTTP \
  --health-checks=app-health-check \
  --global \
  --load-balancing-scheme=EXTERNAL_MANAGED \
  --locality-lb-policy=ROUND_ROBIN

# Add US backend
gcloud compute backend-services add-backend global-app-backend \
  --instance-group=us-app-mig \
  --instance-group-region=us-central1 \
  --global

# Add EU backend
gcloud compute backend-services add-backend global-app-backend \
  --instance-group=eu-app-mig \
  --instance-group-region=europe-west1 \
  --global

# Add APAC backend
gcloud compute backend-services add-backend global-app-backend \
  --instance-group=apac-app-mig \
  --instance-group-region=asia-southeast1 \
  --global

# Result: Global LB routes each user to nearest healthy region
# If a region is unhealthy, traffic automatically fails over to next region
```

### Spanner — Globally Distributed Database

```bash
# Cloud Spanner = Globally distributed relational database
# Horizontally scalable (not possible with traditional RDBMS)
# 99.999% availability SLA (5 nines!)
# Synchronous replication across regions

# Create multi-region Spanner instance
gcloud spanner instances create global-db \
  --config=nam-eur-asia1 \     # Multi-region config: US + EU + APAC
  --description="Global production database" \
  --processing-units=1000      # 1000 = 1 node equivalent

# Create database
gcloud spanner databases create myapp-db \
  --instance=global-db \
  --ddl="CREATE TABLE users (
    user_id STRING(36) NOT NULL,
    email STRING(255) NOT NULL,
    created_at TIMESTAMP NOT NULL OPTIONS (allow_commit_timestamp=true)
  ) PRIMARY KEY (user_id)"

# Query
gcloud spanner databases execute-sql myapp-db \
  --instance=global-db \
  --sql="SELECT COUNT(*) as total FROM users"
```

---

## 37. Disaster Recovery Strategies

### GCP DR Tiers

```
TIER 1 — BACKUP & RESTORE (Cold Standby)
  RTO: Hours     RPO: Hours
  Cost: $
  Implementation:
    - Cloud SQL automated backups → Cloud Storage
    - VM snapshots scheduled
    - Application images in Artifact Registry
    - Restore procedure documented and tested
  
TIER 2 — PILOT LIGHT
  RTO: 30-60 min    RPO: Minutes
  Cost: $$
  Implementation:
    - Cloud SQL Cross-Region Read Replica (always running)
    - Instance templates ready in DR region
    - Cloud Storage multi-region
    - On failover: promote replica, scale up MIG
  
TIER 3 — WARM STANDBY
  RTO: 5-15 min    RPO: Seconds
  Cost: $$$
  Implementation:
    - Cloud SQL HA + Cross-Region Replica
    - Small MIG running in DR region
    - Global Load Balancer routing
    - Failover = adjust LB weights
  
TIER 4 — ACTIVE-ACTIVE (Multi-Region)
  RTO: Seconds     RPO: Near-zero
  Cost: $$$$
  Implementation:
    - Global HTTPS Load Balancer
    - Full MIG in multiple regions
    - Cloud Spanner or Firestore (multi-region)
    - Cloud Storage multi-region
    - Failover = region health check removes bad region
```

```bash
# Automated DR testing with Cloud Functions
# Runs monthly: takes snapshot → restores to test project → validates

cat > dr_test.py << 'EOF'
import subprocess
from datetime import datetime

def run_dr_test(event, context):
    """Monthly DR test — triggered by Cloud Scheduler"""
    timestamp = datetime.utcnow().strftime("%Y%m%d-%H%M%S")
    
    # 1. Trigger Cloud SQL backup
    subprocess.run([
        "gcloud", "sql", "backups", "create",
        "--instance=prod-postgres",
        "--description=DR-Test-" + timestamp
    ], check=True)
    
    # 2. Restore to test instance
    subprocess.run([
        "gcloud", "sql", "instances", "clone",
        "prod-postgres", "dr-test-" + timestamp,
        "--point-in-time=" + datetime.utcnow().isoformat() + "Z"
    ], check=True)
    
    # 3. Run validation queries
    # 4. Delete test instance
    # 5. Report results to Cloud Monitoring
    
    print(f"DR test {timestamp} completed successfully")
EOF
```

---

## 38. Hybrid & Multi-Cloud Architecture

### Cloud VPN

```bash
# Site-to-Site VPN: Connect on-premises to GCP VPC

# Create VPN Gateway
gcloud compute vpn-gateways create ha-vpn-gateway \
  --network=production-vpc \
  --region=us-central1 \
  --stack-type=IPV4_ONLY

# Create on-premises peer gateway
gcloud compute external-vpn-gateways create on-prem-gateway \
  --interfaces=0=ON_PREM_IP_1,1=ON_PREM_IP_2

# Create VPN tunnels (HA VPN = 2 tunnels)
gcloud compute vpn-tunnels create tunnel-1 \
  --vpn-gateway=ha-vpn-gateway \
  --peer-external-gateway=on-prem-gateway \
  --peer-external-gateway-interface=0 \
  --shared-secret=SHARED_SECRET \
  --ike-version=2 \
  --router=production-router \
  --interface=0 \
  --region=us-central1

gcloud compute vpn-tunnels create tunnel-2 \
  --vpn-gateway=ha-vpn-gateway \
  --peer-external-gateway=on-prem-gateway \
  --peer-external-gateway-interface=1 \
  --shared-secret=SHARED_SECRET \
  --ike-version=2 \
  --router=production-router \
  --interface=1 \
  --region=us-central1

# Configure BGP for dynamic routing
gcloud compute routers add-bgp-peer production-router \
  --peer-name=on-prem-peer-1 \
  --peer-asn=65001 \
  --interface=vpn-interface-1 \
  --region=us-central1
```

### Cloud Interconnect

```
DEDICATED INTERCONNECT:
  Direct physical connection between your colocation and Google
  Speed: 10 Gbps or 100 Gbps per circuit
  Best for: > 10 Gbps bandwidth, lowest latency, private network
  
PARTNER INTERCONNECT:
  Connect via a Google network partner (AT&T, Equinix, etc.)
  Speed: 50 Mbps to 10 Gbps
  Best for: Not co-located with Google, smaller bandwidth needs
  
CROSS-CLOUD INTERCONNECT:
  Direct connection from GCP to AWS or Azure
  Avoids public internet for multi-cloud connectivity
```

### Anthos — Multi-Cloud Management

```bash
# Anthos = Run and manage workloads across GCP, AWS, Azure, on-premises

# Register external cluster (AWS EKS) to Anthos
gcloud container hub memberships register eks-cluster \
  --project=my-project \
  --context=arn:aws:eks:us-east-1:ACCOUNT:cluster/my-eks-cluster \
  --kubeconfig=~/.kube/config \
  --enable-workload-identity

# Register GKE cluster
gcloud container hub memberships register gke-cluster \
  --gke-cluster=us-central1/production-cluster \
  --enable-workload-identity

# List all clusters in Anthos fleet
gcloud container hub memberships list

# Apply config to ALL clusters simultaneously (Config Sync)
gcloud alpha container hub config-management enable

# Now push K8s YAML to Git → Config Sync applies to all clusters!
```

---

## 39. VPC Peering & Shared VPC

### VPC Peering

```bash
# VPC Peering: Connect two VPCs privately
# Key rules:
#   - No overlapping CIDR ranges
#   - NOT transitive (A↔B, B↔C does NOT mean A↔C)
#   - Can be cross-project or cross-organization

# In Project A: create peering
gcloud compute networks peerings create peer-a-to-b \
  --network=network-a \
  --peer-project=project-b \
  --peer-network=network-b \
  --export-custom-routes \     # Share custom routes
  --import-custom-routes

# In Project B: accept peering
gcloud compute networks peerings create peer-b-to-a \
  --network=network-b \
  --peer-project=project-a \
  --peer-network=network-a \
  --export-custom-routes \
  --import-custom-routes

# Verify peering status
gcloud compute networks peerings list --network=network-a
# Status should be: ACTIVE
```

### Shared VPC

```bash
# Shared VPC = One VPC shared across multiple projects
# HOST PROJECT: Owns the VPC
# SERVICE PROJECTS: Use subnets from host project

# This is the ENTERPRISE pattern for multi-project orgs:
#   - Centralized network management (networking team controls host)
#   - Each team has their own project (billing isolation)
#   - All teams share the same secure network

# ─── Setup Shared VPC ───
# 1. Enable Shared VPC on host project
gcloud compute shared-vpc enable HOST_PROJECT_ID

# 2. Attach service projects
gcloud compute shared-vpc associated-projects add SERVICE_PROJECT_ID \
  --host-project=HOST_PROJECT_ID

# 3. Grant IAM to service project users on host project subnets
gcloud compute networks subnets add-iam-policy-binding us-subnet \
  --region=us-central1 \
  --member="serviceAccount:compute@SERVICE_PROJECT.iam.gserviceaccount.com" \
  --role="roles/compute.networkUser" \
  --project=HOST_PROJECT_ID

# 4. Service project creates VM using host project subnet
gcloud compute instances create my-vm \
  --zone=us-central1-a \
  --subnet=projects/HOST_PROJECT/regions/us-central1/subnetworks/us-subnet \
  --project=SERVICE_PROJECT_ID
```

---

## 40. Performance Optimization

### Cloud Memorystore (Redis/Memcached)

```bash
# Create Redis instance (Memorystore)
gcloud redis instances create prod-redis \
  --size=5 \              # 5 GB
  --region=us-central1 \
  --tier=STANDARD_HA \   # HA with failover replica
  --redis-version=redis_7_0 \
  --connect-mode=PRIVATE_SERVICE_ACCESS

# Get Redis IP
gcloud redis instances describe prod-redis \
  --region=us-central1 \
  --format="value(host)"

# ─── Caching patterns in Python ───
python3 << 'EOF'
import redis
import json
from functools import wraps

redis_client = redis.Redis(host=REDIS_HOST, port=6379, decode_responses=True)

def cache(ttl=300):
    """Cache decorator — wraps any function with Redis caching"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            cache_key = f"{func.__name__}:{hash(str(args)+str(kwargs))}"
            
            # Try cache first
            cached = redis_client.get(cache_key)
            if cached:
                return json.loads(cached)
            
            # Cache miss — call function
            result = func(*args, **kwargs)
            redis_client.setex(cache_key, ttl, json.dumps(result))
            return result
        return wrapper
    return decorator

@cache(ttl=60)
def get_user_profile(user_id: str):
    # This DB query only runs on cache miss
    return db.query(f"SELECT * FROM users WHERE id = '{user_id}'")
EOF
```

### Cloud Bigtable (High-Performance NoSQL)

```bash
# Bigtable: NoSQL for high-throughput, low-latency workloads
# Scales to petabytes, millions of rows/second
# Use for: Time-series, IoT, click-stream, financial data

gcloud bigtable instances create prod-bigtable \
  --cluster=prod-cluster \
  --cluster-zone=us-central1-a \
  --cluster-num-nodes=3 \
  --display-name="Production Bigtable"

gcloud bigtable tables create events \
  --instance=prod-bigtable \
  --column-families=metadata,payload
```

---

## 41. Cost Optimization

### GCP Cost Optimization Strategies

```bash
# ─── 1. RIGHT-SIZING with Recommender ───
# GCP automatically analyzes usage and recommends rightsizing

gcloud recommender recommendations list \
  --project=PROJECT \
  --location=us-central1-a \
  --recommender=google.compute.instance.MachineTypeRecommender \
  --format="table(name,description,stateInfo.state,primaryImpact.costProjection.cost)"

# ─── 2. COMMITTED USE DISCOUNTS ───
# 1-year: 37% off, 3-year: 55% off

gcloud compute commitments create prod-commitment \
  --plan=TWELVE_MONTH \
  --region=us-central1 \
  --resources=vcpu=20,memory=80GB

# ─── 3. SPOT VMs for batch workloads ───
gcloud compute instances create batch-processor \
  --provisioning-model=SPOT \    # 60-91% savings!
  --instance-termination-action=STOP \
  --machine-type=c2-standard-30 \
  --zone=us-central1-a

# ─── 4. Idle resource detection ───
# Find VMs with < 5% CPU average (likely idle)
gcloud monitoring time-series list \
  'metric.type="compute.googleapis.com/instance/cpu/utilization"
   resource.type="gce_instance"' \
  --format="json" | python3 -c "
import json, sys
data = json.load(sys.stdin)
for ts in data:
    avg = sum(p['value']['doubleValue'] for p in ts['points']) / len(ts['points'])
    if avg < 0.05:
        print(f'Low CPU: {ts[\"resource\"][\"labels\"][\"instance_id\"]} — avg {avg:.1%}')
"

# ─── 5. Budget alerts ───
gcloud billing budgets create \
  --billing-account=BILLING_ACCOUNT \
  --display-name="Production Budget Alert" \
  --budget-amount=1000USD \
  --threshold-rule=percent=0.5 \    # Alert at 50%
  --threshold-rule=percent=0.9 \    # Alert at 90%
  --threshold-rule=percent=1.0 \    # Alert at 100%
  --threshold-rule=percent=1.1     # Alert at 110% overage!

# ─── 6. Storage optimization ───
# Check bucket costs and usage
gcloud storage ls --long --readable-sizes gs://my-large-bucket

# Move old data to cheaper classes
gcloud storage objects update gs://my-bucket/**/*.log \
  --storage-class=ARCHIVE
```

---

## 42. Scalable Distributed Systems

### Pub/Sub — Event-Driven Architecture

```bash
# Pub/Sub = Managed messaging service for async, event-driven systems
# Guaranteed delivery, at-least-once semantics
# Scales to millions of messages/second

# ─── Create topic and subscription ───
gcloud pubsub topics create order-events
gcloud pubsub topics create order-events-dlq   # Dead letter queue

gcloud pubsub subscriptions create order-processor \
  --topic=order-events \
  --ack-deadline=60 \                 # 60 seconds to process
  --max-delivery-attempts=5 \         # Retry 5 times before DLQ
  --dead-letter-topic=order-events-dlq \
  --message-retention-duration=7d     # Keep unacked messages 7 days

# ─── Push subscription (Cloud Run endpoint) ───
gcloud pubsub subscriptions create order-webhook \
  --topic=order-events \
  --push-endpoint=https://my-api.run.app/webhooks/orders \
  --push-auth-service-account=pubsub-sa@PROJECT.iam.gserviceaccount.com \
  --ack-deadline=60

# ─── Publish a message ───
gcloud pubsub topics publish order-events \
  --message='{"order_id":"12345","amount":99.99,"currency":"USD"}' \
  --attribute=version=1,source=web-app

# ─── Pull messages (for testing) ───
gcloud pubsub subscriptions pull order-processor \
  --limit=10 \
  --auto-ack

# ─── Python publisher ───
python3 << 'EOF'
from google.cloud import pubsub_v1
import json

publisher = pubsub_v1.PublisherClient()
topic_path = publisher.topic_path("my-project", "order-events")

def publish_order_event(order: dict):
    data = json.dumps(order).encode("utf-8")
    future = publisher.publish(
        topic_path,
        data,
        version="1",
        source="api",
        order_id=order["id"]
    )
    message_id = future.result(timeout=10)
    print(f"Published message {message_id}")

publish_order_event({"id": "12345", "amount": 99.99, "status": "placed"})
EOF
```

### Cloud Tasks — Task Queue

```bash
# Cloud Tasks: Manage and dispatch asynchronous work
# Difference from Pub/Sub:
#   Pub/Sub: fire-and-forget, fan-out to multiple subscribers
#   Cloud Tasks: explicit task queue, target a specific endpoint

# Create queue
gcloud tasks queues create email-queue \
  --location=us-central1 \
  --max-dispatches-per-second=100 \
  --max-concurrent-dispatches=1000 \
  --max-attempts=5 \
  --min-backoff=1s \
  --max-backoff=300s

# Add task to queue
gcloud tasks create-http-task \
  --queue=email-queue \
  --url=https://my-api.run.app/tasks/send-email \
  --method=POST \
  --body='{"to":"user@example.com","template":"welcome"}' \
  --location=us-central1 \
  --schedule-time=2024-01-16T10:00:00Z   # Schedule for specific time
```

---

# ═══════════════════════════════════════
# PART 5: EXPERT / SPECIALTY LEVEL
# ═══════════════════════════════════════

---

## 43. Advanced GCP Networking

### Cloud Interconnect Architecture

```
ENTERPRISE NETWORK DESIGN:

HQ Data Center                        Google Cloud
─────────────                         ─────────────
                Dedicated Interconnect
Router BGP ←──────────────────────→ Cloud Router BGP
10 Gbps circuit                      (Dynamic routing)

On-prem CIDRs: 192.168.0.0/16        VPC CIDRs: 10.0.0.0/8
                                      Advertised via BGP

REDUNDANCY:
  2× Interconnect circuits (different facilities)
  Cloud Router with MED for primary/backup failover
  VPN as backup if both circuits fail
```

```bash
# Create Dedicated Interconnect (after physical provisioning)
gcloud compute interconnects attachments dedicated create us-interconnect-1 \
  --region=us-central1 \
  --router=production-router \
  --interconnect=my-interconnect \
  --candidate-subnets=169.254.0.0/29 \
  --vlan=100 \
  --bandwidth=10G \
  --description="Primary Interconnect"

# Configure BGP session on Cloud Router
gcloud compute routers update-bgp-peer production-router \
  --peer-name=onprem-bgp \
  --peer-asn=65001 \
  --peer-ip-address=169.254.0.2 \
  --region=us-central1

# Advertise custom routes (GCP → on-premises)
gcloud compute routers update production-router \
  --advertisement-mode=CUSTOM \
  --set-advertisement-ranges=10.0.0.0/8:"All GCP subnets" \
  --region=us-central1

# Check BGP sessions
gcloud compute routers get-status production-router \
  --region=us-central1 \
  --format="json(result.bgpPeerStatus)"
```

### Network Intelligence Center

```bash
# Connectivity Tests: Debug why VMs can't talk to each other
gcloud network-management connectivity-tests create debug-test \
  --source-instance=projects/PROJECT/zones/us-central1-a/instances/vm1 \
  --destination-instance=projects/PROJECT/zones/us-central1-a/instances/vm2 \
  --destination-port=5432 \
  --protocol=TCP

gcloud network-management connectivity-tests describe debug-test \
  --format="json(reachabilityDetails)"
# Shows exactly WHERE and WHY traffic is blocked!

# Network Topology — visualize your entire network
# Console: Network Intelligence Center → Network Topology
# See: VPCs, subnets, firewalls, LBs, peerings, all in one diagram

# Firewall Insights — find unused/redundant firewall rules
gcloud compute firewall-rules list \
  --format="table(name,network,direction,priority,disabled)"
# Console: Network Intelligence Center → Firewall Insights
```

---

## 44. Security Architecture & Compliance

### Security Command Center (SCC)

```bash
# SCC = Unified security management platform
# Aggregates: findings from all security services
# Sources: Container threats, IAM anomalies, storage misconfigs

# Enable SCC
gcloud scc settings update-org-settings \
  --organization=ORG_ID \
  --enable-asset-discovery

# List high-severity findings
gcloud scc findings list \
  --organization=ORG_ID \
  --filter="state=ACTIVE AND severity=HIGH" \
  --format="table(name,category,resourceName,eventTime)"

# Export findings to BigQuery for analysis
gcloud scc findings list \
  --organization=ORG_ID \
  --format=json > findings.json

# Common SCC findings:
# PUBLIC_BUCKET_ACL: Cloud Storage bucket is publicly accessible
# OPEN_FIREWALL: Firewall rule allows all ingress traffic
# SQL_NO_ROOT_PASSWORD: Cloud SQL has no root password
# LEGACY_AUTHORIZATION: GKE using ABAC (legacy auth)
# MFA_NOT_REQUIRED: Users don't have MFA enforced
```

### Binary Authorization (Container Security)

```bash
# Binary Authorization: Only allow cryptographically signed containers to run
# Prevents: Unverified, tampered, or unknown containers from deploying

# Enable Binary Authorization
gcloud services enable binaryauthorization.googleapis.com

# Create attestor (who can sign images)
gcloud container binauthz attestors create qa-attestor \
  --attestation-authority-note=my-note \
  --attestation-authority-note-project=PROJECT

# Create policy (only allow signed images)
cat > binauthz-policy.yaml << 'EOF'
globalPolicyEvaluationMode: ENABLE
defaultAdmissionRule:
  evaluationMode: REQUIRE_ATTESTATION
  enforcementMode: ENFORCED_BLOCK_AND_AUDIT_LOG
  requireAttestationsBy:
    - projects/PROJECT/attestors/qa-attestor
EOF

gcloud container binauthz policy import binauthz-policy.yaml

# In CI/CD: sign image after QA passes
gcloud container binauthz attestations create \
  --attestor=qa-attestor \
  --attestor-project=PROJECT \
  --artifact-url=us-central1-docker.pkg.dev/PROJECT/repo/app@sha256:ABC123 \
  --keyversion=projects/PROJECT/locations/global/keyRings/binauthz/cryptoKeys/qa-key/cryptoKeyVersions/1
```

### VPC Service Controls

```bash
# VPC Service Controls: Create a security perimeter around GCP services
# Prevent data exfiltration even if credentials are compromised

# Create access policy (org-level)
POLICY=$(gcloud access-context-manager policies create \
  --organization=ORG_ID \
  --title="Production Security Perimeter" \
  --format="value(name)")

# Create service perimeter
gcloud access-context-manager perimeters create prod-perimeter \
  --policy=$POLICY \
  --title="Production Perimeter" \
  --resources=projects/PROJECT_NUMBER \
  --restricted-services=bigquery.googleapis.com,storage.googleapis.com \
  --ingress-policies=@ingress.yaml \
  --egress-policies=@egress.yaml

# Now:
# BigQuery and Cloud Storage are only accessible FROM within the perimeter
# Even if a service account key is stolen, attacker can't access data from outside
```

---

## 45. IAM Policy Deep Dive

### IAM Conditions

```bash
# Add conditions to IAM bindings (attribute-based access control)

# Allow access ONLY from corporate IP range
gcloud projects add-iam-policy-binding PROJECT \
  --member="user:alice@company.com" \
  --role="roles/bigquery.dataViewer" \
  --condition='title=CorporateNetwork,description=Only from corporate,expression=request.auth.claims.ip=="192.168.0.0/16"'

# Allow access ONLY during business hours
gcloud resource-manager folders add-iam-policy-binding FOLDER_ID \
  --member="group:developers@company.com" \
  --role="roles/editor" \
  --condition='title=BusinessHours,expression=request.time.getHours("America/Chicago")>=9 && request.time.getHours("America/Chicago")<=17'

# Grant temporary access (expires at specific time)
gcloud projects add-iam-policy-binding PROJECT \
  --member="user:contractor@external.com" \
  --role="roles/viewer" \
  --condition='title=TemporaryAccess,expression=request.time < timestamp("2024-04-01T00:00:00Z")'
```

### Organization Policies

```bash
# Organization Policies: Guardrails for entire organization
# Even OWNERS cannot violate these!

# List available constraints
gcloud org-policies list-available-restrictions \
  --organization=ORG_ID | grep constraints/

# Enforce: No external IP addresses on Compute Engine
gcloud org-policies set-policy - << 'EOF'
name: organizations/ORG_ID/policies/compute.vmExternalIpAccess
spec:
  rules:
  - enforce: true
EOF

# Enforce: Only specific regions allowed
gcloud org-policies set-policy - << 'EOF'
name: organizations/ORG_ID/policies/gcp.resourceLocations
spec:
  rules:
  - values:
      allowedValues:
        - us-central1
        - us-east1
        - europe-west1
EOF

# Enforce: Disable service account key creation
gcloud org-policies set-policy - << 'EOF'
name: organizations/ORG_ID/policies/iam.disableServiceAccountKeyCreation
spec:
  rules:
  - enforce: true
EOF

# Check effective policy on a project
gcloud org-policies describe compute.vmExternalIpAccess \
  --project=PROJECT
```

---

## 46. Encryption & Cloud KMS

### Cloud KMS Architecture

```
ENCRYPTION HIERARCHY:

Google's Root KMS (hardware-backed, managed by Google)
    ↓ encrypts
Customer-Managed Encryption Keys (CMEK) in Cloud KMS
    ↓ encrypts (envelope encryption!)
Data Encryption Keys (DEK) — generated per-object
    ↓ encrypts
Your actual data in GCS, BigQuery, Cloud SQL, etc.

THREE LEVELS OF KEY MANAGEMENT:
  1. Google-Managed Keys:  Default, no cost, no control
     → "Google manages keys, you trust Google"
  
  2. Customer-Managed (CMEK): YOU control in Cloud KMS
     → Rotate, disable, destroy → your data becomes inaccessible!
     → Audit every key use in Cloud Audit Logs
  
  3. Customer-Supplied (CSEK): You provide the key with each request
     → Google never stores the key
     → If you lose the key, data is GONE forever
     → Maximum control, maximum responsibility
  
  4. Cloud HSM: Keys backed by FIPS 140-2 Level 3 hardware
     → Key material never leaves the HSM
     → Required for some compliance frameworks
```

```bash
# ─── Create KMS key ring and key ───
gcloud kms keyrings create production-keys \
  --location=us-central1

gcloud kms keys create data-encryption-key \
  --keyring=production-keys \
  --location=us-central1 \
  --purpose=encryption \
  --rotation-period=90d \          # Rotate every 90 days
  --next-rotation-time=2024-04-01T00:00:00Z

# ─── Enable automatic rotation ───
gcloud kms keys update data-encryption-key \
  --keyring=production-keys \
  --location=us-central1 \
  --rotation-period=90d

# ─── Encrypt/decrypt directly with KMS ───
# Encrypt a file
gcloud kms encrypt \
  --key=data-encryption-key \
  --keyring=production-keys \
  --location=us-central1 \
  --plaintext-file=sensitive.txt \
  --ciphertext-file=sensitive.txt.enc

# Decrypt
gcloud kms decrypt \
  --key=data-encryption-key \
  --keyring=production-keys \
  --location=us-central1 \
  --ciphertext-file=sensitive.txt.enc \
  --plaintext-file=decrypted.txt

# ─── CMEK for Cloud Storage ───
gcloud storage buckets create gs://encrypted-bucket \
  --default-encryption-key=projects/PROJECT/locations/us-central1/keyRings/production-keys/cryptoKeys/data-encryption-key

# ─── CMEK for Cloud SQL ───
gcloud sql instances create secure-db \
  --disk-encryption-key=projects/PROJECT/locations/us-central1/keyRings/production-keys/cryptoKeys/data-encryption-key \
  --database-version=POSTGRES_15 \
  --cpu=4 --memory=16GB \
  --region=us-central1

# ─── Key access control (revoke to instantly lock data!) ───
# Disable key = all CMEK-encrypted data inaccessible immediately
gcloud kms keys versions disable 1 \
  --key=data-encryption-key \
  --keyring=production-keys \
  --location=us-central1

# Re-enable
gcloud kms keys versions enable 1 \
  --key=data-encryption-key \
  --keyring=production-keys \
  --location=us-central1

# Grant encryption/decryption permission to service
gcloud kms keys add-iam-policy-binding data-encryption-key \
  --keyring=production-keys \
  --location=us-central1 \
  --member="serviceAccount:service-ACCOUNT@cloud-sql.iam.gserviceaccount.com" \
  --role="roles/cloudkms.cryptoKeyEncrypterDecrypter"
```

---

## 47. Production Incident Debugging

### Incident Runbook

```
SEVERITY CLASSIFICATION:
  P1 (Critical):  Complete service outage, revenue impact
                  → Immediate response, all hands
  P2 (High):      Significant degradation, many users affected
                  → < 15 min response, senior engineers
  P3 (Medium):    Partial impact, workaround available
                  → < 1 hour response
  P4 (Low):       Minor issue, cosmetic
                  → Scheduled fix

INCIDENT RESPONSE PROCEDURE:
  1. ALERT fires → On-call engineer receives PagerDuty/Cloud Monitoring alert
  2. Assess impact in < 5 minutes
     - What service? What % of users? What error?
  3. Declare incident, open war room (Slack channel, Google Meet)
  4. MITIGATE first (rollback, traffic shift, circuit breaker)
  5. Investigate root cause WHILE mitigated
  6. Fix → Deploy → Verify
  7. Blameless post-mortem within 48 hours
```

### Production Debugging Toolkit

```bash
#!/bin/bash
# production_debug.sh — Quick health check

PROJECT="my-production-project"
REGION="us-central1"

echo "=== GCP Production Health Check ==="
echo "Time: $(date -u)"

echo -e "\n--- Compute Engine Instances ---"
gcloud compute instances list \
  --filter="status!=RUNNING" \
  --format="table(name,zone,status,lastStopTimestamp)"

echo -e "\n--- MIG Status ---"
gcloud compute instance-groups managed list \
  --filter="region:$REGION" \
  --format="table(name,location,targetSize,currentActions.creating,currentActions.deleting)"

echo -e "\n--- Cloud Run Services ---"
gcloud run services list \
  --region=$REGION \
  --format="table(name,status.conditions[0].type,status.traffic[0].percent)"

echo -e "\n--- GKE Nodes ---"
gcloud container clusters list \
  --format="table(name,location,currentNodeCount,status)"

echo -e "\n--- Cloud SQL ---"
gcloud sql instances list \
  --format="table(name,state,databaseVersion)"

echo -e "\n--- Recent ERROR logs ---"
gcloud logging read \
  "severity=ERROR AND timestamp>=\"$(date -u -d '15 minutes ago' +%Y-%m-%dT%H:%M:%S)Z\"" \
  --limit=20 \
  --format="table(timestamp,resource.type,textPayload)" \
  2>/dev/null | head -30

echo -e "\n--- Load Balancer Backend Health ---"
gcloud compute backend-services get-health global-app-backend \
  --global \
  --format="table(backend,status.healthStatus[0].healthState)"
```

### Common Issues and Solutions

```bash
# ISSUE 1: VM can't reach external internet
# Diagnose:
gcloud compute ssh my-vm -- "curl -v https://google.com 2>&1 | head -5"
# Check: Does subnet have Cloud NAT configured?
gcloud compute routers nats list --router=production-router --region=us-central1
# Fix: Create Cloud NAT if missing

# ISSUE 2: VMs failing health checks
# Check instance serial console (boot logs)
gcloud compute instances get-serial-port-output my-vm --zone=us-central1-a
# Check what's listening on port
gcloud compute ssh my-vm -- "sudo ss -tlnp | grep :80"
# Check startup script ran
gcloud compute ssh my-vm -- "sudo journalctl -u google-startup-scripts"

# ISSUE 3: GKE pods in CrashLoopBackOff
kubectl describe pod PODNAME -n production
kubectl logs PODNAME -n production --previous   # Last container's logs
kubectl exec -it PODNAME -n production -- /bin/sh  # Shell into container

# ISSUE 4: Cloud SQL connection refused
# Check if Cloud SQL Proxy is running
# Check IAM: does SA have roles/cloudsql.client?
gcloud projects get-iam-policy PROJECT \
  --flatten="bindings[].members" \
  --filter="bindings.members:my-sa@PROJECT.iam.gserviceaccount.com"

# ISSUE 5: High latency
# Check Cloud Trace for slow spans
gcloud trace sinks list
# Check Cloud Monitoring for latency metrics
gcloud monitoring time-series list \
  'metric.type="run.googleapis.com/request_latencies"' \
  --format=json | python3 -c "
import json, sys
data = json.load(sys.stdin)
for ts in data:
    p99 = [p for p in ts.get('points', []) if p]
    if p99:
        print(f'P99 latency: {p99[0]}')
"
```

---

## 48. Observability at Scale

### SLI/SLO/SLA Framework

```
SLI (Service Level Indicator):
  The measurement you care about
  Examples:
    Availability: % of requests returning 2xx
    Latency: % of requests completing in < 200ms
    Throughput: requests/second processed
    Error rate: % of requests returning 5xx

SLO (Service Level Objective):
  Your internal target for the SLI
  Examples:
    Availability SLO: 99.9% of requests return 2xx
    Latency SLO: 95% of requests complete in < 200ms
    Error rate SLO: < 0.1% of requests return 5xx

SLA (Service Level Agreement):
  Contractual commitment to customers
  Usually lower than SLO (buffer for incidents)
  Breach = financial penalty

ERROR BUDGET:
  Error Budget = 1 - SLO
  If SLO = 99.9%, Error Budget = 0.1%
  In 30 days: 0.1% × 30 × 24 × 60 = 43.2 minutes of allowed downtime
  
  ERROR BUDGET POLICY:
    Budget healthy (> 50%): Ship fast, take risks
    Budget warning (< 50%): Slow down, prioritize reliability
    Budget exhausted: Feature freeze, fix reliability only
```

```bash
# Create SLO in Cloud Monitoring
gcloud alpha monitoring slos create \
  --service=my-api \
  --display-name="API Availability SLO 99.9%" \
  --request-based \
  --good-total-ratio \
  --good-service-filter='
    metric.type="run.googleapis.com/request_count"
    resource.type="cloud_run_revision"
    metric.labels.response_code_class!="5xx"
  ' \
  --total-service-filter='
    metric.type="run.googleapis.com/request_count"
    resource.type="cloud_run_revision"
  ' \
  --goal=0.999 \
  --rolling-period=30d

# Create error budget burn rate alert
# Alert if burning error budget 14× faster than allowed
gcloud alpha monitoring policies create \
  --display-name="Error Budget Fast Burn Alert" \
  --condition-slo-burn-rate \
  --slo=SLO_ID \
  --service=my-api \
  --burn-rate-threshold=14 \
  --condition-window-period=3600s \    # 1 hour window
  --notification-channels=CHANNEL_ID
```

---

## 49. Large-Scale Troubleshooting

### Handling GCP Region Outages

```
REGION OUTAGE RESPONSE:

1. DETECT (Cloud Monitoring alerts fire)
   - Your uptime checks fail
   - LB health checks show backend unhealthy
   - Error rate spikes on your SLO dashboards

2. VERIFY (Is it a GCP outage or your code?)
   - Check: status.cloud.google.com
   - Check: Multiple independent monitors
   - Check: Users reporting issues globally or locally?

3. FAILOVER (if multi-region setup)
   GLOBAL LB: Automatic — LB detects unhealthy backends and routes elsewhere
   CLOUD SQL: Promote read replica in DR region manually
   CLOUD RUN: Add DR region service, update LB backend
   GKE: Add traffic to DR cluster via LB config

4. COMMUNICATE
   - Update your status page
   - Notify stakeholders
   - Set next update time

5. MONITOR
   - Watch error rates in DR region
   - Ensure DR region can handle full traffic
   - Monitor Cloud SQL replica lag

6. RESTORE & FAILBACK
   - Wait for GCP to resolve
   - Validate primary region is healthy
   - Gradual failback (10% → 25% → 50% → 100%)
   - Resync data if needed
```

### Large-Scale Performance Troubleshooting

```bash
# STEP 1: Identify WHERE the latency is
# Cloud Trace shows service map with latency per span
gcloud trace sinks list

# STEP 2: Is it the database?
# Cloud SQL Query Insights
gcloud sql instances patch prod-postgres \
  --insights-config-query-insights-enabled \
  --insights-config-query-string-length=2048 \
  --insights-config-record-application-tags \
  --insights-config-record-client-address

# STEP 3: Is it network latency?
gcloud network-management connectivity-tests create latency-test \
  --source-instance=projects/PROJECT/zones/us-central1-a/instances/app-vm \
  --destination-ip=DB_PRIVATE_IP \
  --destination-port=5432 \
  --protocol=TCP

# STEP 4: Is it CPU/Memory pressure?
gcloud monitoring time-series list \
  'metric.type="compute.googleapis.com/instance/cpu/utilization"
   resource.labels.instance_name="my-app-vm"' \
  --format=json | python3 -c "
import json, sys
data = json.load(sys.stdin)
for ts in data:
    for point in ts['points'][:5]:
        print(f'CPU: {point[\"value\"][\"doubleValue\"]:.1%}')
"

# STEP 5: Profile your application
# Cloud Profiler — continuous CPU/heap profiling
# Add to Python app:
# import googlecloudprofiler
# googlecloudprofiler.start(service='my-api', version='1.2.3')
```

---

## 50. Data Engineering & ML on GCP

### BigQuery — Serverless Data Warehouse

```bash
# BigQuery: Analyze petabytes in seconds, no infrastructure

# ─── Create dataset and table ───
bq mk --dataset \
  --location=US \
  --description="Production analytics" \
  my_project:analytics

bq mk --table \
  my_project:analytics.events \
  timestamp:TIMESTAMP,user_id:STRING,event_type:STRING,properties:JSON

# ─── Load data ───
bq load \
  --source_format=NEWLINE_DELIMITED_JSON \
  my_project:analytics.events \
  gs://my-data-bucket/events/*.jsonl

# ─── Query (SQL) ───
bq query --use_legacy_sql=false '
  SELECT
    event_type,
    COUNT(*) as count,
    COUNT(DISTINCT user_id) as unique_users,
    DATE(timestamp) as date
  FROM `my_project.analytics.events`
  WHERE timestamp >= TIMESTAMP_SUB(CURRENT_TIMESTAMP(), INTERVAL 7 DAY)
  GROUP BY 1, 4
  ORDER BY date DESC, count DESC
'

# ─── Partitioned tables (performance + cost) ───
bq mk --table \
  --time_partitioning_field=timestamp \
  --time_partitioning_type=DAY \
  --clustering_fields=event_type,user_id \
  my_project:analytics.events_partitioned \
  timestamp:TIMESTAMP,user_id:STRING,event_type:STRING

# ─── BigQuery ML (train ML models in SQL!) ───
bq query --use_legacy_sql=false '
  CREATE OR REPLACE MODEL my_project.analytics.churn_model
  OPTIONS(
    model_type="logistic_reg",
    auto_class_weights=True,
    data_split_method="AUTO_SPLIT"
  ) AS
  SELECT
    days_since_last_login,
    total_purchases,
    avg_session_duration,
    IF(churned_within_30_days, 1, 0) AS label
  FROM my_project.analytics.user_features
  WHERE snapshot_date >= "2023-01-01"
'

# Evaluate model
bq query --use_legacy_sql=false '
  SELECT * FROM ML.EVALUATE(MODEL my_project.analytics.churn_model)
'
```

### Vertex AI — Unified ML Platform

```bash
# Vertex AI: End-to-end ML on GCP

# ─── Enable APIs ───
gcloud services enable aiplatform.googleapis.com

# ─── Custom training job ───
gcloud ai custom-jobs create \
  --region=us-central1 \
  --display-name=my-training-job \
  --worker-pool-spec=machine-type=a2-highgpu-1g,replica-count=1,container-image-uri=gcr.io/cloud-aiplatform/training/tf-gpu.2-12:latest,local-package-path=./trainer,python-module=trainer.task \
  --args=--epochs=10,--batch-size=32,--gcs-output-dir=gs://my-bucket/model

# ─── Deploy model endpoint ───
gcloud ai models upload \
  --region=us-central1 \
  --display-name=my-model \
  --container-image-uri=us-docker.pkg.dev/vertex-ai/prediction/sklearn-cpu.1-0:latest \
  --artifact-uri=gs://my-bucket/model/

gcloud ai endpoints create \
  --region=us-central1 \
  --display-name=my-model-endpoint

gcloud ai endpoints deploy-model ENDPOINT_ID \
  --region=us-central1 \
  --model=MODEL_ID \
  --display-name=my-model \
  --machine-type=n1-standard-4 \
  --min-replica-count=1 \
  --max-replica-count=10 \
  --traffic-split=0=100

# ─── Online prediction ───
gcloud ai endpoints predict ENDPOINT_ID \
  --region=us-central1 \
  --json-request='{"instances": [[1.2, 3.4, 5.6, 7.8]]}'
```

---

# APPENDIX

---

<a id="best-practices"></a>
## GCP Architecture Best Practices Checklist

```
IDENTITY & ACCESS:
□ Use Workload Identity for GKE pods (no key files)
□ Service accounts follow least privilege
□ Organization Policies enforce guardrails
□ No broad basic roles (owner/editor) in production
□ All users have 2-Step Verification enforced
□ Cloud Identity enforced for all org users

NETWORK:
□ Custom VPC (not default) for all production
□ Private Google Access enabled on all subnets
□ VPC Flow Logs enabled for security monitoring
□ Cloud NAT for private VM outbound access
□ Cloud Armor on all public-facing load balancers
□ No VMs with public IPs (use Cloud NAT + IAP/SSH)

COMPUTE:
□ Regional MIGs (multi-zone) for all production
□ Health checks configured with appropriate thresholds
□ Rolling update policy: maxUnavailable=0
□ Golden image strategy for VM images
□ Ops Agent installed on all VMs

STORAGE:
□ Uniform bucket-level access (no per-object ACLs)
□ Object versioning on critical buckets
□ Lifecycle policies to optimize costs
□ CMEK for sensitive data
□ No public buckets unless intentional (and CDN-cached)

DATABASE:
□ Cloud SQL: REGIONAL availability type (HA)
□ Automated backups + PITR enabled
□ Private IP only (no public IP)
□ Cloud SQL Auth Proxy for connections
□ Strong passwords in Secret Manager (not code)

OPERATIONS:
□ Cloud Monitoring dashboards for all services
□ Alerting policies for availability + latency + errors
□ Cloud Logging retention configured
□ Log exports to BigQuery for analytics
□ Budget alerts configured

SECURITY:
□ Security Command Center enabled
□ Cloud Audit Logs enabled (admin + data access)
□ Binary Authorization for GKE (production)
□ VPC Service Controls for sensitive projects
□ Secret Manager for all secrets (no hardcoded credentials)
```

---

<a id="exam-tips"></a>
## GCP Certification Exam Tips

### Cloud Digital Leader (CDL)
```
Focus: Business value, not technical details
  - Cloud computing fundamentals
  - GCP product overview (what each does, not how)
  - Transformational benefits (speed, agility, cost)
  - Basic security (shared responsibility)
  - Sustainability (Google's carbon footprint commitments)

Tips:
  - Think "business executive" perspective
  - Focus on WHY companies use cloud, not HOW
  - Know GCP's main product categories
  - 60 questions, 2 hours
```

### Associate Cloud Engineer (ACE)
```
Focus: Implementation — actually building on GCP
  - gcloud CLI proficiency (know common commands)
  - Compute Engine (instance types, pricing, MIGs)
  - GKE (basic Kubernetes operations)
  - Cloud Storage, Cloud SQL, VPC
  - IAM (roles, service accounts)
  - Cloud Monitoring and Logging

Tips:
  - Practice in the console AND CLI (exam tests both)
  - Hands-on labs on Cloud Skills Boost
  - 50 questions, 2 hours
  - Scenario-based: "A company needs X, what do you do?"
```

### Professional Cloud Architect (PCA)
```
Focus: Design decisions, trade-offs, best practices
  - High availability and disaster recovery patterns
  - Choosing the right GCP service for each use case
  - Cost vs performance vs availability trade-offs
  - Security architecture
  - Data architecture (when to use Bigtable vs BigQuery vs Spanner)

Tips:
  - No single "right" answer — pick the BEST option
  - Know the WHEN and WHY of each service
  - Practice design scenarios: "Design an architecture for..."
  - Google's case studies (Dress4Win, TerramEarth, Helicopter)
  - 60 questions, 2 hours
```

---

<a id="interview-questions"></a>
## GCP Interview Questions Bank

### Junior Level

**Q1:** Explain the difference between Cloud Run and Cloud Functions.
```
Cloud Functions: Single-function event handler, simpler, per-invocation billing,
                 language-specific runtimes, max 60 min timeout
Cloud Run:       Containerized HTTP service, any language, any framework,
                 concurrency support (multiple requests per container),
                 better for complex apps or high traffic
Both:            Serverless, auto-scale to zero, pay-per-use
Choose Cloud Run when: You have existing Docker containers, need more control,
                        or handle HTTP traffic with concurrency
```

**Q2:** How does GCP's Global HTTPS Load Balancer differ from regional load balancers?
```
Global: Single anycast IP, routes to nearest healthy backend worldwide,
        uses Google's private backbone, Cloud CDN integration,
        single SSL certificate for all regions
Regional: Serves one region, IP per region, can do internal (private) LB,
          lower cost for single-region apps
Use Global for: Public internet-facing apps with global users
Use Regional Internal for: Microservice-to-microservice within VPC
```

### Senior Level

**Q3:** Design a DR strategy for a banking application with RTO < 5 min and RPO < 30 seconds.
```
Tier 4: Active-Active Multi-Region
- Cloud Spanner (multi-region config: synchronous, 0 data loss)
- Global HTTPS LB routing to US and EU simultaneously
- Cloud Run or GKE regional clusters in both regions
- Cloud Storage multi-region for shared files
- Cloud CDN caching at edge
- Health check-based automatic failover (GCP LB handles this)

RTO: < 30 seconds (LB detects unhealthy region and routes around it)
RPO: Near-zero (Spanner synchronous multi-region replication)
Cost: ~3x single region
Complexity: High — requires careful testing and runbooks
```

**Q4:** How do you implement zero-trust security for a GKE cluster?
```
1. Workload Identity: No service account keys, pod-level IAM
2. Binary Authorization: Only signed images run
3. Private cluster: No public nodes, master accessible via authorized networks
4. VPC Service Controls: Perimeter around the cluster and its GCP dependencies
5. NetworkPolicy: K8s network policies restrict pod-to-pod traffic
6. IAP (Identity-Aware Proxy): No public SSH, all access via IAP tunnel
7. Audit logging: All K8s API calls logged to Cloud Logging
8. Container-optimized OS: Minimal attack surface node OS
9. Shielded GKE nodes: Verified boot, vTPM
10. Policy Controller: OPA-based admission control (prevent insecure configs)
```

---

<a id="cheat-sheet"></a>
## GCP Quick Reference Cheat Sheet

### Service Selection Guide

```
COMPUTE:
  Long-running servers:         Compute Engine + MIG
  Containerized HTTP:           Cloud Run (recommended)
  Event-driven functions:       Cloud Functions
  PaaS (just deploy code):      App Engine
  Kubernetes workloads:         GKE (Standard or Autopilot)
  Batch processing:             Cloud Batch or Spot VMs
  ML training:                  Vertex AI Training + TPUs

DATABASE:
  RDBMS (managed):              Cloud SQL (MySQL/PostgreSQL)
  Globally distributed SQL:     Cloud Spanner
  NoSQL document:               Firestore
  NoSQL wide-column/IoT:        Bigtable
  In-memory cache:              Memorystore (Redis)
  Data warehouse:               BigQuery

STORAGE:
  Object storage:               Cloud Storage
  Block (VM disks):             Persistent Disk
  Shared file system:           Filestore (NFS)

NETWORKING:
  External HTTP load balancer:  Global HTTPS LB
  Internal service mesh:        Internal HTTPS LB
  DNS:                          Cloud DNS
  CDN:                          Cloud CDN
  VPN:                          Cloud VPN
  Dedicated connection:         Cloud Interconnect

MESSAGING:
  Async messaging/fan-out:      Pub/Sub
  Task queue:                   Cloud Tasks
  Streaming analytics:          Dataflow + Pub/Sub

ML/AI:
  Custom ML:                    Vertex AI
  Pre-trained models:           Vision AI, Speech-to-Text, Translation
  SQL-based ML:                 BigQuery ML
  Document processing:          Document AI
```

### Key gcloud Commands

```bash
# Config
gcloud config set project PROJECT
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
gcloud auth application-default login

# Compute
gcloud compute instances list
gcloud compute instances create NAME --machine-type=e2-medium
gcloud compute ssh NAME
gcloud compute instances stop/start/delete NAME

# Storage
gcloud storage ls gs://BUCKET
gcloud storage cp FILE gs://BUCKET/
gcloud storage rsync ./local gs://BUCKET --recursive

# IAM
gcloud projects add-iam-policy-binding PROJECT --member=... --role=...
gcloud iam service-accounts create NAME
gcloud iam service-accounts keys create key.json --iam-account=SA_EMAIL

# GKE
gcloud container clusters create-auto NAME --region=us-central1
gcloud container clusters get-credentials NAME --region=us-central1
kubectl get pods,svc,ingress -A

# Cloud Run
gcloud run deploy NAME --image=IMAGE --region=us-central1
gcloud run services list
gcloud run services update-traffic NAME --to-latest

# Cloud SQL
gcloud sql instances list
gcloud sql connect INSTANCE --user=root
gcloud sql backups create --instance=INSTANCE

# Secret Manager
gcloud secrets create NAME --data-file=-
gcloud secrets versions access latest --secret=NAME
```

---

## 🗺️ YOUR GCP CERTIFICATION ROADMAP

```
MONTH 1-2: CLOUD DIGITAL LEADER
  □ Part 1 of this guide (Modules 1-11)
  □ Cloud Skills Boost: Cloud Digital Leader learning path
  □ Google's own Digital Leader study guide
  □ Take exam — build confidence!

MONTH 3-5: ASSOCIATE CLOUD ENGINEER
  □ All of Part 1 + Part 2 (Modules 12-25)
  □ Cloud Skills Boost: ACE Quest
  □ Hands-on: Build a 3-tier app on GCP from scratch
  □ Practice exams: Whizlabs, ExamTopics
  □ Key: Practice gcloud CLI daily

MONTH 6-8: PROFESSIONAL CLOUD ARCHITECT
  □ All of Parts 1-4 (Modules 1-42)
  □ Study Google's case studies: Dress4Win, TerramEarth
  □ Cloud Skills Boost: Professional Architect Quest
  □ Focus: Design patterns, trade-offs, DR strategies
  □ Hardest GCP exam — allow extra prep time

MONTH 9-12: PROFESSIONAL DEVOPS ENGINEER
  □ Parts 3-4 in depth (Modules 26-42)
  □ Build: Full CI/CD pipeline with Cloud Build
  □ Deep dive: GKE, Cloud Run, Artifact Registry
  □ SLO/SLI/Error budget concepts

YEAR 2: SPECIALTIES
  □ Data Engineer (BigQuery, Dataflow, Pub/Sub)
  □ ML Engineer (Vertex AI, BigQuery ML)
  □ Security Engineer (SCC, KMS, VPC SC)
  □ Network Engineer (Interconnect, BGP, NIC)

RESOURCES:
  □ cloud.google.com/docs (authoritative source)
  □ cloudskillsboost.google (official labs)
  □ cloud.google.com/architecture (reference patterns)
  □ Google Cloud YouTube channel (free content)
  □ Pluralsight / Coursera for structured video learning
  □ Anton Antonov (GCP YouTuber — excellent content)
```

---

*You now hold a complete GCP mastery guide — from first principles to expert-level architecture and debugging.
The cloud rewards those who build, break, and learn from production systems.
Build real projects. Earn real certifications. Design systems that scale.

Welcome to Google Cloud. ☁️*
