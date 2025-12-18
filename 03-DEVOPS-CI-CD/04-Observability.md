---
title: Observability
description: Monitoring with Prometheus and Grafana.
---

---
title: Environment Strategy
description: Dev vs Prod configuration.
---

# Observability Strategy

## 🔭 Monitoring Stack

The observability solution is based on the standard LGTM/Prometheus stack, deployed in containers alongside the application.

### Active Components

#### Prometheus (Metrics Collector)
- **Internal Port:** 9090
- **Function:** Scrapes metrics from exporters every 15 seconds (standard configuration).
- **Configuration (prometheus.yml):** Defines targets (cAdvisor, Node Exporter, Backend).

#### Grafana (Visualization)
- **External Port:** 3030 (Mapped to 3000).
- **URL:** `http://<VPS-IP>:3030` (or subdomain `grafana.yourdomain.com`).
- **Credentials:** Admin / (Password in `.env`).
- **Function:** Interactive dashboards to visualize CPU, RAM, Network, and container status.

#### cAdvisor (Container Metrics)
- **Port:** 8080 (Internal).
- **Function:** Exposes resource usage metrics per container (CPU usage, Memory working set, Network I/O).

#### Node Exporter (Host Metrics)
- **Port:** 9100.
- **Function:** Exposes host server metrics (Total disk usage, system CPU load, available RAM).

---

## 📊 Key Dashboards

### 1. Docker Container Monitoring (Via cAdvisor)

Shows the health of each service (apex_backend, apex_java, postgres).

**Key Metrics:**
- **% CPU Usage per container** (Detect infinite loops or blocked processes).
- **Memory Usage** (Detect memory leaks, especially in Java/RabbitMQ).
- **Network Traffic** (RX/TX).

### 2. Node Exporter Full

VPS health monitor.

**Typical Alerts:**
- **Disk Full:** Free space < 10%.
- **High Load:** Load Average > N CPUs.
- **OOM Kill Detect:** If the kernel kills processes due to lack of RAM.

---

## 🚨 Alert System (AlertManager)

(Recommended configuration for next phase)

Notifications via Slack/Discord or Email when:
- A key container (apex_backend) goes down (`up == 0`).
- API response latency > 2s for more than 1 minute.
- Server CPU > 90% for 5 minutes.

---

## 🔗 Quick Access

- **Grafana:** `http://<YOUR_IP>:3030`
- **Prometheus UI:** `http://<YOUR_IP>:9090` (Only accessible via SSH tunnel or allowed IP).