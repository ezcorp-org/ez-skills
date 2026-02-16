# Default Coding Standards

Sensible defaults when no project-specific STANDARDS.md exists.

## Code Style

- Prefer early returns over deeply nested conditionals.
- Keep functions short — if it needs a comment explaining a section, extract that section.
- No commented-out code. Delete it; git has history.
- No obvious comments. Comments explain *why*, not *what*.
- Don't add unnecessary type annotations where the type is obvious from context (e.g., `const name: string = "foo"`).
- Don't add JSDoc to trivial functions whose signature already explains everything.

## DRY

- If you're copying code, stop and refactor. Three similar lines is fine; three similar blocks is not.
- BUT: don't create premature abstractions for one-time operations. Wait for the third occurrence.

## Architecture

- Keep it simple. Don't add layers, abstractions, or indirection until complexity demands it.
- No feature flags or backwards-compatibility shims when you can just change the code.
- Don't design for hypothetical future requirements.

## Error Handling

- Only validate at system boundaries (user input, external APIs, file I/O).
- Trust internal code and framework guarantees. Don't add defensive checks for impossible states.
- No empty catch blocks. Handle errors or let them propagate.
- Don't add null checks where types guarantee the value is present.

## Naming

- Names should be descriptive and unambiguous.
- Booleans: prefix with `is`, `has`, `should`, `can`.
- Functions: start with a verb describing what they do.
- No abbreviations unless universally understood (`id`, `url`, `config`).
- Avoid verbose names that add no clarity over shorter alternatives (e.g., `userAccountData` vs `user`).

## Testing

- Tests describe behavior, not implementation.
- One assertion concept per test.
- No testing of private methods — test through public API.
- Prefer real implementations over mocks when feasible.

## Git

- Commit messages: imperative mood, explain *why* not *what*.
- Keep commits atomic — one logical change per commit.
- No merge commits in feature branches (rebase).

## Security

- Never hardcode secrets, tokens, or credentials.
- Validate and sanitize all external input.
- Use parameterized queries for database access.
- Keep dependencies updated; audit for known vulnerabilities.

## Documentation

- Don't add docstrings/comments to code you didn't change.
- README updates only when adding new features or changing setup.
- API docs for public interfaces only.
