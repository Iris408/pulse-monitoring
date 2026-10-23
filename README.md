![Backend CI](https://github.com/Iris408/pulse-infrastructure-monitor/actions/workflows/backend-ci.yml/badge.svg)

# 💓 Pulse

### Infrastructure Health Monitoring Platform

Pulse is a containerised infrastructure monitoring platform built with Python, FastAPI, Prometheus, and Grafana. It monitors system health, exposes operational metrics, provides Grafana dashboards, and supports configurable Slack and email alerting.

## Key Features

- CPU, memory, disk, and uptime monitoring
- Configurable warning and critical thresholds
- FastAPI `/health` and `/metrics` endpoints
- Prometheus-compatible metrics and scraping
- Grafana infrastructure dashboard
- Slack and email notifications
- Alert cooldowns and recovery notifications
- Structured application logging
- Docker Compose deployment with pinned Prometheus and Grafana image versions
- GitHub Actions CI with a full-stack Docker Compose smoke test
- Weekly Dependabot checks for Docker images and GitHub Actions

## Current Status

**Current release: v2.3.2** ✅  
**Lifecycle: Maintenance**

The core Pulse monitoring and observability stack is complete and portfolio-ready.

Pulse provides system resource monitoring, configurable Slack and email alerting, FastAPI health and metrics endpoints, Prometheus metric collection, Grafana visualisation, structured logging, Docker Compose deployment, and GitHub Actions CI.

v2.3.2 introduced a modular application structure separating the API, monitoring, alerting, logging, and dashboard components to improve maintainability.

Recent maintenance work expanded CI to start the complete Docker Compose stack, verify the API, Prometheus, and Grafana health endpoints, and confirm that Prometheus is successfully scraping Pulse. Prometheus and Grafana images are pinned to explicit versions, and Dependabot checks Docker images and GitHub Actions weekly.

Future work is focused on maintenance, security and dependency updates, alert reliability, documentation, and infrastructure improvements.

See the [Roadmap](./docs/roadmap.md) for release history and maintenance work.

## Technology Stack

| Area | Technologies |
| --- | --- |
| Backend | Python, FastAPI, psutil |
| Observability | Prometheus, Grafana |
| Alerting | Slack Webhooks, SMTP Email |
| Infrastructure | Docker, Docker Compose |
| CI | GitHub Actions, pytest, Docker Compose smoke tests |
| Dependency updates | Dependabot |
| Configuration | python-dotenv |
| Utilities | requests, colorama |

## Architecture

```text
System Resources
       │
       ▼
  Pulse Monitor
       │
       ├──────────► Structured Logging
       │
       ├──────────► Slack / Email Alerts
       │
       ▼
    FastAPI
 /health  /metrics
       ▲
       │ scrapes
       │
   Prometheus
       │
       │ queries
       ▼
    Grafana
```

Prometheus periodically scrapes metrics exposed by Pulse, while Grafana queries Prometheus to provide visual monitoring of CPU, memory, disk usage, and system uptime.

For a more in-depth breakdown of the system design, see the [Architecture Documentation](./docs/architecture.md).

## Screenshots

### Terminal Monitor

<img src="./screenshots/system-health-monitor.png" width="600" alt="Pulse terminal system monitor">

### Grafana Dashboard

<img src="./screenshots/grafana-dashboard.png" width="600" alt="Pulse Grafana infrastructure dashboard">

## Quick Start

### Local

Clone the repository:

```bash
git clone https://github.com/Iris408/pulse-infrastructure-monitor.git
cd pulse-infrastructure-monitor
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Create the environment configuration:

```bash
cp .env.example .env
```

Run the terminal monitor:

```bash
python3 main.py
```

Or start the FastAPI service:

```bash
python3 -m uvicorn health_api:app --reload --port 8000
```

### Docker

Start the complete monitoring stack:

```bash
docker compose up --build -d
```

Check service status:

```bash
docker compose ps
```

Stop the stack:

```bash
docker compose down
```

Prometheus and Grafana use explicit image versions in `docker-compose.yml` to make deployments more predictable. For detailed installation and configuration instructions, see the [Setup Guide](./docs/setup.md).

## Monitoring Endpoints

When running locally:

| Service | URL |
| --- | --- |
| API Root | `http://localhost:8000/` |
| Health | `http://localhost:8000/health` |
| Metrics | `http://localhost:8000/metrics` |
| Swagger UI | `http://localhost:8000/docs` |
| Prometheus | `http://localhost:9090` |
| Grafana | `http://localhost:3000` |

## Testing and CI

GitHub Actions runs automated backend tests on pushes and pull requests to `main`. The workflow also builds and starts the Docker Compose monitoring stack, checks the API, Prometheus, and Grafana health endpoints, and verifies that Prometheus is scraping Pulse successfully.

On failure, the workflow collects the Compose service logs. It shuts down the stack and removes its CI volumes afterward.

Dependabot checks Docker images and GitHub Actions weekly and opens pull requests for available updates.

## Project Structure

```text
pulse-infrastructure-monitor/
├── app/
│   ├── api/
│   ├── monitoring/
│   ├── alerts/
│   ├── logging/
│   └── dashboard/
├── tests/
├── config/
├── docs/
├── screenshots/
├── examples/
├── .github/
│   ├── dependabot.yml
│   └── workflows/
├── Dockerfile
├── docker-compose.yml
└── README.md
```

## Documentation

Detailed engineering documentation is available in [`docs/`](./docs/).


## License

This project is licensed under the terms in the [MIT License](./LICENSE).

## Author

Built by [Iris408](https://github.com/Iris408)