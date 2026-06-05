# Service Map

This document describes the planned service boundaries for the NutriCore platform.

The goal of the service map is to define clear business responsibilities, data ownership, and communication patterns before extracting services into independently deployable components.

## Service Boundary Principles

NutriCore services are organized around business capabilities rather than technical layers.

A service boundary should be considered stable when it has:

- A clear business responsibility
- Independent data ownership
- A well-defined API
- Clear security rules
- A reason to scale, deploy, or evolve independently
- Minimal need for direct database access across domains

Services should communicate through APIs or events. Direct access to another service database is not allowed.

## Planned Services

## API Gateway

### Responsibility

The API Gateway is the external entry point for client applications.

It is responsible for routing requests to internal services and applying cross-cutting concerns at the edge.

### Main Capabilities

- Request routing
- Authentication token forwarding
- Rate limiting direction
- Request validation direction
- API versioning direction
- Centralized external API entry point

### Data Ownership

The API Gateway should not own business data.

### Communication

- Receives external HTTP requests
- Routes requests to internal services through HTTP APIs

---

## Auth Service

### Responsibility

The Auth Service is responsible for identity, authentication, authorization, and access control.

### Main Capabilities

- User registration direction
- Login and logout
- Access token generation
- Refresh token rotation
- Role-based access control
- Permission validation
- Account security workflows

### Data Ownership

Owns:

- Credentials
- Refresh tokens
- Roles
- Permissions
- Authentication-related audit records

Does not own:

- Nutrition professional profile details
- Client profile details
- Nutrition plans
- Chat messages

### Communication

Synchronous:

- Login
- Token refresh
- Token validation direction
- Permission checks where needed

Asynchronous:

- UserRegistered event
- UserDeactivated event
- SecurityEventCreated event

---

## User Service

### Responsibility

The User Service manages platform user profiles and relationships between nutrition professionals and clients.

### Main Capabilities

- Nutrition professional profiles
- Client profiles
- Account settings
- Professional-client relationship management
- Basic profile search direction

### Data Ownership

Owns:

- User profile data
- Professional profile data
- Client profile data
- Professional-client relationship records

Does not own:

- Credentials
- Tokens
- Nutrition plans
- Chat messages
- Appointment records

### Communication

Synchronous:

- Fetch profile by user id
- Fetch professional-client relationship
- Validate relationship access

Asynchronous:

- ProfileCreated event
- ProfileUpdated event
- ClientAssignedToProfessional event

---

## Nutrition Service

### Responsibility

The Nutrition Service manages nutrition-specific workflows.

### Main Capabilities

- Nutrition plans
- Meal recommendations direction
- Client progress tracking
- Professional notes
- Plan lifecycle management
- Client goals and measurements direction

### Data Ownership

Owns:

- Nutrition plans
- Progress records
- Goals
- Professional nutrition notes
- Plan status history

Does not own:

- User credentials
- Chat messages
- Files
- PDF documents

### Communication

Synchronous:

- Create nutrition plan
- Get active plan
- Update progress
- Fetch client nutrition summary

Asynchronous:

- NutritionPlanCreated event
- ProgressUpdated event
- NutritionPlanCompleted event

---

## Chat Service

### Responsibility

The Chat Service manages secure communication between nutrition professionals and clients.

### Main Capabilities

- Conversation creation
- Message sending
- Message history
- Read status direction
- Secure communication workflows
- WebSocket direction for real-time communication

### Data Ownership

Owns:

- Conversations
- Messages
- Message delivery status
- Read receipts direction

Does not own:

- User profiles
- Authentication tokens
- Appointment records
- Nutrition plans

### Communication

Synchronous:

- Create conversation
- Send message
- Fetch conversation history

Asynchronous:

- MessageSent event
- MessageRead event
- ConversationCreated event

---

## Scheduling Service

### Responsibility

The Scheduling Service manages appointments, availability, reminders, and calendar-related workflows.

### Main Capabilities

- Professional availability
- Appointment booking
- Appointment cancellation
- Appointment rescheduling
- Reminder scheduling
- Calendar integration direction

### Data Ownership

Owns:

- Availability slots
- Appointments
- Scheduling rules
- Calendar sync references

Does not own:

- User profiles
- Chat messages
- Nutrition plans

### Communication

Synchronous:

- Create appointment
- Check availability
- Reschedule appointment
- Cancel appointment

