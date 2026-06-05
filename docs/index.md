# NutriCore Documentation

This page is the central index for NutriCore platform documentation.

NutriCore documentation is organized around architecture, security, development workflow, local infrastructure, and product planning.

## Platform Overview

| Document | Description |
|---|---|
| [Roadmap](roadmap.md) | Product and engineering milestones for the platform |
| [Architecture Overview](architecture/overview.md) | High-level platform architecture, core domains, communication direction, and engineering principles |
| [Service Map](architecture/service-map.md) | Planned service boundaries, responsibilities, data ownership, and communication patterns |

## Architecture

| Document | Description |
|---|---|
| [API Guidelines](architecture/api-guidelines.md) | REST API design principles, versioning, validation, error format, and review checklist |
| [Event-Driven Communication](architecture/event-driven-communication.md) | RabbitMQ direction, event naming, event ownership, reliability, retry strategy, and async workflows |

## Security

| Document | Description |
|---|---|
| [Security Model](security/security-model.md) | Authentication, authorization, roles, access control, file security, chat security, and audit direction |

## Development

| Document | Description |
|---|---|
| [Local Development](development/local-development.md) | Planned Docker-based local development environment and infrastructure dependencies |
| [Contributing](../CONTRIBUTING.md) | Branch naming, commit style, pull request expectations, code review guidelines, and definition of done |

## Architecture Decision Records

| ADR | Status | Description |
|---|---|---|
| [ADR 0001](adr/0001-use-platform-oriented-repository-structure.md) | Accepted | Use a platform-oriented repository structure |
| [ADR 0002](adr/0002-start-with-modular-boundaries.md) | Accepted | Start with modular boundaries before service extraction |

## Documentation Principles

NutriCore documentation should be:

- Easy to navigate
- Close to the repository
- Updated together with architecture changes
- Written for contributors, reviewers, and future maintainers
- Clear enough to understand without private context

## Current Focus

The current documentation focus is platform foundation:

- Architecture direction
- Service boundaries
- Security model
- API and event communication standards
- Local development planning
- Contribution workflow
