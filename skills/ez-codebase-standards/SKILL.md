---
name: ez-codebase-standards
description: establish, generate, create, or update coding standards or guidelines for a project. Also use when user mentions "standards", "coding conventions", "style guide", "STANDARDS.md", "project conventions", "code style", "coding rules", "analyze codebase conventions", "document patterns".
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash
  - AskUserQuestion
---

# Codebase Standards

Generate a comprehensive `STANDARDS.md` for a project by analyzing its codebase and asking the user about their preferences.

**Relationship to CLAUDE.md:** STANDARDS.md is a machine-generated reference describing project conventions — consumed by tools like slop-cleaner. CLAUDE.md is human-curated instructions for Claude's behavior. They complement each other; don't duplicate between them.

## Workflow

### Step 1: Scan project structure

Use Glob and Bash to identify languages, frameworks, config files, directory layout, and build tools.

**Edge cases — handle before proceeding:**

- **Empty/new project:** If fewer than 5 source files exist, ask about intended stack and tech choices. Generate aspirational standards based on answers rather than codebase analysis.
- **Existing STANDARDS.md:** Check `.claude/STANDARDS.md` and `STANDARDS.md` at project root. If found, read it and ask the user: "Existing standards found. Update with new findings, or regenerate from scratch?" If updating, preserve any sections marked `<!-- manual -->`.
- **Monorepo:** If multiple distinct apps/packages exist (e.g., `packages/`, `apps/`), ask user: "Generate one unified STANDARDS.md with language-specific subsections, or separate files per package?" Default to unified.

### Step 2: Read representative files

Sample 5-10 files across different areas (entry points, models, utilities, tests, configs). Identify existing patterns for naming, error handling, imports, formatting, and architecture.

### Step 3: Ask user preferences

Use AskUserQuestion. Aim for 1-2 rounds. If the codebase clearly demonstrates a pattern, don't ask about it — just document it.

Focus on areas where the codebase is inconsistent or where conventions aren't obvious:

- Code style preferences not captured by linters (e.g., early returns vs single return, comments philosophy)
- Architecture boundaries and patterns (e.g., layering rules, where business logic lives)
- Testing philosophy (unit vs integration balance, coverage expectations, mocking approach)
- Git workflow (commit message format, branch naming, PR conventions)

### Step 4: Generate STANDARDS.md

Write to the project's `.claude/` directory (create if needed). For the update case, use Edit to modify existing sections.

See `references/example-standards.md` in this skill for the expected format and structure.

Cover these sections:

- **Code Style** — formatting, imports, naming conventions
- **Architecture** — directory structure, layering rules, dependency direction
- **Naming Conventions** — files, functions, variables, types, constants
- **Error Handling** — strategy, error types, logging
- **Testing** — framework, patterns, coverage expectations
- **Git Conventions** — commit messages, branches, PR process
- **Security** — input validation, secrets handling, dependency policy
- **Documentation** — when to document, comment style, API docs

### Step 5: Present summary

Show what was generated and where it was saved.

## Guidelines

- Base standards on what the codebase *actually does*, not idealized practices. Describe the project's conventions, not generic best practices.
- If the project has linter/formatter configs (eslint, prettier, ruff, etc.), reference them rather than duplicating their rules.
- Keep standards actionable and specific. "Use descriptive names" is useless. "Prefix boolean variables with `is`/`has`/`should`" is useful.
- When the codebase is inconsistent, pick the dominant pattern and note it as the standard.
- STANDARDS.md should be under 300 lines. Be concise.
