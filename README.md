# High Availability Web Infrastructure with Monitoring

## Overview

This project demonstrates the implementation of a high-availability web infrastructure using HAProxy for load balancing, Apache HTTP Server for backend web services, Prometheus for monitoring, and Grafana for visualization. The infrastructure was deployed across multiple Linux virtual machines to simulate a production-style environment with monitoring, failover handling, and observability.

The primary objective of this project was to understand how load balancing, monitoring, health checks, and infrastructure observability work together in real-world system engineering and DevOps environments.

---

# Architecture

```text
                    Client Browser
                           |
                           v
              +-----------------------+
              |      HAProxy LB       |
              |    192.168.1.20       |
              +-----------------------+
                    |           |
                    |           |
                    v           v
          +----------------+  +----------------+
          |     Web1       |  |     Web2       |
          | 192.168.1.21   |  | 192.168.1.22   |
          | Apache Server  |  | Apache Server  |
          +----------------+  +----------------+
                    |                  |
                    |                  |
            +-------------------------------+
            |         Node Exporter         |
            +-------------------------------+
                           |
                           v
              +-----------------------+
              |      Prometheus       |
              |    Metrics Storage    |
              +-----------------------+
                           |
                           v
              +-----------------------+
              |        Grafana        |
              | Infrastructure Metrics|
              +-----------------------+
```

---

# Technologies Used

| Category           | Technologies                 |
| ------------------ | ---------------------------- |
| Load Balancer      | HAProxy                      |
| Web Server         | Apache HTTP Server           |
| Monitoring         | Prometheus                   |
| Visualization      | Grafana                      |
| Metrics Collection | Node Exporter                |
| Operating System   | Linux (Ubuntu / Rocky Linux) |
| Networking         | TCP/IP, DNS, HTTP/HTTPS      |
| Version Control    | Git, GitHub                  |

---

# Infrastructure Details

| Component     | IP Address   | Purpose                            |
| ------------- | ------------ | ---------------------------------- |
| Load Balancer | 192.168.1.20 | HAProxy + Prometheus + Grafana     |
| Web1          | 192.168.1.21 | Apache HTTP Server + Node Exporter |
| Web2          | 192.168.1.22 | Apache HTTP Server + Node Exporter |

---

# Key Features

* High availability web infrastructure
* Round-robin load balancing
* Automatic backend health checks
* Backend failover handling
* Real-time monitoring and observability
* CPU and node failure alerting
* Grafana dashboard visualization
* Infrastructure validation through failure simulation

---

# HAProxy Configuration

HAProxy was configured as the central load balancer to distribute incoming traffic across multiple backend web servers.

## Features Implemented

* Round-robin traffic distribution
* Backend health checks
* Automatic failover handling
* Frontend and backend separation

## Example Backend Configuration

```haproxy
frontend web_cluster
    bind *:80
    default_backend web_servers

backend web_servers
    balance roundrobin
    server web1 192.168.1.21:80 check inter 3s fall 2 rise 2
    server web2 192.168.1.22:80 check inter 3s fall 2 rise 2
```

## Validation

Traffic distribution was validated using:

```bash
for i in {1..10}; do curl 192.168.1.20; done
```

Expected behavior:

```text
Web1
Web2
Web1
Web2
```

---

# Apache Web Servers

Apache HTTP Server was configured on two backend nodes to serve web content.

## Web1

```text
192.168.1.21
```

## Web2

```text
192.168.1.22
```

Each server was configured with unique responses to validate load balancing functionality.

Example:

```html
This is Web1
```

```html
This is Web2
```

---

# Monitoring Stack

## Prometheus

Prometheus was deployed on the load balancer node to collect and store infrastructure metrics.

### Metrics Collected

* CPU usage
* Memory utilization
* Disk usage
* Node uptime
* System availability

## Prometheus Targets

```yaml
scrape_configs:
  - job_name: "web_servers"
    static_configs:
      - targets:
        - 192.168.1.21:9100
        - 192.168.1.22:9100
```

---

# Node Exporter

Node Exporter was installed on backend web servers to expose system-level metrics to Prometheus.

## Metrics Endpoint

```text
http://192.168.1.21:9100/metrics
http://192.168.1.22:9100/metrics
```

---

# Grafana Dashboard

Grafana was integrated with Prometheus to visualize infrastructure metrics.

## Dashboard Metrics

* CPU utilization
* Memory usage
* Node health
* System uptime
* Infrastructure availability

Grafana provided real-time monitoring dashboards for backend infrastructure analysis.

---

# Alerting Rules

Prometheus alert rules were implemented to detect infrastructure issues.

## High CPU Usage Alert

```yaml
- alert: HighCPUUsage
  expr: avg by(instance) (rate(node_cpu_seconds_total{mode!="idle"}[1m])) > 0.8
  for: 1m
```

## Node Down Alert

```yaml
- alert: NodeDown
  expr: up == 0
  for: 30s
```

---

# Testing and Validation

## Load Balancing Validation

Verified round-robin traffic distribution across backend servers.

## Failover Testing

* Stopped backend web server services
* Verified automatic traffic rerouting
* Confirmed service continuity during node failure

## Monitoring Validation

* Simulated high CPU utilization
* Verified Prometheus alert triggering
* Observed infrastructure metrics in Grafana

## Health Check Validation

HAProxy health checks successfully removed failed backend nodes from traffic routing.

---

# Commands Used

## HAProxy Service

```bash
sudo systemctl status haproxy
```

## Apache Service

```bash
sudo systemctl status apache2
```

## Prometheus Targets

```text
http://192.168.1.20:9090/targets
```

## Grafana Dashboard

```text
http://192.168.1.20:3000
```

---

# Screenshots

## Grafana Dashboard

Add screenshot:

```text
screenshots/grafana-dashboard.png
```

## Prometheus Targets

Add screenshot:

```text
screenshots/prometheus-targets.png
```

## Load Balancing Validation

Add screenshot:

```text
screenshots/loadbalancer-test.png
```

---

# Skills Demonstrated

* Linux System Administration
* Load Balancing
* High Availability Infrastructure
* Infrastructure Monitoring
* Observability
* System Reliability Testing
* Infrastructure Troubleshooting
* Networking Fundamentals
* Alerting and Monitoring
* Virtual Machine Administration

---

# Future Improvements

* Add HTTPS with SSL certificates
* Integrate Alertmanager for notifications
* Monitor HAProxy metrics
* Add centralized logging
* Automate deployment using Bash scripts or Ansible
* Deploy infrastructure using Docker containers

---

# Learning Outcomes

This project helped strengthen understanding of:

* High availability architectures
* Reverse proxy and load balancing concepts
* Infrastructure monitoring workflows
* Metrics collection and visualization
* Health checks and failover handling
* Real-world DevOps and system engineering practices

---

# Author

Lokesh Singh

GitHub: [https://github.com/lokisingh121](https://github.com/lokisingh121)
