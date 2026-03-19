# n8n Dockerized Environment Specification

## Overview

This repository defines a containerized deployment of n8n using Docker. The setup is designed to provide a consistent, portable, and isolated runtime environment for workflow automation, ensuring reproducibility across different infrastructures.

---

## Architecture

The environment is composed of a single primary service:

* **n8n Service**

  * Runs as a containerized application using the official image
  * Exposes an HTTP interface for workflow orchestration
  * Persists state and configuration via a mounted volume

Optional extensions may include external database services and reverse proxy layers for production-grade deployments.

---

## Container Configuration

### Base Image

* Official image: `n8nio/n8n`
* Runtime: Node.js-based execution environment
* Process model: Single-process application container

### Networking

* Port: `5678/tcp`
* Protocol: HTTP (default)
* Internal container networking via Docker bridge

### Storage

* Persistent volume mapping:

  * Container path: `/home/node/.n8n`
  * Purpose:

    * Workflow definitions
    * Credentials (encrypted)
    * Execution data
    * Application configuration

---

## Environment Variables

The system relies on environment-based configuration to ensure flexibility and separation of concerns.

### Core Configuration

* `N8N_HOST`

  * Defines the hostname used by the application

* `N8N_PORT`

  * Internal application port

* `N8N_PROTOCOL`

  * Communication protocol (`http` or `https`)

### Authentication

* `N8N_BASIC_AUTH_ACTIVE`

  * Enables HTTP Basic Authentication layer

* `N8N_BASIC_AUTH_USER`

  * Username for access control

* `N8N_BASIC_AUTH_PASSWORD`

  * Password for access control

### Persistence & Security

* Credentials are stored encrypted within the application data directory
* Sensitive values are externalized via environment variables

---

## Data Management

### Persistent Data

All runtime data is stored within a mounted volume to ensure durability across container restarts.

Includes:

* Workflow JSON definitions
* Execution logs
* User credentials (encrypted at rest)
* System configuration files

### Ephemeral Data

* Container filesystem outside mounted volumes is considered non-persistent
* Rebuilds or container recreation will discard non-mounted data

---

## Security Model

* Basic Authentication layer for access restriction
* Secrets managed through environment variables
* No hardcoded credentials within version-controlled files
* Optional integration with external TLS termination (e.g., reverse proxy)

---

## Extensibility

The architecture supports modular expansion, including:

### Database Layer

* External database support (e.g., PostgreSQL)
* Enables scalability and improved data management

### Reverse Proxy

* Integration with Nginx or equivalent
* Enables:

  * TLS termination
  * Domain routing
  * Load balancing

### Cloud Deployment

* Compatible with infrastructure providers such as:

  * AWS
  * DigitalOcean

---

## Repository Structure

```
.
├── docker-compose.yml   # Service orchestration definition
├── .env                 # Environment variable definitions (excluded from version control)
├── .gitignore           # VCS exclusion rules
└── data/                # Persistent application data (volume mount target)
```

---

## Compliance & Best Practices

* Separation of configuration and code via environment variables
* Stateless container design with externalized persistence
* Minimal container responsibility (single-service focus)
* Version-controlled infrastructure definition
* Secure handling of credentials and secrets

---

## Notes

This specification is intended for development and scalable deployment scenarios. Production environments may require additional layers such as TLS encryption, monitoring, logging aggregation, and orchestration platforms.
