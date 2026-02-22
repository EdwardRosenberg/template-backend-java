# GitHub Copilot Instructions

These instructions define how GitHub Copilot and Copilot agents should operate when making changes to this repository and any repositories derived from `template-backend-java`.

This template inherits all base instructions from `template-base`. The rules below extend them with Java-stack-specific guidance.

## Core Principles

### Clarity Over Cleverness
- Write code that is easy to read, understand, and maintain
- Prefer explicit and straightforward solutions over clever or terse implementations
- Use clear variable and function names that convey intent
- Avoid unnecessary abstractions or overly complex patterns

### Small, Focused Changes
- Keep pull requests small and focused on a single concern
- Each commit should represent a logical, atomic unit of work
- Avoid bundling unrelated changes together

### Scope Discipline
- **No drive-by refactors**: Do not refactor code unrelated to the task at hand
- **No gratuitous formatting**: Only format files you're meaningfully modifying
- **Stay on task**: Resist the urge to "fix" unrelated issues or tech debt

### Observability and Error Handling
- Make all critical-path flows observable with logs, metrics, or traces that provide actionable context.
- Ensure errors are accurately handled and logged with sufficient detail for debugging without leaking sensitive data.
- Apply the "throw early, catch late" principle: validate and throw at the source of failure, and catch at higher boundaries where contextual logging and recovery occur.

## Quality Gates

### Testing Requirements
- **Always** add or update tests when modifying functionality
- Run `mvn -B test` before and after making changes
- If tests exist for the area you're modifying, ensure they pass
- **Exception**: Documentation-only changes do not require tests

### Build and Validation
- Always run `mvn -B clean verify` before finalizing changes
- Fix any build errors or warnings introduced by your changes
- Do not introduce new compiler warnings or Checkstyle violations

## Java-Specific Conventions

### Code Style
- Follow standard Java naming conventions: `CamelCase` for classes, `camelCase` for methods/variables, `UPPER_SNAKE_CASE` for constants
- Use 4-space indentation (enforced by `.editorconfig`)
- Prefer `final` for method parameters and local variables where it improves clarity
- Use `Optional` instead of returning `null` from methods

### Dependencies
- Declare all dependencies in `pom.xml`; do not use `provided` scope unless targeting a container
- Use the Spring Boot parent BOM for version management — do not override managed versions without justification
- Keep test dependencies scoped to `test`

### Maven
- Use the Maven Wrapper (`./mvnw`) for all build commands to ensure consistent Maven versions
- Do not commit `target/` or generated sources
- Checkstyle is enforced — run `mvn checkstyle:check` before submitting a PR

## Agent Delegation

When an agent is delegated a task in this repository, it **must**:

1. **Declare scope upfront**: List the files that will be modified before making any changes.
2. **Follow this task structure**:
   - **Goal**: One-sentence description of what the task accomplishes
   - **Constraints**: What must not change (e.g., "do not modify pom.xml parent version")
   - **Acceptance Criteria**: Measurable conditions for success (e.g., "all tests pass", "no new Checkstyle warnings")
   - **Files in Scope**: Explicit list of files the agent will touch
3. **Self-validate before submitting**:
   - Run `mvn -B clean verify`
   - Confirm no new Checkstyle violations: `mvn checkstyle:check`
   - Confirm PR title follows Conventional Commits format
   - Confirm no files outside the declared scope were modified

## Pull Request Hygiene

### PR Title Requirements

PR titles **must** follow the Conventional Commits format:

```
<type>(<optional scope>): <description>
```

**Valid types:** `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`, `ci`, `build`

### PR Descriptions Must Include
1. **Clear summary** of what changed and why
2. **Type of change** (bug fix, feature, refactor, docs, config, etc.)
3. **Testing performed** — output of `mvn -B test`
4. **Build status** — confirmation that `mvn -B clean verify` passes
5. **Breaking changes** — if any, clearly document what breaks and migration path
6. **Related issues** — reference related issues or PRs

## What to Avoid

### Anti-Patterns
- ❌ Reformatting entire files when making a small change
- ❌ Mixing refactoring with feature work or bug fixes
- ❌ Adding dependencies for functionality easily implemented with standard libraries
- ❌ Over-engineering solutions to simple problems
- ❌ Ignoring existing error handling patterns

## Summary

When in doubt, remember:
1. **Small changes** are easier to review and less risky
2. **`mvn -B clean verify` must pass** before merging
3. **Stay focused** on the task at hand
4. **Follow existing patterns** in the codebase
5. **Document your changes** clearly in the PR

