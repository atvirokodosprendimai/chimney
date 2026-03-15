# Chimney

GitHub API caching proxy and dashboard server.

Serves static HTML dashboards and provides a server-side caching proxy to the GitHub REST API. Server-side caching with an authenticated GitHub token raises the rate limit from 60 to 5,000 req/hr.

## Features

- Two-layer cache: Dragonfly (Redis-compatible) + in-memory fallback
- ETag-aware conditional fetching with stale-on-error resilience
- OpenTelemetry instrumentation (traces, metrics, logs)
- Blue/green deployment with zero-downtime updates
- Multi-arch Docker images (amd64/arm64)

## Quick Start

```bash
go build -o chimney .
GITHUB_TOKEN=ghp_... ./chimney -addr :8080 -docs ./docs
```

## Configuration

| Flag | Default | Description |
|------|---------|-------------|
| `-addr` | `:8080` | Listen address |
| `-docs` | `./docs` | Static files directory |
| `-repo` | `atvirokodosprendimai/wgmesh` | Default GitHub repo |
| `-redis` | `127.0.0.1:6379` | Dragonfly/Redis address |

| Env Var | Description |
|---------|-------------|
| `GITHUB_TOKEN` | GitHub API token (5,000 req/hr) |
| `GITHUB_ORG` | GitHub org for multi-repo discovery |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | OpenTelemetry collector endpoint |

## Endpoints

| Path | Description |
|------|-------------|
| `GET /` | Dashboard static files |
| `GET /healthz` | Health check |
| `GET /api/version` | Version info |
| `GET /api/github/*` | Cached GitHub API proxy |
| `GET /api/pipeline/summary` | Aggregated pipeline status |
| `GET /api/cache/stats` | Cache statistics |

## Deployment

See `deploy/` for Docker Compose configs, Caddyfiles, and deployment scripts.

## Previously

Chimney was part of [wgmesh](https://github.com/atvirokodosprendimai/wgmesh). Extracted to its own repo for independent development and deployment.
