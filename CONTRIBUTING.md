# Contributing to NutriCore

Thank you for your interest in contributing to NutriCore.

NutriCore is developed as a production-oriented health platform with a strong focus on clean architecture, secure workflows, maintainability, and clear engineering practices.

This document describes how contributions should be planned, implemented, reviewed, and merged.

## Development Workflow

NutriCore follows an issue-based and pull-request based workflow.

Every meaningful change should start with an issue that describes the problem, expected outcome, and acceptance criteria.

The general workflow is:

1. Pick or create an issue
2. Move the issue to `In Progress`
3. Create a focused branch
4. Implement the change
5. Open a pull request
6. Request review
7. Address feedback
8. Merge after approval
9. Move the issue to `Done`

## Branch Naming

Use short and descriptive branch names.

Recommended prefixes:

```text
feature/
docs/
infra/
ci/
test/
refactor/
fix/
```

Examples:

```text
docs/security-model
docs/service-map
infra/local-development-design
ci/repository-quality-checks
feature/auth-token-strategy
test/auth-login-flow
```

## Commit Messages

NutriCore uses conventional commit style.

Format:

```text
type(scope): short description
```

Examples:

```text
docs(architecture): add service communication overview
docs(security): define access control strategy
infra: design local development environment
ci: add repository quality checks
feat(auth): add refresh token rotation
test(auth): add login integration tests
refactor(user): simplify profile validation
```

Common commit types:

| Type | Purpose |
|---|---|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation changes |
| `infra` | Infrastructure changes |
| `ci` | CI/CD changes |
| `test` | Tests |
| `refactor` | Code restructuring without behavior change |
| `chore` | Maintenance work |

## Pull Request Guidelines

Pull requests should be small, focused, and easy to review.

A pull request should include:

- Clear summary of the change
- Link to the related issue
- Description of the implementation or documentation update
- Testing notes where applicable
- Screenshots or diagrams if relevant
- Updated documentation if the change affects architecture or workflow

Use closing keywords when the PR completes an issue:

```text
Closes #issue_number
```

Example:

```text
Closes #10
```

## Pull Request Checklist

Before opening a pull request, check that:

- [ ] The change is focused and does not mix unrelated work
- [ ] The related issue is linked
- [ ] Documentation is updated if needed
- [ ] Tests are added or updated where applicable
- [ ] The project builds locally if code is changed
- [ ] Secrets or private credentials are not committed
- [ ] The change follows repository conventions

## Code Review Principles

Code review is used to improve quality, share knowledge, and protect architecture.

Reviewers should look for:

- Correctness
- Readability
- Maintainability
- Security impact
- Test coverage
- Domain boundary violations
- Unnecessary complexity
- Documentation gaps
- Consistency with existing conventions

Review feedback should be specific, respectful, and actionable.

## Architecture Review Expectations

Changes that affect architecture should be reviewed carefully.

Architecture-impacting changes include:

- New service boundaries
- Cross-service communication
- Database ownership changes
- Security model changes
- Event schema changes
- Infrastructure decisions
- Public API changes
- Authentication or authorization behavior

When needed, create or update an ADR under:

```text
docs/adr/
```

## Documentation Expectations

Documentation is part of the product.

Update documentation when a change affects:

- Architecture
- Security
- Local development
- Service responsibilities
- API behavior
- Infrastructure
- CI/CD
- Development workflow

Documentation should be clear enough for a new contributor or reviewer to understand the system without private context.

## Security Expectations

NutriCore handles sensitive user, communication, file, and document data.

Contributors must not commit:

- Real credentials
- Private keys
- Access tokens
- Refresh tokens
- Production secrets
- Personal user data
- Private documents
- Sensitive test data

Use `.env.example` for safe local configuration examples.

Security-sensitive changes should be documented and reviewed carefully.

## Definition of Done

A task is considered done when:

- The issue acceptance criteria are satisfied
- The implementation or documentation is complete
- The pull request has been reviewed
- Required checks pass
- Related documentation is updated
- The branch is merged into `main`
- The issue is closed or moved to `Done`

## Engineering Principles

NutriCore follows these engineering principles:

- Prefer simple, readable solutions
- Keep changes small and reviewable
- Design around domain boundaries
- Avoid premature distributed complexity
- Treat security as a core concern
- Keep documentation close to the code
- Use pull requests for meaningful changes
- Make decisions explicit through ADRs
