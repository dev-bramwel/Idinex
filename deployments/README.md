# Deployments

This directory contains all deployment-related configuration for IDINEX.

## Structure

### 1. docker/
Everything related to building Docker images.

```text
docker/
├── backend.Dockerfile
├── frontend.Dockerfile
└── postgres.Dockerfile
```
### 2. nginx/
Everything related to the reverse proxy.
```text
nginx/
├── nginx.conf
└── default.conf
```
nginx.conf

- Global Nginx settings.

- Worker processes.

- Compression.

- Logging.

- Timeouts.

default.conf

- Your application routing.

### development
Everything needed for local development.
```text
development/
├── docker-compose.dev.yml
└── README.md
```
The compose file starts

- Backend
- Frontend
- PostgreSQL

The README explains how this happens
```bash
docker compose -f deployments/development/docker-compose.dev.yml up
```
### production/
Eventually this will describe how your server is deployed.
```text
production/
├── docker-compose.prod.yml
├── .env.production.example
└── README.md
```
The README explains:

- Server requirements
- Environment variables
- Deployment steps
- Backup procedure

## Philosophy

Application code should never contain deployment-specific configuration.

Infrastructure belongs here.

All deployment changes should be version controlled.