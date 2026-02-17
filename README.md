# ez-skills

Claude Code marketplace for code quality skills.

## Skills

### ez-slop-cleaner

Reviews recent git changes and fixes bad practices against your project's coding standards.

```
/ez-slop-cleaner                    # review uncommitted changes
/ez-slop-cleaner --last 3           # review last 3 commits
/ez-slop-cleaner --staged           # review staged changes only
/ez-slop-cleaner --dry-run          # report issues without fixing
/ez-slop-cleaner src/auth/ --last 1 # scope to specific paths
```

### ez-codebase-standards

Analyzes your codebase and generates a `STANDARDS.md` by reading existing patterns and asking about your preferences.

```
/ez-codebase-standards
```

## Install

### Using the Skills CLI (Recommended)

Works with Claude Code, Cursor, Codex, OpenCode, and [35+ agents](https://github.com/vercel-labs/skills#supported-agents):

```
npx skills add ezcorp-org/ez-skills
```

Install globally (available across all projects):

```
npx skills add ezcorp-org/ez-skills -g -y
```

Install a specific skill:

```
npx skills add ezcorp-org/ez-skills --skill ez-slop-cleaner
```

### Using Claude Code Plugin Marketplace

Register the marketplace first:

```
/plugin marketplace add ezcorp-org/ez-skills
```

Then install the plugin:

```
/plugin install ezcorp@ez-skills
```

## Usage

1. Run `/ez-codebase-standards` on your project first to generate `STANDARDS.md`
2. Run `/ez-slop-cleaner` after making changes to clean up code against those standards

## License

MIT
