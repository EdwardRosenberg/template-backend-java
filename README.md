# template-backend-java

**Tier 1 — Java Stack Template**

A stack-level template for Java backend services. It inherits organization-wide governance from [`template-base`](https://github.com/EdwardRosenberg/template-base) and adds Java/Maven-specific build tooling and quality gates — without any application boilerplate.

## What This Template Provides

| File / Directory | Purpose |
|---|---|
| `pom.xml` | Parent POM with `pluginManagement` for compiler, Surefire, and Checkstyle |
| `checkstyle.xml` | Checkstyle rules (Google Style, pragmatic modern-Java adjustments) |
| `.mvn/wrapper/` | Maven Wrapper pinned to Maven 3.9.9 — use `./mvnw` for consistent builds |
| `.github/workflows/ci.yml` | CI that calls the base reusable workflow with `backend-tech-stack: java` |
| `.github/workflows/pr-title-lint.yml` | Delegates PR title validation to the base reusable workflow |
| `.github/dependabot.yml` | Automated dependency updates for GitHub Actions and Maven |
| `sync-config.yml` | Declares `template-base` as the parent for platform-level sync |

## What This Template Does NOT Provide

- A runnable application (no `main` class, no HTTP endpoints)
- Framework-specific dependencies (Spring Boot, Quarkus, Micronaut)
- Application configuration (`application.properties`, `application.yml`)

For a ready-to-run application scaffold, use a **Tier 2 archetype template** such as [`template-backend-java-spring-boot`](https://github.com/EdwardRosenberg/template-backend-java-spring-boot).

## Hierarchy

```
template-base  (Tier 0 — org-wide governance)
    └── template-backend-java  (Tier 1 — this repo, Java stack tooling)
            └── template-backend-java-spring-boot  (Tier 2 — Spring Boot archetype)
```

## Using This Template

### As a Parent POM

In your Tier 2 archetype or project `pom.xml`:

```xml
<parent>
  <groupId>com.example</groupId>
  <artifactId>template-backend-java</artifactId>
  <version>0.0.1-SNAPSHOT</version>
</parent>
```

> **Note:** When published to a Maven repository, reference the published coordinates.
> During local development, `mvn install` this POM first.

### Calling the CI Workflow

The `ci.yml` in this template already calls the base reusable workflow with correct Java inputs. Archetype templates that inherit this template can reference it directly or override inputs as needed.

## Build Commands

```bash
# Validate the POM parses correctly (no compilable source — this is a parent POM)
./mvnw validate

# Check for Checkstyle violations in derived projects
./mvnw checkstyle:check

# Full verify (use in derived projects that have source)
./mvnw -B clean verify
```

## CI/CD

CI runs on every push and pull request to `main` and `develop` branches via GitHub Actions. It calls the reusable workflow from `template-base` with the Java stack configuration.

## Quality Gates

| Gate | Enforced By | Runs When |
|---|---|---|
| Compiler warnings | `maven-compiler-plugin` (`-Xlint:all -Werror`) | `mvn compile` |
| Checkstyle | `maven-checkstyle-plugin` + `checkstyle.xml` | `mvn validate` |
| Unit tests | `maven-surefire-plugin` | `mvn test` |
| PR title lint | `.github/workflows/pr-title-lint.yml` | Every PR |

## Sync

This template participates in platform-level sync via `sync-config.yml`. When `template-base` updates shared paths (`.editorconfig`, CI workflows, copilot instructions), the platform's sync workflow will open a PR against this repo with the changes.

## Contributing

See [docs/pr-title-guidelines.md](docs/pr-title-guidelines.md) for PR title requirements.

All PRs must pass `./mvnw -B clean verify` before merging.

