# Local Development Environment

This document describes the planned local development environment for the NutriCore platform.

The goal is to provide a consistent developer experience for running platform dependencies locally and preparing the system for service-based development.

## Goals

The local environment should make it easy to:

- Run required platform dependencies locally
- Develop services independently
- Test service communication
- Validate database and messaging integrations
- Keep environment setup predictable for contributors
- Prepare the platform for Docker-based orchestration

## Planned Local Dependencies

NutriCore will use Docker Compose for local infrastructure dependencies.

The initial local environment should include:

| Dependency | Purpose |
|---|---|
| PostgreSQL | Transactional data for core services |
| MongoDB | Flexible document/message-oriented storage where appropriate |
| RabbitMQ | Asynchronous communication and background workflows |
| MinIO | S3-compatible object storage for uploaded files and generated documents |
| Redis | Token/session/cache direction where needed |
| MailHog | Local email testing |
| Prometheus | Metrics collection direction |
| Grafana | Local observability dashboards direction |

## PostgreSQL

PostgreSQL is the primary relational database for transactional business data.

Planned usage:

- Auth Service
- User Service
- Nutrition Service
- Scheduling Service
- File metadata
- Document metadata
- Notification delivery records

Each service should own its schema or database.

Direct cross-service database access is not allowed.

## MongoDB

MongoDB may be used where flexible document-like data is a better fit.

Planned usage direction:

- Chat message history
- Flexible metadata
- Future document-oriented read models

MongoDB should not be used as a default choice for all services. Its usage should be justified by the data model.

## RabbitMQ

RabbitMQ is used for asynchronous workflows.

Planned use cases:

- Notification delivery
- Appointment reminders
- File metadata processing
- Document generation
- Analytics updates
- Background processing

Messaging should be used when asynchronous processing provides a clear benefit.

## MinIO

MinIO provides S3-compatible object storage for local development.

Planned usage:

- Uploaded client files
- Generated PDF documents
- Archived documents
- File processing workflows

Services should store metadata in their own database and object references in MinIO.

## Redis

Redis may be used for short-lived data and performance-oriented workflows.

Potential usage:

- Refresh token/session direction
- Rate limiting direction
- Temporary verification data
- Caching direction

Redis should not become the source of truth for business data.

## MailHog

MailHog is used for local email testing.

Planned usage:

- Registration emails
- Appointment reminders
- Notification testing
- Document generation notifications

## Observability Direction

The local environment should support observability from the beginning.

Planned local observability:

- Health checks
- Application metrics
- Prometheus scraping direction
- Grafana dashboards direction
- Structured logging direction

Observability should be introduced gradually as services are implemented.

## Docker Compose Direction

The platform should provide a root-level `docker-compose.yml` for local dependencies.

Expected direction:

```text
docker-compose.yml
.env.example
scripts/
  start-local.sh
  stop-local.sh
  reset-local.sh
```

The local environment should support:

- Starting all dependencies with one command
- Reusing consistent ports
- Providing default credentials only for local development
- Keeping secrets out of version control
- Resetting local state when needed

## Environment Variables

The repository should provide `.env.example` with safe local defaults.

Real secrets must not be committed.

Expected examples:

```text
POSTGRES_USER=nutricore
POSTGRES_PASSWORD=nutricore
POSTGRES_DB=nutricore
MONGODB_USERNAME=nutricore
MONGODB_PASSWORD=nutricore
RABBITMQ_DEFAULT_USER=nutricore
RABBITMQ_DEFAULT_PASS=nutricore
MINIO_ROOT_USER=nutricore
MINIO_ROOT_PASSWORD=nutricore-local-password
```

## Port Direction

Suggested local ports:

| Service | Port |
|---|---:|
| PostgreSQL | 5432 |
| MongoDB | 27017 |
| RabbitMQ | 5672 |
| RabbitMQ Management | 15672 |
| MinIO API | 9000 |
| MinIO Console | 9001 |
| Redis | 6379 |
| MailHog SMTP | 1025 |
| MailHog UI | 8025 |
| Prometheus | 9090 |
| Grafana | 3000 |

Ports may change if conflicts appear.

## Development Principles

- Local setup should be simple and repeatable
- Infrastructure dependencies should be containerized
- Secrets should not be committed
- Each service should own its data
- Local dependencies should match production direction where reasonable
- Documentation should explain how to start, stop, and reset the environment

## Future Implementation

A future infrastructure task should add:

- Root-level `docker-compose.yml`
- `.env.example`
- Local setup scripts
- Health checks
- Basic Prometheus and Grafana configuration
- Documentation updates
