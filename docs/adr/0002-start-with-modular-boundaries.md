# ADR 0002: Start with Modular Boundaries Before Service Extraction

## Status

Accepted

## Context

NutriCore is designed as a cloud-native health platform for nutrition professionals.

The platform includes multiple business capabilities, including identity and access management, user profiles, nutrition workflows, secure messaging, scheduling, file management, document generation, notifications, and analytics.

It is tempting to start by creating many independent microservices immediately. However, premature service extraction can introduce unnecessary distributed complexity before domain boundaries are clearly understood.

Microservices add operational cost, including:

- Network communication
- Distributed transactions
- Eventual consistency
- Deployment coordination
- Observability requirements
- Cross-service testing complexity
- Service-to-service security
- Data synchronization challenges

For NutriCore, the primary goal is to build clear business capabilities first and extract services only when the boundaries are stable enough to justify independent deployment and ownership.

## Decision

NutriCore will follow a modular-first architecture strategy.

The system will be designed around clear domain boundaries from the beginning, but service extraction will happen gradually.

The platform will start by documenting and implementing business capabilities as clearly separated modules or services with explicit responsibilities, APIs, and data ownership rules.

A domain may become an independent service when it has:

- A clear business responsibility
- Stable domain boundaries
- Independent data ownership
- Clear API contracts
- A reason to scale independently
- A reason to deploy independently
- Acceptable operational complexity
- Well-understood communication patterns

Microservices will be introduced as an architectural evolution, not as a starting assumption.

## Rationale

This decision reduces the risk of building a distributed system with unclear boundaries.

A modular-first approach allows the team to:

- Learn the domain before splitting the system too aggressively
- Keep early development faster and easier to reason about
- Avoid distributed complexity while the product model is still evolving
- Design future service boundaries intentionally
- Keep business logic cohesive
- Improve testability and maintainability
- Make service extraction based on evidence rather than speculation

This approach also makes the architecture more credible because it reflects real-world engineering trade-offs.

## Service Extraction Criteria

A module or domain can be considered for service extraction when most of the following are true:

- The domain has a stable responsibility
- The domain owns a clear set of data
- The domain has minimal direct dependency on other domains
- The domain has independent scaling needs
- The domain has different deployment frequency
- The domain has security or isolation requirements
- The domain communicates through well-defined APIs or events
- The domain can be tested independently
- The operational cost is justified

## Examples

### Good Candidates for Early Separation

Auth Service may become independent early because authentication and authorization are security-critical and have clear responsibilities.

File Service may become independent because file storage, metadata handling, access validation, and future scanning workflows have distinct operational concerns.

Notification Service may become independent because it processes asynchronous events and does not need to block user-facing workflows.

### Candidates That Should Wait

Nutrition workflows should not be split too early because the domain model may evolve significantly during early product development.

Analytics should likely start as a later capability because it depends on events and data produced by other domains.

Chat and scheduling should be designed with clear boundaries first, then extracted when communication and scaling patterns become clearer.

## Consequences

### Positive

- Reduces premature distributed complexity
- Keeps early development understandable
- Encourages domain-driven boundaries
- Improves maintainability
- Makes future service extraction more intentional
- Helps contributors understand system evolution
- Supports realistic engineering decision-making

### Negative

- Requires discipline to keep module boundaries clean
- May require refactoring when extracting services later
- Some deployment independence is delayed
- Cross-domain boundaries need documentation and review
- The team must avoid turning the modular system into a tightly coupled monolith

## Implementation Guidelines

To support this decision:

- Keep domain responsibilities explicit
- Avoid direct database access across domains
- Define API contracts early
- Use events only where asynchronous workflows are justified
- Document important decisions through ADRs
- Keep architecture documentation updated
- Use pull requests to review boundary changes
- Treat service extraction as an architectural milestone

## Current Direction

NutriCore will continue to document platform architecture, service maps, security rules, and communication patterns before implementing each service.

The initial implementation should focus on correctness, clarity, and maintainability.

Distributed deployment will be introduced gradually as the platform matures.
