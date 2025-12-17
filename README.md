
# 🐳 Docker & DevOps Portfolio: From Containers to CI/CD
**Author:** Saleem Ali
**Focus:** Containerization, Microservices, & DevOps Engineering
**Tech Stack:** Docker Engine, Docker Compose, Linux, GitHub Actions, CI/CD

---

## 📋 Comprehensive Lab Index
*Click any module to jump to the code.*

| Module | Labs Covered | Key Concepts |
| :--- | :--- | :--- |
| **1. Docker Fundamentals** | Labs 1–6, 25, 28 | Engine, CLI Commands, Lifecycle, Cleanup |
| **2. Image Engineering** | Labs 7, 8, 21, 23, 24, 27 | Dockerfiles, Layers, Multi-stage, Alpine, Args |
| **3. Storage & Config** | Labs 9–12 | Env Vars, Ports, Volumes vs. Bind Mounts |
| **4. Networking & Security** | Labs 13–15, 20, 26 | Bridges, User-Defined Nets, Non-root, Healthchecks |
| **5. Orchestration** | Labs 16, 17 | Docker Compose, Multi-Service Stacks (Web+DB) |
| **6. DevOps & CI/CD** | Labs 18, 19, 22, 29, 30 | Debugging, Private Registries, GitHub Actions |

---

## 🟢 Module 1: Docker Fundamentals & Lifecycle

### 📦 Lab 1-3: Setup & The "Hello World"
* [cite_start]**Goal:** Verify the Docker Engine is active[cite: 2814].
```bash
# Check version and info
docker --version
docker info

# Run your first container (pulls automatically)
docker run hello-world

```

### 📦 Lab 4 & 6: Managing Containers (Interactive vs Detached)

* 
**Goal:** Run Nginx in the background and Ubuntu interactively.



```bash
# Detached Mode (Background Service)
docker run -d --name my_web_server -p 8080:80 nginx

# Interactive Mode (Shell Access)
# -it = Interactive + TTY
docker run -it ubuntu /bin/bash

# Lifecycle Management
docker ps               # List running
docker stop my_web_server
docker start my_web_server
docker rm my_web_server # Remove container

```

### 📦 Lab 25: System Cleanup (Prune)

* 
**Goal:** Free up disk space by removing unused objects.



```bash
# The "Nuke" command (Use with caution!)
docker system prune -a --volumes

```

---

## 🔵 Module 2: Image Engineering (Dockerfiles)

### 🛠️ Lab 7 & 23: The Optimal Dockerfile

* 
**Goal:** Build a lightweight image using Alpine Linux.


* **Best Practice:** Use specific tags (`node:14-alpine`) instead of `latest`.

```dockerfile
# Lab 23: Use Minimal Base Image
FROM node:14-alpine

# Lab 27: Build-time Arguments
ARG APP_VERSION=1.0.0

# Working Directory
WORKDIR /app

# Lab 8: Cache Optimization (Copy package.json FIRST)
COPY package*.json ./
RUN npm install

# Copy source code
COPY . .

# Lab 21: Entrypoint vs CMD
# ENTRYPOINT is the executable, CMD is the default arg
ENTRYPOINT ["node"]
CMD ["app.js"]

```

### 🛠️ Lab 24: Tagging & Versioning

* 
**Goal:** Manage image versions for production.



```bash
# Build with a specific tag
docker build -t my-app:v1.0 .

# Retag for a registry
docker tag my-app:v1.0 myusername/my-app:latest

```

---

## 🟠 Module 3: Storage & Configuration

### 💾 Lab 9: Environment Variables

* 
**Goal:** Inject configuration at runtime (12-Factor App).



```bash
# Injecting variables without changing code
docker run -e DATABASE_URL="postgres://localhost:5432" -e APP_ENV="production" my-app

```

### 💾 Lab 11 & 12: Volumes vs. Bind Mounts

* 
**Goal:** Persist data (Database) vs. Live Code Reloading (Dev).



```bash
# Named Volume (Production Database Persistence)
# Managed by Docker, hard to lose accidentally
docker run -d -v db_data:/var/lib/mysql mysql

# Bind Mount (Development)
# Maps host folder to container (Live coding)
docker run -d -v $(pwd):/app -p 3000:3000 node_app

```

---

## 🟣 Module 4: Networking & Security

### 🌐 Lab 14: User-Defined Bridge Networks

* 
**Goal:** Enable containers to talk by name (DNS Resolution).


* **Note:** *Never* use the default bridge for production communication.

```bash
# 1. Create Network
docker network create backend-net

# 2. Attach containers
docker run -d --name db --net backend-net mongo
docker run -d --name api --net backend-net my-api

# 3. Verify (API can now reach DB via hostname "db")
docker exec -it api ping db

```

### 🔒 Lab 26: Security Best Practices

* 
**Goal:** Run as Non-Root and Limit Resources.



```dockerfile
# Create a user and switch to it
RUN adduser -D appuser
USER appuser

```

```bash
# Limit Resources at Runtime
docker run --memory="512m" --cpus="1.0" my-app

```

### 💓 Lab 20: Healthchecks

* 
**Goal:** Auto-restart unhealthy containers.



```dockerfile
# In Dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/ || exit 1

```

---

## 🟤 Module 5: Orchestration (Docker Compose)

### 🎼 Lab 16 & 17: The Web + DB Stack

* 
**Goal:** Define Infrastructure as Code (IaC) for a full application stack.


* **File:** `docker-compose.yml`

```yaml
version: '3.8'

services:
  frontend:
    image: nginx:alpine
    ports:
      - "80:80"
    depends_on:
      - backend
    networks:
      - app-net

  backend:
    build: ./api
    environment:
      - DB_HOST=db
    networks:
      - app-net

  db:
    image: postgres:13-alpine
    volumes:
      - db_data:/var/lib/postgresql/data
    networks:
      - app-net

# Define persistent volumes
volumes:
  db_data:

# Define networks
networks:
  app-net:

```

### 🎼 Commands

```bash
docker compose up -d  # Start all services in background
docker compose logs -f # Follow logs
docker compose ps     # Check status
docker compose down   # Stop and remove everything

```

---

## 🏆 Module 6: DevOps & CI/CD Pipeline

### 🚀 Lab 29: Private Registry

* 
**Goal:** Host your own images securely.



```bash
# Run local registry
docker run -d -p 5000:5000 --name registry registry:2

# Push to it
docker tag my-app localhost:5000/my-app
docker push localhost:5000/my-app

```

### 🚀 Lab 30: CI/CD Integration

* 
**Goal:** Automate building and testing.


* **Workflow:** Code Push -> Build Image -> Run Tests -> Push to Hub.
* **Command for CI Runner:**

```bash
# Build and run tests, then exit with code
docker compose -f docker-compose.test.yml up --build --abort-on-container-exit

```

---

**Status:** Completed
**Skills Acquired:** Image Optimization, Orchestration, Security, CI/CD Integration.

```


```
