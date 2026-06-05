# Event-Driven Communication Strategy

This document defines the event-driven communication strategy for the NutriCore platform.

The goal is to use asynchronous communication where it provides clear value, while avoiding unnecessary complexity when synchronous communication is simpler and more appropriate.

## Goals

NutriCore uses event-driven communication to support:

- Background processing
- Notifications
- File processing workflows
- Document generation workflows
- Appointment reminders
- Analytics updates
- Loose coupling between services
- Future scalability of long-running processes

Messaging should not be used everywhere by default.

The platform should choose communication patterns based on business needs, consistency requirements, and operational complexity.

## Communication Principles

NutriCore follows these communication principles:

- Use synchronous APIs when the caller needs an immediate response
- Use events when work can happen asynchronously
- Avoid distributed complexity without a clear reason
- Keep event payloads focused and stable
- Avoid leaking sensitive data through events
- Prefer explicit event names and clear ownership
- Design event consumers to be idempotent
- Document important event contracts

## Synchronous Communication

Synchronous HTTP APIs should be used when:

- A user action requires an immediate result
- The caller needs direct validation
- The operation is part of a request-response workflow
- The result must be shown immediately in the UI
- The operation requires strong consistency

Examples:

| Workflow | Communication |
|---|---|
| Login | Synchronous |
| Token refresh | Synchronous |
| Fetch user profile | Synchronous |
| Create appointment | Synchronous command + optional event |
| Send chat message | Synchronous/WebSocket + event |
| Upload file metadata | Synchronous command + optional event |
| Request document generation | Synchronous command + async processing |

## Asynchronous Communication

Asynchronous messaging should be used when:

- Work can happen in the background
- Multiple services react to the same business event
- The operation is long-running
- Retry behavior is needed
- Temporary failure should not block the user request
- Eventual consistency is acceptable

Examples:

| Workflow | Communication |
|---|---|
| Appointment reminder | Asynchronous |
| Notification delivery | Asynchronous |
| File metadata extraction | Asynchronous |
| PDF generation | Asynchronous |
| Document archival | Asynchronous |
| Analytics update | Asynchronous |
| Audit event processing | Asynchronous |

## Message Broker Direction

NutriCore plans to use RabbitMQ for asynchronous communication.

RabbitMQ is a good fit for:

- Work queues
- Background jobs
- Notification processing
- Document generation tasks
- File processing tasks
- Retryable workflows
- Clear producer-consumer communication

RabbitMQ should not be used only to make the architecture look distributed.

Each event-driven workflow should have a clear business reason.

## Event Naming

Events should describe something that already happened.

Use past-tense names.

Examples:

```text
UserRegistered
ProfileCreated
ClientAssignedToProfessional
NutritionPlanCreated
ProgressUpdated
AppointmentCreated
AppointmentCancelled
AppointmentReminderRequested
MessageSent
FileUploaded
FileMetadataExtracted
DocumentGenerationRequested
DocumentGenerated
NotificationSent
NotificationFailed
```

Avoid vague names:

```text
UserEvent
NotificationEvent
DataChanged
UpdateEvent
```

## Event Ownership

Each event should have a clear owning service.

| Event | Owner |
|---|---|
| `UserRegistered` | Auth Service |
| `ProfileCreated` | User Service |
| `ClientAssignedToProfessional` | User Service |
| `NutritionPlanCreated` | Nutrition Service |
| `ProgressUpdated` | Nutrition Service |
| `AppointmentCreated` | Scheduling Service |
| `AppointmentCancelled` | Scheduling Service |
| `MessageSent` | Chat Service |
| `FileUploaded` | File Service |
| `DocumentGenerated` | Document Service |
| `NotificationSent` | Notification Service |

Only the owning service should publish the source-of-truth event for its domain.

## Event Payload Principles

Event payloads should contain enough information for consumers to process the event without exposing unnecessary sensitive data.

Events should include:

- Event id
- Event type
- Event version
- Occurred-at timestamp
- Producer service
- Correlation id
- Relevant resource identifiers
- Minimal business data needed by consumers

Example event payload:

- `eventId`: `evt-123`
- `eventType`: `AppointmentCreated`
- `eventVersion`: `1`
- `occurredAt`: `2026-06-05T10:00:00Z`
- `producer`: `scheduling-service`
- `correlationId`: `corr-123`
- `appointmentId`: `appointment-123`
- `clientId`: `client-123`
- `professionalId`: `professional-123`
- `scheduledAt`: `2026-06-06T12:00:00Z`

