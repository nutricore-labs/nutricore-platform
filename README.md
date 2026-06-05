# NutriCore Platform

NutriCore is a cloud-native health platform for nutrition professionals.

The platform is designed around secure client communication, scheduling workflows, nutrition planning, file processing, document generation, and scalable service boundaries.

This repository acts as the central entry point for the NutriCore ecosystem. It contains platform-level documentation, architecture decisions, local development planning, service boundaries, and engineering workflow guidelines.

## Engineering Focus

NutriCore is built as a production-oriented backend platform with a focus on:

- Clean architecture
- Domain-driven service boundaries
- Security-first design
- API-first development
- Event-driven communication where it provides clear value
- Local development and infrastructure planning
- Pull-request based development workflow
- Documented architecture decisions

## Repository Role

The `nutricore-platform` repository is responsible for:

- Platform overview
- Architecture documentation
- Service map and domain boundaries
- Security model
- API guidelines
- Event-driven communication strategy
- Local development environment planning
- Roadmap and engineering workflow
- Cross-service decisions and documentation

Service implementation repositories can be added as service boundaries become stable.

## Documentation

| Document | Purpose |
|---|---|
| [Documentation Index](docs/index.md) | Central navigation page for NutriCore technical documentation |
| [Roadmap](docs/roadmap.md) | Product and engineering milestones |
| [Architecture Overview](docs/architecture/overview.md) | High-level platform architecture |
| [Service Map](docs/architecture/service-map.md) | Planned services, responsibilities, data ownership, and communication patterns |
| [Security Model](docs/security/security-model.md) | Authentication, authorization, roles, file security, chat security, and access control |
| [API Guidelines](docs/architecture/api-guidelines.md) | REST API conventions, versioning, validation, error format, and review checklist |
| [Event-Driven Communication](docs/architecture/event-driven-communication.md) | RabbitMQ direction, event ownership, reliability, retry strategy, and async workflows |
| [Local Development](docs/development/local-development.md) | Planned Docker-based local development environment |
| [Contributing](CONTRIBUTING.md) | Branch naming, commit style, pull requests, code review, and definition of done |

## Architecture Direction

NutriCore follows a modular-first architecture strategy.

The platform starts with clear domain boundaries and evolves toward independently deployable services when those boundaries become stable. Service extraction is driven by business responsibility, data ownership, security requirements, communication patterns, and operational need.

The goal is not to create microservices for the sake of technology. The goal is to design a system that can grow without becoming unclear, tightly coupled, or difficult to operate.

## Planned Platform Capabilities

- Identity and access management
- Nutrition professional and client profiles
- Nutrition plans and progress tracking
- Secure messaging
- Appointment scheduling
- Calendar integration direction
- File upload and metadata processing
- PDF document generation
- Notifications and background workflows
- Analytics and reporting direction

## Planned Technology Direction

- Java
- Spring Boot
- Spring Security
- PostgreSQL
- MongoDB
- RabbitMQ
- Redis
- MinIO / S3-compatible object storage
- Docker
- Kubernetes
- Helm
- GitHub Actions
- Prometheus
- Grafana

## Development Workflow

NutriCore follows an issue-based and pull-request based workflow:

1. Create or pick an issue
2. Move it to `In Progress`
3. Create a focused branch
4. Implement the change
5. Open a pull request
6. Review and merge
7. Move the issue to `Done`

For details, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Current Status

NutriCore is in the platform foundation phase.

Current focus:

- Organization and repository setup
- Architecture documentation
- Service boundary design
- Security model definition
- API and event communication guidelines
- Local development planning
- CI and repository quality checks

## Project Philosophy

NutriCore is designed as a serious engineering initiative rather than a feature-only prototype.

The project prioritizes clear architecture, maintainable code, explicit decisions, secure workflows, and realistic backend engineering practices.
