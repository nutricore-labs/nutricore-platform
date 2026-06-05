# Architecture Overview

NutriCore is a cloud-native health platform for nutrition professionals, designed around secure communication, client management, scheduling workflows, file processing, document generation, and scalable service boundaries.

This document describes the high-level architecture direction of the platform and the main engineering principles behind the system design.

## Platform Goals

NutriCore is designed to support nutrition professionals and their clients through a secure and reliable digital platform.

The platform focuses on:

- Managing nutrition professional and client profiles
- Supporting secure client-professional communication
- Scheduling appointments and calendar-based workflows
- Managing nutrition plans, progress, and recommendations
- Processing files and health-related documents
- Generating PDF documents and archived records
- Sending notifications through asynchronous workflows
- Providing a foundation for scalable service extraction

## Architectural Direction

NutriCore follows a modular-first architecture that evolves toward distributed services as domain boundaries become stable.

The initial goal is not to create microservices for the sake of technology. Instead, the platform is designed around clear business capabilities, data ownership, security requirements, and communication patterns.

Service extraction should be driven by:

- Stable domain boundaries
- Independent data ownership
- Clear business responsibility
- Operational need
- Scalability requirements
- Security and compliance concerns

This approach keeps the system understandable while still preparing it for future microservice-based deployment.

## Core Domains

The platform is organized around the following core domains:

### Identity and Access Management

Responsible for authentication, authorization, roles, permissions, access tokens, refresh tokens, and secure access to platform resources.

### User Management

Responsible for nutrition professional profiles, client profiles, account settings, and relationships between professionals and clients.

### Nutrition Workflows

Responsible for nutrition plans, client progress tracking, recommendations, notes, and domain-specific workflows used by nutrition professionals.

### Secure Communication

Responsible for client-professional messaging, conversation history, message delivery, and secure communication workflows.

### Scheduling

Responsible for appointments, availability, reminders, calendar synchronization, and scheduling-related workflows.

### File Management

Responsible for secure file uploads, metadata extraction, storage references, access control, and file lifecycle management.

### Document Processing

Responsible for PDF generation, document templates, archival workflows, and generated reports.

### Notifications

Responsible for email notifications, reminder events, background communication, and asynchronous processing.

### Analytics and Reporting

Responsible for progress summaries, activity insights, reporting, and future analytical capabilities.

## High-Level System View

NutriCore is planned as a platform composed of independently understandable domain services.

At a high level, the system contains:

- API Gateway for external request routing
- Identity and Access service for authentication and authorization
- Domain services for users, nutrition workflows, chat, scheduling, files, documents, and notifications
- Relational storage for transactional data
- Document-oriented storage where flexible metadata or message history makes sense
- Message broker for asynchronous workflows
- Object storage for uploaded files
- Observability stack for metrics, logs, and monitoring

## Communication Strategy

NutriCore uses both synchronous and asynchronous communication patterns.

Synchronous APIs are preferred when:

- A user expects an immediate response
- The operation requires direct validation
- The data is needed immediately by the caller
- The workflow is simple and request-response oriented

Asynchronous messaging is preferred when:

- The operation can happen in the background
- The workflow crosses multiple services
- The platform needs retries or eventual consistency
- The operation involves notifications, document generation, file processing, or archival

This prevents unnecessary coupling while avoiding overuse of messaging where a simple API call is enough.

## Data Ownership

Each domain should own its data model.

Services should not directly access another service's database. Cross-domain communication should happen through APIs or events.

Planned storage direction:

- PostgreSQL for transactional business data
- MongoDB for flexible document-like data where appropriate
- Object storage for uploaded files and generated documents
- RabbitMQ for asynchronous workflows and background processing

## Security Principles

Security is a core platform concern.

The architecture should support:

- Authentication with access and refresh tokens
- Role-based access control
- Clear separation between client and professional permissions
- Secure handling of uploaded files
- Restricted access to private communication
- Auditable workflows for sensitive actions
- Service-to-service security direction for future distributed deployment

## Observability Direction

The platform should be observable from the beginning.

Planned observability areas:

- Application metrics
- Structured logs
- Request tracing direction
- Health checks
- Service-level monitoring
- Infrastructure dashboards

Observability is treated as part of the architecture, not as an afterthought.

## Engineering Principles

NutriCore follows these engineering principles:

- Start simple, but design with clear boundaries
- Prefer domain-driven boundaries over technology-driven boundaries
- Keep services readable, testable, and independently understandable
- Document architectural decisions with ADRs
- Use pull requests and code review for all meaningful changes
- Automate checks through CI/CD
- Treat developer experience as part of the product
- Avoid premature distributed complexity
- Make security and observability first-class concerns

## Current Status

The platform is currently in the foundation phase.

The main focus is:

- Repository and organization setup
- Architecture documentation
- Service map definition
- Security model design
- Development workflow setup
- Local infrastructure planning
