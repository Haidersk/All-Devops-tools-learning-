# 🚀 DevOps Complete Documentation
### From Beginner to Advanced — Errors, Tools, Concepts & Best Practices

---

## 📋 Table of Contents

1. [What is DevOps?](#what-is-devops)
2. [DevOps Lifecycle](#devops-lifecycle)
3. [HTTP Status Code Errors](#http-status-code-errors)
4. [CI/CD Pipeline Errors](#cicd-pipeline-errors)
5. [Docker & Container Errors](#docker--container-errors)
6. [Kubernetes Errors](#kubernetes-errors)
7. [Git & Version Control Errors](#git--version-control-errors)
8. [Infrastructure & Cloud Errors](#infrastructure--cloud-errors)
9. [Nginx / Load Balancer Errors](#nginx--load-balancer-errors)
10. [DevOps Tools — Complete Guide](#devops-tools--complete-guide)
11. [Debugging Cheat Sheet](#debugging-cheat-sheet)
12. [Best Practices](#best-practices)

---

## What is DevOps?

DevOps is a combination of **Development (Dev)** and **Operations (Ops)**. It is a culture, practice, and set of tools that help teams **build, test, and deliver software faster and more reliably**.

### Before DevOps (The Old Way)
- Developers wrote code → threw it over the wall to Ops
- Ops deployed it → things broke → blame game started
- Releases took weeks or months

### After DevOps (The New Way)
- Dev and Ops teams work together
- Code is automatically tested and deployed
- Releases happen in hours or even minutes

### Core Principles
- **Collaboration** — Dev and Ops work as one team
- **Automation** — automate everything possible
- **Continuous Integration** — merge and test code frequently
- **Continuous Delivery** — always have deployable code
- **Monitoring** — watch everything in production

---

## DevOps Lifecycle

```
Plan → Code → Build → Test → Release → Deploy → Operate → Monitor → (back to Plan)
```

| Stage | What Happens | Tools Used |
|---|---|---|
| **Plan** | Define features, tasks, sprints | Jira, Trello, Azure Boards |
| **Code** | Write and review code | Git, GitHub, GitLab, VS Code |
| **Build** | Compile and package the application | Maven, Gradle, npm, Docker |
| **Test** | Run automated tests | Jest, Selenium, JUnit, Postman |
| **Release** | Prepare the release artifact | Jenkins, GitHub Actions, GitLab CI |
| **Deploy** | Push to staging/production | Kubernetes, Ansible, Terraform |
| **Operate** | Keep the system running | Linux, Nginx, AWS, GCP |
| **Monitor** | Watch logs, metrics, alerts | Prometheus, Grafana, ELK Stack |

---

## HTTP Status Code Errors

> These are the errors you see in browsers, APIs, and server logs every day.

### 🔴 5xx — Server-Side Errors
> **The server is at fault.** The client's request was fine, but the server couldn't handle it.

| Code | Name | Why It Happens | How to Fix |
|---|---|---|---|
| **500** | Internal Server Error | App crashed, unhandled exception, missing env variable | Check app logs immediately |
| **501** | Not Implemented | Server doesn't support the HTTP method used | Check API documentation |
| **502** | Bad Gateway | App server crashed or returned invalid response to proxy | Check if app process is running |
| **503** | Service Unavailable | Server overloaded, pod not ready, or deployment in progress | Check pod status, health checks |
| **504** | Gateway Timeout | Upstream server too slow — DB query, slow API | Optimize queries, increase timeout |
| **507** | Insufficient Storage | Server ran out of disk space | Free up disk space |

### 🟡 4xx — Client-Side Errors
> **The client made a bad request.** Server is fine but the request is wrong.

| Code | Name | Why It Happens | How to Fix |
|---|---|---|---|
| **400** | Bad Request | Malformed JSON, missing required fields | Validate request body/params |
| **401** | Unauthorized | No auth token sent, token expired | Send valid Bearer token |
| **403** | Forbidden | Token valid but user lacks permission | Check role/scope/permissions |
| **404** | Not Found | Wrong URL, route not defined, resource deleted | Check URL and routes |
| **405** | Method Not Allowed | Using POST instead of GET (or vice versa) | Use the correct HTTP method |
| **408** | Request Timeout | Client took too long to send request | Retry or increase timeout |
| **409** | Conflict | Duplicate entry, resource already exists | Check for existing resource first |
| **413** | Payload Too Large | File/body size exceeds server limit | Increase `client_max_body_size` in Nginx |
| **415** | Unsupported Media Type | Sent XML but server expects JSON | Set correct `Content-Type` header |
| **422** | Unprocessable Entity | Data fails validation — wrong type or format | Fix data format |
| **429** | Too Many Requests | Rate limit hit | Implement backoff/retry logic |

### 🔵 3xx — Redirects

| Code | Name | Why It Happens |
|---|---|---|
| **301** | Moved Permanently | URL changed forever |
| **302** | Found (Temporary Redirect) | Resource temporarily elsewhere |
| **304** | Not Modified | Browser cache still valid |
| **307** | Temporary Redirect | Redirect but keep HTTP method |
| **308** | Permanent Redirect | Permanent but keep HTTP method |

### Quick Reference — Who Is Responsible?

| Error Range | Whose Fault | Where to Look |
|---|---|---|
| **4xx** | Developer / Client | Request format, auth, URL |
| **5xx** | Server / DevOps | App logs, server health, resources |
| **502 / 504** | Proxy → App | Is app running? Is it slow? |
| **503** | App / Infra | Is pod ready? Is server overloaded? |

---

## CI/CD Pipeline Errors

> CI = Continuous Integration (build & test automatically)
> CD = Continuous Delivery/Deployment (deploy automatically)

### Common Errors & Root Causes

| Error | Why It Happens | Fix |
|---|---|---|
| **Build Failed** | Missing dependency, wrong language version, syntax error | Check build logs line by line |
| **Environment Variable Not Found** | Secrets only in local `.env`, not added to pipeline settings | Add secrets to CI/CD environment variables |
| **Pipeline Timeout** | Long-running tests, infinite loops, no timeout set | Set timeout limits, optimize tests |
| **Artifact Not Found** | Deploy step ran before build step | Fix job order / dependencies |
| **Permission Denied on Runner** | CI runner has no access to registry or cloud | Add proper IAM roles/tokens to runner |
| **Test Failures Blocking Deploy** | Different OS or dependency versions in CI vs local | Use Docker-based runners for consistency |
| **Flaky Tests** | Tests pass sometimes, fail sometimes — timing/external API | Mock external APIs, fix race conditions |
| **Docker Build Fails in CI** | Docker not installed on runner, or wrong context | Use Docker-in-Docker or specify build context |

### CI/CD Pipeline Flow (Example)

```
Push Code to GitHub
        ↓
GitHub Actions Triggered
        ↓
Install Dependencies → Run Tests → Build Docker Image
        ↓
Push Image to Registry (ECR / Docker Hub)
        ↓
Deploy to Staging → Run Smoke Tests
        ↓
Deploy to Production
```

### Root Cause
Most CI/CD errors happen because **local environment ≠ pipeline environment**.

---

## Docker & Container Errors

### Common Errors

| Error | Why It Happens | Fix |
|---|---|---|
| **Image Not Found** | Wrong image name/tag, or not pushed to registry | Check image name and tag exactly |
| **Port Already in Use** | Another container or process using same port | Use `-p 8081:80` or stop existing process |
| **Container Exits Immediately** | Main process crashed or completed — Docker stops when process stops | Check logs with `docker logs <container>` |
| **OOM Killed** | Container hit memory limit | Increase memory limit or fix memory leak |
| **Volume Mount Issues** | Wrong path, permissions, or file doesn't exist on host | Use absolute paths, check permissions |
| **Cannot Connect to Docker Daemon** | Docker not running, or user lacks permission | Run `sudo systemctl start docker` |
| **Layer Cache Invalidated** | `COPY` placed before `RUN npm install` | Put dependency installation before copying code |
| **Multi-stage Build Failure** | Wrong stage name or wrong artifact path | Verify stage names match exactly |

### Good vs Bad Dockerfile

```dockerfile
# ❌ BAD — cache breaks on every file change
COPY . .
RUN npm install

# ✅ GOOD — dependencies cached separately
COPY package.json package-lock.json ./
RUN npm install
COPY . .
```

### Useful Docker Commands

```bash
# See running containers
docker ps

# See all containers including stopped
docker ps -a

# Check container logs
docker logs <container-id>

# Enter a running container
docker exec -it <container-id> /bin/sh

# Check resource usage
docker stats

# Remove all stopped containers
docker container prune

# Check image layers and sizes
docker history <image-name>
```

### Root Cause
Docker errors mostly happen due to **incorrect Dockerfile structure**, **missing context**, or **environment mismatches** between dev and prod images.

---

## Kubernetes Errors

> Kubernetes (K8s) is an orchestration tool that manages containers at scale.

### Common Errors

| Error | Why It Happens | Fix |
|---|---|---|
| **CrashLoopBackOff** | Pod keeps crashing — app error, missing env var, wrong command | `kubectl logs <pod>` to find the crash reason |
| **ImagePullBackOff** | Can't pull image — wrong name, private registry, no secret | Check image name, add `imagePullSecret` |
| **Pending Pod** | Not enough CPU/memory on nodes, or no matching node | Check resource requests, node capacity |
| **OOMKilled** | Container exceeded memory limit | Increase memory limit in pod spec |
| **CreateContainerConfigError** | Referenced ConfigMap or Secret doesn't exist | Create the missing ConfigMap/Secret |
| **Service Not Reachable** | Labels on Service and Pod don't match | Double-check `selector` labels |
| **Ingress Not Working** | Ingress controller not installed, wrong annotations | Install nginx-ingress controller |
| **PVC Stuck in Pending** | No StorageClass defined or provisioner unavailable | Define StorageClass or use existing one |
| **Evicted Pod** | Node ran out of disk or memory | Free up resources, set resource limits |
| **RBAC Forbidden** | Service account lacks permission | Create proper Role and RoleBinding |

### Kubernetes Debugging Commands

```bash
# Check pod status
kubectl get pods
kubectl get pods -n <namespace>

# Describe a pod (shows events and errors)
kubectl describe pod <pod-name>

# Check pod logs
kubectl logs <pod-name>
kubectl logs <pod-name> --previous   # logs from crashed pod

# Check if service endpoints exist (if empty = label mismatch)
kubectl get endpoints <service-name>

# Check events sorted by time
kubectl get events --sort-by=.metadata.creationTimestamp

# Enter a running pod
kubectl exec -it <pod-name> -- /bin/sh

# Check resource usage
kubectl top pods
kubectl top nodes
```

### Root Cause
Most K8s errors happen because of **resource misconfigurations in YAML**, **label mismatches**, or **insufficient cluster resources**.

---

## Git & Version Control Errors

### Common Errors

| Error | Why It Happens | Fix |
|---|---|---|
| **Merge Conflict** | Two people edited same file/line | Manually resolve conflicts, then commit |
| **Detached HEAD** | Checked out a commit directly instead of a branch | `git checkout main` or `git switch main` |
| **Push Rejected** | Remote has changes you don't have locally | `git pull --rebase` then push |
| **Cannot Pull, Uncommitted Changes** | Local changes would be overwritten | `git stash` then pull, then `git stash pop` |
| **Rebase Conflict** | Branches diverged significantly | Resolve conflicts during each rebase step |
| **Force Push Broke History** | Someone used `git push --force` on shared branch | Never force push to `main` — use `--force-with-lease` |
| **Large File Error** | File exceeds GitHub's 100MB limit | Use Git LFS for large files |
| **`.gitignore` Not Working** | File was already tracked before being added to `.gitignore` | `git rm --cached <file>` then commit |

### Git Branching Strategy (GitFlow)

```
main          ──────────────────────────────────────►
                  ↑                    ↑
develop       ────┴────────────────────┴────────────►
                  ↑         ↑
feature/login ────┘         │
                        feature/api ──────────────►
```

### Useful Git Commands

```bash
# Create and switch to new branch
git checkout -b feature/my-feature

# Stash uncommitted changes
git stash
git stash pop

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# See commit history
git log --oneline --graph

# Cherry-pick a specific commit
git cherry-pick <commit-hash>

# Compare branches
git diff main..feature/my-feature
```

---

## Infrastructure & Cloud Errors

### Common Errors

| Error | Why It Happens | Fix |
|---|---|---|
| **Terraform State Lock** | Previous apply crashed — state file still locked | `terraform force-unlock <lock-id>` |
| **Resource Already Exists** | Manually created resource that Terraform now wants to create | `terraform import` to bring it under Terraform management |
| **IAM Permission Denied** | Role/user lacks required cloud permissions | Add correct IAM policies to the role |
| **Rate Limit Exceeded** | Too many API calls to cloud provider | Add retry logic, reduce parallel operations |
| **SSL Certificate Error** | Expired cert, wrong domain, propagation in progress | Renew cert, check DNS, wait for propagation |
| **DNS Not Resolving** | DNS record missing, wrong TTL, still propagating | Check DNS records, wait for TTL to expire |
| **S3 Bucket Access Denied** | Bucket policy or IAM policy not set correctly | Review bucket policy and IAM permissions |
| **EC2 Cannot Connect** | Security group blocking port, key pair wrong | Open port in security group, use correct key |

### Terraform Basic Commands

```bash
# Initialize — download providers
terraform init

# Preview changes
terraform plan

# Apply changes
terraform apply

# Destroy all resources
terraform destroy

# Show current state
terraform show

# Import existing resource
terraform import aws_instance.web i-1234567890abcdef0
```

---

## Nginx / Load Balancer Errors

### 502 Bad Gateway
```
nginx: connect() failed (111: Connection refused)
```
**Why:** App server not running, wrong `proxy_pass` port, or app crashed.

```bash
# Check if app is running
systemctl status your-app
pm2 status
kubectl get pods

# Check Nginx error logs
tail -f /var/log/nginx/error.log

# Test Nginx config
nginx -t
```

### 503 Service Unavailable
**Why:** All backends are down, pod not ready, or health check failing.

```bash
# Check pod readiness
kubectl get pods
kubectl describe pod <pod-name>

# Check Nginx upstream health
nginx -t && systemctl reload nginx
```

### 504 Gateway Timeout
**Why:** App taking too long to respond — slow DB, slow API, or timeout too low.

```nginx
# Increase timeout in nginx.conf
proxy_connect_timeout 60s;
proxy_send_timeout 60s;
proxy_read_timeout 60s;
```

### Common Nginx Config

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_connect_timeout 60s;
        proxy_read_timeout 60s;
    }

    # Increase upload size
    client_max_body_size 50M;
}
```

---

## DevOps Tools — Complete Guide

### 📦 Version Control

| Tool | Use Case | Level |
|---|---|---|
| **Git** | Local version control | Beginner |
| **GitHub** | Cloud Git hosting, PRs, Actions | Beginner |
| **GitLab** | Git + built-in CI/CD | Intermediate |
| **Bitbucket** | Git hosting with Jira integration | Intermediate |

---

### 🔁 CI/CD Tools

| Tool | Use Case | Level |
|---|---|---|
| **GitHub Actions** | CI/CD built into GitHub | Beginner |
| **Jenkins** | Self-hosted CI/CD server | Intermediate |
| **GitLab CI** | Built-in CI/CD in GitLab | Intermediate |
| **CircleCI** | Cloud CI/CD with fast builds | Intermediate |
| **ArgoCD** | GitOps-based deployment for K8s | Advanced |
| **Tekton** | Kubernetes-native CI/CD pipelines | Advanced |

---

### 🐳 Containerization

| Tool | Use Case | Level |
|---|---|---|
| **Docker** | Build and run containers | Beginner |
| **Docker Compose** | Multi-container local setups | Beginner |
| **Podman** | Docker alternative, daemonless | Intermediate |
| **BuildKit** | Faster, advanced Docker builds | Advanced |

---

### ☸️ Container Orchestration

| Tool | Use Case | Level |
|---|---|---|
| **Kubernetes (K8s)** | Production container orchestration | Advanced |
| **Minikube** | Local Kubernetes for learning | Intermediate |
| **K3s** | Lightweight Kubernetes | Intermediate |
| **Helm** | Package manager for Kubernetes | Advanced |
| **OpenShift** | Enterprise Kubernetes by Red Hat | Advanced |

---

### 🏗️ Infrastructure as Code (IaC)

| Tool | Use Case | Level |
|---|---|---|
| **Terraform** | Provision cloud infrastructure | Intermediate |
| **Ansible** | Configuration management, automation | Intermediate |
| **Pulumi** | IaC using real programming languages | Advanced |
| **CloudFormation** | AWS-native IaC | Intermediate |
| **Chef / Puppet** | Configuration management (older) | Advanced |

---

### ☁️ Cloud Platforms

| Platform | Key Services | Level |
|---|---|---|
| **AWS** | EC2, S3, RDS, EKS, Lambda, IAM | Beginner → Advanced |
| **Google Cloud (GCP)** | GKE, Cloud Run, BigQuery, GCS | Intermediate |
| **Azure** | AKS, Azure DevOps, Blob Storage | Intermediate |
| **DigitalOcean** | Droplets, Kubernetes, simple cloud | Beginner |

---

### 📊 Monitoring & Logging

| Tool | Use Case | Level |
|---|---|---|
| **Prometheus** | Metrics collection and alerting | Intermediate |
| **Grafana** | Visualize metrics and dashboards | Intermediate |
| **ELK Stack** | Elasticsearch + Logstash + Kibana for logs | Advanced |
| **Loki** | Log aggregation (lighter than ELK) | Intermediate |
| **Datadog** | Full observability platform | Intermediate |
| **New Relic** | APM and monitoring | Intermediate |
| **PagerDuty** | Incident alerting and on-call | Advanced |
| **Jaeger** | Distributed tracing | Advanced |

---

### 🔐 Security (DevSecOps)

| Tool | Use Case | Level |
|---|---|---|
| **Vault (HashiCorp)** | Secret management | Advanced |
| **Trivy** | Container vulnerability scanner | Intermediate |
| **SonarQube** | Code quality and security scanning | Intermediate |
| **Snyk** | Dependency vulnerability scanning | Beginner |
| **OWASP ZAP** | Web application security testing | Advanced |
| **Falco** | Runtime security for containers | Advanced |

---

### 🌐 Networking & Service Mesh

| Tool | Use Case | Level |
|---|---|---|
| **Nginx** | Reverse proxy, load balancer | Beginner |
| **HAProxy** | High-performance load balancer | Intermediate |
| **Istio** | Service mesh for microservices | Advanced |
| **Linkerd** | Lightweight service mesh | Advanced |
| **Traefik** | Dynamic reverse proxy | Intermediate |

---

### 📬 Messaging & Event Streaming

| Tool | Use Case | Level |
|---|---|---|
| **RabbitMQ** | Message queue for async processing | Intermediate |
| **Apache Kafka** | High-throughput event streaming | Advanced |
| **Redis** | In-memory cache and message broker | Intermediate |
| **AWS SQS** | Managed message queue on AWS | Intermediate |

---

### 📋 Project Management

| Tool | Use Case | Level |
|---|---|---|
| **Jira** | Sprint planning, bug tracking | Beginner |
| **Confluence** | Team documentation | Beginner |
| **Trello** | Simple kanban boards | Beginner |
| **Slack** | Team communication | Beginner |
| **PagerDuty** | Incident management | Intermediate |

---

## Debugging Cheat Sheet

### When Something is Down — Where to Look

```
User reports issue
        ↓
Check HTTP status code (4xx or 5xx?)
        ↓
4xx → Check request (URL, auth, body)
5xx → Check server
        ↓
Check app logs
        ↓
Check if process/pod is running
        ↓
Check resource usage (CPU, Memory, Disk)
        ↓
Check network (firewall, DNS, SSL)
        ↓
Check recent deployments / changes
```

### Quick Commands

```bash
# Check disk space
df -h

# Check memory usage
free -m

# Check CPU usage
top
htop

# Check which process is using a port
lsof -i :3000
netstat -tulpn | grep 3000

# Check system logs
journalctl -xe
tail -f /var/log/syslog

# Check Nginx logs
tail -f /var/log/nginx/error.log
tail -f /var/log/nginx/access.log

# Check Docker logs
docker logs <container-id> --tail 100 -f

# Check Kubernetes logs
kubectl logs <pod-name> --tail=100 -f
kubectl describe pod <pod-name>
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Best Practices

### CI/CD
- Always run tests before deploying
- Use separate environments — dev, staging, production
- Never hardcode secrets — use environment variables or Vault
- Keep pipelines fast — parallelize jobs where possible
- Use branch protection rules — no direct push to `main`

### Docker
- Use small base images (`alpine`, `distroless`)
- Never run containers as root
- Always pin image versions (`node:18-alpine` not `node:latest`)
- Use `.dockerignore` to exclude unnecessary files
- Use multi-stage builds to reduce image size

### Kubernetes
- Always set resource `requests` and `limits`
- Use `readinessProbe` and `livenessProbe` on all pods
- Store secrets in Kubernetes Secrets or Vault, not ConfigMaps
- Use namespaces to separate environments
- Enable RBAC and follow least privilege principle

### Git
- Write meaningful commit messages
- Use feature branches — never commit directly to `main`
- Review code via Pull Requests
- Use `git rebase` to keep history clean
- Tag releases with semantic versioning (`v1.2.3`)

### Security
- Rotate secrets regularly
- Scan Docker images for vulnerabilities before deploying
- Enable audit logging on cloud accounts
- Use HTTPS everywhere — never HTTP in production
- Implement rate limiting on all public APIs

---

## Learning Path — Beginner to Advanced

```
BEGINNER
├── Learn Linux basics (files, permissions, processes)
├── Learn Git (clone, commit, push, pull, branches)
├── Learn Docker (build, run, push images)
└── Learn basic CI/CD (GitHub Actions)

INTERMEDIATE
├── Learn Kubernetes basics (pods, services, deployments)
├── Learn Terraform (provision cloud infra)
├── Learn Ansible (configuration management)
├── Learn Nginx (reverse proxy, load balancing)
└── Learn Prometheus + Grafana (monitoring)

ADVANCED
├── Kubernetes advanced (Helm, RBAC, service mesh)
├── GitOps with ArgoCD
├── DevSecOps (Vault, Trivy, SonarQube)
├── Kafka / event-driven architecture
└── Multi-cloud and hybrid infrastructure
```

---

*Documentation Version: 1.0 | Last Updated: April 2026*
*Covers: HTTP Errors, CI/CD, Docker, Kubernetes, Git, Terraform, Nginx, Monitoring, Security*
