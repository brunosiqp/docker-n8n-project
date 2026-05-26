# n8n Dockerized Environment

## Overview

This repository provides a containerized deployment environment for n8n using Docker and Docker Compose.

The project focuses on portability, reproducibility, modularity, and simplified infrastructure management for workflow automation environments.

The current implementation supports both PostgreSQL and Microsoft SQL Server as external database providers.

---

# Features

- Containerized n8n deployment
- Docker Compose orchestration
- Persistent application storage
- Externalized environment configuration
- PostgreSQL support
- Microsoft SQL Server support
- HTTP Basic Authentication
- Modular and extensible architecture
- Production-ready foundation

---

# Architecture

The environment is composed of the following services:

| Service | Purpose |
|---|---|
| n8n | Workflow automation platform |
| PostgreSQL | Default relational database |
| Microsoft SQL Server | Alternative enterprise database backend |

Optional production layers may include:

- Reverse proxy
- TLS termination
- Monitoring stack
- Logging aggregation
- Container orchestration

---

# Repository Structure

```txt
.
├── docker-compose.yml
├── docker-compose.mssql.yml
├── .env.example
├── .env.mssql.example
├── .gitignore
├── README.md
├── docs/
│   └── mssql.md
└── data/
```

---

# Services

## n8n

### Image

```txt
n8nio/n8n
```

### Exposed Port

```txt
5678/tcp
```

### Persistent Storage

```txt
/home/node/.n8n
```

### Responsibilities

- Workflow orchestration
- Credential management
- Execution management
- REST API exposure
- Webhook handling

---

## PostgreSQL

### Image

```txt
postgres:15
```

### Responsibilities

- Primary relational database backend
- Workflow persistence
- Execution data storage

### Persistent Storage

```txt
/var/lib/postgresql/data
```

---

## Microsoft SQL Server

### Image

```txt
mcr.microsoft.com/mssql/server:2022-latest
```

### Responsibilities

- Alternative enterprise database backend
- MSSQL compatibility layer for n8n deployments

### Persistent Storage

```txt
/var/opt/mssql
```

---

# Environment Variables

## Core n8n Configuration

| Variable | Description |
|---|---|
| `N8N_HOST` | Application hostname |
| `N8N_PORT` | Internal application port |
| `N8N_PROTOCOL` | HTTP or HTTPS |

---

## Authentication

| Variable | Description |
|---|---|
| `N8N_BASIC_AUTH_ACTIVE` | Enables Basic Authentication |
| `N8N_BASIC_AUTH_USER` | Authentication username |
| `N8N_BASIC_AUTH_PASSWORD` | Authentication password |

---

## Database Provider

| Variable | Description |
|---|---|
| `DB_TYPE` | Database provider selection |

Supported values:

```txt
postgresdb
mssql
```

---

## PostgreSQL Variables

| Variable |
|---|
| `DB_POSTGRESDB_HOST` |
| `DB_POSTGRESDB_PORT` |
| `POSTGRES_DB` |
| `POSTGRES_USER` |
| `POSTGRES_PASSWORD` |

---

## MSSQL Variables

| Variable |
|---|
| `DB_MSSQLDB_HOST` |
| `DB_MSSQLDB_PORT` |
| `DB_MSSQLDB_DATABASE` |
| `DB_MSSQLDB_USER` |
| `DB_MSSQLDB_PASSWORD` |

---

# Deployment

## Clone Repository

```bash
git clone https://github.com/brunosiqp/docker-n8n-project.git
cd docker-n8n-project
```

---

## Configure Environment

Create a local environment file:

```bash
cp .env.example .env
```

---

# Running with PostgreSQL

Default deployment:

```bash
docker compose up -d
```

---

# Running with Microsoft SQL Server

Configure:

```env
DB_TYPE=mssql
```

Start services:

```bash
docker compose up -d
```

Optional override deployment:

```bash
docker compose -f docker-compose.yml -f docker-compose.mssql.yml up -d
```

---

# Data Persistence

The environment uses Docker volumes to ensure data durability across:

- Container restarts
- Container recreation
- Host reboot scenarios

Persisted data includes:

- Workflows
- Credentials
- Executions
- Database files
- Application configuration

---

# Security Model

Current security controls include:

- HTTP Basic Authentication
- Environment-based secret management
- Externalized credentials
- Persistent encrypted credential storage

Recommended production enhancements:

- TLS termination
- Reverse proxy isolation
- Secrets manager integration
- Firewall restrictions
- VPN/private networking

---

# Extensibility

The project architecture supports future integrations such as:

- Nginx
- Traefik
- Redis
- RabbitMQ
- Kubernetes
- Docker Swarm
- Azure SQL
- AWS RDS
- External monitoring stacks

---

# Compatibility

| Component | Status |
|---|---|
| PostgreSQL | Stable |
| Microsoft SQL Server | Experimental |
| Docker Compose | Supported |
| Linux Hosts | Supported |
| Windows Hosts | Supported |
| macOS Hosts | Supported |

---

# Best Practices

- Keep secrets outside version control
- Use `.env` files for runtime configuration
- Persist all critical application data
- Avoid modifying containers directly
- Use external databases for production workloads
- Isolate services using Docker networks

---

# Notes

This repository is intended for development, testing, and scalable deployment scenarios.

Production environments may require additional infrastructure layers including:

- TLS encryption
- Centralized logging
- Monitoring
- Backup automation
- High availability configurations
- Container orchestration platforms
