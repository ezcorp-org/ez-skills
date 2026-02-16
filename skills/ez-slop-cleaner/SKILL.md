---
name: ez-slop-cleaner
description: clean up code, review recent changes for bad practices, fix sloppy code, run a code quality pass, or remove slop. Also triggers on "clean up", "code review", "fix slop", "quality check", "pre-commit review", "clean up AI-generated code", "enforce standards", "review my changes", "clean up this mess", "code quality".
allowed-tools:
  - Read
  - Edit
  - Glob
  - Grep
  - Bash
---

# Slop Cleaner

Review and fix code against project standards. Analyzes git changes and cleans up bad practices.

## Step 1: Load Standards

**Non-negotiable first step.** Search for standards in order:

1. `.claude/STANDARDS.md`
2. `STANDARDS.md` (project root)
3. If neither exists, tell the user: "No project standards found. Run `/codebase-standards` first to generate them, or I'll use sensible defaults."
4. If user opts for defaults, load `references/default-standards.md` from this skill.

## Step 2: Determine Scope

Parse arguments to determine what to review:

| Argument | Scope |
|----------|-------|
| `--last N` | Last N commits (default: 1) |
| `--uncommitted` | Staged + unstaged changes |
| `--all` | Uncommitted changes combined with last commit |
| `--staged` | Staged changes only (`git diff --cached`) |
| `--dry-run` | Report issues without fixing (combinable with other flags) |
| `<paths...>` | Narrow review to specific files/directories (combinable with flags) |
| *(no args)* | Uncommitted changes; if none, fall back to last commit |

Get the diff:
```bash
# Uncommitted (staged + unstaged vs HEAD)
git diff HEAD

# Staged only
git diff --cached

# Last N commits
git diff HEAD~N..HEAD

# Uncommitted + last commit (all recent work)
git diff HEAD~1..HEAD   # last commit
git diff HEAD           # uncommitted changes
# Combine both diffs for review

# With path scoping — append paths to any diff command
git diff HEAD -- src/foo.ts src/bar/
```

**Skip list** — exclude from review:
- Binary files
- Lock files: `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
- Generated files: `*.min.js`, `*.min.css`, `dist/`, `build/`, `.next/`
- Files in `.gitignore`

**Empty diff** — if the diff is empty after determining scope, report "No changes to review" and stop.

**Context window protection:**
- If >10 files changed, prioritize the most-changed files first.
- If >20 files changed, warn user: "Large scope (N files). Consider narrowing with path arguments or `--last 1`."

Also read the full file for each changed file — diffs alone miss broader context issues like DRY violations.

## Step 3: Review Against Standards

For each changed file, check for:

- **Standards violations** — anything contradicting project STANDARDS.md
- **Over-engineering** — unnecessary abstractions, premature generalization, feature flags for one-time use, backwards-compat hacks
- **DRY violations** — repeated code within or across changed files
- **Dead code** — unused imports, unreachable branches, commented-out code, unnecessary `_var` renames
- **Security issues** — injection vulnerabilities, hardcoded secrets, unsafe deserialization (OWASP top 10)
- **Naming/formatting inconsistencies** — deviations from project conventions
- **Unnecessary error handling** — catching impossible errors, defensive code for trusted internal calls
- **Sloppy comments** — obvious comments (`// increment i`), stale comments, `// TODO` without context
- **AI slop patterns** — unnecessary type annotations on obvious types, excessive JSDoc on trivial functions, overly defensive null checks where types guarantee presence, verbose variable names that add no clarity over shorter alternatives

## Step 4: Fix Issues

If `--dry-run` is set, skip to Step 5 with the issue list instead of fixing.

Fix each issue directly using Edit tool. For each fix, briefly explain:
- What was changed
- Why (which standard or principle it violates)

Do NOT just report issues — fix them. This is a cleaner, not a linter.

## Step 5: Summary

Present a summary table:

```
| File | Issues Fixed | Categories |
|------|-------------|------------|
| src/foo.ts | 3 | DRY, dead code, naming |
| src/bar.ts | 1 | over-engineering |
```

For `--dry-run`, show "Issues Found" instead of "Issues Fixed" and list what would be changed.

If no issues found, say so.
