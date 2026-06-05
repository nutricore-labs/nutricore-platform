# API Guidelines

This document defines the API design guidelines for the NutriCore platform.

The goal is to keep service APIs consistent, predictable, secure, and easy to evolve as the platform grows.

## Goals

NutriCore APIs should be:

- Consistent across services
- Easy to understand and consume
- Secure by default
- Versioned intentionally
- Documented with OpenAPI
- Designed around domain capabilities
- Stable enough for future frontend and service integrations

## API Design Principles

APIs should expose business capabilities, not internal implementation details.

A good API should:

- Represent a clear domain action or resource
- Avoid leaking database structure
- Use predictable request and response formats
- Return meaningful error responses
- Validate input explicitly
- Avoid unnecessary coupling between services

## URL Structure

Use resource-oriented URLs.

Examples:

```text
/api/v1/profiles
/api/v1/clients/{clientId}
/api/v1/nutrition-plans
/api/v1/appointments
/api/v1/files
/api/v1/documents
```

Use nouns for resources.

Prefer:

```text
POST /api/v1/appointments
GET /api/v1/clients/{clientId}
PATCH /api/v1/nutrition-plans/{planId}
```

Avoid action-heavy URLs unless the operation is clearly not a simple resource operation.

Acceptable action-style examples:

```text
POST /api/v1/auth/login
POST /api/v1/auth/refresh
POST /api/v1/documents/{documentId}/generate
POST /api/v1/files/{fileId}/archive
```

## HTTP Methods

Use HTTP methods consistently.

| Method | Purpose |
|---|---|
| `GET` | Retrieve data |
| `POST` | Create resources or trigger commands |
| `PUT` | Replace a resource |
| `PATCH` | Partially update a resource |
| `DELETE` | Delete or deactivate a resource |

Use `POST` for command-like operations that do not map cleanly to CRUD.

## Versioning

Public APIs should be versioned through the URL.

Initial version:

```text
/api/v1
```

Breaking changes should require a new version.

Examples of breaking changes:

- Removing a field
- Renaming a field
- Changing response shape
- Changing validation behavior in a way that breaks existing clients
- Changing authorization behavior for existing endpoints

Non-breaking changes may include:

- Adding optional fields
- Adding new endpoints
- Adding new enum values when clients are prepared for unknown values

## Request Format

Request bodies should use JSON unless the endpoint explicitly handles file uploads.

Example:

```json
{
  "clientId": "client-123",
  "scheduledAt": "2026-06-05T10:00:00Z",
  "durationMinutes": 60
}
```

Use clear field names.

Prefer:

```json
{
  "nutritionPlanId": "plan-123"
}
```

Avoid unclear abbreviations:

```json
{
  "npid": "plan-123"
}
```

## Response Format

Successful responses should return predictable JSON.

Example:

```json
{
  "id": "appointment-123",
  "clientId": "client-123",
  "status": "SCHEDULED",
  "scheduledAt": "2026-06-05T10:00:00Z"
}
```

Collection responses should support pagination where needed.

Example:

```json
{
  "items": [],
  "page": 0,
  "size": 20,
  "totalElements": 0,
  "totalPages": 0
}
```

## Error Response Format

APIs should return consistent error responses.

Suggested format:

```json
{
  "timestamp": "2026-06-05T10:00:00Z",
  "status": 400,
  "error": "Bad Request",
  "code": "VALIDATION_ERROR",
  "message": "Request validation failed.",
  "path": "/api/v1/appointments",
  "details": [
    {
      "field": "scheduledAt",
      "message": "scheduledAt must be in the future"
    }
  ]
}
```

Error responses should be useful for clients without exposing sensitive internal details.

## Status Codes

Use HTTP status codes consistently.

| Status | Usage |
|---:|---|
| `200 OK` | Successful read or update |
| `201 Created` | Resource created |
| `204 No Content` | Successful operation without response body |
| `400 Bad Request` | Invalid request |
| `401 Unauthorized` | Missing or invalid authentication |
| `403 Forbidden` | Authenticated but not allowed |
| `404 Not Found` | Resource does not exist or is not visible to the caller |
| `409 Conflict` | Business conflict |
| `422 Unprocessable Entity` | Valid JSON but invalid business input |
| `500 Internal Server Error` | Unexpected server error |

Use `404` instead of `403` when revealing resource existence would be sensitive.

## Validation

Validate input at API boundaries.

Validation should cover:

- Required fields
- Field length
- Field format
- Enum values
- Date/time constraints
- Ownership and relationship constraints
- Business rules

Validation errors should return structured details when possible.

## Pagination

List endpoints should support pagination.

Suggested query parameters:

```text
page=0
size=20
sort=createdAt,desc
```

Example:

```text
GET /api/v1/clients?page=0&size=20&sort=createdAt,desc
```

Default page size should be reasonable.

Maximum page size should be limited to protect the system.

## Filtering and Search

Filtering should use explicit query parameters.

Examples:

```text
GET /api/v1/appointments?status=SCHEDULED
GET /api/v1/clients?professionalId=professional-123
GET /api/v1/files?ownerId=client-123&type=PDF
```

Avoid overly dynamic filtering until there is a real need.

## Idempotency

Critical command endpoints should consider idempotency.

Examples:

- Document generation requests
- File archival requests
- Appointment booking retries

Idempotency keys may be introduced for operations where client retries could create duplicates.

## Security Requirements

APIs must enforce authentication and authorization at service boundaries.

Security expectations:

- Protected endpoints require authentication
- Access control must check role and domain relationship
- Sensitive resources require ownership validation
- Tokens must not contain unnecessary domain data
- Error messages must not leak sensitive information
- File and document access must be validated before returning references

## OpenAPI Documentation

Each service should provide OpenAPI documentation.

Expected direction:

- Public endpoints documented
- Request and response schemas defined
- Error responses documented
- Authentication requirements included
- API examples provided for important workflows

OpenAPI definitions should be reviewed when API changes are introduced.

## Backward Compatibility

APIs should avoid breaking clients unnecessarily.

Before introducing a breaking change:

- Check whether the change can be additive
- Consider versioning
- Document migration steps
- Update OpenAPI documentation
- Communicate the change through release notes or PR description

## Internal APIs

Internal service APIs should still follow consistent rules.

Internal does not mean unsafe.

Internal APIs should:

- Validate input
- Enforce authorization where needed
- Avoid direct database sharing
- Use clear contracts
- Document important endpoints

## API Review Checklist

Before merging an API change, check:

- [ ] The endpoint represents a clear domain capability
- [ ] The URL and method are consistent
- [ ] Request and response formats are documented
- [ ] Error responses follow the standard format
- [ ] Authentication and authorization are considered
- [ ] Validation rules are defined
- [ ] Pagination is considered for list endpoints
- [ ] OpenAPI documentation is updated
- [ ] Breaking changes are avoided or versioned

## Current Status

These guidelines define the initial API direction for NutriCore.

They will evolve as service implementations become more concrete.
