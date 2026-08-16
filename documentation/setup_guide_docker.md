# Setup Guide for Docker

## Overview

This guide walks through installing Docker, configuring it securely, and running the application stack using Docker Compose. Follow each step in order to ensure a consistent and secure environment.

---

## Prerequisites

| Requirement | Minimum Version |
|-------------|----------------|
| OS | Ubuntu 22.04 / Debian 12 / CentOS 8+ / macOS 13+ |
| RAM | 4 GB (8 GB recommended) |
| Disk | 20 GB free |
| CPU | 2 cores |
| Docker Engine | 24.x or later |
| Docker Compose | v2.x or later |
| User privileges | sudo / root |

---

## Step 1: Install Docker Engine

### Ubuntu / Debian

```bash
# Remove old versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# Update package index and install dependencies
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg

# Add Docker's official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the Docker repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Verify Installation

```bash
docker --version
# Expected: Docker version 24.x.x, build xxxxxxx

docker compose version
# Expected: Docker Compose version v2.x.x
```

---

## Step 2: Post-Installation Security Setup

```bash
# Add your user to the docker group (avoids using sudo for every command)
sudo usermod -aG docker $USER

# Apply group change (or log out and back in)
newgrp docker

# Verify docker runs without sudo
docker run hello-world
```

> ⚠️ **Security Note**: Users in the `docker` group have root-equivalent access. Only add trusted users.

---

## Step 3: Configure the Docker Daemon (Secure Defaults)

Create or edit the Docker daemon configuration file:

```bash
sudo nano /etc/docker/daemon.json
```

Paste the following secure configuration:

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "no-new-privileges": true,
  "icc": false,
  "live-restore": true,
  "userland-proxy": false,
  "default-ulimits": {
    "nofile": {
      "Name": "nofile",
      "Hard": 64000,
      "Soft": 64000
    }
  }
}
```

Restart Docker to apply changes:

```bash
sudo systemctl restart docker
sudo systemctl enable docker
```

---

## Step 4: Project Directory Structure

```
project-root/
├── docker-compose.yml
├── .env
├── app/
│   ├── Dockerfile
│   └── src/
├── nginx/
│   └── nginx.conf
├── postgres/
│   └── init.sql
└── certs/
    ├── server.crt
    └── server.key
```

---

## Step 5: Configure Environment Variables

Create a `.env` file in the project root. **Never commit this file to version control.**

```bash
# .env

# Application
APP_ENV=production
APP_PORT=8000
SECRET_KEY=your-very-strong-secret-key

# Database
DB_HOST=postgres
DB_PORT=5432
DB_NAME=appdb
DB_USER=appuser
DB_PASSWORD=strong-db-password

# Redis
REDIS_HOST=redis
REDIS_PORT=6379

# Logging
LOG_LEVEL=info
```

Add `.env` to `.gitignore`:

```bash
echo ".env" >> .gitignore
```

---

## Step 6: Docker Compose File

```yaml
# docker-compose.yml
version: "3.9"

services:
  app:
    build:
      context: ./app
      dockerfile: Dockerfile
    container_name: app
    restart: unless-stopped
    env_file: .env
    ports:
      - "127.0.0.1:8000:8000"
    depends_on:
      - postgres
      - redis
    networks:
      - backend
    read_only: true
    security_opt:
      - no-new-privileges:true
    tmpfs:
      - /tmp

  postgres:
    image: postgres:15-alpine
    container_name: postgres
    restart: unless-stopped
    env_file: .env
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - pg_data:/var/lib/postgresql/data
      - ./postgres/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - backend
    security_opt:
      - no-new-privileges:true

  redis:
    image: redis:7-alpine
    container_name: redis
    restart: unless-stopped
    command: redis-server --requirepass ${REDIS_PASSWORD}
    networks:
      - backend

  nginx:
    image: nginx:alpine
    container_name: nginx
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./certs:/etc/nginx/certs:ro
    depends_on:
      - app
    networks:
      - frontend
      - backend

volumes:
  pg_data:

networks:
  frontend:
  backend:
    internal: true
```

---

## Step 7: Build and Start the Stack

```bash
# Build images and start all services in detached mode
docker compose up --build -d

# Verify all containers are running
docker compose ps

# View logs for a specific service
docker compose logs -f app

# View all logs
docker compose logs -f
```

---

## Step 8: Verify the Setup

```bash
# Check container health
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Test application endpoint
curl -k https://localhost/health

# Connect to the database
docker compose exec postgres psql -U appuser -d appdb
```

---

## Common Commands

```bash
# Stop all services
docker compose down

# Stop and remove volumes (wipes data)
docker compose down -v

# Rebuild a single service
docker compose up --build -d app

# Restart a service
docker compose restart nginx

# Open a shell inside a container
docker compose exec app sh

# Scan image for vulnerabilities
docker scout cves app:latest
```

---

## Security Checklist

- [ ] Docker daemon configured with `no-new-privileges` and `icc: false`
- [ ] `.env` file excluded from version control
- [ ] Docker socket (`/var/run/docker.sock`) not mounted in app containers
- [ ] App container runs as a non-root user
- [ ] Backend network set to `internal: true` (no direct internet access)
- [ ] App port bound to `127.0.0.1` only (not `0.0.0.0`)
- [ ] Images scanned with Trivy or Docker Scout before deployment
- [ ] Log rotation configured in `daemon.json`

---

## Troubleshooting

| Problem | Command | Fix |
|---------|---------|-----|
| Container won't start | `docker compose logs app` | Check for missing env vars or port conflicts |
| DB connection refused | `docker compose exec postgres pg_isready` | Wait for DB to finish initializing |
| Port already in use | `lsof -i :8000` | Kill the conflicting process or change the port |
| Permission denied on volume | `ls -la ./data` | Fix file ownership: `sudo chown -R 1000:1000 ./data` |
| Image build fails | `docker compose build --no-cache` | Rebuild without cache |

---

## Related Documents

- [RBAC Policy](./rbac_policy.md)
- [External Threat Test](./external_threat_test.md)
- [Results](./results.md)

---

*Last updated: June 2026 | Owner: DevOps Team*
