# Project Standards — Example Template

Use this structure when generating STANDARDS.md. Adapt sections to the project's stack. Remove sections that don't apply.

---

## Code Style

- Prefer early returns over nested conditionals.
- Max function length: ~40 lines. Extract when a section needs a comment.
- Imports: group by stdlib, external deps, internal modules — separated by blank lines.
- No commented-out code. Git has history.

## Architecture

- `src/routes/` — HTTP handlers only, no business logic.
- `src/services/` — business logic, called by routes.
- `src/db/` — database queries and migrations.
- Dependencies flow inward: routes → services → db. Never reverse.

## Naming Conventions

- Files: `kebab-case.ts`
- Functions: `camelCase`, verb-first (`getUser`, `validateInput`)
- Types/interfaces: `PascalCase`
- Booleans: prefix with `is`/`has`/`should` (`isActive`, `hasPermission`)
- Constants: `UPPER_SNAKE_CASE`

## Error Handling

- Validate at system boundaries (API input, env vars, file I/O).
- Use custom error classes extending `AppError` with error codes.
- Never swallow errors. Log and re-throw or handle explicitly.

## Testing

- Framework: vitest
- Pattern: arrange-act-assert, one concept per test.
- Prefer real implementations over mocks. Mock only external services.
- Test files: `*.test.ts` co-located with source.

## Git Conventions

- Commits: imperative mood, explain *why* (`Fix race condition in queue` not `fixed stuff`)
- Branches: `feat/`, `fix/`, `chore/` prefixes
- PRs: require 1 review, squash merge to main

## Security

- No hardcoded secrets — use environment variables.
- Sanitize all user input before database queries.
- Parameterized queries only, no string concatenation.

## Documentation

- Don't document obvious code. Comments explain *why*.
- Update README when adding features or changing setup.
- API docs for public interfaces via JSDoc.
