# Security Model

NutriCore is designed as a health platform where users interact with sensitive personal, professional, communication, file, and document data.

Security is treated as a core architecture concern rather than an implementation detail.

## Security Goals

The security model focuses on:

- Protecting private client-professional communication
- Restricting access to user profiles and nutrition records
- Securing uploaded files and generated documents
- Separating user roles and permissions clearly
- Preventing unauthorized access across professional-client relationships
- Supporting auditable security-sensitive workflows
- Preparing the platform for future service-to-service security

## User Roles

NutriCore starts with three primary roles.

### Platform Admin

Platform administrators manage platform-level operations.

Responsibilities may include:

- Managing platform configuration
- Reviewing system-level audit events
- Supporting account moderation workflows
- Managing high-level operational visibility

Platform administrators should not have unrestricted access to private user data unless an explicit support or compliance workflow requires it.

### Nutrition Professional

Nutrition professionals use the platform to manage clients and nutrition workflows.

They can:

- Manage their professional profile
- Manage assigned clients
- Create and update nutrition plans
- Communicate with assigned clients
- Schedule appointments
- Upload and access files related to their clients
- Generate client-related documents

They must not access clients who are not assigned to them.

### Client

Clients use the platform to communicate with their nutrition professional and follow assigned plans.

They can:

- Manage their own profile
- View their nutrition plans
- Track progress
- Communicate with their assigned professional
- View appointments
- Upload requested files
- Access generated documents related to them

They must not access other clients' data.

## Authentication Strategy

NutriCore uses token-based authentication.

The planned direction is:

- Access tokens for short-lived API access
- Refresh tokens for session continuation
- Refresh token rotation to reduce token replay risk
- Logout flow that invalidates active refresh tokens
- Token validation for protected API endpoints

Access tokens should contain only the claims required for authorization decisions.

Sensitive profile or domain data should not be stored inside tokens.

## Token Strategy

### Access Token

Access tokens are short-lived and used for authenticated API requests.

Expected claims:

- Subject/user identifier
- Role
- Token type
- Issued-at time
- Expiration time
- Optional permission or scope direction

### Refresh Token

Refresh tokens are longer-lived and stored server-side.

The platform should support:

- Refresh token rotation
- Refresh token revocation
- Logout invalidation
- Device/session tracking direction
- Suspicious token reuse detection direction

## Authorization Strategy

Authorization is based on both role and domain relationship.

Role-based access control alone is not enough.

For example:

- A nutrition professional can access client data only if the client is assigned to that professional
- A client can access only their own plans, appointments, messages, files, and documents
- Admin workflows should be explicit and auditable

Authorization should be enforced at service boundaries.

## Access Control Rules

Initial access control rules:

| Resource | Platform Admin | Nutrition Professional | Client |
|---|---|---|---|
| Own profile | Yes | Yes | Yes |
| Professional profile | Limited | Own profile | View assigned professional |
| Client profile | Limited | Assigned clients only | Own profile |
| Nutrition plans | Limited | Assigned clients only | Own plans only |
| Chat conversations | Limited/audited | Assigned conversations only | Own conversations only |
| Appointments | Limited | Assigned appointments only | Own appointments only |
| Uploaded files | Limited/audited | Assigned clients only | Own files only |
| Generated documents | Limited/audited | Assigned clients only | Own documents only |
| Platform settings | Yes | No | No |

## Professional-Client Relationship

The professional-client relationship is a core security boundary.

Many authorization decisions depend on whether a relationship exists between a nutrition professional and a client.

The User Service owns the source of truth for this relationship.

Other services may need to validate access by checking this relationship through an API or cached authorization model.

## Service-to-Service Security Direction

As NutriCore evolves toward distributed services, internal service communication should be protected.

Planned direction:

- Internal services should not trust requests only because they come from the private network
- Service-to-service calls should carry authenticated identity or service credentials
- Sensitive operations should validate both caller identity and domain authorization
- Internal APIs should not expose unrestricted data access
- Future direction may include mTLS, signed internal tokens, or API gateway/internal gateway policies

## File Security

Uploaded files are sensitive platform resources.

The file security model should support:

- Authenticated upload and download
- File ownership and access rules
- Relationship-based access validation
- Metadata storage separate from object storage
- Restricted object storage access
- Generated signed URLs direction
- Virus scanning direction for future implementation
- File lifecycle and archival direction

Users should not access files directly from object storage without authorization checks.

## Document Security

Generated PDFs and archived documents may contain sensitive client information.

Document workflows should support:

- Access validation before generation
- Access validation before download
- Document ownership metadata
- Document generation audit records
- Restricted storage access
- Archival and retention direction

## Chat Security

Private communication between clients and professionals must be protected.

The chat security model should support:

- Access only for conversation participants
- Relationship validation before conversation creation
- Message history access control
- Secure WebSocket authentication direction
- Read status visibility rules
- Message event publishing without leaking sensitive content unnecessarily

## Audit Direction

Security-sensitive actions should be auditable.

Examples:

- Login attempts
- Token refresh events
- Logout events
- Failed authorization attempts
- File upload and download
- Document generation and download
- Admin access to sensitive resources
- Professional-client relationship changes

Audit records should be designed carefully to avoid storing unnecessary sensitive data.

## Security Principles

NutriCore follows these security principles:

- Deny by default
- Authenticate every protected request
- Authorize based on role and domain relationship
- Avoid storing sensitive data in tokens
- Keep service databases private
- Validate access at service boundaries
- Treat files and documents as sensitive resources
- Make admin access explicit and auditable
- Prepare service-to-service security early
- Prefer clear security rules over hidden assumptions

## Current Status

This document defines the initial security model.

Implementation details will be refined during the design and implementation of the Auth Service, User Service, File Service, Chat Service, and Document Service.
