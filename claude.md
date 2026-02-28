# Claude Code Instructions — template-backend-java

This is a **Tier 1 Java stack template**. It provides Maven build tooling and quality gates inherited by all Java archetype templates (Tier 2). It contains **no runnable application** — only a parent POM, Checkstyle config, and CI wiring.

## Repository Purpose

Provide a shared Maven parent POM with `pluginManagement` for compiler, Surefire, and Checkstyle. Archetype templates like `template-backend-java-spring-boot` declare this as their parent and inherit all quality gates.

## Tech Stack

- **Language**: Java 25
- **Build tool**: Maven (via Maven Wrapper `./mvnw`)
- **Linter**: Checkstyle (Google Style, pragmatic adjustments — see `checkstyle.xml`)
- **Test framework**: JUnit 5 + AssertJ + Mockito (managed in `dependencyManagement`)
- **CI**: Calls `template-base` reusable workflow with `backend-tech-stack: java`

## Build Commands

```bash
./mvnw validate              # Verify POM parses correctly
./mvnw checkstyle:check      # Check style violations (for derived projects with source)
./mvnw -B clean verify       # Full build + test (use in derived projects)
```

This is a parent POM (`<packaging>pom</packaging>`) — there is no compilable source in this repo. `./mvnw validate` is the primary validation command.

## What You Can Change

- `pom.xml` — Plugin versions, dependency versions in `dependencyManagement`, compiler flags
- `checkstyle.xml` — Checkstyle rules (changes affect all Java archetype templates)
- `sync-config.yml` — Sync paths and parent/child declarations
- `.github/workflows/ci.yml` — CI configuration for this template
- `.github/dependabot.yml` — Dependency update configuration

## What You Must NOT Change

- Do not add application source code (`src/main/java/`)
- Do not add framework-specific dependencies (Spring Boot, Quarkus, etc.)
- Do not add `application.properties` or `application.yml`
- Do not change `<packaging>` from `pom`

## Java Conventions

- **Java version**: 25 — use modern language features (records, sealed classes, pattern matching)
- **Compiler flags**: `-Xlint:all -Werror` — all warnings are errors
- **No star imports**: Checkstyle enforces `AvoidStarImport`
- **No unused imports**: Checkstyle enforces `UnusedImports`
- **Braces required**: Always use braces for `if`/`else`/`for`/`while` blocks
- **Naming**: Follow standard Java conventions (`CamelCase` for types, `camelCase` for methods/variables, `UPPER_SNAKE_CASE` for constants)
- **Javadoc**: Warn on missing public method Javadoc but don't fail the build

## Maven Conventions

- Dependency **versions** belong in `<properties>` and are referenced via `${...}`
- New dependencies go in `<dependencyManagement>` — children opt in by declaring the dependency without a version
- Plugin configuration goes in `<pluginManagement>` — children inherit automatically
- Always use `-B` (batch mode) in CI commands to suppress download progress output

## Quality Gates

| Gate | Enforced By | Command |
|---|---|---|
| Compiler warnings as errors | `maven-compiler-plugin` | `./mvnw compile` |
| Checkstyle | `maven-checkstyle-plugin` | `./mvnw validate` |
| Unit tests | `maven-surefire-plugin` | `./mvnw test` |
| PR title lint | `pr-title-lint.yml` workflow | Automatic on PR |

## Sync Awareness

**Parent**: `template-base` (Tier 0)
**Children**: `template-backend-java-spring-boot` (Tier 2)

Files from `template-base` are synced via PRs: `.editorconfig`, `.gitignore`, CI workflows, copilot-instructions, issue/PR templates.

When modifying `pom.xml` properties or plugin versions, consider the impact on `template-backend-java-spring-boot` which inherits this parent POM.

## Versioning

When making changes, bump the `<version>` in `pom.xml` as part of your commit using semver:

- **PATCH** (e.g., `0.0.1` → `0.0.2`) — dependency version updates, checkstyle rule tweaks, doc changes
- **MINOR** (e.g., `0.0.2` → `0.1.0`) — new plugins in `pluginManagement`, new managed dependencies
- **MAJOR** (e.g., `0.1.0` → `1.0.0`) — breaking changes like requiring a new Java version or removing managed dependencies

Always update the version in the same commit as your functional changes.

## PR Conventions

- Titles must follow Conventional Commits: `<type>(<scope>): <description>`
- Run `./mvnw validate` before submitting
- If modifying `checkstyle.xml`, verify rules against the Spring Boot archetype: `cd ../template-backend-java-spring-boot && ./mvnw checkstyle:check`
