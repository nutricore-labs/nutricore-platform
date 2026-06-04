# ADR 0001: Use a Platform-Oriented Repository Structure

## Status

Accepted

## Context

NutriCore is designed as a multi-domain platform with several core capabilities, including authentication, user management, nutrition workflows, secure messaging, scheduling, file processing, document generation, and notifications.

The project should be understandable for contributors and reviewers without requiring them to inspect every service repository independently.

## Decision

NutriCore will use a platform-oriented repository structure.

The `nutricore-platform` repository acts as the central entry point for:

- Architecture documentation
- Service maps
- Local orchestration
- Development workflows
- Cross-service decisions
- Roadmaps and planning

Individual services may live in separate repositories when their boundaries become stable.

## Consequences

### Positive

- Easier onboarding for contributors
- Clear system-level documentation
- Better visibility into architecture decisions
- More realistic startup-style repository organization
- Separation between platform documentation and service implementation

### Negative

- Requires discipline to keep documentation synchronized
- Cross-repository changes need clear coordination
