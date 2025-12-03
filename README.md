# Monitoring Stack (Prometheus + Grafana)

This repository provides observability for the microservices in the FRI Food RSO project.

It includes:
- Prometheus for scraping metrics
- Grafana for visual dashboards
- Unified docker-compose setup

The monitoring stack collects metrics from all microservices and visualizes them in real-time dashboards.

---

## 🚀 Features

- Automatic scraping of metrics from all microservices
- Prometheus-based time series monitoring
- Grafana dashboards (HTTP requests, service health, latency)
- Works both locally and inside Docker networks
- Easy extension for new services

---

## 📦 Files

prometheus.yml – Prometheus scrape configuration  
docker-compose.yml – starts Prometheus + Grafana  
dashboards/ – (optional) Grafana dashboards in JSON format  
provisioning/ – auto-load dashboards into Grafana  

---

## 🧩 prometheus.yml Example

Configure Prometheus to scrape your microservices:

global:
  scrape_interval: 10s

scrape_configs:
  - job_name: partner-service
    static_configs:
      - targets: ["partner-service:8000"]

  - job_name: offer-service
    static_configs:
      - targets: ["offer-service:8001"]

When scraping local FastAPI servers (not inside Docker), replace targets with:

host.docker.internal:PORT

---

## ▶️ Run Monitoring Stack

Start Prometheus + Grafana:

docker compose up -d

Stop:

docker compose down

---

## 🌐 Access Interfaces

Prometheus UI:
http://localhost:9090

Grafana UI:
http://localhost:3000  
Default login:
admin / admin

You will be prompted to change your password on first login.

---

## 📊 Adding Prometheus Datasource in Grafana

1. Open Grafana  
2. Go to Connections -> Data Sources  
3. Add Prometheus  
4. Set URL to:  
   http://prometheus:9090  
5. Save & Test  

---

## 📈 Suggested PromQL Queries

HTTP total requests:
sum by (method, path) (http_requests_total)

Latency histogram:
histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))

Service health:
up
