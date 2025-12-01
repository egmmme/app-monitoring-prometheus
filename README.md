# app-monitoring-prometheus

A complete monitoring setup for a Node.js application using Prometheus and Grafana.

## Project Structure

```
app-monitoring-prometheus/
├── docker-compose.yml          # Docker Compose configuration
├── prometheus.yml              # Prometheus configuration
├── alert.rules.yml             # Prometheus alert rules
├── grafana/                    # Grafana configuration
│   ├── prometheus-dashboard.json  # Pre-configured dashboard
│   └── provisioning/           # Grafana provisioning configs
│       ├── datasources/        # Datasource configurations
│       └── dashboards/         # Dashboard configurations
├── app/                        # Node.js application
│   ├── index.js               # Express server with metrics
│   ├── package.json           # Node.js dependencies
│   └── Dockerfile             # Docker configuration
└── README.md                  # This file
```

## Features

- **Node.js Application**: Express server with Prometheus metrics endpoint
- **Prometheus**: Metrics collection and alerting
- **Grafana**: Pre-configured dashboard for visualization
- **Docker Compose**: Easy deployment of all services

## Prerequisites

- Docker + Docker Compose, or
- Podman (Podman Desktop on Windows recommended)

## Quick Start

1. Clone the repository:

```bash
git clone https://github.com/egmmme/app-monitoring-prometheus.git
cd app-monitoring-prometheus
```

2. Start all services (choose one runtime):

Docker Compose

```bash
docker-compose up -d
```

Podman (Windows/Podman Desktop)

```powershell
# First-time setup (once):
podman machine init
podman machine start

# Start the stack in detached mode:
podman compose up -d
# If the compose subcommand is unavailable, use:
# podman-compose up -d
```

3. Access the services:
   - **Application**: http://localhost:3000
   - **Prometheus**: http://localhost:9090
   - **Grafana**: http://localhost:3001 (admin/admin)

## Application Endpoints

- `GET /` - Root endpoint
- `GET /health` - Health check endpoint
- `GET /metrics` - Prometheus metrics endpoint
- `GET /api/data` - Sample endpoint with random delay

## Metrics

The application exposes the following custom metrics:

- `http_requests_total` - Total number of HTTP requests
- `http_request_duration_seconds` - Duration of HTTP requests

Additionally, default Node.js metrics are collected automatically.

## Alert Rules

The following alerts are configured:

- **HighRequestRate**: Triggers when request rate exceeds 100 req/s
- **HighErrorRate**: Triggers when 5xx error rate is high
- **SlowResponseTime**: Triggers when 95th percentile response time > 2s
- **AppDown**: Triggers when the application is down

## Grafana Dashboard

The pre-configured dashboard includes:

- Request rate over time
- 95th percentile response time
- Requests by status code
- Application status
- Memory usage
- CPU usage

## Stopping the Services

Docker Compose

```bash
docker-compose down
```

Podman

```powershell
podman compose down
# If using podman-compose:
# podman-compose down
```

To remove volumes as well:

Docker Compose

```bash
docker-compose down -v
```

Podman

```powershell
podman compose down -v
# Or
# podman-compose down -v
```

## Development

To modify the application:

1. Edit files in the `app/` directory
2. Rebuild and restart:

Docker Compose

```bash
docker-compose up -d --build app
```

Podman

```powershell
podman compose up -d --build app
# Or
# podman-compose up -d --build app
```

## Notes for Podman Users

- The existing `docker-compose.yml` works with Podman via `podman compose` or `podman-compose`.
- Named volumes (e.g., `prometheus-data`, `grafana-data`) are created separately from Docker volumes; data from prior Docker runs is not automatically shared.
- Service DNS names (e.g., `app`, `prometheus`, `grafana`) resolve the same in Podman compose networks. Prometheus scrapes `app:3000` as configured.
- If `podman compose` is not available, install or enable the Compose plugin, or use `podman-compose`.
- On Windows, ensure the Podman machine is running: `podman machine start`.

## License

This project is licensed under the terms in the LICENSE file.