Events should not include:

- Passwords
- Access tokens
- Refresh tokens
- Private document content
- Full chat message content unless explicitly required
- Sensitive file content
- Unnecessary personal data

## Event Versioning

Events should be versioned.

Use an integer version field, for example: `eventVersion = 1`.

Non-breaking changes may include:

- Adding optional fields
- Adding metadata
- Adding new event types

Breaking changes may include:

- Removing fields
- Renaming fields
- Changing field meaning
- Changing nested structure
- Changing required fields

Breaking changes should introduce a new event version and migration strategy.

## Reliability Expectations

Event-driven workflows should be designed for reliability.

Consumers should be prepared for:

- Duplicate messages
- Temporary broker failures
- Consumer restarts
- Out-of-order processing in some workflows
- Failed downstream dependencies

Consumers should be idempotent where possible.

A consumer should be able to safely process the same event more than once without corrupting data.

## Retry Strategy

Retry behavior should be explicit.

Initial direction:

- Retry transient failures
- Do not retry permanent validation errors forever
- Log failed processing attempts
- Move repeatedly failing messages to a dead-letter queue direction
- Make failed events observable

Examples of retryable failures:

- Temporary email provider failure
- Temporary object storage failure
- Temporary database connection issue

Examples of non-retryable failures:

- Invalid event payload
- Missing required event fields
- Unauthorized operation
- Unsupported event version

## Dead-Letter Queue Direction

Dead-letter queues should be used for messages that cannot be processed after retries.

Dead-lettered messages should be:

- Logged
- Observable
- Investigated
- Reprocessed manually or through a controlled workflow where appropriate

Dead-letter queues should not become invisible storage for broken workflows.

## Correlation and Traceability

Events should carry correlation identifiers.

Correlation id helps connect:

- Incoming user request
- Service operation
- Published event
- Consumed event
- Background processing result
- Logs and metrics

This improves debugging and observability across distributed workflows.

## Security Considerations

Events may cross service boundaries and must be designed carefully.

Security expectations:

- Do not publish sensitive data unless necessary
- Avoid putting private document contents into events
- Avoid putting access or refresh tokens into events
- Use identifiers and fetch additional data through authorized APIs when needed
- Validate event payloads before processing
- Restrict broker access to trusted services
- Log event metadata without leaking sensitive content

## Planned Event-Driven Workflows

### Appointment Reminder Workflow

1. Scheduling Service creates an appointment
2. Scheduling Service publishes `AppointmentCreated`
3. Notification Service schedules or prepares reminder delivery
4. Notification Service sends reminder when appropriate
5. Notification Service publishes `NotificationSent` or `NotificationFailed`

### File Processing Workflow

1. File Service stores uploaded file metadata
2. File Service publishes `FileUploaded`
3. File processing worker extracts metadata or performs validation
4. File Service updates file processing status
5. Related services may react to completed processing

### Document Generation Workflow

1. User requests document generation
2. Document Service creates a generation job
3. Document Service publishes `DocumentGenerationRequested`
4. Worker generates PDF
5. Generated document is stored in object storage
6. Document Service publishes `DocumentGenerated`
7. Notification Service may notify the user

### Analytics Update Workflow

1. Domain service publishes business event
2. Analytics Service consumes relevant events
3. Analytics Service updates read model or reporting snapshot
4. Dashboard data becomes eventually consistent

## When Not to Use Events

Do not use events when:

- The caller needs an immediate answer
- The workflow is simple request-response
- The operation requires strong consistency
- Messaging adds complexity without business value
- A direct API call is easier to understand and operate

Examples:

- Login
- Token refresh
- Fetching current user profile
- Validating immediate access
- Simple read operations

## Event Review Checklist

Before introducing a new event, check:

- [ ] The event represents something that already happened
- [ ] The owning service is clear
- [ ] The business reason for the event is clear
- [ ] The payload avoids unnecessary sensitive data
- [ ] The event has a version
- [ ] Consumers can handle duplicate delivery
- [ ] Failure and retry behavior are considered
- [ ] The event name is specific and past-tense
- [ ] The event is documented

## Current Status

This document defines the initial event-driven communication direction for NutriCore.

Event contracts will become more concrete as individual services are implemented.
