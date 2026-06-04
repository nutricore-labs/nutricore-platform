# NutriCore Platform

NutriCore is a cloud-native health platform for nutrition professionals, designed around secure communication, scheduling workflows, document processing, and scalable service boundaries.

This repository is the central entry point for the NutriCore ecosystem. It contains platform-level documentation, architecture decisions, local orchestration, service maps, and development workflows.

## Goals

- Build a production-oriented backend platform with clear domain boundaries
- Start with a modular architecture and evolve toward distributed services
- Design secure workflows for nutrition professionals and their clients
- Provide reliable communication, scheduling, file processing, and document generation
- Practice real-world engineering workflows: issues, pull requests, CI/CD, code review, and documentation

## Core Domains

- Identity and access management
- User and professional profiles
- Nutrition plans and client progress
- Secure messaging
- Appointment scheduling
- File storage and metadata processing
- PDF document generation
- Notifications and asynchronous workflows
- Analytics and reporting

## Architecture Approach

NutriCore is designed as a platform-oriented system.

The project starts with clearly defined domain boundaries and evolves toward a microservices architecture as the system grows. Service extraction is driven by business capabilities, data ownership, and communication patterns rather than by technology alone.

## Repository Map

| Repository | Purpose | Status |
|---|---|---|
| `nutricore-platform` | Architecture, documentation, orchestration, and development workflows | Active |
| `nutricore-auth-service` | Authentication, authorization, JWT, refresh tokens, and role management | Planned |
| `nutricore-user-service` | Client and nutrition professional profile management | Planned |
| `nutricore-nutrition-service` | Nutrition plans, recommendations, client progress, and domain workflows | Planned |
| `nutricore-chat-service` | Secure messaging and communication workflows | Planned |
| `nutricore-scheduling-service` | Appointments, availability, reminders, and calendar integration | Planned |
| `nutricore-file-service` | Secure file upload, storage, metadata, and access control | Planned |
| `nutricore-document-service` | PDF generation, document workflows, and archival processing | Planned |
| `nutricore-notification-service` | Email notifications and event-based communication | Planned |
| `nutricore-infra` | Docker, Kubernetes, Helm, monitoring, and deployment infrastructure | Planned |

## Engineering Principles

- Clean, readable, and testable code
- Domain-driven service boundaries
- Security-first design
- Automated testing and continuous integration
- Documented architecture decisions
- Observable services and production-oriented infrastructure
- Pull request based development

## Planned Tech Stack

- Java
- Spring Boot
- Spring Security
- PostgreSQL
- MongoDB
- RabbitMQ
- Docker
- Kubernetes
- Helm
- GitHub Actions
- Prometheus
- Grafana

## Current Status

The project is in the initial architecture and platform setup phase.

Initial focus:

- Organization setup
- Repository structure
- Architecture documentation
- Authentication service design
- Local development environment
- CI/CD foundation