Asynchronous:

- AppointmentCreated event
- AppointmentCancelled event
- AppointmentReminderRequested event

---

## File Service

### Responsibility

The File Service manages secure file upload, storage references, metadata, and access control.

### Main Capabilities

- Secure file upload
- File metadata extraction
- File access validation
- Storage reference management
- File lifecycle direction
- Virus scanning direction for future implementation

### Data Ownership

Owns:

- File metadata
- Storage references
- File access rules
- File lifecycle state

Does not own:

- Generated PDF business logic
- Nutrition plans
- Chat messages

### Communication

Synchronous:

- Upload file
- Download file
- Fetch file metadata
- Validate file access

Asynchronous:

- FileUploaded event
- FileMetadataExtracted event
- FileArchived event

---

## Document Service

### Responsibility

The Document Service manages generated documents, PDF creation, templates, and archival workflows.

### Main Capabilities

- PDF generation
- Document templates
- Generated report management
- Document archival
- Document generation status tracking

### Data Ownership

Owns:

- Document templates
- Generated document records
- Document generation jobs
- Archival status

Does not own:

- Raw uploaded files
- User profiles
- Nutrition plan source data

### Communication

Synchronous:

- Request document generation
- Fetch generated document status
- Download generated document reference

Asynchronous:

- DocumentGenerationRequested event
- DocumentGenerated event
- DocumentArchived event

---

## Notification Service

### Responsibility

The Notification Service manages outgoing notifications and asynchronous communication with users.

### Main Capabilities

- Email notifications
- Appointment reminders
- Message notifications
- Document generation notifications
- Event-based notification processing

### Data Ownership

Owns:

- Notification templates
- Notification delivery records
- Notification preferences direction
- Delivery status

Does not own:

- Appointments
- Chat messages
- Nutrition plans
- User credentials

### Communication

Synchronous:

- Fetch notification preferences direction
- Update notification preferences direction

Asynchronous:

- Consumes AppointmentCreated
- Consumes MessageSent
- Consumes DocumentGenerated
- Consumes NutritionPlanCreated
- Emits NotificationSent
- Emits NotificationFailed

---

## Analytics Service

### Responsibility

The Analytics Service provides reporting, insights, and aggregated platform data.

### Main Capabilities

- Client progress summaries
- Professional dashboard metrics
- Platform activity insights
- Reporting direction
- Aggregated read models

### Data Ownership

Owns:

- Aggregated reporting models
- Analytics snapshots
- Dashboard read models

Does not own:

- Source-of-truth transactional data
- Credentials
- Chat source messages
- Appointment source records

### Communication

Synchronous:

- Fetch dashboard summary
- Fetch reporting data

Asynchronous:

- Consumes ProgressUpdated
- Consumes AppointmentCreated
- Consumes NutritionPlanCreated
- Consumes MessageSent

---

## Infrastructure Repository

### Responsibility

The infrastructure repository contains deployment and operational configuration.

### Main Capabilities

- Docker Compose
- Kubernetes manifests
- Helm charts
- Monitoring configuration
- Environment configuration examples
- Deployment documentation

### Data Ownership

The infrastructure repository does not own business data.

## Communication Summary

| Area | Preferred Communication |
|---|---|
| Login and token refresh | Synchronous HTTP |
| Profile lookup | Synchronous HTTP |
| Nutrition plan creation | Synchronous HTTP + event |
| Message sending | Synchronous HTTP/WebSocket + event |
| Appointment reminders | Asynchronous messaging |
| File metadata processing | Asynchronous messaging |
| PDF generation | Asynchronous messaging |
| Notifications | Asynchronous messaging |
| Analytics updates | Asynchronous messaging |

## Storage Direction

| Service | Planned Storage |
|---|---|
| Auth Service | PostgreSQL, Redis direction |
| User Service | PostgreSQL |
| Nutrition Service | PostgreSQL |
| Chat Service | MongoDB |
| Scheduling Service | PostgreSQL |
| File Service | PostgreSQL + Object Storage |
| Document Service | PostgreSQL + Object Storage |
| Notification Service | PostgreSQL |
| Analytics Service | PostgreSQL or analytical read model direction |

## Notes

This service map is expected to evolve as the domain becomes clearer.

Service extraction should happen only when a boundary is stable enough to justify independent deployment, independent data ownership, and operational complexity.
