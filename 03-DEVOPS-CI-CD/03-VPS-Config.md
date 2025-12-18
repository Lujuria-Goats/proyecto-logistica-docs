---
title: VPS Configuration
description: Docker compose and security.
---

# VPS Configuration (Virtual Private Server)

## 🖥️ Server Specifications

Currently deployed on a Cloud instance (e.g., DigitalOcean Droplet, AWS EC2, Azure VM).

- **OS:** Linux (Recommended: Ubuntu 22.04 LTS or higher)
- **Recommended Resources:**
  - **CPU:** 2 vCPUs (Minimum to support Java + .NET + Databases)
  - **RAM:** 4GB+ (Java and RabbitMQ consume significant memory)
  - **Disk:** 40GB+ SSD (For logs and database)

---

## 🐳 Container Stack (Docker Compose)

All infrastructure is defined as code in `docker-compose.yml`.

### Service Structure

#### apex_proxy (Nginx Proxy Manager):
- Main entry point (Gateway). Handles SSL certificates (Let's Encrypt) and redirects traffic to appropriate containers.
- **Ports:** 80, 443, 81 (Admin UI).

#### apex_backend (.NET 8):
- Main API. Connected to internal network to communicate with DB and RabbitMQ.
- **Host Port:** 8082 (mapped to 8080).

#### apex_java (Spring/Java):
- Optimization microservice.
- **Host Port:** 8081 (mapped to 8080).

#### apex_db (PostgreSQL 15):
- Relational database.
- **Volume:** `postgres_data` (Persistent in `/var/lib/postgresql/data`).

#### rabbitmq:
- Message broker. Includes Management plugin.
- **Ports:** 5672 (AMQP), 15672 (Web UI).

---

## Secrets Management (`.env`)

The `.env` file should NOT be uploaded to the repository. It must be manually created on the server in the same path as `docker-compose.yml`.

**Example content:**
```env
POSTGRES_PASSWORD=SuperSecretPass123!
RABBITMQ_USER=admin
RABBITMQ_PASS=AnotherSecret123
JWT_SECRET=VeryLongKeyToSignTokens...
CLOUDINARY_URL=...
AZURE_VISION_KEY=...
```

---

## 🔧 Management Commands

### Initial Deployment
```bash
git clone <repo_url> /opt/apex-vision
cd /opt/apex-vision

nano .env

docker-compose up -d --build
```

### Update (Manual CD)
```bash
git pull origin main
docker-compose up -d --build --force-recreate
docker image prune -f 
```

---

## SSH Access and Security

- **User:** root or user with sudo privileges.
- **Firewall (UFW):**
  - **Allow:** 22 (SSH), 80 (HTTP), 443 (HTTPS).
  - **Optional (admin only):** 8081, 8082, 3030, 8000, 9000 (Portainer).
- **Recommendation:** Close DB ports (5432) and RabbitMQ (15672) to the public and use SSH tunnels or VPN.