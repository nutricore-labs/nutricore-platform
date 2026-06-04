# Development Workflow

NutriCore follows a pull-request based development workflow.

## Branch Naming

Use short, descriptive branch names:

```text
feature/auth-service-design
feature/local-orchestration
feature/service-map
docs/security-model
ci/platform-checks
infra/docker-compose
```

## Commit Style

Use conventional commits:

```text
feat(auth): add authentication domain model
docs(architecture): add service communication overview
ci: add GitHub Actions workflow
infra: add local PostgreSQL container
test(auth): add integration tests for login flow
```

## Pull Requests

Each pull request should include:

- Clear description of the change
- Related issue
- Screenshots or diagrams if relevant
- Testing notes
- Documentation updates if needed
  
## Main Branch

The ```main``` branch should always represent a stable state.

Changes should be merged through pull requests after review.
