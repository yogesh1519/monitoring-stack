# Monitoring & Observability Stack — Prometheus + Grafana

A Node.js application instrumented with custom metrics, monitored by Prometheus, and visualized in real time through a Grafana dashboard — the full observability stack, running entirely free via Docker Compose.

## Architecture

```
docker-compose up
      │
      ├── app (Node.js + Express)
      │     ├── /health   → application endpoint
      │     └── /metrics  → Prometheus-format metrics (prom-client)
      │
      ├── prometheus
      │     └── scrapes app:3000/metrics every 5s
      │           → stores time-series data
      │           → localhost:9090
      │
      └── grafana
            └── queries Prometheus as a data source
                  → dashboards visualizing live metrics
                  → localhost:3001
```

## Tech Stack

- **Application**: Node.js + Express
- **Metrics instrumentation**: prom-client (custom counters + default Node.js process metrics)
- **Metrics collection**: Prometheus
- **Visualization**: Grafana
- **Orchestration**: Docker Compose

## What this demonstrates

- Instrumenting an application with custom metrics (HTTP request counters by method/route/status) rather than relying only on infrastructure-level monitoring
- Understanding the Prometheus pull model — Prometheus scrapes targets on an interval, rather than the app pushing data out
- Configuring Prometheus scrape targets via `prometheus.yml`
- Connecting Grafana to a data source and building dashboards from PromQL queries
- Multi-container orchestration with Docker Compose, including internal container-to-container networking (Grafana reaching Prometheus via its service name, not localhost)
- The observability side of DevOps — not just deploying and automating, but knowing whether a running system is healthy

## How to run

```bash
docker compose up -d
```

- App: http://localhost:3000/health
- Raw metrics: http://localhost:3000/metrics
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3001 (default login: admin / admin, prompts password change on first login)

## Dashboard setup

1. In Grafana, add a Prometheus data source pointing to `http://prometheus:9090`
2. Create a new dashboard with panels querying:
   - `http_requests_total` — request volume over time
   - `process_resident_memory_bytes` — memory usage over time

## How to stop

```bash
docker compose down
```
