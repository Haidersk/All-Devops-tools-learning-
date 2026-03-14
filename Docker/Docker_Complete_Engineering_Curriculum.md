# Docker Mastery — Complete Engineering Curriculum
## Zero to Senior DevOps Engineer

> **Covers:** Containerization Fundamentals · Docker Core · Docker Compose · Networking · Storage · CI/CD · Security · Production Architecture · Expert Internals
> **Track:** DevOps Career Path | Beginner → Senior Engineer
> **Based on:** Official Docker Documentation — https://docs.docker.com/

---

## How to Use This Document

- Work through every lesson in order — concepts stack on each other
- Run every CLI command yourself — do not just read them
- Complete every hands-on lab — this is where real learning happens
- Attempt every practice question before reading the answer
- Return to earlier sections when later lessons reference them

---

## Table of Contents

### PHASE 1 — Foundations (Beginner)
- [Lesson 1 — Containerization Fundamentals and Docker Architecture](#lesson-1--containerization-fundamentals-and-docker-architecture)
- [Lesson 2 — Docker Images and Docker Hub](#lesson-2--docker-images-and-docker-hub)
- [Lesson 3 — Docker Containers — Run, Manage, Debug](#lesson-3--docker-containers--run-manage-debug)
- [Lesson 4 — Writing Dockerfiles](#lesson-4--writing-dockerfiles)
- [Lesson 5 — Building and Tagging Images](#lesson-5--building-and-tagging-images)

### PHASE 2 — Intermediate
- [Lesson 6 — Docker Storage — Volumes and Bind Mounts](#lesson-6--docker-storage--volumes-and-bind-mounts)
- [Lesson 7 — Docker Networking](#lesson-7--docker-networking)
- [Lesson 8 — Docker Compose — Multi-Container Applications](#lesson-8--docker-compose--multi-container-applications)
- [Lesson 9 — Multi-Stage Builds and Image Optimisation](#lesson-9--multi-stage-builds-and-image-optimisation)
- [Lesson 10 — Environment Variables, Health Checks, Resource Limits](#lesson-10--environment-variables-health-checks-resource-limits)
- [Lesson 11 — Private Container Registries](#lesson-11--private-container-registries)

### PHASE 3 — DevOps Level
- [Lesson 12 — Docker in CI/CD Pipelines](#lesson-12--docker-in-cicd-pipelines)
- [Lesson 13 — Docker Security Best Practices](#lesson-13--docker-security-best-practices)
- [Lesson 14 — Docker Logging Strategies](#lesson-14--docker-logging-strategies)
- [Lesson 15 — Docker Secrets Management](#lesson-15--docker-secrets-management)
- [Lesson 16 — Docker Build Caching and Optimisation](#lesson-16--docker-build-caching-and-optimisation)
- [Lesson 17 — Docker Swarm](#lesson-17--docker-swarm)
- [Lesson 18 — Docker with Kubernetes](#lesson-18--docker-with-kubernetes)
- [Lesson 19 — Docker with Terraform](#lesson-19--docker-with-terraform)

### PHASE 4 — Advanced Architecture
- [Lesson 20 — Microservices Architecture with Docker](#lesson-20--microservices-architecture-with-docker)
- [Lesson 21 — Scalable Container Platforms](#lesson-21--scalable-container-platforms)
- [Lesson 22 — Observability and Monitoring for Containers](#lesson-22--observability-and-monitoring-for-containers)

### PHASE 5 — Expert Level
- [Lesson 23 — Docker Internals — Namespaces, Cgroups, Overlay FS](#lesson-23--docker-internals--namespaces-cgroups-overlay-fs)
- [Lesson 24 — Container Security Hardening](#lesson-24--container-security-hardening)
- [Lesson 25 — Debugging Container Issues in Production](#lesson-25--debugging-container-issues-in-production)
- [Lesson 26 — DevOps Interview Preparation — Docker Edition](#lesson-26--devops-interview-preparation--docker-edition)

---

---

# PHASE 1 — FOUNDATIONS
## Docker Beginner Level

---

## Lesson 1 — Containerization Fundamentals and Docker Architecture

> **Level:** Absolute Beginner
> **Goal:** Understand what containers are, why Docker exists, and install Docker

---

### 1.1 The Problem Docker Solves

Before Docker, deploying software looked like this:

**Developer:** "It works on my machine."
**Ops Engineer:** "Well, your machine is not the server."

This problem — environment inconsistency — caused endless production failures. An app worked in development because the developer had Python 3.9, a specific version of a library, a particular OS configuration. The production server had Python 3.6, different library versions, and different environment variables. The app crashes. Nobody knows why at first.

> **Docker solves this by packaging the application AND its entire environment into one portable unit called a container.** If it works in the container on your laptop, it works in the container on the production server. Same container, same behaviour, anywhere.

---

### 1.2 Virtual Machines vs Containers

Both VMs and containers provide isolation. But they do it very differently.

```
VIRTUAL MACHINE STACK              CONTAINER STACK
─────────────────────              ─────────────────
  App A  |  App B                    App A  |  App B
  Libs   |  Libs                     Libs   |  Libs
  OS     |  OS                       ──────────────
  ───────────────            Container Runtime (Docker)
  Hypervisor                         Host OS
  Host OS                            Hardware
  Hardware
```

| Feature | Virtual Machine | Container |
|---|---|---|
| **Startup time** | 1–5 minutes | Milliseconds |
| **Size** | GBs (full OS) | MBs (app + libs only) |
| **OS** | Each VM has its own OS | Shares host OS kernel |
| **Isolation** | Full hardware-level isolation | Process-level isolation |
| **Overhead** | High (full OS running) | Very low |
| **Portability** | Limited | High — run anywhere |
| **Use case** | Full OS isolation needed | App packaging, microservices |

> **Key Insight:** Containers share the host OS kernel. They are not separate operating systems — they are isolated processes on the same OS. This is why they start in milliseconds and use far less memory than VMs.

> **Real-world usage:** They are not mutually exclusive. In production, containers typically run *inside* VMs. Azure runs Docker containers on VM infrastructure. The VM provides hardware isolation; Docker provides application packaging.

---

### 1.3 What is Docker?

Docker is a platform for building, shipping, and running containers. It has three main parts:

**Docker Client (`docker` CLI):**
The command-line tool you type commands into. It talks to the Docker Daemon.

**Docker Daemon (`dockerd`):**
A background service running on your machine. It does the actual work — building images, creating containers, managing networks and volumes.

**Docker Registry:**
A storage server for Docker images. Docker Hub is the public registry. You can run private registries too.

**The Architecture:**
```
You type:  docker run nginx
                │
                ▼
         Docker Client (CLI)
                │  REST API call
                ▼
         Docker Daemon (dockerd)
                │
    ┌───────────┼────────────┐
    ▼           ▼            ▼
 Images     Containers   Networks/Volumes
                │
                ▼
         Container Runtime
         (containerd + runc)
                │
                ▼
         Linux Kernel
         (namespaces, cgroups)
```

**The Flow When You Run a Container:**
1. You run `docker run nginx`
2. Docker client sends the request to the daemon
3. Daemon checks if the `nginx` image exists locally
4. If not — daemon pulls it from Docker Hub
5. Daemon creates a container from the image
6. Container process starts using Linux namespaces and cgroups
7. You get an isolated, running nginx server

---

### 1.4 Core Docker Concepts — Know These Cold

| Concept | What It Is | Analogy |
|---|---|---|
| **Image** | Read-only template — a snapshot of an app and its environment | A recipe or a class definition |
| **Container** | A running instance of an image | A dish made from the recipe / an object |
| **Dockerfile** | Instructions to build an image | The recipe itself |
| **Registry** | Storage server for images (Docker Hub) | An app store for images |
| **Volume** | Persistent storage that outlives a container | An external hard drive |
| **Network** | Virtual network connecting containers | An office internal network |

> **The One-Line Summary:** An **image** is the blueprint. A **container** is what runs. A **Dockerfile** builds the image. A **registry** stores and distributes images.

---

### 1.5 Installing Docker

**macOS:**
```bash
# Option 1: Docker Desktop (recommended — includes GUI)
# Download from: https://docs.docker.com/desktop/mac/install/
# After install:
docker --version
docker run hello-world
```

**Ubuntu Linux:**
```bash
# Remove old versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# Install prerequisites
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release

# Add Docker's official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Add your user to the docker group (avoid typing sudo every time)
sudo usermod -aG docker $USER
newgrp docker

# Verify
docker --version
docker run hello-world
```

**Windows:**
```
Download Docker Desktop from: https://docs.docker.com/desktop/windows/install/
Requires WSL 2 backend (Docker Desktop will prompt you)
After install, open PowerShell or Windows Terminal:
docker --version
docker run hello-world
```

---

### 1.6 Hands-On Lab 1 — Your First Container

**Objective:** Install Docker, run your first containers, understand what's happening.

```bash
# Step 1: Verify Docker is running
docker version
docker info

# Step 2: Run the hello-world container
# This pulls an image from Docker Hub and runs it
docker run hello-world

# Step 3: Run an Nginx web server
docker run -d -p 8080:80 --name my-nginx nginx
# -d = detached mode (runs in background)
# -p 8080:80 = map host port 8080 to container port 80
# --name my-nginx = give the container a name

# Step 4: Test it works
curl http://localhost:8080
# or open http://localhost:8080 in your browser

# Step 5: See running containers
docker ps

# Step 6: See ALL containers (including stopped ones)
docker ps -a

# Step 7: Stop the container
docker stop my-nginx

# Step 8: Start it again
docker start my-nginx

# Step 9: Remove the container
docker stop my-nginx
docker rm my-nginx

# Step 10: List downloaded images
docker images
```

**What just happened?**
- `docker run` told the daemon to create and start a container
- Docker pulled `nginx:latest` from Docker Hub (first time only — cached after)
- Docker created a container with isolated networking
- Port mapping made the container's port 80 reachable at localhost:8080
- The container ran Nginx in the foreground (or background with `-d`)

---

### 1.7 Practice Questions — Lesson 1

**Q1.** What is the key difference between a Docker image and a Docker container?

- A) Images run applications; containers store application files
- B) An image is a read-only template; a container is a running instance of an image
- C) Images are stored in Docker Hub; containers are stored locally
- D) There is no difference — they are the same thing

<details>
<summary>Answer and Explanation</summary>

**Answer: B.** An image is like a recipe — static, read-only, reusable. A container is like the dish made from that recipe — a live, running process. You can create many containers from one image. This distinction is fundamental and appears in every Docker interview.

</details>

---

**Q2.** A developer says "my app works on my machine but fails on the test server." Which Docker feature most directly addresses this problem?

- A) Docker volumes
- B) Docker networking
- C) Docker images packaging the app and all its dependencies together
- D) Docker Hub

<details>
<summary>Answer and Explanation</summary>

**Answer: C.** The whole point of Docker images is to package the app, libraries, runtime, and environment configuration together. The same image runs identically everywhere. No more "works on my machine."

</details>

---

**Q3 — Scenario.** Your company runs 50 microservices. The ops team says VMs are too slow to provision and waste too much RAM. Each service needs isolation but should start in seconds. What do you recommend?

<details>
<summary>Answer</summary>

Migrate each microservice to a Docker container. Containers start in milliseconds, use a fraction of the RAM of a full VM (no separate OS per service), and can still be isolated from each other via Docker networking and namespaces. The services can be managed by Kubernetes or Docker Swarm for orchestration at scale.

</details>

---

---

## Lesson 2 — Docker Images and Docker Hub

> **Level:** Beginner
> **Goal:** Understand image layers, pull/push images, use Docker Hub

---

### 2.1 How Docker Images Work — Layers

A Docker image is not a single file — it is a series of **layers** stacked on top of each other. Each instruction in a Dockerfile creates a new layer. Layers are immutable and cached.

```
Layer 5: COPY app.js .          ← Your application code
Layer 4: RUN npm install        ← Dependencies installed
Layer 3: COPY package.json .    ← Package manifest
Layer 2: RUN apt-get update     ← OS packages
Layer 1: FROM node:18-alpine    ← Base image (Node.js on Alpine Linux)
```

**Why layers matter:**
- **Caching:** If layer 3 has not changed, Docker reuses the cached layer for layers 3, 4, 5. Only changed layers and everything above them are rebuilt. This makes builds fast.
- **Sharing:** If two images use `FROM node:18-alpine`, both share the same base layers on disk. Not duplicated.
- **Efficiency:** Only layers that change need to be pushed or pulled from registries.

---

### 2.2 Image Naming and Tagging

Images follow this naming convention:

```
[registry]/[username/organisation]/[image-name]:[tag]

Examples:
nginx                                  → Docker Hub official image, latest tag
nginx:1.25                             → Specific version
node:18-alpine                         → Node 18 on Alpine Linux
mycompany/api-service:v2.1.0           → Private image with semantic version
myregistry.azurecr.io/myapp:prod-1234  → Azure Container Registry image
```

**Tags to know:**
- `:latest` — most recent build (default if you omit the tag — avoid in production)
- `:alpine` — based on Alpine Linux (~5MB) — very small
- `:slim` — reduced Debian-based image — smaller than default
- `:x.y.z` — specific version — always pin versions in production

> **Production Rule:** Never use `:latest` in production Dockerfiles or deployments. Always pin to a specific version like `node:18.17.1-alpine3.18`. Latest changes without warning and breaks your builds silently.

---

### 2.3 Essential Image Commands

```bash
# Pull an image from Docker Hub (without running it)
docker pull nginx
docker pull node:18-alpine
docker pull python:3.11-slim

# List locally stored images
docker images
docker image ls

# Inspect image metadata and layers
docker inspect nginx
docker image inspect nginx --format '{{.Config.Cmd}}'

# See image layer history
docker history nginx

# Remove an image
docker rmi nginx
docker image rm nginx

# Remove all unused images (dangling images with no tag)
docker image prune

# Remove ALL unused images (not just dangling)
docker image prune -a

# Search Docker Hub from the CLI
docker search nginx
docker search --filter is-official=true node
```

---

### 2.4 Docker Hub

Docker Hub (https://hub.docker.com) is the default public registry. It hosts:
- **Official images:** Maintained by Docker or the software vendor (nginx, node, python, postgres)
- **Verified publishers:** Software companies publishing official images (Microsoft, Elastic, MongoDB)
- **Community images:** Anyone can publish — use with caution

**Using Docker Hub:**
```bash
# Step 1: Create a free account at hub.docker.com

# Step 2: Login from CLI
docker login
# Enter your Docker Hub username and password

# Step 3: Tag your image for Docker Hub
# Format: docker tag [local-image] [dockerhub-username]/[repo-name]:[tag]
docker tag my-app:v1.0 johndoe/my-app:v1.0

# Step 4: Push to Docker Hub
docker push johndoe/my-app:v1.0

# Step 5: Pull from another machine
docker pull johndoe/my-app:v1.0

# Logout when done
docker logout
```

---

### 2.5 Hands-On Lab 2 — Explore Images

```bash
# Step 1: Pull specific versions
docker pull nginx:1.25-alpine
docker pull node:18-alpine
docker pull python:3.11-slim

# Step 2: Compare image sizes
docker images | grep -E "nginx|node|python"
# Notice: alpine images are MUCH smaller

# Step 3: Explore image layers
docker history nginx:1.25-alpine
docker history node:18-alpine

# Step 4: Inspect image metadata
docker inspect nginx:1.25-alpine | head -80
# Look for: Config, Env, Cmd, ExposedPorts

# Step 5: Tag an image with your own name
docker tag nginx:1.25-alpine myapp:local-nginx

# Step 6: Verify the tag
docker images | grep myapp

# Step 7: Remove the tag (does not delete the original)
docker rmi myapp:local-nginx

# Cleanup
docker image prune -a
```

---

---

## Lesson 3 — Docker Containers — Run, Manage, Debug

> **Level:** Beginner
> **Goal:** Master container lifecycle, logs, exec, inspect

---

### 3.1 Container Lifecycle

```
        docker run
             │
             ▼
          CREATED
             │
        docker start
             │
             ▼
          RUNNING ◄──── docker restart
             │
     ┌───────┴────────┐
     ▼                ▼
  docker stop     docker kill
     │                │
     ▼                ▼
  STOPPED          STOPPED
     │
  docker rm
     │
     ▼
  DELETED
```

---

### 3.2 The `docker run` Command — Full Reference

`docker run` is the most important Docker command. It creates AND starts a container in one step.

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARGS]
```

**Essential Options:**

| Flag | Purpose | Example |
|---|---|---|
| `-d` | Detached — run in background | `docker run -d nginx` |
| `-it` | Interactive + TTY — attach terminal | `docker run -it ubuntu bash` |
| `--name` | Give container a name | `--name my-app` |
| `-p` | Port mapping `host:container` | `-p 8080:80` |
| `-v` | Volume mount | `-v /data:/app/data` |
| `-e` | Set environment variable | `-e DB_HOST=localhost` |
| `--rm` | Auto-remove when stopped | `docker run --rm ubuntu echo hi` |
| `--network` | Connect to a network | `--network mynet` |
| `--memory` | Memory limit | `--memory 512m` |
| `--cpus` | CPU limit | `--cpus 0.5` |
| `--restart` | Restart policy | `--restart unless-stopped` |

**Restart Policies:**

| Policy | Behaviour |
|---|---|
| `no` | Never restart (default) |
| `always` | Always restart, even after daemon restart |
| `unless-stopped` | Restart unless manually stopped |
| `on-failure:5` | Restart on crash, max 5 times |

> **Production Rule:** Always set `--restart unless-stopped` (or in Docker Compose: `restart: unless-stopped`) for production containers. Without it, containers that crash or survive a server reboot will not come back automatically.

---

### 3.3 Container Management Commands

```bash
# List running containers
docker ps

# List all containers (running + stopped)
docker ps -a

# List container IDs only (useful for scripting)
docker ps -q

# Stop a container gracefully (SIGTERM, then SIGKILL after 10s)
docker stop my-nginx

# Kill immediately (SIGKILL — no graceful shutdown)
docker kill my-nginx

# Start a stopped container
docker start my-nginx

# Restart a container
docker restart my-nginx

# Remove a stopped container
docker rm my-nginx

# Force remove a running container
docker rm -f my-nginx

# Remove all stopped containers
docker container prune

# Rename a container
docker rename my-nginx web-server
```

---

### 3.4 Docker Logs — Essential for Debugging

```bash
# View container logs
docker logs my-nginx

# Follow log output in real time (like tail -f)
docker logs -f my-nginx

# Show last 50 lines
docker logs --tail 50 my-nginx

# Show logs with timestamps
docker logs -t my-nginx

# Combine: follow last 20 lines with timestamps
docker logs -f -t --tail 20 my-nginx

# Show logs since a time
docker logs --since 30m my-nginx     # last 30 minutes
docker logs --since 2024-01-15 my-nginx
```

---

### 3.5 Docker Exec — Run Commands Inside Containers

```bash
# Open an interactive shell inside a running container
docker exec -it my-nginx bash
# (Use 'sh' if bash is not available — e.g., Alpine-based images)
docker exec -it my-alpine sh

# Run a single command (non-interactive)
docker exec my-nginx ls /etc/nginx

# Run as root (even if container runs as non-root)
docker exec -u root -it my-nginx bash

# Set environment variable for the exec session
docker exec -e MY_VAR=hello -it my-nginx bash

# Inside the container you can:
# - Inspect files: cat /etc/nginx/nginx.conf
# - Check processes: ps aux
# - Check network: netstat -tlnp
# - Check environment: env
# - Install debug tools temporarily: apt-get install -y curl
exit  # Return to host
```

---

### 3.6 Docker Inspect — Deep Container Metadata

```bash
# Full JSON output of container configuration
docker inspect my-nginx

# Filter for specific fields
docker inspect my-nginx --format '{{.NetworkSettings.IPAddress}}'
docker inspect my-nginx --format '{{.State.Status}}'
docker inspect my-nginx --format '{{.HostConfig.RestartPolicy.Name}}'
docker inspect my-nginx --format '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'

# Container resource stats (live)
docker stats my-nginx

# Container stats for all running containers
docker stats

# One-shot stats (no live update)
docker stats --no-stream
```

---

### 3.7 Hands-On Lab 3 — Container Operations

```bash
# Step 1: Run containers in different modes

# Interactive — drops you into the container shell
docker run -it --rm ubuntu bash
# Inside Ubuntu: ls, pwd, cat /etc/os-release, exit

# Detached — runs in background
docker run -d --name webserver -p 8080:80 nginx

# With restart policy
docker run -d --name api --restart unless-stopped -p 3000:3000 node:18-alpine node -e "require('http').createServer((req, res) => res.end('Hello Docker!')).listen(3000)"

# Step 2: Inspect and monitor
docker ps
docker inspect webserver --format '{{.NetworkSettings.IPAddress}}'
docker stats --no-stream

# Step 3: Test logs
curl http://localhost:8080   # generates nginx access log
docker logs webserver
docker logs -f webserver &  # background log following
curl http://localhost:8080
kill %1  # stop the background log follow

# Step 4: Exec into running container
docker exec -it webserver bash
cat /etc/nginx/nginx.conf
ls /usr/share/nginx/html
echo "Hello from inside Docker!" > /usr/share/nginx/html/index.html
exit
curl http://localhost:8080  # Should show your message

# Step 5: Container lifecycle practice
docker stop webserver
docker ps          # not shown
docker ps -a       # shown as Exited
docker start webserver
docker ps          # running again

# Step 6: Cleanup
docker stop webserver api
docker rm webserver api
docker ps -a
```

---

### 3.8 Practice Questions — Lessons 1–3

**Q1.** A container is running in production and consuming too much memory. Without stopping it, how do you see its current memory usage?

<details>
<summary>Answer</summary>

`docker stats <container-name>` shows live CPU %, memory usage/limit, network I/O, and disk I/O for running containers. Use `--no-stream` for a one-time snapshot.

</details>

---

**Q2.** You have a container that crashes repeatedly and keeps restarting. How do you investigate what went wrong?

<details>
<summary>Answer</summary>

1. `docker logs <container-name>` — check the application's last output before it crashed
2. `docker logs --tail 100 <container-name>` — see the last 100 lines
3. `docker inspect <container-name>` — check the exit code and state
4. `docker inspect <container-name> --format '{{.State.ExitCode}}'` — exit code 137 means OOM kill (memory), 1 means app error, 2 means misuse of shell

</details>

---

**Q3 — Scenario.** You need to run a database migration script inside a running PostgreSQL container. The script is a bash command. How do you do this without restarting the container?

<details>
<summary>Answer</summary>

`docker exec -it postgres-container psql -U postgres -d mydb -c "ALTER TABLE users ADD COLUMN created_at TIMESTAMP DEFAULT NOW();"` or exec into bash first: `docker exec -it postgres-container bash` then run commands interactively.

</details>

---

---

## Lesson 4 — Writing Dockerfiles

> **Level:** Beginner → Intermediate
> **Goal:** Write production-quality Dockerfiles

---

### 4.1 Dockerfile — Every Instruction Explained

A Dockerfile is a text file containing instructions to build a Docker image. Each instruction creates a new image layer.

```dockerfile
# Comment
INSTRUCTION arguments
```

**Complete Instruction Reference:**

| Instruction | Purpose | Example |
|---|---|---|
| `FROM` | Base image to start from | `FROM node:18-alpine` |
| `WORKDIR` | Set working directory | `WORKDIR /app` |
| `COPY` | Copy files from host to image | `COPY . .` |
| `ADD` | Like COPY but supports URLs and tar extraction | `ADD app.tar.gz /app` |
| `RUN` | Execute commands during build | `RUN npm install` |
| `CMD` | Default command when container starts | `CMD ["node", "server.js"]` |
| `ENTRYPOINT` | Fixed command, args passed via CMD | `ENTRYPOINT ["node"]` |
| `ENV` | Set environment variables | `ENV NODE_ENV=production` |
| `ARG` | Build-time variable | `ARG VERSION=1.0` |
| `EXPOSE` | Document which port the app uses | `EXPOSE 3000` |
| `VOLUME` | Create a mount point | `VOLUME /data` |
| `USER` | Run as specific user | `USER nodejs` |
| `LABEL` | Add metadata | `LABEL version="1.0"` |
| `HEALTHCHECK` | Define container health check | `HEALTHCHECK CMD curl -f http://localhost/` |
| `STOPSIGNAL` | Signal sent on docker stop | `STOPSIGNAL SIGTERM` |

---

### 4.2 CMD vs ENTRYPOINT — Understanding the Difference

This confuses almost everyone at first.

**CMD:** Default command. Can be overridden by passing arguments to `docker run`.

**ENTRYPOINT:** Fixed command. Arguments to `docker run` are passed AS ARGUMENTS to the entrypoint.

```dockerfile
# CMD alone — entire command is replaceable
CMD ["node", "server.js"]
# docker run myapp → runs: node server.js
# docker run myapp python app.py → runs: python app.py (CMD overridden)

# ENTRYPOINT alone — fixed command
ENTRYPOINT ["node"]
# docker run myapp → runs: node  (fails — no script specified)
# docker run myapp server.js → runs: node server.js

# ENTRYPOINT + CMD — best practice for production
ENTRYPOINT ["node"]
CMD ["server.js"]
# docker run myapp → runs: node server.js  (CMD is default arg)
# docker run myapp other.js → runs: node other.js  (CMD overridden, ENTRYPOINT fixed)
```

**Shell form vs Exec form:**
```dockerfile
# Shell form — runs in /bin/sh -c, signal handling issues
CMD node server.js                  # BAD in production

# Exec form — runs directly, proper signal handling
CMD ["node", "server.js"]           # GOOD — always use this
```

> **Production Rule:** Always use exec form (JSON array syntax) for CMD and ENTRYPOINT. Shell form wraps commands in `/bin/sh -c`, which means `docker stop` sends SIGTERM to the shell, not your application. Your app will not shut down gracefully.

---

### 4.3 COPY vs ADD

```dockerfile
# COPY — simple, explicit, use this by default
COPY package.json .
COPY src/ ./src/
COPY --chown=node:node . .    # Copy with ownership set

# ADD — extra features, use only when needed
ADD https://example.com/file.tar.gz /app/   # Fetch from URL (avoid — not cached properly)
ADD app.tar.gz /app/                         # Extract tar automatically

# Best Practice: Use COPY unless you specifically need ADD's extra features
```

---

### 4.4 A Real-World Dockerfile — Node.js API

```dockerfile
# ─── Stage: Production build ──────────────────────────────────────
FROM node:18-alpine

# Metadata labels
LABEL maintainer="devops@company.com" \
      version="1.0" \
      description="Company API Service"

# Set working directory
WORKDIR /app

# Copy package files FIRST (layer caching optimisation — see Lesson 9)
COPY package*.json ./

# Install production dependencies only
RUN npm ci --only=production && \
    npm cache clean --force

# Copy application source code
COPY . .

# Create non-root user and set ownership
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001 && \
    chown -R nodejs:nodejs /app

# Switch to non-root user
USER nodejs

# Document the port (does not actually publish it)
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -qO- http://localhost:3000/health || exit 1

# Default command
CMD ["node", "server.js"]
```

---

### 4.5 A Real-World Dockerfile — Python Flask API

```dockerfile
FROM python:3.11-slim

# Prevent Python from writing .pyc files and enable unbuffered logging
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /app

# Install system dependencies (separate layer for caching)
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create and switch to non-root user
RUN useradd --create-home --no-log-init appuser
USER appuser

EXPOSE 5000

HEALTHCHECK --interval=30s --timeout=5s \
  CMD curl -f http://localhost:5000/health || exit 1

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "--workers", "4", "app:app"]
```

---

### 4.6 .dockerignore — The Dockerfile's .gitignore

Always create a `.dockerignore` file. It prevents copying unnecessary files into the image — keeps images small and builds fast.

```
# .dockerignore
node_modules/
npm-debug.log
.git/
.gitignore
.env
.env.*
*.md
README.md
docs/
tests/
test/
coverage/
.nyc_output/
dist/          # Only if you're copying source and building inside Docker
__pycache__/
*.pyc
*.pyo
.pytest_cache/
.DS_Store
Thumbs.db
docker-compose*.yml
Dockerfile*
```

> **Why this matters:** Without `.dockerignore`, `COPY . .` copies `node_modules` (potentially millions of files, gigabytes of data) into the build context, making builds extremely slow and images huge.

---

### 4.7 Hands-On Lab 4 — Write and Build Your First Dockerfiles

**Node.js App:**
```bash
mkdir node-docker-lab && cd node-docker-lab

# Create a simple server
cat > server.js << 'EOF'
const http = require('http');
const port = process.env.PORT || 3000;
const server = http.createServer((req, res) => {
  if (req.url === '/health') {
    res.writeHead(200);
    res.end(JSON.stringify({ status: 'healthy', timestamp: new Date() }));
  } else {
    res.writeHead(200, {'Content-Type': 'text/html'});
    res.end('<h1>Hello from Docker!</h1><p>Container is running.</p>');
  }
});
server.listen(port, () => console.log(`Server running on port ${port}`));
EOF

# Create package.json
cat > package.json << 'EOF'
{
  "name": "docker-lab",
  "version": "1.0.0",
  "main": "server.js"
}
EOF

# Create .dockerignore
cat > .dockerignore << 'EOF'
node_modules/
*.md
.git/
EOF

# Create Dockerfile
cat > Dockerfile << 'EOF'
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
USER nodejs
EXPOSE 3000
CMD ["node", "server.js"]
EOF

# Build the image
docker build -t node-lab:v1 .

# Run the container
docker run -d --name node-lab -p 3000:3000 node-lab:v1

# Test it
curl http://localhost:3000
curl http://localhost:3000/health

# Check logs
docker logs node-lab

# Cleanup
docker stop node-lab && docker rm node-lab
cd .. && rm -rf node-docker-lab
```

---

---

## Lesson 5 — Building and Tagging Images

> **Level:** Beginner → Intermediate
> **Goal:** Master the build process, tagging strategy, pushing to registries

---

### 5.1 The `docker build` Command

```bash
# Basic build (uses Dockerfile in current directory)
docker build -t myapp:v1.0 .

# Use a different Dockerfile
docker build -t myapp:v1.0 -f Dockerfile.prod .

# Build with build arguments
docker build --build-arg NODE_ENV=production -t myapp:prod .

# Build with no cache (forces fresh download and install)
docker build --no-cache -t myapp:v1.0 .

# Build and show output in plain text (not compressed)
docker build --progress=plain -t myapp:v1.0 .

# Build for a specific platform (cross-compilation)
docker buildx build --platform linux/amd64,linux/arm64 -t myapp:v1.0 .
```

---

### 5.2 Tagging Strategy for Production

A good tagging strategy makes deployments traceable and rollbacks easy.

```bash
# Semantic versioning (most stable)
docker tag myapp myapp:1.0.0
docker tag myapp myapp:1.0
docker tag myapp myapp:1

# Git commit SHA (fully traceable — preferred in CI/CD)
GIT_SHA=$(git rev-parse --short HEAD)
docker tag myapp myapp:${GIT_SHA}

# Environment tags
docker tag myapp myapp:staging
docker tag myapp myapp:production

# Combined — SHA + semantic version
docker tag myapp myapp:v1.0.0-${GIT_SHA}

# Multiple tags on same image (points to same layer set)
docker build -t myapp:v1.0.0 -t myapp:latest -t myapp:${GIT_SHA} .
```

**In a real CI/CD pipeline:**
```bash
VERSION="1.2.3"
GIT_SHA=$(git rev-parse --short HEAD)
REGISTRY="mycompany.azurecr.io"

docker build \
  -t ${REGISTRY}/myapp:${VERSION} \
  -t ${REGISTRY}/myapp:${GIT_SHA} \
  -t ${REGISTRY}/myapp:latest \
  .

docker push ${REGISTRY}/myapp:${VERSION}
docker push ${REGISTRY}/myapp:${GIT_SHA}
docker push ${REGISTRY}/myapp:latest
```

---

### 5.3 Hands-On Lab 5 — Build, Tag, and Push

```bash
# Step 1: Create a simple Python app
mkdir python-docker-lab && cd python-docker-lab

cat > app.py << 'EOF'
from http.server import HTTPServer, BaseHTTPRequestHandler
import json, os

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/health':
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()
            self.wfile.write(json.dumps({"status": "healthy"}).encode())
        else:
            self.send_response(200)
            self.send_header('Content-type', 'text/html')
            self.end_headers()
            self.wfile.write(b'<h1>Python Docker App</h1>')
    def log_message(self, format, *args): pass

HTTPServer(('0.0.0.0', 8000), Handler).serve_forever()
EOF

cat > Dockerfile << 'EOF'
FROM python:3.11-slim
WORKDIR /app
COPY app.py .
EXPOSE 8000
CMD ["python", "app.py"]
EOF

# Step 2: Build with multiple tags
docker build -t python-lab:v1.0.0 -t python-lab:latest .

# Step 3: View the image
docker images | grep python-lab
docker history python-lab:v1.0.0

# Step 4: Run and test
docker run -d --name python-lab -p 8000:8000 python-lab:v1.0.0
curl http://localhost:8000
curl http://localhost:8000/health

# Step 5: Push to Docker Hub (substitute your username)
docker login
docker tag python-lab:v1.0.0 <your-dockerhub-username>/python-lab:v1.0.0
docker push <your-dockerhub-username>/python-lab:v1.0.0

# Step 6: Pull it back (proves it works from registry)
docker rmi python-lab:v1.0.0 python-lab:latest
docker run -d --name test-pull -p 8001:8000 <your-dockerhub-username>/python-lab:v1.0.0
curl http://localhost:8001

# Cleanup
docker stop python-lab test-pull
docker rm python-lab test-pull
cd .. && rm -rf python-docker-lab
```

---

---

# PHASE 2 — INTERMEDIATE

---

## Lesson 6 — Docker Storage — Volumes and Bind Mounts

> **Level:** Intermediate
> **Goal:** Understand persistent storage, volumes vs bind mounts

---

### 6.1 The Container Storage Problem

Containers are ephemeral — when you remove a container, everything written to its filesystem is gone. This is a problem for:
- Databases (you need data to survive container restarts)
- Log files (you need logs after container crashes)
- User-uploaded files
- Configuration files you want to edit without rebuilding

**Docker has three storage options:**

```
┌──────────────────────────────────────────────────────────────┐
│                    HOST MACHINE                               │
│                                                              │
│  /host/path      Docker Volume     tmpfs (RAM only)          │
│      │                │                 │                    │
│      │ bind mount     │  volume mount   │ tmpfs mount        │
│      ▼                ▼                 ▼                    │
│  ┌─────────────────────────────────────────────────────┐     │
│  │              CONTAINER FILESYSTEM                    │     │
│  │          /app/data   /var/lib/db   /tmp/cache        │     │
│  └─────────────────────────────────────────────────────┘     │
└──────────────────────────────────────────────────────────────┘
```

---

### 6.2 Docker Volumes — The Recommended Approach

Volumes are managed by Docker. Docker creates and manages the storage location on the host (typically `/var/lib/docker/volumes/`). You never interact with the host path directly.

**When to use volumes:**
- Databases — PostgreSQL, MySQL, MongoDB data
- Application data that must persist
- Sharing data between containers
- When you want Docker to manage the storage lifecycle

```bash
# Create a named volume
docker volume create db-data

# List volumes
docker volume ls

# Inspect a volume (shows where it lives on the host)
docker volume inspect db-data

# Run PostgreSQL with a named volume
docker run -d \
  --name postgres-db \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_DB=myapp \
  -v db-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15-alpine

# Remove a volume (only when no container is using it)
docker volume rm db-data

# Remove all unused volumes
docker volume prune

# Run container with anonymous volume (volume name auto-generated)
docker run -d -v /var/lib/mysql mysql:8
```

---

### 6.3 Bind Mounts — Mount Host Directories

A bind mount maps a specific path on your host machine directly into the container. Changes are visible immediately in both directions.

**When to use bind mounts:**
- Local development — live reload your code changes without rebuilding
- Providing configuration files from the host
- Writing logs to a specific host location

```bash
# Syntax: -v /absolute/host/path:/container/path
docker run -d \
  --name dev-server \
  -p 3000:3000 \
  -v $(pwd)/src:/app/src \
  -v $(pwd)/config:/app/config:ro \
  node:18-alpine \
  node /app/src/server.js
# :ro = read-only (container cannot modify the host files)

# Modern syntax using --mount (more explicit)
docker run -d \
  --name dev-server \
  --mount type=bind,source=$(pwd)/src,target=/app/src,readonly \
  node:18-alpine
```

---

### 6.4 Volumes vs Bind Mounts — When to Use Which

| Aspect | Named Volume | Bind Mount |
|---|---|---|
| **Managed by** | Docker | You (host filesystem) |
| **Portability** | Works on any OS | Path must exist on host |
| **Use in production** | Yes — databases, persistent data | Config files, rarely |
| **Use in development** | Less common | Yes — live code reload |
| **Performance** | Better on Docker Desktop | Slower on Docker Desktop (macOS/Win) |
| **Backup** | `docker volume` tools | Standard filesystem backup |

---

### 6.5 Hands-On Lab 6 — Persistent Database

```bash
# Step 1: Create a volume
docker volume create pg-data
docker volume inspect pg-data

# Step 2: Run PostgreSQL with persistent storage
docker run -d \
  --name mydb \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=mysecret \
  -e POSTGRES_DB=testdb \
  -v pg-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15-alpine

# Step 3: Create some data
docker exec -it mydb psql -U admin -d testdb -c "
  CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT, email TEXT);
  INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
  INSERT INTO users (name, email) VALUES ('Bob', 'bob@example.com');
  SELECT * FROM users;
"

# Step 4: Delete the container (data should survive)
docker stop mydb
docker rm mydb
docker ps -a  # container gone

# Step 5: Create a new container with the SAME volume
docker run -d \
  --name mydb-restored \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=mysecret \
  -e POSTGRES_DB=testdb \
  -v pg-data:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15-alpine

# Step 6: Data is still there!
docker exec -it mydb-restored psql -U admin -d testdb -c "SELECT * FROM users;"

# Step 7: Cleanup
docker stop mydb-restored
docker rm mydb-restored
docker volume rm pg-data
```

---

---

## Lesson 7 — Docker Networking

> **Level:** Intermediate
> **Goal:** Master container networking, port mapping, network types

---

### 7.1 Docker Network Architecture

Every container connects to a network. Docker has three built-in network drivers:

| Network | Description | Use Case |
|---|---|---|
| **bridge** | Default. Isolated virtual network on host. | Single-host apps, default choice |
| **host** | Container shares host's network stack | High performance, no isolation |
| **none** | No networking at all | Maximum isolation, offline processing |
| **overlay** | Multi-host networking | Docker Swarm, multi-node clusters |
| **macvlan** | Container gets its own MAC/IP on LAN | Legacy apps needing LAN access |

```bash
# List networks
docker network ls

# Inspect the default bridge network
docker network inspect bridge
```

---

### 7.2 Bridge Networks — Default and Custom

**Default bridge network (`bridge`):**
- All containers on the default bridge can communicate by IP
- But NOT by container name (no DNS resolution)
- Less secure — all containers can reach each other

**Custom bridge network (recommended):**
- Containers can communicate by container name (automatic DNS)
- Isolated from other custom networks
- More control, more security

```bash
# Create a custom bridge network
docker network create myapp-network

# Inspect it
docker network inspect myapp-network

# Run containers on the custom network
docker run -d \
  --name api-server \
  --network myapp-network \
  -p 3000:3000 \
  node:18-alpine \
  node -e "require('http').createServer((req,res)=>res.end('API')).listen(3000)"

docker run -d \
  --name db-server \
  --network myapp-network \
  -e POSTGRES_PASSWORD=secret \
  postgres:15-alpine

# Containers can now reach each other by NAME
docker exec api-server ping db-server   # Works! DNS resolved
docker exec api-server nslookup db-server

# Connect an existing container to a network
docker network connect myapp-network existing-container

# Disconnect
docker network disconnect myapp-network existing-container

# Remove network (all containers must be removed or disconnected first)
docker network rm myapp-network

# Remove all unused networks
docker network prune
```

---

### 7.3 Port Mapping — Exposing Containers to the World

**The difference between EXPOSE and -p:**
- `EXPOSE` in Dockerfile: **Documentation only.** It tells humans which port the app uses. It does NOT make the port accessible from outside Docker.
- `-p` / `--publish` in `docker run`: **Actually publishes the port.** Maps a host port to a container port.

```bash
# Map host port 8080 to container port 80
docker run -d -p 8080:80 nginx

# Map to specific host IP (only accessible from that IP)
docker run -d -p 127.0.0.1:8080:80 nginx   # localhost only

# Let Docker choose a random host port
docker run -d -p 80 nginx

# See what port was chosen
docker port <container-name> 80

# Map multiple ports
docker run -d -p 80:80 -p 443:443 nginx

# Map UDP port
docker run -d -p 53:53/udp dns-server
```

---

### 7.4 Container DNS and Service Discovery

On a custom bridge network, Docker runs an internal DNS server. Containers can reach each other by name.

```bash
# Create network
docker network create backend-net

# Run two services
docker run -d --name redis --network backend-net redis:alpine
docker run -d --name worker --network backend-net \
  python:3.11-alpine python -c "
import socket, time
while True:
    try:
        ip = socket.gethostbyname('redis')
        print(f'Redis found at: {ip}')
    except Exception as e:
        print(f'Redis not found: {e}')
    time.sleep(5)
"

# See worker resolving redis by name
docker logs worker

# Cleanup
docker stop redis worker && docker rm redis worker
docker network rm backend-net
```

---

### 7.5 Hands-On Lab 7 — Multi-Container Networking

```bash
# Build a 3-container system: nginx (proxy) → node api → redis (cache)

# Step 1: Create network
docker network create lab-net

# Step 2: Start Redis
docker run -d \
  --name redis \
  --network lab-net \
  redis:7-alpine

# Step 3: Start a Node.js API that uses Redis
docker run -d \
  --name api \
  --network lab-net \
  -e REDIS_HOST=redis \
  -p 3000:3000 \
  node:18-alpine \
  node -e "
const http = require('http');
http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'application/json'});
  res.end(JSON.stringify({
    message: 'API response',
    redis_host: process.env.REDIS_HOST,
    timestamp: new Date()
  }));
}).listen(3000, () => console.log('API on port 3000'));
"

# Step 4: Test containers can communicate
curl http://localhost:3000
docker exec api ping redis -c 3     # ping by name — works!
docker exec api nslookup redis       # DNS resolution

# Step 5: Verify isolation (default bridge cannot reach custom)
docker run --rm alpine ping api      # FAILS — different network, no access

# Cleanup
docker stop redis api
docker rm redis api
docker network rm lab-net
```

---

### 7.6 Practice Questions — Lessons 6–7

**Q1 — Scenario.** You run a MySQL container without a volume. You store 3 months of production data. The container crashes and needs to be recreated. What happens to the data?

<details>
<summary>Answer</summary>

All data is lost. Container filesystems are ephemeral — when a container is removed, its writable layer is deleted. This is why databases in Docker MUST use named volumes (`-v db-data:/var/lib/mysql`). The volume persists independently of the container lifecycle.

</details>

---

**Q2.** Two containers on the same custom bridge network need to communicate. Container A is running an API on port 8080. What hostname and port should Container B use to call it?

<details>
<summary>Answer</summary>

Container B should use `http://container-a-name:8080` — the container's name as the hostname. On custom bridge networks, Docker's built-in DNS automatically resolves container names to their internal IPs. This does NOT work on the default bridge network.

</details>


---

---

## Lesson 8 — Docker Compose — Multi-Container Applications

> **Level:** Intermediate
> **Goal:** Define and manage multi-container apps with Docker Compose

---

### 8.1 What is Docker Compose?

Running one container with `docker run` is manageable. Running 5 containers — web, API, database, cache, queue — with all their networks, volumes, environment variables, and port mappings as separate CLI commands becomes a nightmare to maintain and share.

**Docker Compose solves this:** Define your entire multi-container application in a single `docker-compose.yml` file. One command starts everything. One command stops everything.

```bash
docker compose up -d     # Start all services
docker compose down      # Stop and remove all services
docker compose logs -f   # Follow all service logs
docker compose ps        # Status of all services
```

> **Note on versions:** The old `docker-compose` (Python-based, v1) is deprecated. The modern version is `docker compose` (part of Docker CLI, v2). Use `docker compose` — without the hyphen.

---

### 8.2 Docker Compose File Structure

```yaml
# docker-compose.yml
version: "3.9"   # Compose file format version

services:        # Each service = one container
  web:           # Service name (also used as hostname in networks)
    # ... config

  api:
    # ... config

  db:
    # ... config

volumes:         # Named volumes used by services
  db-data:

networks:        # Custom networks
  backend:
  frontend:
```

---

### 8.3 Complete Compose Reference — Every Key Field

```yaml
services:
  api:
    # Image to use (either image: or build:, not both)
    image: node:18-alpine

    # OR build from Dockerfile
    build:
      context: ./api            # Directory containing Dockerfile
      dockerfile: Dockerfile    # Dockerfile name (default: Dockerfile)
      args:
        NODE_ENV: production

    # Container name (default: <project>_<service>_<number>)
    container_name: api-server

    # Ports: HOST:CONTAINER
    ports:
      - "3000:3000"

    # Environment variables
    environment:
      - NODE_ENV=production
      - DB_HOST=db
      - DB_PORT=5432
    # OR using env_file:
    env_file:
      - .env

    # Volumes
    volumes:
      - ./src:/app/src           # bind mount
      - api-data:/app/data       # named volume

    # Dependencies — db starts before api
    depends_on:
      db:
        condition: service_healthy   # Wait for health check to pass

    # Networks this service connects to
    networks:
      - backend

    # Restart policy
    restart: unless-stopped

    # Resource limits
    deploy:
      resources:
        limits:
          memory: 512m
          cpus: '0.5'

    # Health check
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

    # Override default command
    command: ["node", "server.js"]

    # Labels
    labels:
      - "com.company.service=api"
      - "com.company.version=1.0"
```

---

### 8.4 Real-World Compose Example — Full Stack Web Application

**Project structure:**
```
myapp/
├── docker-compose.yml
├── docker-compose.override.yml    ← dev overrides (not committed sometimes)
├── .env
├── api/
│   ├── Dockerfile
│   └── server.js
└── web/
    ├── Dockerfile
    └── index.html
```

**docker-compose.yml:**
```yaml
version: "3.9"

services:
  # ─── NGINX reverse proxy ────────────────────────────────────────
  nginx:
    image: nginx:1.25-alpine
    container_name: nginx-proxy
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/ssl:/etc/nginx/ssl:ro
    depends_on:
      - api
      - web
    networks:
      - frontend
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "nginx", "-t"]
      interval: 30s
      timeout: 10s
      retries: 3

  # ─── Node.js API ─────────────────────────────────────────────────
  api:
    build:
      context: ./api
      dockerfile: Dockerfile
    container_name: api-service
    expose:
      - "3000"        # Only accessible internally (no host port)
    environment:
      - NODE_ENV=production
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      postgres:
        condition: service_healthy
      redis:
        condition: service_healthy
    networks:
      - frontend
      - backend
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost:3000/health"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 30s

  # ─── PostgreSQL Database ─────────────────────────────────────────
  postgres:
    image: postgres:15-alpine
    container_name: postgres-db
    environment:
      - POSTGRES_DB=${DB_NAME}
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./db/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - backend
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER} -d ${DB_NAME}"]
      interval: 10s
      timeout: 5s
      retries: 5
      start_period: 10s

  # ─── Redis Cache ─────────────────────────────────────────────────
  redis:
    image: redis:7-alpine
    container_name: redis-cache
    command: redis-server --requirepass ${REDIS_PASSWORD} --appendonly yes
    volumes:
      - redis-data:/data
    networks:
      - backend
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "redis-cli", "--pass", "${REDIS_PASSWORD}", "ping"]
      interval: 10s
      timeout: 3s
      retries: 5

volumes:
  postgres-data:
    driver: local
  redis-data:
    driver: local

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true    # No internet access from backend network
```

**.env file:**
```bash
DB_NAME=myappdb
DB_USER=appuser
DB_PASSWORD=strongpassword123
REDIS_PASSWORD=redissecret456
```

> **Security Note:** Never commit `.env` to Git. Add `.env` to `.gitignore`. Commit a `.env.example` with placeholder values.

---

### 8.5 Docker Compose Commands — Full Reference

```bash
# Start all services (detached)
docker compose up -d

# Start and rebuild images
docker compose up -d --build

# Start specific service only
docker compose up -d postgres

# Stop all services (keeps containers)
docker compose stop

# Stop and REMOVE containers, networks (keeps volumes)
docker compose down

# Remove everything including volumes (DANGEROUS — data lost)
docker compose down -v

# View running services
docker compose ps

# View logs for all services
docker compose logs

# Follow logs for specific service
docker compose logs -f api

# Execute command in running service
docker compose exec api bash
docker compose exec postgres psql -U appuser -d myappdb

# Scale a service (run multiple instances)
docker compose up -d --scale api=3

# Pull latest images
docker compose pull

# Show resource usage
docker compose top

# List images used by compose
docker compose images

# Validate compose file
docker compose config
```

---

### 8.6 Development vs Production Compose

Use multiple Compose files — base + environment-specific overrides:

**docker-compose.yml (base — shared config):**
```yaml
version: "3.9"
services:
  api:
    image: myapp/api:${VERSION:-latest}
    networks:
      - app-net
networks:
  app-net:
```

**docker-compose.override.yml (dev — auto-loaded):**
```yaml
version: "3.9"
services:
  api:
    build: ./api         # Build from source in dev
    volumes:
      - ./api:/app       # Live reload
    environment:
      - NODE_ENV=development
      - DEBUG=*
    ports:
      - "3000:3000"      # Expose port in dev
```

**docker-compose.prod.yml (production):**
```yaml
version: "3.9"
services:
  api:
    restart: unless-stopped
    deploy:
      resources:
        limits:
          memory: 512m
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
```

```bash
# Dev (auto-loads base + override)
docker compose up -d

# Production
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```

---

### 8.7 Hands-On Lab 8 — Full Stack App with Compose

```bash
mkdir compose-lab && cd compose-lab

# Create the API
mkdir api && cat > api/server.js << 'EOF'
const http = require('http');
const os = require('os');

http.createServer((req, res) => {
  if (req.url === '/health') {
    res.writeHead(200, {'Content-Type': 'application/json'});
    res.end(JSON.stringify({ status: 'healthy', hostname: os.hostname() }));
  } else {
    res.writeHead(200, {'Content-Type': 'application/json'});
    res.end(JSON.stringify({
      message: 'Hello from API',
      hostname: os.hostname(),
      env: process.env.NODE_ENV,
      db_host: process.env.DB_HOST
    }));
  }
}).listen(3000, () => console.log('API running on port 3000'));
EOF

cat > api/Dockerfile << 'EOF'
FROM node:18-alpine
WORKDIR /app
COPY server.js .
EXPOSE 3000
HEALTHCHECK --interval=15s --timeout=5s CMD wget -qO- http://localhost:3000/health || exit 1
CMD ["node", "server.js"]
EOF

# Create docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: "3.9"
services:
  api:
    build: ./api
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - DB_HOST=redis
    depends_on:
      redis:
        condition: service_healthy
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 3s
      retries: 5

volumes: {}
networks: {}
EOF

# Start everything
docker compose up -d

# Watch it come up
docker compose ps

# Test
curl http://localhost:3000
curl http://localhost:3000/health

# View logs
docker compose logs api
docker compose logs redis

# Scale the API (load balancing demo)
docker compose up -d --scale api=3
docker compose ps

# Tear down
docker compose down
cd .. && rm -rf compose-lab
```

---

---

## Lesson 9 — Multi-Stage Builds and Image Optimisation

> **Level:** Intermediate
> **Goal:** Build small, efficient production images

---

### 9.1 Why Image Size Matters

Large images:
- Take longer to push and pull from registries
- Use more disk space on every server
- Increase container startup time
- Have more attack surface (more packages = more vulnerabilities)
- Cost more in registry storage

**Goal:** Make images as small as possible while keeping everything the app needs to run.

---

### 9.2 Multi-Stage Builds — The Key Technique

A multi-stage build uses multiple `FROM` statements. Earlier stages compile/build. The final stage contains ONLY the runtime artifacts — no build tools, no compilers, no test dependencies.

**Before multi-stage (bad — 1.2GB image):**
```dockerfile
FROM node:18                    # 1.1GB base image
WORKDIR /app
COPY package*.json .
RUN npm install                 # Includes devDependencies
COPY . .
RUN npm run build               # TypeScript compilation
CMD ["node", "dist/server.js"]
# Final image contains: node, npm, TypeScript compiler, all devDeps
# Size: ~1.2GB
```

**After multi-stage (good — 85MB image):**
```dockerfile
# ─── STAGE 1: Build ───────────────────────────────────────────
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci                      # Install ALL deps (including dev)
COPY . .
RUN npm run build               # Compile TypeScript → JavaScript

# ─── STAGE 2: Production ──────────────────────────────────────
FROM node:18-alpine AS production
WORKDIR /app
# Copy ONLY what's needed to run
COPY package*.json ./
RUN npm ci --only=production    # Production deps only
COPY --from=builder /app/dist ./dist    # Built output from stage 1

RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
USER nodejs

EXPOSE 3000
CMD ["node", "dist/server.js"]
# Final image contains: node runtime, prod deps, built JS only
# Size: ~85MB
```

**Java/Spring Boot multi-stage:**
```dockerfile
# ─── STAGE 1: Build with Maven ────────────────────────────────
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline -B    # Cache dependencies layer
COPY src ./src
RUN mvn package -DskipTests

# ─── STAGE 2: Runtime ─────────────────────────────────────────
FROM eclipse-temurin:17-jre-alpine AS production
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
# Maven (600MB) not in final image. Only JRE + JAR file.
```

**Go multi-stage (smallest possible — scratch base):**
```dockerfile
FROM golang:1.21-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Use scratch (empty image) — absolute minimum
FROM scratch
COPY --from=builder /app/main /main
EXPOSE 8080
CMD ["/main"]
# Final image: single binary. ~8MB total.
```

---

### 9.3 Layer Caching Optimisation

The order of Dockerfile instructions determines build cache efficiency. Put instructions that change rarely at the TOP. Put instructions that change often at the BOTTOM.

```dockerfile
# BAD — code changes invalidate dependency install every time
FROM node:18-alpine
WORKDIR /app
COPY . .                        # Code changes → cache miss
RUN npm ci                      # Reinstalls ALL deps every time!
CMD ["node", "server.js"]

# GOOD — dependency layer cached separately from code
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./           # Rarely changes → cache hit
RUN npm ci                      # Only runs when package.json changes
COPY . .                        # Code changes only invalidate this and below
CMD ["node", "server.js"]
```

**The Rule:** Arrange Dockerfile instructions from least-to-most-frequently-changing. Dependencies before code. System packages before app packages.

---

### 9.4 Image Size Reduction Techniques

```dockerfile
# 1. Use Alpine or slim base images
FROM node:18-alpine         # ~180MB
# NOT: FROM node:18         # ~1.1GB

# 2. Combine RUN commands into one layer
# BAD: 3 layers
RUN apt-get update
RUN apt-get install -y curl
RUN rm -rf /var/lib/apt/lists/*

# GOOD: 1 layer
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

# 3. Use --no-install-recommends
RUN apt-get install -y --no-install-recommends curl git

# 4. Clean up in the same RUN layer
RUN pip install --no-cache-dir -r requirements.txt

# 5. Remove unnecessary files in the same layer they're created
RUN curl -L https://example.com/installer.sh | bash && \
    rm -f installer.sh && \
    rm -rf /tmp/*
```

---

### 9.5 Hands-On Lab 9 — Multi-Stage Build

```bash
mkdir multistage-lab && cd multistage-lab

# Create a TypeScript app
cat > server.ts << 'EOF'
const http = require('http');
const server = http.createServer((req: any, res: any) => {
  res.writeHead(200, {'Content-Type': 'application/json'});
  res.end(JSON.stringify({ message: 'TypeScript compiled app', version: '1.0' }));
});
server.listen(3000, () => console.log('Server on 3000'));
EOF

cat > tsconfig.json << 'EOF'
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "strict": true
  },
  "include": ["*.ts"]
}
EOF

cat > package.json << 'EOF'
{
  "name": "multistage-demo",
  "scripts": { "build": "tsc" },
  "devDependencies": { "typescript": "^5.0.0", "@types/node": "^18.0.0" }
}
EOF

# Single stage Dockerfile (large)
cat > Dockerfile.large << 'EOF'
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npx tsc
CMD ["node", "dist/server.js"]
EOF

# Multi-stage Dockerfile (optimised)
cat > Dockerfile << 'EOF'
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npx tsc

FROM node:18-alpine AS production
WORKDIR /app
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/server.js"]
EOF

# Build both and compare sizes
docker build -t single-stage -f Dockerfile.large .
docker build -t multi-stage -f Dockerfile .

docker images | grep -E "single-stage|multi-stage"
# See the size difference!

# Run multi-stage version
docker run -d --name ms-test -p 3000:3000 multi-stage
curl http://localhost:3000

# Cleanup
docker stop ms-test && docker rm ms-test
cd .. && rm -rf multistage-lab
```

---

---

## Lesson 10 — Environment Variables, Health Checks, Resource Limits

> **Level:** Intermediate

---

### 10.1 Environment Variables

Environment variables are the standard way to configure containers without rebuilding images. They separate configuration from code.

```bash
# Set via docker run
docker run -e DB_HOST=mydb -e DB_PORT=5432 myapp

# Set from a file
docker run --env-file .env myapp

# In Dockerfile (sets default — overridable at runtime)
ENV NODE_ENV=production
ENV APP_PORT=3000

# In Docker Compose
services:
  api:
    environment:
      - NODE_ENV=production
      - DB_HOST=postgres
    env_file:
      - .env
```

**In application code (Node.js):**
```javascript
const dbHost = process.env.DB_HOST || 'localhost';
const dbPort = parseInt(process.env.DB_PORT) || 5432;
const debug = process.env.DEBUG === 'true';
```

> **Production Rule:** Never hardcode configuration in application code or Dockerfiles. Use environment variables for all configuration that changes between environments (dev/staging/production). Never put secrets in environment variables — use Key Vault or Docker Secrets (Lesson 15).

---

### 10.2 Health Checks

A health check tells Docker whether the container is working correctly. Docker uses this to decide if the container is ready to receive traffic and whether to restart it.

**Container states with health checks:**
- `starting` — health check running for the first time
- `healthy` — health check passing
- `unhealthy` — health check has failed N consecutive times

```dockerfile
# In Dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=15s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

# Options:
# --interval    How often to run the check (default: 30s)
# --timeout     How long to wait for response (default: 30s)
# --start-period Give app time to start before first check (default: 0s)
# --retries     Consecutive failures before 'unhealthy' (default: 3)
```

```bash
# In docker run
docker run -d \
  --health-cmd="curl -f http://localhost:80 || exit 1" \
  --health-interval=30s \
  --health-timeout=5s \
  --health-retries=3 \
  nginx

# Check health status
docker inspect --format '{{.State.Health.Status}}' my-container
docker inspect --format '{{json .State.Health}}' my-container | jq
```

---

### 10.3 Resource Limits

Without limits, a single misbehaving container can consume all host resources and bring down every other container on the host.

```bash
# Memory limit: container killed (OOM) if exceeds 512MB
docker run -d --memory 512m --memory-swap 512m nginx
# --memory-swap same as --memory means no swap allowed

# CPU limit: container gets max 0.5 CPU cores
docker run -d --cpus 0.5 nginx

# CPU shares: relative weight (default 1024)
docker run -d --cpu-shares 512 nginx    # half the default priority

# Combine limits
docker run -d \
  --name api \
  --memory 256m \
  --memory-swap 256m \
  --cpus 0.25 \
  myapp:latest

# In Docker Compose
services:
  api:
    deploy:
      resources:
        limits:
          memory: 512m
          cpus: '0.5'
        reservations:   # Guaranteed minimum
          memory: 128m
          cpus: '0.1'
```

> **Production Rule:** Always set memory limits on all containers. Without them, a memory leak in one container can trigger the OOM killer on the host, killing random processes. Start generous (2x expected usage), monitor actual consumption, then tune.

---

---

## Lesson 11 — Private Container Registries

> **Level:** Intermediate

---

### 11.1 Why Private Registries?

Docker Hub is public by default. In production you need private registries because:
- Your application images contain proprietary code
- You need control over what images are allowed in production
- Faster pulls from a registry in the same cloud region
- Compliance requirements (no images stored outside your cloud)

**Options:**
- **Docker Hub Private Repos** — easiest, limited free tier
- **Azure Container Registry (ACR)** — for Azure deployments
- **AWS ECR** — for AWS deployments
- **GitHub Container Registry (GHCR)** — integrated with GitHub Actions
- **Harbor** — self-hosted, open source

---

### 11.2 Azure Container Registry

```bash
# Create ACR
az acr create \
  --resource-group myapp-rg \
  --name mycompanyacr \
  --sku Standard

# Login to ACR
az acr login --name mycompanyacr

# Build and push using ACR Tasks (builds in Azure — no local Docker needed)
az acr build \
  --registry mycompanyacr \
  --image myapp:v1.0 \
  .

# Tag local image for ACR
docker tag myapp:v1.0 mycompanyacr.azurecr.io/myapp:v1.0

# Push
docker push mycompanyacr.azurecr.io/myapp:v1.0

# Pull
docker pull mycompanyacr.azurecr.io/myapp:v1.0

# List images in ACR
az acr repository list --name mycompanyacr --output table
az acr repository show-tags --name mycompanyacr --repository myapp --output table

# Enable image scanning
az acr update --name mycompanyacr --enable-content-trust
```

---

### 11.3 GitHub Container Registry (GHCR)

```bash
# Login with GitHub personal access token
echo $GITHUB_TOKEN | docker login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin

# Tag and push
docker tag myapp:v1.0 ghcr.io/your-org/myapp:v1.0
docker push ghcr.io/your-org/myapp:v1.0
```

**In GitHub Actions:**
```yaml
- name: Login to GHCR
  uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}  # Automatically provided

- name: Build and push
  uses: docker/build-push-action@v5
  with:
    push: true
    tags: ghcr.io/${{ github.repository }}/myapp:${{ github.sha }}
```

---

---

# PHASE 3 — DEVOPS LEVEL

---

## Lesson 12 — Docker in CI/CD Pipelines

> **Level:** DevOps Engineer
> **Goal:** Integrate Docker into real CI/CD workflows

---

### 12.1 The Docker CI/CD Pattern

Every professional CI/CD pipeline involving Docker follows this pattern:

```
Code Push to Git
       │
       ▼
CI Pipeline triggers
       │
  ┌────▼────┐
  │  TEST   │  Run unit tests (optionally in Docker)
  └────┬────┘
       │
  ┌────▼────┐
  │  BUILD  │  docker build -t myapp:${SHA} .
  └────┬────┘
       │
  ┌────▼────┐
  │  SCAN   │  Scan image for vulnerabilities
  └────┬────┘
       │
  ┌────▼────┐
  │  PUSH   │  Push image to registry
  └────┬────┘
       │
  ┌────▼───────┐
  │  DEPLOY   │  Pull new image, update containers
  └────────────┘
```

---

### 12.2 GitHub Actions — Complete Docker Pipeline

**.github/workflows/docker.yml:**
```yaml
name: Docker Build and Deploy

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}/myapp

jobs:
  # ─── JOB 1: Run tests ─────────────────────────────────────────────
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run tests in Docker
        run: |
          docker build --target test -t myapp:test .
          docker run --rm myapp:test npm test

  # ─── JOB 2: Build and push image ─────────────────────────────────
  build:
    name: Build and Push
    runs-on: ubuntu-latest
    needs: test
    permissions:
      contents: read
      packages: write
    outputs:
      image-digest: ${{ steps.build.outputs.digest }}
      image-tag: ${{ steps.meta.outputs.tags }}

    steps:
      - uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to registry
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=sha,prefix=,suffix=,format=short
            type=ref,event=branch
            type=semver,pattern={{version}}

      - name: Build and push
        id: build
        uses: docker/build-push-action@v5
        with:
          context: .
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha        # GitHub Actions cache
          cache-to: type=gha,mode=max

  # ─── JOB 3: Scan for vulnerabilities ─────────────────────────────
  scan:
    name: Security Scan
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ needs.build.outputs.image-tag }}
          format: 'sarif'
          output: 'trivy-results.sarif'
          severity: 'CRITICAL,HIGH'
          exit-code: '1'       # Fail pipeline on critical vulnerabilities

      - name: Upload scan results
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: 'trivy-results.sarif'

  # ─── JOB 4: Deploy to production ─────────────────────────────────
  deploy:
    name: Deploy
    runs-on: ubuntu-latest
    needs: [build, scan]
    if: github.ref == 'refs/heads/main'
    environment: production

    steps:
      - name: Deploy to Azure Container Apps
        uses: azure/login@v2
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      - run: |
          az containerapp update \
            --name myapp \
            --resource-group myapp-rg \
            --image ${{ needs.build.outputs.image-tag }}
```

---

### 12.3 Azure DevOps Pipeline

```yaml
trigger:
  branches:
    include: [main]

variables:
  imageRepository: 'myapp'
  containerRegistry: 'mycompanyacr.azurecr.io'
  dockerfilePath: '$(Build.SourcesDirectory)/Dockerfile'
  tag: '$(Build.BuildId)'

stages:
  - stage: Build
    jobs:
      - job: Docker
        pool:
          vmImage: ubuntu-latest
        steps:
          - task: Docker@2
            displayName: Build image
            inputs:
              command: build
              repository: $(imageRepository)
              dockerfile: $(dockerfilePath)
              containerRegistry: MyACRServiceConnection
              tags: |
                $(tag)
                latest

          - task: Docker@2
            displayName: Push image
            inputs:
              command: push
              repository: $(imageRepository)
              containerRegistry: MyACRServiceConnection
              tags: |
                $(tag)
                latest

          - task: AquaSecurityTrivy@1
            displayName: Scan image
            inputs:
              image: $(containerRegistry)/$(imageRepository):$(tag)
              severity: HIGH,CRITICAL
              exitCode: 1
```

---

---

## Lesson 13 — Docker Security Best Practices

> **Level:** DevOps Engineer
> **Goal:** Harden containers for production

---

### 13.1 The Security Threat Model for Containers

Containers share the host OS kernel. If an attacker breaks out of a container and gains root on the container — and the container is running as root — they may be able to escalate to the host.

**Security layers to defend:**

```
Application vulnerabilities (your code)
       │
Image vulnerabilities (outdated base image packages)
       │
Container runtime security (running as root)
       │
Host kernel security (namespace escape)
       │
Network security (unnecessary exposed ports)
```

---

### 13.2 Never Run as Root

```dockerfile
# BAD — container runs as root
FROM node:18-alpine
WORKDIR /app
COPY . .
CMD ["node", "server.js"]
# Process runs as UID 0 (root) — if compromised, attacker has root in container

# GOOD — create and use non-root user
FROM node:18-alpine
WORKDIR /app
COPY . .

RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001 && \
    chown -R nodejs:nodejs /app

USER nodejs          # Switch to non-root
CMD ["node", "server.js"]
# Process runs as UID 1001 — limited blast radius if compromised
```

---

### 13.3 Use Read-Only Filesystems

```bash
# Make container filesystem read-only
# App cannot write anywhere except explicitly allowed volumes
docker run -d \
  --read-only \
  --tmpfs /tmp \              # Allow writes to /tmp (in RAM)
  --tmpfs /var/run \          # Allow writes to /var/run
  -v app-data:/app/data \     # Named volume for persistent data
  myapp

# In Docker Compose
services:
  api:
    read_only: true
    tmpfs:
      - /tmp
      - /var/run
```

---

### 13.4 Drop Linux Capabilities

Linux capabilities break up root privileges into distinct units. Drop all capabilities and add back only what's needed.

```bash
docker run -d \
  --cap-drop ALL \            # Drop ALL capabilities
  --cap-add NET_BIND_SERVICE \ # Allow binding to port < 1024
  --security-opt no-new-privileges \  # Prevent privilege escalation
  --read-only \
  myapp
```

---

### 13.5 Image Scanning with Trivy

```bash
# Install Trivy
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin

# Scan an image
trivy image nginx:latest

# Scan only for HIGH and CRITICAL
trivy image --severity HIGH,CRITICAL nginx:latest

# Scan your custom image
trivy image myapp:v1.0

# Output in JSON for CI/CD processing
trivy image --format json --output results.json myapp:v1.0

# Fail (exit 1) if any CRITICAL vulnerabilities found
trivy image --exit-code 1 --severity CRITICAL myapp:v1.0
```

---

### 13.6 Security Checklist

```
□ Base image uses a specific version tag (not :latest)
□ Base image is official or from a verified publisher
□ Base image is Alpine or slim to minimise attack surface
□ Container runs as non-root user
□ .dockerignore excludes .git, .env, credentials, test files
□ No secrets in Dockerfile or image layers
□ COPY preferred over ADD (no remote URL fetching)
□ Image scanned with Trivy or Snyk before deployment
□ Read-only filesystem enabled where possible
□ Unnecessary Linux capabilities dropped
□ No SSH server in containers (use docker exec instead)
□ Resource limits set (memory, CPU)
□ Health checks configured
□ Ports explicitly declared with EXPOSE
```

---

---

## Lesson 14 — Docker Logging Strategies

> **Level:** DevOps Engineer

---

### 14.1 Docker Logging Drivers

By default Docker captures stdout/stderr from containers and stores them as JSON files on the host. In production, you need logs to go somewhere centralised.

| Driver | Description | Use Case |
|---|---|---|
| `json-file` | Default. JSON files on host. | Dev, small deployments |
| `syslog` | Forward to syslog daemon | Traditional Linux infra |
| `journald` | Forward to systemd journal | systemd-based hosts |
| `fluentd` | Forward to Fluentd collector | EFK stack (Elastic+Fluent+Kibana) |
| `awslogs` | Forward to AWS CloudWatch | AWS deployments |
| `splunk` | Forward to Splunk | Enterprise Splunk installations |
| `none` | Disables logging | Security-sensitive containers |

---

### 14.2 Configuring Logging

```bash
# Per-container logging driver
docker run -d \
  --log-driver json-file \
  --log-opt max-size=10m \      # Rotate when file reaches 10MB
  --log-opt max-file=3 \        # Keep 3 rotated files (30MB max total)
  --name api \
  myapp

# Global default in /etc/docker/daemon.json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  }
}
sudo systemctl restart docker
```

**In Docker Compose:**
```yaml
services:
  api:
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
        labels: "service=api,env=production"
```

---

### 14.3 Centralised Logging with Fluentd (EFK Stack)

**The EFK Stack Architecture:**
```
Container logs (stdout/stderr)
         │
         ▼ Docker fluentd logging driver
      Fluentd (collector and forwarder)
         │
         ▼
  Elasticsearch (storage and indexing)
         │
         ▼
    Kibana (search, dashboards, alerts)
```

```yaml
# docker-compose.yml with EFK logging
version: "3.9"
services:
  api:
    image: myapp:latest
    logging:
      driver: fluentd
      options:
        fluentd-address: localhost:24224
        tag: myapp.api
        fluentd-async-connect: "true"

  fluentd:
    image: fluent/fluentd:v1.16-debian-1
    volumes:
      - ./fluentd/fluent.conf:/fluentd/etc/fluent.conf
    ports:
      - "24224:24224"
      - "24224:24224/udp"

  elasticsearch:
    image: elasticsearch:8.11.0
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
    volumes:
      - es-data:/usr/share/elasticsearch/data

  kibana:
    image: kibana:8.11.0
    ports:
      - "5601:5601"
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200

volumes:
  es-data:
```

---

---

## Lesson 15 — Docker Secrets Management

> **Level:** DevOps Engineer

---

### 15.1 The Problem with Environment Variables for Secrets

Environment variables are visible in:
- `docker inspect` output
- Process listings (`/proc/<pid>/environ`)
- Log files (if your app accidentally logs env vars)
- CI/CD pipeline logs

For truly sensitive data (database master passwords, TLS private keys, API keys), Docker Secrets or an external secrets manager is needed.

---

### 15.2 Docker Secrets (Swarm Mode)

Docker Secrets is built into Docker Swarm and stores secrets encrypted.

```bash
# Create a secret from stdin (never put secrets in files you commit)
echo "my-super-secret-db-password" | docker secret create db_password -

# Create from a file
docker secret create tls_cert ./cert.pem

# List secrets (shows names only — values never retrievable)
docker secret ls

# Use in a Swarm service
docker service create \
  --name myapp \
  --secret db_password \    # Mounted at /run/secrets/db_password
  --secret tls_cert \
  myapp:latest
```

**In application code:**
```python
import os

def get_secret(name):
    """Read Docker secret or fall back to env var"""
    secret_file = f"/run/secrets/{name}"
    if os.path.exists(secret_file):
        with open(secret_file, 'r') as f:
            return f.read().strip()
    return os.environ.get(name.upper())

db_password = get_secret('db_password')
```

---

### 15.3 Docker Compose Secrets (Development)

```yaml
version: "3.9"
services:
  api:
    image: myapp
    secrets:
      - db_password
      - api_key
    environment:
      - DB_PASSWORD_FILE=/run/secrets/db_password   # App reads from file

secrets:
  db_password:
    file: ./secrets/db_password.txt    # Dev only — don't commit
  api_key:
    external: true                     # Production: managed externally
```

> **Production Pattern:** In production on Kubernetes or Azure Container Apps, use the platform's secret management (Kubernetes Secrets, Azure Key Vault). Never commit secret values to Git, even in `.env` files.

---

---

## Lesson 16 — Docker Build Caching and Optimisation

> **Level:** DevOps Engineer

---

### 16.1 How Docker Build Cache Works

Docker caches each layer. When rebuilding, Docker checks if anything that would affect a layer has changed. If not, it reuses the cached layer.

**Cache invalidation rules:**
- `FROM` — invalidated if base image digest changes
- `RUN` — invalidated if the command string changes
- `COPY`/`ADD` — invalidated if any copied file's content changes
- Once a layer's cache is invalid, ALL subsequent layers must rebuild

---

### 16.2 BuildKit — Modern Build Engine

Enable Docker BuildKit for faster, smarter builds:

```bash
# Enable for one build
DOCKER_BUILDKIT=1 docker build -t myapp .

# Enable permanently (add to ~/.bashrc or ~/.zshrc)
export DOCKER_BUILDKIT=1

# Or enable in Docker daemon config /etc/docker/daemon.json:
{ "features": { "buildkit": true } }
```

**BuildKit features:**
- Parallel build stages
- Improved cache import/export
- Secret mounts (secrets available during build, not in image)
- SSH forwarding (clone private repos during build)
- Better output and progress display

---

### 16.3 Registry-Based Cache (CI/CD)

In CI/CD, each build starts fresh with no local cache. Use registry-based cache to persist across runs:

```dockerfile
# Dockerfile with cache mount (BuildKit only)
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./

# Cache mount: npm cache persists between builds without being in the image
RUN --mount=type=cache,target=/root/.npm \
    npm ci

COPY . .
CMD ["node", "server.js"]
```

```bash
# GitHub Actions with registry cache
docker build \
  --cache-from type=registry,ref=myregistry.azurecr.io/myapp:cache \
  --cache-to type=registry,ref=myregistry.azurecr.io/myapp:cache,mode=max \
  -t myregistry.azurecr.io/myapp:v1.0 \
  .
```

---

---

## Lesson 17 — Docker Swarm

> **Level:** DevOps Engineer
> **Goal:** Understand Docker's native clustering and orchestration

---

### 17.1 What is Docker Swarm?

Docker Swarm is Docker's built-in container orchestration. It clusters multiple Docker hosts into a single logical Docker host. You deploy services and Swarm handles placing containers across nodes.

> **Swarm vs Kubernetes:** Swarm is simpler. Kubernetes is more powerful and the industry standard. Learn Swarm to understand orchestration concepts. Use Kubernetes in production at scale.

**Swarm Concepts:**

| Concept | Description |
|---|---|
| **Node** | A Docker host in the Swarm (manager or worker) |
| **Manager node** | Controls the Swarm, schedules tasks |
| **Worker node** | Runs containers, no orchestration role |
| **Service** | The desired state: run N replicas of an image |
| **Task** | A single container instance within a service |
| **Stack** | A group of services deployed together (like Compose) |

---

### 17.2 Setting Up a Swarm

```bash
# Initialise Swarm on the first manager node
docker swarm init --advertise-addr <MANAGER-IP>
# This outputs a token to join worker nodes

# Join a worker node (run on worker machines)
docker swarm join \
  --token <TOKEN-FROM-INIT> \
  <MANAGER-IP>:2377

# List all nodes
docker node ls

# Promote a worker to manager (for HA — always have odd number of managers)
docker node promote worker-node-1
```

---

### 17.3 Deploying Services

```bash
# Deploy a replicated service
docker service create \
  --name api \
  --replicas 3 \
  --publish published=3000,target=3000 \
  --update-delay 10s \
  --update-parallelism 1 \
  myapp:v1.0

# List services
docker service ls

# See which node each task is running on
docker service ps api

# Scale up/down
docker service scale api=5

# Rolling update (one container at a time, 10s delay between)
docker service update \
  --image myapp:v2.0 \
  --update-delay 10s \
  --update-parallelism 1 \
  api

# Rollback if something goes wrong
docker service rollback api
```

---

### 17.4 Docker Stack — Compose for Swarm

```bash
# Deploy a Compose file as a Swarm stack
docker stack deploy -c docker-compose.yml myapp

# List stacks
docker stack ls

# List services in a stack
docker stack services myapp

# Remove a stack
docker stack rm myapp
```


---

---

## Lesson 18 — Docker with Kubernetes

> **Level:** DevOps Engineer → Senior
> **Goal:** Understand how Docker and Kubernetes work together

---

### 18.1 Docker's Role in a Kubernetes World

A common misconception: "Kubernetes replaced Docker." Not quite.

**What actually happened:**
- Kubernetes originally used Docker as its container runtime
- Kubernetes later standardised on the **Container Runtime Interface (CRI)**
- Docker doesn't implement CRI directly — Kubernetes uses **containerd** (which Docker is built on)
- `docker build` and Docker images are still the standard way to build containers
- Kubernetes runs those images — it just doesn't use the Docker daemon to do so

```
You write code
      │
docker build → creates an OCI-standard container image
      │
docker push  → image stored in registry
      │
Kubernetes pulls the image from registry
      │
containerd (or another CRI runtime) runs the container
```

> **The practical reality:** You still use Docker (or `docker build`) to build and test images locally. Kubernetes runs those images in production. The two work together — they are not mutually exclusive.

---

### 18.2 Local Kubernetes with Docker Desktop

Docker Desktop includes a single-node Kubernetes cluster.

```bash
# Enable Kubernetes in Docker Desktop settings
# Docker Desktop → Settings → Kubernetes → Enable Kubernetes

# Verify
kubectl version --client
kubectl cluster-info
kubectl get nodes

# Run a container in Kubernetes (same Docker image)
kubectl run nginx --image=nginx:alpine
kubectl get pods
kubectl describe pod nginx
kubectl logs nginx

# Expose it
kubectl expose pod nginx --type=NodePort --port=80
kubectl get services

# Delete
kubectl delete pod nginx
kubectl delete service nginx
```

---

### 18.3 Dockerfile → Kubernetes Deployment

```bash
# Step 1: Build and push image (standard Docker workflow)
docker build -t mycompanyacr.azurecr.io/myapp:v1.0 .
docker push mycompanyacr.azurecr.io/myapp:v1.0

# Step 2: Create Kubernetes deployment from that image
cat > deployment.yaml << 'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: mycompanyacr.azurecr.io/myapp:v1.0
          ports:
            - containerPort: 3000
          resources:
            limits:
              memory: 256Mi
              cpu: 500m
          livenessProbe:      # Same concept as Docker HEALTHCHECK
            httpGet:
              path: /health
              port: 3000
            initialDelaySeconds: 15
EOF

kubectl apply -f deployment.yaml
kubectl get pods
kubectl rollout status deployment/myapp
```

---

---

## Lesson 19 — Docker with Terraform

> **Level:** DevOps Engineer
> **Goal:** Provision container infrastructure with Terraform

---

### 19.1 Docker Provider for Terraform

Terraform can manage Docker resources — images, containers, networks, volumes — using the Docker provider.

```hcl
# main.tf
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0"
    }
  }
}

provider "docker" {}

# Pull an image
resource "docker_image" "nginx" {
  name         = "nginx:1.25-alpine"
  keep_locally = false
}

# Create a network
resource "docker_network" "app_net" {
  name   = "app-net"
  driver = "bridge"
}

# Create a volume
resource "docker_volume" "app_data" {
  name = "app-data"
}

# Run a container
resource "docker_container" "nginx" {
  name  = "nginx-tf"
  image = docker_image.nginx.image_id

  ports {
    internal = 80
    external = 8080
  }

  networks_advanced {
    name = docker_network.app_net.name
  }

  volumes {
    volume_name    = docker_volume.app_data.name
    container_path = "/usr/share/nginx/html"
  }

  restart = "unless-stopped"

  healthcheck {
    test         = ["CMD", "curl", "-f", "http://localhost"]
    interval     = "30s"
    timeout      = "5s"
    retries      = 3
    start_period = "10s"
  }
}

output "container_ip" {
  value = docker_container.nginx.network_data[0].ip_address
}
```

```bash
terraform init
terraform plan
terraform apply
terraform destroy
```

---

### 19.2 Terraform — Provision Azure Container Apps

```hcl
# Provision Azure infrastructure that runs containers
resource "azurerm_resource_group" "main" {
  name     = "myapp-rg"
  location = "centralindia"
}

resource "azurerm_container_registry" "main" {
  name                = "mycompanyacr"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  sku                 = "Standard"
  admin_enabled       = false
}

resource "azurerm_container_app_environment" "main" {
  name                = "myapp-env"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name
}

resource "azurerm_container_app" "api" {
  name                         = "api"
  container_app_environment_id = azurerm_container_app_environment.main.id
  resource_group_name          = azurerm_resource_group.main.name
  revision_mode                = "Single"

  template {
    container {
      name   = "api"
      image  = "${azurerm_container_registry.main.login_server}/myapp:latest"
      cpu    = 0.5
      memory = "1Gi"

      env {
        name  = "NODE_ENV"
        value = "production"
      }
    }
    min_replicas = 1
    max_replicas = 10
  }

  ingress {
    external_enabled = true
    target_port      = 3000
  }
}
```

---

---

# PHASE 4 — ADVANCED ARCHITECTURE

---

## Lesson 20 — Microservices Architecture with Docker

> **Level:** Advanced
> **Goal:** Design and run a real microservices system with Docker

---

### 20.1 Microservices vs Monolith

**Monolith:** All functionality in one deployable unit.
```
Single App
├── User service code
├── Order service code
├── Payment service code
└── Notification service code
All deployed together. One database. One process.
```

**Microservices:** Each service is independent, deployed separately.
```
user-service   → own database, own container, own repo
order-service  → own database, own container, own repo
payment-service → own database, own container, own repo
notification-service → own database, own container, own repo
```

**Benefits of microservices:**
- Independent deployments (change payment service without redeploying user service)
- Independent scaling (scale only the overloaded service)
- Technology freedom (Python for ML service, Go for performance-critical service)
- Fault isolation (payment service outage doesn't bring down user accounts)

**Costs of microservices:**
- Network latency between services
- Distributed system complexity (transactions, eventual consistency)
- More infrastructure to manage
- Requires mature DevOps practices

> **Real-World Guidance:** Don't start with microservices. Build a monolith first, identify natural service boundaries after you understand the domain, then extract services. Premature microservices decomposition is one of the most common architecture mistakes.

---

### 20.2 Inter-Service Communication Patterns

**Synchronous (REST/gRPC):**
```
Order Service → HTTP POST → Payment Service
                           → waits for response
```
Use when: You need the response immediately to continue.

**Asynchronous (Message Queue):**
```
Order Service → publishes "order.placed" event → Message Queue
                                                      ↓
                                             Payment Service subscribes
                                             Notification Service subscribes
                                             Inventory Service subscribes
```
Use when: You don't need an immediate response, and multiple services might need the event.

---

### 20.3 Complete Microservices Docker Compose

```yaml
version: "3.9"

services:
  # ─── API Gateway ──────────────────────────────────────────────────
  gateway:
    image: nginx:1.25-alpine
    ports:
      - "80:80"
    volumes:
      - ./gateway/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - user-service
      - order-service
    networks:
      - public
    restart: unless-stopped

  # ─── User Service ─────────────────────────────────────────────────
  user-service:
    build: ./services/user
    expose:
      - "3001"
    environment:
      - DB_HOST=user-db
      - REDIS_HOST=redis
    depends_on:
      user-db:
        condition: service_healthy
    networks:
      - public
      - user-net
    restart: unless-stopped

  user-db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=users
      - POSTGRES_USER=user_svc
      - POSTGRES_PASSWORD=${USER_DB_PASSWORD}
    volumes:
      - user-db-data:/var/lib/postgresql/data
    networks:
      - user-net   # ONLY accessible from user-service
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user_svc -d users"]
      interval: 10s
      retries: 5

  # ─── Order Service ────────────────────────────────────────────────
  order-service:
    build: ./services/order
    expose:
      - "3002"
    environment:
      - DB_HOST=order-db
      - MESSAGE_QUEUE_HOST=rabbitmq
    depends_on:
      order-db:
        condition: service_healthy
      rabbitmq:
        condition: service_healthy
    networks:
      - public
      - order-net
      - messaging
    restart: unless-stopped

  order-db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=orders
      - POSTGRES_USER=order_svc
      - POSTGRES_PASSWORD=${ORDER_DB_PASSWORD}
    volumes:
      - order-db-data:/var/lib/postgresql/data
    networks:
      - order-net   # Isolated — only order-service can reach it
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U order_svc -d orders"]
      interval: 10s
      retries: 5

  # ─── Notification Service ─────────────────────────────────────────
  notification-service:
    build: ./services/notification
    environment:
      - MESSAGE_QUEUE_HOST=rabbitmq
      - SMTP_HOST=${SMTP_HOST}
    depends_on:
      rabbitmq:
        condition: service_healthy
    networks:
      - messaging
    restart: unless-stopped

  # ─── Message Queue ────────────────────────────────────────────────
  rabbitmq:
    image: rabbitmq:3.12-management-alpine
    environment:
      - RABBITMQ_DEFAULT_USER=admin
      - RABBITMQ_DEFAULT_PASS=${RABBITMQ_PASSWORD}
    ports:
      - "15672:15672"  # Management UI (dev only)
    volumes:
      - rabbitmq-data:/var/lib/rabbitmq
    networks:
      - messaging
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "rabbitmq-diagnostics", "ping"]
      interval: 30s
      retries: 5

  # ─── Shared Redis Cache ───────────────────────────────────────────
  redis:
    image: redis:7-alpine
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis-data:/data
    networks:
      - public
    restart: unless-stopped

volumes:
  user-db-data:
  order-db-data:
  rabbitmq-data:
  redis-data:

networks:
  public:       # Services accessible from gateway
  user-net:     # Isolated network for user service and its DB
  order-net:    # Isolated network for order service and its DB
  messaging:    # Message queue network
```

> **Security Design:** Each database is on an isolated network accessible only by its service. `user-db` cannot be reached from `order-service`. This limits the blast radius of a compromised service — an attacker in `order-service` cannot read the users database.

---

---

## Lesson 21 — Scalable Container Platforms

> **Level:** Advanced

---

### 21.1 Horizontal vs Vertical Scaling

**Vertical Scaling (scale up):** Give the container more CPU and RAM. Simple, but has hard limits. Requires downtime to resize.

**Horizontal Scaling (scale out):** Run more container instances behind a load balancer. Preferred for stateless apps. Linear scaling potential.

```
Vertical:          Horizontal:
Container          Container Container Container
[2CPU, 4GB]  →    [1CPU,2GB] [1CPU,2GB] [1CPU,2GB]
                       ↑ Load Balancer ↑
```

**Stateless vs Stateful containers:**
- **Stateless** (API servers, web frontends): Scale horizontally freely — all instances identical
- **Stateful** (databases): Horizontal scaling is complex — requires replication, partitioning, consistency management

---

### 21.2 Service Discovery

When containers scale, their IPs change. Services need to find each other dynamically.

**DNS-based discovery (Docker/Kubernetes built-in):**
```
Order service wants to call user service
  └── DNS lookup: user-service → 10.0.1.45:3001
  └── If order-service has 3 replicas, DNS round-robins between all three
```

**Health check integration:**
- Load balancer continuously health-checks all instances
- Unhealthy instance removed from DNS/load balancer rotation
- New healthy instance automatically added to rotation

---

---

## Lesson 22 — Observability and Monitoring for Containers

> **Level:** Advanced

---

### 22.1 The Three Pillars of Observability

```
OBSERVABILITY
      │
  ┌───┼───┐
  ▼   ▼   ▼
Logs Metrics Traces

Logs:    What happened? (Events, errors, state changes)
Metrics: How is it performing? (CPU, memory, requests/sec, latency)
Traces:  Why did it take so long? (Request path through services)
```

---

### 22.2 Prometheus + Grafana Stack

The industry standard for container monitoring.

```yaml
version: "3.9"
services:
  # Your application
  api:
    build: ./api
    ports:
      - "3000:3000"
    networks:
      - monitoring

  # Prometheus: scrapes metrics from containers
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prometheus-data:/prometheus
    ports:
      - "9090:9090"
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.retention.time=15d'
    networks:
      - monitoring

  # Grafana: dashboards on top of Prometheus
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000"
    volumes:
      - grafana-data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_PASSWORD}
    networks:
      - monitoring

  # Node Exporter: exposes host machine metrics to Prometheus
  node-exporter:
    image: prom/node-exporter:latest
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.sysfs=/host/sys'
    networks:
      - monitoring

  # cAdvisor: exposes container metrics to Prometheus
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    privileged: true
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
    ports:
      - "8080:8080"
    networks:
      - monitoring

volumes:
  prometheus-data:
  grafana-data:

networks:
  monitoring:
```

**prometheus.yml:**
```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'docker-containers'
    static_configs:
      - targets: ['cadvisor:8080']

  - job_name: 'host-metrics'
    static_configs:
      - targets: ['node-exporter:9100']

  - job_name: 'api'
    static_configs:
      - targets: ['api:3000']
    metrics_path: '/metrics'
```

---

---

# PHASE 5 — EXPERT LEVEL

---

## Lesson 23 — Docker Internals — Namespaces, Cgroups, Overlay FS

> **Level:** Expert

---

### 23.1 Linux Namespaces — How Isolation Works

Linux namespaces are the kernel feature that makes containers possible. Each namespace type isolates a different aspect of the system.

| Namespace | What It Isolates | Container Impact |
|---|---|---|
| `pid` | Process IDs | Container sees only its own processes (PID 1 = container init) |
| `net` | Network stack | Container gets its own network interfaces, IP, routing table |
| `mnt` | Mount points | Container sees its own filesystem tree |
| `uts` | Hostname | Container has its own hostname |
| `ipc` | IPC (shared memory, semaphores) | Container isolated from host IPC |
| `user` | User/Group IDs | Container UIDs mapped differently from host |
| `cgroup` | cgroup membership | Container sees only its own resource limits |

**Seeing namespaces in action:**
```bash
# Run a container and inspect its PID namespace
docker run -d --name demo nginx

# See the container's PID 1 from the HOST
CONTAINER_PID=$(docker inspect demo --format '{{.State.Pid}}')
echo "Container main process PID on host: $CONTAINER_PID"

# List processes FROM THE CONTAINER'S PERSPECTIVE
docker exec demo ps aux
# PID 1 = nginx master (inside the container's PID namespace)

# See ALL namespaces of the container process
sudo ls -la /proc/$CONTAINER_PID/ns/

# Enter the container's network namespace directly (without docker exec)
sudo nsenter -t $CONTAINER_PID --net ip addr
# Shows the same network config as: docker exec demo ip addr
```

---

### 23.2 Control Groups (Cgroups) — How Resource Limits Work

Cgroups (Control Groups) are the Linux kernel feature that limits and tracks resource usage for groups of processes.

```bash
# Run a container with memory limit
docker run -d --name limited --memory 100m nginx

# Find the container's cgroup
CONTAINER_PID=$(docker inspect limited --format '{{.State.Pid}}')

# See memory limit set by Docker via cgroups
cat /sys/fs/cgroup/memory/docker/$(docker inspect limited --format '{{.Id}}')/memory.limit_in_bytes
# Output: 104857600  (= 100 * 1024 * 1024 = 100MB)

# Current memory usage
cat /sys/fs/cgroup/memory/docker/$(docker inspect limited --format '{{.Id}}')/memory.usage_in_bytes

# CPU shares
cat /sys/fs/cgroup/cpu/docker/$(docker inspect limited --format '{{.Id}}')/cpu.shares
```

---

### 23.3 Union File Systems and Image Layers

Docker uses a **union filesystem** (OverlayFS on modern Linux) to implement image layers.

```
CONTAINER WRITABLE LAYER  ← Everything written at runtime goes here
─────────────────────────────────────────────────────────────────
IMAGE LAYER 5: COPY app.js          (read-only)
IMAGE LAYER 4: RUN npm install      (read-only)
IMAGE LAYER 3: COPY package.json    (read-only)
IMAGE LAYER 2: RUN apt-get update   (read-only)
IMAGE LAYER 1: FROM node:18-alpine  (read-only, shared across all node images)
```

**How OverlayFS works:**
- **Lower layers (lowerdir):** Read-only image layers
- **Upper layer (upperdir):** Container's writable layer
- **Merged view (merged):** What the container sees — lower + upper combined

```bash
# See OverlayFS mounts for a running container
docker inspect my-container --format '{{json .GraphDriver}}' | jq

# On the host, see the actual overlay mount
CONTAINER_ID=$(docker inspect my-container --format '{{.Id}}')
cat /proc/mounts | grep $CONTAINER_ID
# Shows: overlay / overlay rw,lowerdir=.../layer5:.../layer4:...,upperdir=.../upper,...
```

**Copy-on-Write (CoW):**
When a container writes to a file that exists in a read-only layer:
1. The file is copied from the lower layer to the writable upper layer
2. The copy is modified
3. The container sees the modified version (upper layer takes precedence)
4. The original in the lower layer is unchanged — other containers sharing that layer see the original

---

### 23.4 Container Runtime Architecture

```
Docker CLI (docker)
       │ REST API
       ▼
Docker Daemon (dockerd)
       │ gRPC
       ▼
containerd (high-level runtime)
  ├── Image management (pull, push, store)
  ├── Snapshot management (OverlayFS)
  └── Container lifecycle management
       │ OCI runtime spec
       ▼
runc (low-level OCI runtime)
  ├── Creates namespaces
  ├── Configures cgroups
  ├── Sets up OverlayFS mount
  └── Executes container process
```

> **Why this matters:** Understanding this stack helps you debug deep container issues. If `docker` CLI hangs but containers keep running, the problem is in `dockerd`. If containers fail to start, it might be `runc` or a namespace/cgroup issue. `crictl` can interact with `containerd` directly when debugging Kubernetes.

---

---

## Lesson 24 — Container Security Hardening

> **Level:** Expert

---

### 24.1 Seccomp Profiles

Seccomp (Secure Computing Mode) restricts which system calls a container can make. Containers don't need access to all 300+ Linux syscalls.

```bash
# Check if seccomp is enabled
docker info | grep seccomp

# Run with default seccomp profile (blocks ~44 dangerous syscalls)
docker run --security-opt seccomp=/etc/docker/seccomp/default.json nginx

# Run without seccomp (for debugging only — never in production)
docker run --security-opt seccomp=unconfined nginx

# Custom seccomp profile — allow only what your app needs
cat > custom-seccomp.json << 'EOF'
{
  "defaultAction": "SCMP_ACT_ERRNO",
  "syscalls": [
    {
      "names": ["read", "write", "open", "close", "stat", "fstat",
                "mmap", "mprotect", "munmap", "brk", "rt_sigaction",
                "rt_sigprocmask", "ioctl", "access", "execve", "exit",
                "getdents", "arch_prctl", "set_tid_address", "openat",
                "newfstatat", "exit_group", "futex", "socket", "connect",
                "accept4", "sendto", "recvfrom", "setsockopt", "getsockname",
                "clone", "fork", "wait4", "kill", "fcntl", "lstat"],
      "action": "SCMP_ACT_ALLOW"
    }
  ]
}
EOF
docker run --security-opt seccomp=custom-seccomp.json myapp
```

---

### 24.2 AppArmor Profiles

AppArmor provides mandatory access control (MAC) — fine-grained control over what files, capabilities, and network operations a container can perform.

```bash
# Check AppArmor status
sudo aa-status
docker info | grep apparmor

# Docker installs a default AppArmor profile: docker-default
# You can create custom profiles for enhanced security

# Example custom profile
cat > /etc/apparmor.d/docker-myapp << 'EOF'
#include <tunables/global>
profile docker-myapp flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/base>

  network tcp,
  network udp,

  /app/** r,
  /app/server.js r,
  /tmp/** rw,

  deny /etc/shadow r,
  deny /etc/passwd w,
  deny /proc/*/mem rw,
}
EOF

sudo apparmor_parser -r -W /etc/apparmor.d/docker-myapp
docker run --security-opt apparmor=docker-myapp myapp
```

---

### 24.3 Complete Production Security Configuration

```bash
docker run -d \
  --name hardened-app \
  \
  # Run as non-root user
  --user 1001:1001 \
  \
  # Read-only root filesystem
  --read-only \
  --tmpfs /tmp:rw,noexec,nosuid,size=100m \
  \
  # Drop ALL capabilities, add back only what's needed
  --cap-drop ALL \
  --cap-add NET_BIND_SERVICE \
  \
  # Prevent privilege escalation
  --security-opt no-new-privileges \
  \
  # Custom seccomp profile
  --security-opt seccomp=./custom-seccomp.json \
  \
  # Resource limits
  --memory 256m \
  --memory-swap 256m \
  --cpus 0.5 \
  \
  # No access to host PID/IPC namespaces
  # (default — just documenting explicitly)
  \
  # Network
  --network app-network \
  \
  myapp:v1.0
```

---

---

## Lesson 25 — Debugging Container Issues in Production

> **Level:** Expert

---

### 25.1 The Debugging Toolkit

**Level 1 — Basic (always start here):**
```bash
# Container status and recent events
docker ps -a
docker events --filter container=myapp --since 1h

# Logs (the first thing to check)
docker logs myapp --tail 100
docker logs myapp --since 30m -t

# Resource usage
docker stats myapp --no-stream
```

**Level 2 — Inspection:**
```bash
# Full container configuration
docker inspect myapp

# Check exit code (why did it stop?)
docker inspect myapp --format '{{.State.ExitCode}}'
# Exit 0 = clean exit
# Exit 1 = application error
# Exit 137 = OOM kill (memory limit exceeded)
# Exit 139 = segfault
# Exit 143 = SIGTERM received (graceful shutdown requested)

# Check health check output
docker inspect myapp --format '{{json .State.Health}}' | jq '.Log[-3:]'

# Processes in the container
docker top myapp aux
```

**Level 3 — Interactive investigation:**
```bash
# Shell into running container
docker exec -it myapp sh

# If the container keeps crashing (can't exec into it):
# Option 1: Override entrypoint to get a shell
docker run -it --entrypoint sh myapp:v1.0

# Option 2: Copy files out of a stopped container
docker create --name crashed myapp:v1.0
docker cp crashed:/app/logs/error.log ./error.log
docker rm crashed

# Option 3: Run a debug sidecar sharing the container's namespaces
docker run -it \
  --pid container:myapp \
  --network container:myapp \
  --volumes-from myapp \
  --cap-add SYS_PTRACE \
  nicolaka/netshoot \
  bash
# netshoot has: curl, dig, netstat, tcpdump, strace, ss, and more
```

---

### 25.2 Common Production Issues and Solutions

**Issue 1: Container OOM (Out of Memory) Kill**
```bash
# Symptoms:
docker inspect myapp --format '{{.State.ExitCode}}'   # Returns 137
docker inspect myapp --format '{{.State.OOMKilled}}'  # Returns true
journalctl -k | grep -i "oom"   # Host kernel OOM killer logs

# Solutions:
# 1. Increase memory limit
docker update --memory 1g myapp

# 2. Find the memory leak
docker stats myapp   # Watch memory climb over time
docker exec myapp cat /proc/1/status | grep VmRSS   # Current RSS

# 3. Enable core dumps for offline analysis
docker run --ulimit core=-1 --security-opt seccomp=unconfined myapp
```

**Issue 2: Container Networking Failure**
```bash
# Container can't reach another container
docker exec myapp curl -v http://api-service:3000/health
# Could be: wrong network, wrong service name, service not running

# Diagnose from inside container
docker exec myapp nslookup api-service    # DNS working?
docker exec myapp ping api-service        # Connectivity?
docker exec myapp telnet api-service 3000 # Port open?

# Check if containers are on the same network
docker inspect myapp --format '{{json .NetworkSettings.Networks}}' | jq 'keys'
docker inspect api-service --format '{{json .NetworkSettings.Networks}}' | jq 'keys'
# Both must share at least one network

# Check NSG/firewall (if on cloud VM)
# Check Docker network rules
docker network inspect app-network
```

**Issue 3: Image Won't Build**
```bash
# Verbose build output
DOCKER_BUILDKIT=1 docker build --progress=plain -t myapp . 2>&1 | tee build.log

# Build without cache to eliminate caching issues
docker build --no-cache -t myapp .

# Check what's being sent to build context
docker build -t myapp . 2>&1 | head -5
# "Sending build context to Docker daemon  950MB" ← too large, check .dockerignore

# Inspect failed intermediate layer
docker build -t myapp . 2>&1 | grep "Step [0-9]"
# If it fails at step 4, build an image up to step 3 and investigate
```

**Issue 4: Container Starts Then Immediately Exits**
```bash
# Check logs immediately after start
docker run myapp; docker logs $(docker ps -lq)

# Common causes:
# 1. Entrypoint/CMD is wrong — it exits immediately
docker run -it --entrypoint sh myapp   # Get shell to check manually

# 2. Missing environment variable causing crash
docker run -e DB_HOST=localhost myapp

# 3. Port already in use
# docker: Error response from daemon: driver failed ... bind: address already in use
lsof -i :3000   # What's using the port?
docker ps | grep 3000   # Another container?
```

---

### 25.3 Network Debugging with netshoot

```bash
# netshoot is a Docker troubleshooting container packed with network tools
# Run it sharing a target container's network namespace
docker run -it --rm \
  --network container:myapp \
  nicolaka/netshoot

# Inside netshoot:
ss -tlnp                    # Open ports
netstat -rn                 # Routing table
dig +short api-service      # DNS lookup
curl -v http://api-service  # HTTP test with verbose
tcpdump -i eth0 port 3000   # Capture packets
nmap -p 3000 api-service    # Port scan
```

---

---

## Lesson 26 — DevOps Interview Preparation — Docker Edition

> **Level:** Senior Engineer

---

### 26.1 Core Interview Questions — With Strong Answers

**Q: "Explain the difference between a Docker image and a Docker container."**
> An image is a read-only, layered filesystem snapshot containing the application code, runtime, libraries, and configuration. It's built from a Dockerfile and stored in a registry. A container is a running instance of an image — it adds a writable layer on top and runs as an isolated process using Linux namespaces and cgroups. You can create multiple containers from the same image. When the container is removed, its writable layer is gone; the image remains unchanged.

---

**Q: "How do you minimise Docker image size in production?"**
> Several techniques: First, choose Alpine or slim base images — `node:18-alpine` is ~180MB versus `node:18` at 1.1GB. Second, use multi-stage builds — compile in one stage, copy only runtime artifacts to a minimal final stage. Third, combine RUN commands into single layers with `&&` and clean up in the same layer (removing package caches, temp files). Fourth, use `.dockerignore` to exclude node_modules, .git, test files from the build context. Fifth, copy package manifests before source code to maximise layer caching.

---

**Q: "How do you handle secrets in Docker? Should you use environment variables?"**
> Environment variables are acceptable for non-sensitive configuration but are not ideal for secrets because they're visible in `docker inspect`, process listings, and can leak to logs. For secrets I prefer Docker Secrets (for Swarm) or an external secrets manager like Azure Key Vault or HashiCorp Vault. In Kubernetes, Kubernetes Secrets (ideally encrypted at rest and synced from a vault). The pattern is: the secret never touches the container image, is mounted as a file at `/run/secrets/` (Docker Secrets) or as an environment variable injected at runtime by the orchestrator from the vault, and the value is never logged or exposed.

---

**Q: "A container keeps crashing and restarting. Walk me through how you diagnose it."**
> First, `docker logs <container> --tail 100` to see the last output before it crashed — the application usually tells you exactly what went wrong. Next, `docker inspect <container> --format '{{.State.ExitCode}}'` — exit code 137 means OOM kill (increase memory limit), exit code 1 means application error (back to logs), exit code 139 is a segfault (binary or library issue). If the container crashes before I can `exec` into it, I use `docker run -it --entrypoint sh` to get a shell in the image and debug manually. I'd also check `docker stats` history and `docker events` to correlate the crash with any system events or resource spikes.

---

**Q: "What is the difference between CMD and ENTRYPOINT?"**
> Both define what runs when a container starts. ENTRYPOINT sets the fixed executable — it's always run and can't be overridden without `--entrypoint`. CMD provides default arguments to the ENTRYPOINT, or if there's no ENTRYPOINT, it's the default command. The common production pattern is: `ENTRYPOINT ["node"]` and `CMD ["server.js"]` — running `docker run myapp` executes `node server.js`, but `docker run myapp debug.js` runs `node debug.js`. Always use exec form (JSON array) for both to ensure proper signal handling — shell form wraps in `/bin/sh -c` which means SIGTERM goes to the shell, not the app.

---

**Q: "Explain Docker networking. How do two containers on the same host communicate?"**
> Docker creates virtual networks. The default bridge network lets containers communicate by IP, but without DNS resolution by name. The recommended approach is custom bridge networks — when you create one and connect containers to it, Docker runs an internal DNS server that resolves container names to their current IPs automatically. So `api-service` can call `http://db-service:5432` by name without knowing the IP. Containers on different custom networks are isolated and cannot communicate unless explicitly connected to a shared network. For production microservices I put each tier on its own network — databases on an internal-only network, frontend services on a network shared with the gateway, and so on.

---

### 26.2 Scenario-Based Questions

**Q: "You need to deploy a web application that has a Node.js API, PostgreSQL database, and Redis cache. Walk me through your Docker setup."**

Strong answer covers:
- Separate containers for each service
- Custom Docker network (not default bridge) for DNS resolution
- Named volume for PostgreSQL data persistence
- Named volume for Redis persistence (`--appendonly yes`)
- Docker Compose for local dev and simple deployments
- `depends_on` with `condition: service_healthy` for proper startup ordering
- Non-root user in Dockerfiles
- Health checks on all services
- `restart: unless-stopped` on all services
- `.env` file for credentials, `.env.example` committed to Git
- Resource limits to prevent one service from starving others

---

**Q: "How would you set up a CI/CD pipeline that builds and deploys a Docker container?"**

Strong answer covers:
- On push to main: build image tagged with git SHA
- Run unit tests (either before build or using a `test` build stage)
- Run `trivy` image scan — fail pipeline on CRITICAL vulnerabilities
- Push to private registry (ACR/ECR/GHCR)
- Deploy to staging automatically, production with manual approval
- Use OIDC/Workload Identity Federation for authentication — no stored secrets in CI
- Store image tag in pipeline output to pass between jobs
- Rollback strategy: re-deploy previous image tag

---

### 26.3 Docker CLI Cheat Sheet — Commands You Must Know Cold

```bash
# ─── BUILD ──────────────────────────────────────────────────────────
docker build -t myapp:v1.0 .
docker build -t myapp:v1.0 --no-cache .
docker build -f Dockerfile.prod -t myapp:prod .
docker build --build-arg ENV=production -t myapp:prod .

# ─── IMAGES ─────────────────────────────────────────────────────────
docker images
docker pull nginx:alpine
docker push myregistry/myapp:v1.0
docker tag myapp:v1.0 myregistry/myapp:v1.0
docker rmi myapp:v1.0
docker image prune -a
docker history myapp:v1.0
docker inspect myapp:v1.0

# ─── CONTAINERS ─────────────────────────────────────────────────────
docker run -d --name api -p 3000:3000 --restart unless-stopped myapp
docker run -it --rm ubuntu bash
docker ps
docker ps -a
docker stop api
docker start api
docker restart api
docker rm api
docker rm -f api
docker container prune
docker rename api api-service

# ─── LOGS AND DEBUGGING ─────────────────────────────────────────────
docker logs api -f --tail 50
docker exec -it api sh
docker inspect api
docker inspect api --format '{{.State.ExitCode}}'
docker stats api --no-stream
docker top api
docker cp api:/app/logs/error.log ./
docker events --since 1h

# ─── VOLUMES ────────────────────────────────────────────────────────
docker volume create db-data
docker volume ls
docker volume inspect db-data
docker volume rm db-data
docker volume prune

# ─── NETWORKS ───────────────────────────────────────────────────────
docker network create app-net
docker network ls
docker network inspect app-net
docker network connect app-net api
docker network disconnect app-net api
docker network rm app-net
docker network prune

# ─── COMPOSE ────────────────────────────────────────────────────────
docker compose up -d
docker compose up -d --build
docker compose down
docker compose down -v
docker compose ps
docker compose logs -f api
docker compose exec api sh
docker compose scale api=3
docker compose config
docker compose pull

# ─── REGISTRY ───────────────────────────────────────────────────────
docker login
docker login myregistry.azurecr.io
docker logout

# ─── SYSTEM ─────────────────────────────────────────────────────────
docker info
docker version
docker system df           # Disk usage
docker system prune        # Remove stopped containers, unused networks, dangling images
docker system prune -a     # Remove ALL unused images too
```

---

### 26.4 Dockerfile Best Practices Checklist

```
□ Use specific version tags — FROM node:18.17.1-alpine3.18 not FROM node:latest
□ Use Alpine or slim base images unless specific OS packages are needed
□ Create and switch to non-root user before CMD
□ COPY package files before source code (layer caching)
□ Use npm ci instead of npm install (reproducible installs)
□ Use --no-cache for pip, --no-install-recommends for apt
□ Clean up package caches in the same RUN command that installs
□ Use multi-stage builds for compiled languages
□ Use exec form (JSON arrays) for CMD and ENTRYPOINT
□ Add HEALTHCHECK to all production images
□ Add meaningful LABEL metadata (version, maintainer)
□ Create .dockerignore file
□ Never store secrets in Dockerfile or image
□ Scan final image with Trivy before pushing
□ Set USER to non-root before CMD
□ Minimise the number of layers (combine RUN commands where logical)
```

---

## Official Docker Documentation Reference

| Topic | URL |
|---|---|
| Docker Documentation | https://docs.docker.com/ |
| Dockerfile Reference | https://docs.docker.com/engine/reference/builder/ |
| Docker CLI Reference | https://docs.docker.com/engine/reference/commandline/docker/ |
| Docker Compose | https://docs.docker.com/compose/ |
| Docker Networking | https://docs.docker.com/network/ |
| Docker Storage | https://docs.docker.com/storage/ |
| Docker Security | https://docs.docker.com/engine/security/ |
| Docker Hub | https://hub.docker.com/ |
| BuildKit | https://docs.docker.com/build/buildkit/ |
| Trivy Scanner | https://aquasecurity.github.io/trivy/ |
| Docker Swarm | https://docs.docker.com/engine/swarm/ |
| netshoot | https://github.com/nicolaka/netshoot |

---

## Learning Path from Here

```
Week 1–2:   Lessons 1–5   Run containers, write Dockerfiles, push to Hub
Week 3–4:   Lessons 6–8   Volumes, Networking, Docker Compose — build a full stack app
Week 5–6:   Lessons 9–11  Optimise images, private registries
Week 7–8:   Lessons 12–15 CI/CD pipelines, security, logging, secrets
Week 9–10:  Lessons 16–19 Build caching, Swarm, Kubernetes integration, Terraform
Week 11–12: Lessons 20–22 Microservices architecture, monitoring
Week 13–14: Lessons 23–25 Internals, security hardening, production debugging
Week 15–16: Lesson 26     Interview prep + build a real portfolio project
```

**Build Something Real:**
Deploy a complete microservices application:
- 3+ services (web, API, background worker)
- PostgreSQL with volume persistence
- Redis for caching
- Nginx as reverse proxy
- Docker Compose for local dev
- CI/CD pipeline in GitHub Actions
- Image scanning with Trivy
- Monitoring with Prometheus + Grafana

Put it on GitHub. This is your portfolio proof-of-work.

---

*Built on: Official Docker Documentation — https://docs.docker.com/ | Docker Security Best Practices | Production DevOps Patterns*
