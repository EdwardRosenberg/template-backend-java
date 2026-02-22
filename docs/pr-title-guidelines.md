# Pull Request Title Guidelines

## Overview

This repository enforces **Conventional Commits** format for pull request titles.

## PR Title Format

```
<type>(<optional scope>): <description>
```

Pattern: `^(feat|fix|chore|docs|refactor|test|perf|ci|build)(\(.+\))?(!)?: .+`

## Valid Commit Types

| Type | Description |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `chore` | Maintenance tasks, dependency updates |
| `docs` | Documentation changes |
| `refactor` | Code restructuring without behavior change |
| `test` | Adding or updating tests |
| `perf` | Performance improvements |
| `ci` | CI/CD pipeline changes |
| `build` | Build system or dependency changes (`pom.xml`) |

## Examples

### ✅ Valid
```
feat: add health check endpoint
fix(auth): correct token expiry calculation
chore: update Spring Boot to 4.0.1
docs: add API usage examples
refactor: extract validation logic into service
test: add integration tests for order endpoint
ci: add Checkstyle step to CI workflow
build: upgrade Maven compiler plugin to 3.13.0
chore!: rename base package (breaking change)
```

### ❌ Invalid
```
Update README          ← missing type
Add new endpoint       ← missing type
WIP                    ← missing type and description
```

## Breaking Changes

Append `!` after the type (or scope) to signal a breaking change:

```
feat!: remove deprecated v1 endpoints
refactor(api)!: rename response envelope fields
```

## Automated Enforcement

This repository runs a PR title lint check on every PR via `.github/workflows/pr-title-lint.yml`. PRs with non-conforming titles will fail the check and cannot be merged until the title is corrected.

